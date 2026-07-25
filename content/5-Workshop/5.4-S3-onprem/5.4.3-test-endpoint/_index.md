---
title: "Dataset Step 3: Adapt and Merge Sources"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

Create a bounded SWE-bench Lite pseudo-trajectory sample and merge it with the simulator set:

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

The shared feature contract supports three mutually exclusive indicators: `source_simulator`, `source_mini_llm_agent`, or `source_swe_bench_lite`. This supervised merge contains the simulator and SWE-bench Lite sources; include Mini Agent records only after assigning independently reviewed labels.

The SWE-bench adapter creates agent-neutral pseudo-trajectories for workflow coverage; it is not proof that the risk model generalizes to real coding-agent behavior.
