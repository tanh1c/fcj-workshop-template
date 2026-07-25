---
title: "Dataset bước 3: Adapt và merge các nguồn"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

Tạo bounded SWE-bench Lite pseudo-trajectory sample rồi merge với simulator set:

```bash
python data_generation/swe_bench_adapter.py \
  --limit 20 \
  --output data_generation/swe_bench_lite_trajectories.jsonl

python data_generation/merge_trajectories.py \
  --inputs \
    data_generation/sample_trajectories.jsonl \
    data_generation/swe_bench_lite_trajectories.jsonl \
  --output data_generation/combined_trajectories.jsonl
```

Shared feature contract hỗ trợ ba indicator loại trừ nhau: `source_simulator`, `source_mini_llm_agent` hoặc `source_swe_bench_lite`. Supervised merge này chứa simulator và SWE-bench Lite; chỉ thêm Mini Agent records sau khi gắn labels được review độc lập.

SWE-bench adapter tạo agent-neutral pseudo-trajectories để bao phủ workflow; nó không chứng minh risk model generalize sang hành vi coding-agent thực.
