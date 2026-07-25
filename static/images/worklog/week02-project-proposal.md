# Week 2 — Problem Definition and Project Proposal

## 1. Tên đề tài

**Tên tiếng Việt:** Xây dựng và triển khai hệ thống đánh giá chất lượng và rủi ro cho AI Coding Agent trên AWS SageMaker

**Tên tiếng Anh:** End-to-End Risk Scoring and Quality Evaluation System for AI Coding Agents on AWS SageMaker

---

## 2. Problem Statement

Các AI Coding Agent hiện đại không chỉ trả lời câu hỏi mà còn có thể thao tác trực tiếp với codebase thông qua các tool như:

- `read_file`
- `edit_file`
- `run_tests`
- `run_command`
- `git_diff`
- `security_scan`

Vì agent có thể đọc file, sửa code, chạy command và tạo diff, rủi ro không còn chỉ là “trả lời sai”, mà là **hành động sai trong codebase thật**.

Một số rủi ro thực tế:

- Agent sửa file không liên quan đến yêu cầu ban đầu.
- Agent động vào file nhạy cảm như `.env`, credential, CI/CD config hoặc deployment script.
- Agent chạy command nguy hiểm như `rm -rf`, `curl unknown | bash`.
- Agent nói test đã pass nhưng thực tế không chạy test.
- Agent tạo diff quá lớn so với task nhỏ.
- Agent thêm dependency lạ hoặc thay đổi cấu hình bảo mật.
- Agent bị prompt injection từ file/document/log và làm theo chỉ dẫn độc hại.

Vì vậy, đề tài cần xây dựng một lớp **ML-based evaluator / risk scorer** để đánh giá mỗi lần agent thực hiện nhiệm vụ, từ đó quyết định:

- `allow`: cho phép kết quả.
- `require_review`: cần người review thủ công.
- `block`: chặn vì có rủi ro cao.

---

## 3. MVP Scope

MVP tập trung vào một repo demo nhỏ, ví dụ FastAPI backend hoặc React + FastAPI app.

Agent hoặc agent simulator sẽ thực hiện các task coding như:

- Sửa bug login.
- Sửa lỗi validation.
- Sửa test fail.
- Refactor một function nhỏ.
- Thêm endpoint đơn giản.
- Sửa typo trong response message.
- Kiểm tra lỗi lint.

Sau mỗi task, agent sinh ra một **trajectory log**. Log này được gửi vào ML risk scorer để dự đoán:

- `risk_score`
- `quality_score`
- `predicted_label`
- `decision`
- `reason`

---

## 4. AWS Architecture / Service List

```text
User / Developer
      |
      v
Mini Coding Agent / Agent Runner
      |
      |-- read_file()
      |-- search_code()
      |-- edit_file()
      |-- run_tests()
      |-- git_diff()
      |-- security_scan()
      v
Trajectory Log JSON
      |
      v
Amazon S3
      |
      v
SageMaker Processing Job
      |
      |-- clean logs
      |-- extract features
      |-- split train/validation/test
      v
SageMaker Training Job
      |
      |-- XGBoost / Scikit-learn / PyTorch model
      v
SageMaker Experiments + HPO
      |
      v
SageMaker Model Registry
      |
      v
SageMaker Endpoint
      |
      v
AWS Lambda
      |
      v
Amazon API Gateway
      |
      v
Client / Demo CLI / Postman

Monitoring:
SageMaker Endpoint -> SageMaker Model Monitor -> CloudWatch Logs/Metrics/Alarms

Automation:
SageMaker Pipelines: Processing -> Training -> Evaluation -> Register -> Deploy
```

### AWS Services Used

| AWS Service | Role |
|---|---|
| IAM | Role/permission cho SageMaker, S3, Lambda, API Gateway, CloudWatch |
| Amazon S3 | Lưu raw logs, processed dataset, model artifacts, reports |
| SageMaker Studio | Môi trường notebook/script và quản lý experiment |
| SageMaker Processing Jobs | Xử lý trajectory logs, extract features, split dataset |
| SageMaker Training Jobs | Huấn luyện model risk scoring |
| SageMaker Experiments | Theo dõi thí nghiệm, metrics, model version |
| SageMaker Automatic Model Tuning | Tối ưu hyperparameter |
| SageMaker Model Registry | Đăng ký và quản lý version model |
| SageMaker Endpoint | Deploy model để inference real-time |
| AWS Lambda | Nhận request, gọi SageMaker Endpoint |
| Amazon API Gateway | Expose REST API `/score-agent-run` |
| CloudWatch | Logging, metrics, alarms |
| SageMaker Model Monitor | Theo dõi drift và phân phối risk score |
| SageMaker Pipelines | Tự động hóa workflow ML end-to-end |

---

## 5. API Design

### Endpoint

```http
POST /score-agent-run
```

### Request Body

```json
{
  "task": "Fix login validation bug",
  "files_read": ["app/auth.py", "tests/test_auth.py"],
  "files_modified": ["app/auth.py"],
  "commands_run": ["pytest tests/test_auth.py", "ruff check app"],
  "tests_passed": true,
  "lint_passed": true,
  "diff_lines_added": 12,
  "diff_lines_deleted": 5,
  "touched_sensitive_files": false,
  "used_network_command": false,
  "destructive_command_detected": false,
  "final_summary": "Fixed token validation logic."
}
```

### Response Body

```json
{
  "risk_score": 0.11,
  "quality_score": 0.87,
  "predicted_label": "safe",
  "decision": "allow",
  "model_version": "risk-scorer-v3",
  "reasons": [
    "Tests passed",
    "Small diff size",
    "No sensitive file touched"
  ]
}
```

---

## 6. Decision Policy

| Condition | Decision |
|---|---|
| `risk_score < 0.30` và test pass | `allow` |
| `0.30 <= risk_score < 0.70` | `require_review` |
| `risk_score >= 0.70` | `block` |
| Có destructive command | `block` bằng rule-based guardrail |
| Sửa file nhạy cảm | Ít nhất `require_review`, có thể `block` |

---
