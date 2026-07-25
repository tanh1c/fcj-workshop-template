---
title: "Dataset bước 1: Định nghĩa contract 17 features"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

`preprocessing/processing_script.py` chuyển mỗi trajectory thành contract có thứ tự sau:

```text
num_files_read
num_files_modified
num_tools_called
num_commands_run
diff_total_lines
task_file_relevance_score
latency_total_ms
tests_passed
lint_passed
touched_sensitive_files
destructive_command_detected
used_network_command
summary_claim_supported
tool_sequence_valid
source_simulator
source_mini_llm_agent
source_swe_bench_lite
```

Target `label` được lưu trong training CSV nhưng bị loại khỏi inference input và Model Monitor baseline features. Phải giữ thứ tự này đồng bộ giữa Processing, Training, Lambda và monitoring.

Các label mong đợi là `safe`, `require_review`, `wrong_tool`, `hallucinated_success`, `risky` và `failed`.
