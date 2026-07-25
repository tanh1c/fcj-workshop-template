---
title: "Dataset Step 1: Define the 17-Feature Contract"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

`preprocessing/processing_script.py` converts each trajectory into this ordered contract:

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

The target `label` is stored in training CSVs but excluded from inference input and Model Monitor baseline features. Keep this order synchronized across Processing, Training, Lambda, and monitoring.

Expected labels are `safe`, `require_review`, `wrong_tool`, `hallucinated_success`, `risky`, and `failed`.
