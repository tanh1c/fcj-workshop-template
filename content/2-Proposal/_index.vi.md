---
title: "Đề xuất dự án"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Tổng quan dự án

Đề xuất ban đầu là xây dựng **hệ thống đánh giá chất lượng và rủi ro end-to-end cho AI Coding Agent trên AWS SageMaker**. Dự án hoàn thiện hiện đã chứng minh workflow này bằng managed training, evaluation, governance, serving evidence và monitoring acceptance, đồng thời vẫn giữ human review và deterministic safety rules làm lớp kiểm soát cho các quyết định rủi ro cao.

Coding agent có thể đọc source file, sửa code, chạy command và test, rồi tóm tắt kết quả. Những hành động đó tạo ra bằng chứng vận hành hữu ích, nhưng cũng phát sinh rủi ro khi agent chạm file nhạy cảm, thử destructive command, bỏ qua verification hoặc claim thành công mà thiếu bằng chứng. Project ghi lại trajectory, chuyển dữ liệu thành contract gồm 17 features dùng chung, rồi kết hợp XGBoost risk score với hard safety signals.

## Vấn đề cần giải quyết

Một câu trả lời như “tests passed” không đủ đáng tin nếu trajectory không có test command, chứa thay đổi không liên quan hoặc có hành động không an toàn. Vì vậy, project giải quyết câu hỏi:

> Làm thế nào để đánh giá chất lượng và rủi ro của một lần chạy AI coding agent từ bằng chứng hành vi thông qua một AWS ML workflow có governance?

Quá trình đánh giá tập trung vào việc agent có:

- Đọc và sửa file liên quan đến task.
- Verify thay đổi bằng test hoặc lint.
- Dùng tool sequence hợp lệ và có bằng chứng hỗ trợ claim cuối.
- Chạm file nhạy cảm, dùng network command hoặc thử destructive command.

## Mục tiêu và phạm vi đã triển khai

Phần triển khai hoàn thiện gồm:

- Agent-neutral trajectory schema dùng JSON và JSONL records.
- Dữ liệu có kiểm soát từ simulator, Mini LLM Coding Agent và nguồn SWE-bench Lite pseudo-trajectory.
- Contract 17 features dùng chung cho Processing, Training, inference và Model Monitor.
- Amazon S3 lưu raw data, processed splits, reports, captured requests và model artifacts.
- SageMaker Processing và bước preprocess trong Pipeline để tạo train, validation và test datasets.
- Managed SageMaker XGBoost Training, held-out evaluation, Experiments và automatic model tuning tại `us-east-1`.
- SageMaker Pipeline có registration gate `risky_recall >= 0.85`.
- Các version trong SageMaker Model Registry được giữ ở trạng thái `PendingManualApproval`.
- Serving acceptance lịch sử qua SageMaker Endpoint ngắn hạn, Lambda và API Gateway tại `ap-southeast-1`.
- Endpoint Data Capture, CloudWatch operational/safety metrics, alarms, dashboard evidence và Model Monitor acceptance.
- Quy trình cleanup cho các tài nguyên trả phí ngắn hạn.
- Một External/OOD diagnostic local trên 40 public trajectories lấy mẫu `20 + 20` từ các revision được pin với seed `42`, dùng hai annotator AI-assisted A/B độc lập cùng adjudication và frozen 17-feature model mà không retrain, tune threshold hoặc gọi AWS.

### Ngoài phạm vi

- Claim chất lượng production từ dataset hiện tại chủ yếu là synthetic.
- Tự động approve hoặc deploy model chỉ vì model vượt qua một metric gate.
- Thay thế human review bằng ML score.
- Duy trì Endpoint real-time hoặc tài nguyên demo trả phí liên tục.
- Xây dựng trọn vẹn enterprise CI/CD hoặc multi-account production platform.

## Kiến trúc AWS đã triển khai

![Kiến trúc AWS split-Region hoàn thiện cho AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-architecture.webp)

*Figure 1. Kiến trúc split-Region đã hoàn thiện. Managed ML và governance evidence được giữ tại `us-east-1`; serving demo ngắn hạn cùng operational evidence đã được nghiệm thu tại `ap-southeast-1`.*

Project không thể chạy hoàn toàn tại `ap-southeast-1` vì quota SageMaker Training cần thiết không được cấp tại Region này. Quota cho `1 x ml.m5.large` được duyệt tại `us-east-1`, nên managed Training và các bước phụ thuộc gồm evaluation, HPO, Pipeline, Model Registry và Model Monitor được chạy tại `us-east-1`. Processing, Studio và serving evidence ngắn hạn trước đó vẫn nằm tại `ap-southeast-1`.

| Region | Workload đã nghiệm thu |
|---|---|
| `us-east-1` | Managed XGBoost Training, held-out evaluation artifacts, Experiments/HPO, SageMaker Pipeline, Model Registry và Model Monitor acceptance. |
| `ap-southeast-1` | SageMaker Studio và Processing lịch sử, cùng Endpoint ngắn hạn, Lambda, API Gateway, Data Capture và CloudWatch serving evidence. |

Pipeline chủ động dừng tại bước registration có governance. Model packages `agent-risk-scorer/1` và `agent-risk-scorer/2` đã hoàn tất với trạng thái `PendingManualApproval`; không package nào được approve hoặc deploy lên Endpoint.

Endpoint/API lịch sử sử dụng một XGBoost artifact được train local trước đó. Diagram thể hiện đây là serving track riêng để không tạo hiểu nhầm rằng managed Registry artifacts đã được deploy. Endpoint, API, Lambda, monitoring schedule, dashboard, alarms và Studio app đã được cleanup sau khi thu thập evidence; S3 artifacts, reports, captured requests, logs và metrics vẫn được giữ làm bằng chứng lâu dài.

## Vòng đời dữ liệu và ML

![Vòng đời dữ liệu và ML có governance cho AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-ml-flow.webp)

*Figure 2. Vòng đời dữ liệu và ML đã triển khai với managed Training, held-out evaluation, HPO, risky-recall gate và manual approval trước mọi release operation riêng biệt.*

Vòng đời có governance gồm:

1. Simulator, Mini LLM Coding Agent và SWE-bench Lite adapter tạo trajectory JSON/JSONL records.
2. Raw records được lưu trong Amazon S3.
3. SageMaker Processing chuyển trajectory thành contract 17 features dùng chung và tạo train, validation, test CSV.
4. SageMaker XGBoost `1.7-1` chạy managed Training trên `1 x ml.m5.large` tại `us-east-1`.
5. Held-out evaluation ghi lại accuracy, macro F1, risky recall, risky false-negative rate và hallucinated-success recall.
6. SageMaker Experiments và HPO gồm ba child jobs lưu tuning evidence cùng selected hyperparameters.
7. Pipeline kiểm tra điều kiện `risky_recall >= 0.85`.
8. Execution đạt gate sẽ register model với trạng thái `PendingManualApproval`; execution không đạt sẽ dừng mà không register.
9. Manual approval và deployment là các release decision riêng. Pipeline không có bước deploy Endpoint.

## Contract 17 features dùng chung

Model-ready record dùng 17 fields sau:

```json
{
  "num_files_read": 2,
  "num_files_modified": 1,
  "num_tools_called": 4,
  "num_commands_run": 1,
  "diff_total_lines": 7,
  "task_file_relevance_score": 0.95,
  "latency_total_ms": 1250,
  "tests_passed": 1,
  "lint_passed": 1,
  "touched_sensitive_files": 0,
  "destructive_command_detected": 0,
  "used_network_command": 0,
  "summary_claim_supported": 1,
  "tool_sequence_valid": 1,
  "source_simulator": 0,
  "source_mini_llm_agent": 1,
  "source_swe_bench_lite": 0
}
```

Một response đại diện từ `POST /score-agent-run` là:

```json
{
  "risk_score": 0.6003,
  "quality_score": 0.3997,
  "predicted_label": "failed",
  "decision": "require_review"
}
```

Model score hỗ trợ quá trình review chứ không phải safety control duy nhất. Destructive command và sensitive-file behavior vẫn chịu deterministic rules ngay cả khi model confidence thấp.

## AWS Services đã sử dụng

| AWS Service | Cách sử dụng đã triển khai | Lý do lựa chọn |
|---|---|---|
| Amazon S3 | Lưu raw trajectories, processed splits, evaluation reports, captured requests, monitoring reports và model artifacts. | Object storage bền vững cung cấp một giao diện chi phí thấp cho dữ liệu và artifacts trong toàn bộ ML lifecycle. |
| Amazon SageMaker Studio | Hỗ trợ quá trình development lịch sử và thu thập Console evidence. | Cung cấp workspace tích hợp AWS để development và kiểm tra SageMaker resources. |
| SageMaker Processing | Thực hiện feature engineering, dataset splitting, evaluation processing và monitoring baseline. | Chạy các managed data jobs có thể lặp lại với S3 input/output mà không cần duy trì processing server. |
| SageMaker Training | Chạy managed XGBoost Training tại `us-east-1`. | Cung cấp isolated managed XGBoost job với configuration, metrics, artifacts và billable duration được ghi nhận. |
| SageMaker Experiments và HPO | Theo dõi experiment, tuning job, child jobs, metrics và selected hyperparameters. | Giữ metadata có thể so sánh giữa các trials đồng thời giới hạn automatic tuning ở ba child jobs chạy tuần tự. |
| SageMaker Pipelines | Orchestrate preprocess, train, evaluate, metric check và conditional registration. | Giúp ML workflow có thể tái lập và thực thi risky-recall registration gate. |
| SageMaker Model Registry | Giữ model packages đã hoàn tất trong `agent-risk-scorer` với yêu cầu manual approval. | Bổ sung model versioning và governance boundary giữa registration, approval và deployment. |
| SageMaker Endpoint và Runtime | Cung cấp historical real-time inference ngắn hạn bằng local artifact trước đó. | Hỗ trợ synchronous scoring cho live API demo; Endpoint chỉ tồn tại ngắn hạn để kiểm soát chi phí. |
| SageMaker Model Monitor | Tạo accepted baseline, data-quality execution, violations và CloudWatch data metrics. | Cung cấp managed baseline comparison và drift evidence tích hợp với SageMaker cùng CloudWatch. |
| AWS Lambda | Chuyển trajectory thành feature payload, gọi SageMaker Runtime và phát Embedded Metric Format safety metrics. | Serverless execution phù hợp với request-driven adapter và không cần duy trì API server. |
| Amazon API Gateway | Expose HTTP API route lịch sử `POST /score-agent-run`. | Cung cấp managed HTTP entry point cho Lambda mà không cần web server riêng. |
| Amazon CloudWatch | Giữ native service metrics, `AgentRiskScorer` metrics, logs, dashboard evidence và bảy alarms đã tắt actions. | Tập trung AWS-native logs, metrics, dashboards và alarms để vận hành scoring path. |
| AWS IAM | Tách CLI, SageMaker execution và Lambda execution permissions theo least privilege. | Service-specific roles giảm credential exposure và giới hạn từng component ở các actions cần thiết. |

## Evidence triển khai đã nghiệm thu

| Hạng mục | Kết quả đã nghiệm thu |
|---|---|
| Managed Training | Job `agent-risk-xgboost-1784625353` hoàn tất trên `1 x ml.m5.large` với SageMaker XGBoost `1.7-1`; training time và billable time đều là 140 giây. |
| Held-out evaluation | 183 test rows; accuracy `1.00`, macro F1 `1.00`, risky recall `1.00`, risky false-negative rate `0.00` và hallucinated-success recall `1.00`. |
| HPO | Random tuning job `agent-risk-hpo-1784643415` hoàn tất ba child jobs và lưu selected configuration trong SageMaker Experiments. |
| Pipeline và Registry | Conditional registration hoàn tất; package versions `/1` và `/2` vẫn ở trạng thái `PendingManualApproval`. |
| Serving API | Endpoint, Lambda và API Gateway ngắn hạn trả risk-aware decision qua `POST /score-agent-run`. |
| Data Capture | Capture JSON input và output với tỷ lệ lấy mẫu 100% vào S3. |
| Model Monitor | Baseline gồm 854 rows và 17 features; accepted run có trạng thái `CompletedWithViolations` và ghi nhận drift thực ở `diff_total_lines` cùng `latency_total_ms`. |
| CloudWatch | Phát Lambda EMF metrics và 101 Model Monitor data metrics; dashboard cùng bảy alarms được nghiệm thu trước khi cleanup. |
| External/OOD diagnostic | 40 public samples; A/B full-axis agreement `3/40 = 7.5%`; 37 mẫu được adjudicate; final labels `failed=28`, `safe=10`, `risky=2`; accuracy `0.0500`, macro F1 `0.1212`, risky recall `0.5000` và risky FNR `0.5000`. Frozen model được đánh giá local mà không retrain hoặc gọi AWS. |

Các held-out scores hoàn hảo đến từ labels chủ yếu synthetic và được thiết kế tách biệt rõ. Kết quả chứng minh managed ML/MLOps workflow chạy đúng; chúng không chứng minh production accuracy hoặc khả năng generalize sang repository thực tế.

## Tiến độ và trạng thái triển khai

| Giai đoạn | Công việc đã hoàn tất |
|---|---|
| Foundation | Xác định bài toán risk scoring, trajectory schema, safety rules, IAM boundaries và S3 structure. |
| Data | Xây simulator, Mini LLM Agent, SWE-bench Lite adapter, feature extraction và Processing outputs. |
| Managed ML | Hoàn tất Training, held-out evaluation, Experiments, HPO, Pipeline và conditional Registry integration tại `us-east-1`. |
| Serving | Nghiệm thu Endpoint, Lambda và API Gateway ngắn hạn tại `ap-southeast-1` bằng historical local artifact. |
| Monitoring | Nghiệm thu Data Capture, EMF metrics, CloudWatch dashboard/alarms và Model Monitor evidence. |
| Delivery | Thu thập evidence, cleanup paid resources và hoàn thiện report, demo runbook cùng bilingual workshop. |

## Ngân sách

Dự án được tài trợ bằng AWS promotional credit. Cả hai Cost Explorer view được cung cấp đều hiển thị `$0.00`; đây là kết quả net được hiển thị, không phải bằng chứng rằng các tài nguyên AWS không có chi phí kinh tế. Các Billing view đã chụp không cung cấp tổng gross usage cost hoặc breakdown theo service đủ tin cậy, nên báo cáo không ước tính hai số liệu này. Số dư, currency, status và ngày hết hạn credit cũng được lược bỏ vì không thể chép lại chắc chắn từ ảnh Credits được cung cấp. Các ảnh Billing được giữ làm submission evidence riêng thay vì công bố trên website này.

| Bằng chứng hoặc kiểm soát chi phí | Phạm vi ghi nhận |
|---|---|
| Managed Training | `1 x ml.m5.large` trong 140 giây billable. |
| HPO | Giới hạn ở ba child jobs chạy tuần tự. |
| Historical serving | Real-time Endpoint `ml.t2.medium` tồn tại ngắn hạn. |
| Processing và monitoring | Processing và Model Monitor jobs chỉ chạy khi cần acceptance evidence. |
| Cleanup | Endpoint, Lambda, API Gateway, Model Monitor schedule, dashboard, alarms và Studio apps đã được xóa sau verification. |
| Tài nguyên được giữ lại | Chỉ giữ các S3 artifacts, reports, captures, logs và metrics bền vững có chi phí thấp. |

## Kiểm soát chi phí và bảo mật

- Các lần chạy Processing, Training, HPO, Pipeline, Endpoint và Model Monitor có phí chỉ được thực hiện khi cần acceptance evidence.
- Endpoint và serving stack chỉ tồn tại ngắn hạn; dashboard, alarms, monitoring schedule và Studio app cũng được xóa sau verification.
- S3 artifacts, reports, captures, logs và metrics có chi phí thấp được giữ lại làm evidence.
- Mini Agent không có unrestricted generic shell tool.
- CLI, SageMaker và Lambda dùng các IAM identity hoặc role riêng.
- Registry approval luôn là manual; metric gate không bao giờ tự cho phép production deployment.

## Rủi ro, giới hạn và phương án xử lý

| Rủi ro hoặc giới hạn | Phương án xử lý |
|---|---|
| Labels chủ yếu synthetic và được thiết kế dễ phân tách có thể làm metrics cao hơn thực tế. | Xem kết quả là workflow validation; mức giảm từ synthetic macro F1 `1.00` xuống external `0.1212` xác nhận distribution shift đáng kể. |
| External labels còn không chắc chắn. | Xem pilot 40 mẫu là diagnostic vì labels là multi-agent/AI-assisted và full-axis A/B agreement chỉ `7.5%`; cần independent human labels trước mọi production claim. |
| Chỉ có hai External/OOD samples mang nhãn risky. | Báo cáo Wilson 95% interval rộng `[0.0945, 0.9055]` cho risky recall; không tune threshold từ pilot này. |
| Domain hoặc behavior drift có thể làm giảm độ tin cậy. | Review parser/default-value behavior, thu thập dataset đại diện lớn hơn và chỉ thực hiện governed retraining/evaluation sau human labeling. |
| ML có thể bỏ sót hành động nguy hiểm. | Giữ hard rules cho destructive commands, sensitive files và yêu cầu human review cho risky decisions. |
| Cross-Region resources có thể tạo hiểu nhầm về deployment path chưa xảy ra. | Tách managed governance và historical serving thành hai tracks; deployment chỉ diễn ra qua release operation rõ ràng. |
| Real-time và monitoring resources tạo chi phí liên tục. | Dùng acceptance window ngắn, confirmation gate và cleanup ngay sau kiểm tra. |
| Model/runtime dependency versions có thể khác nhau. | Pin evaluation và inference environment tương thích cho các managed release sau này. |

## Deliverables

- Proposal song ngữ, final report, architecture description, demo script và evidence inventory.
- Agent-neutral trajectory schema và ba nguồn dữ liệu có kiểm soát.
- Contract 17 features dùng chung cho processing, training, inference và monitoring.
- Managed Training, evaluation, Experiments/HPO, Pipeline và Registry evidence.
- Historical serverless scoring API và real-time inference evidence.
- Data Capture, Model Monitor, CloudWatch dashboard, metrics và alarm evidence.
- Cleanup procedures và durable artifacts đã được giữ lại.
- External/OOD machine-readable evidence gồm annotation coverage, per-source metrics, rule-only baseline và một risky false negative đã redacted; raw public trajectories cùng annotation packages không được đưa lên website.

## Hướng phát triển

Hướng phát triển tiếp theo nên bắt đầu bằng dataset trajectory đại diện lớn hơn, independent human labeling và review parser/default-value behavior. Chỉ sau đó project mới nên thực hiện governed retraining/evaluation, calibration hoặc cost-sensitive learning, dùng runtime images được pin tương thích và xây quy trình release có review để deploy model đã được approve từ Registry. Việc áp dụng production cũng cần CI/CD controls, security review, access auditing và cost governance vượt ngoài phạm vi kỳ thực tập này.

## Tài liệu tham khảo đã tìm hiểu

- [Amazon SageMaker Processing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)
- [XGBoost algorithm with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)
- [SageMaker real-time inference](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html)
- [Invoke SageMaker endpoints](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-test-endpoints.html)
- [Using Lambda with API Gateway](https://docs.aws.amazon.com/lambda/latest/dg/services-apigateway.html)
- [HTTP APIs in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html)
- [IAM roles for AWS services](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
