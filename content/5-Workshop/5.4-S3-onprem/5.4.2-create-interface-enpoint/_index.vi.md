---
title: "Dataset bước 2: Sinh Simulator và Mini-Agent logs"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

Sinh labeled simulator records có thể tái lập bằng fixed seed:

```bash
python data_generation/generate_scenarios.py \
  --count 1200 \
  --output data_generation/sample_trajectories.jsonl \
  --seed 42
```

Chỉ chạy restricted Mini LLM Agent trong controlled demo repository:

```bash
python agent/agent_runner.py \
  --task "Fix login validation bug" \
  --output runs/run_login_local.json
```

Agent tool policy chặn destructive commands và unsafe paths. Trajectory logger ghi tool results thực, file đã deduplicate, commands, test/lint states, diff lines, safety flags và mức độ hỗ trợ cho final summary.

Output này chưa có supervised `label`; dùng nó cho local/API scoring hoặc thêm human label được review độc lập trước khi đưa vào training JSONL. Kiểm tra JSON/JSONL trước khi upload và loại bỏ credentials, personal data hoặc source content không liên quan.
