---
title: "Dataset Step 2: Generate Simulator and Mini-Agent Logs"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

Generate reproducible labeled simulator records with a fixed seed:

```bash
python data_generation/generate_scenarios.py \
  --count 1200 \
  --output data_generation/sample_trajectories.jsonl \
  --seed 42
```

Run the restricted Mini LLM Agent only in the controlled demo repository:

```bash
python agent/agent_runner.py \
  --task "Fix login validation bug" \
  --output runs/run_login_local.json
```

The agent tool policy blocks destructive commands and unsafe paths. Its trajectory logger records actual tool results, deduplicated files, commands, test/lint states, diff lines, safety flags, and whether the final summary is supported.

This output has no supervised `label`; use it for local/API scoring or add an independently reviewed human label before including it in training JSONL. Inspect JSON/JSONL before upload and remove any credentials, personal data, or unrelated source content.
