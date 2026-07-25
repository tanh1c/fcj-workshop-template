---
title: "Dataset Step 4: Validate Labels and Splits"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

Run a free local preprocessing preview before using SageMaker:

```bash
python preprocessing/processing_script.py \
  --input data_generation/combined_trajectories.jsonl \
  --output-dir data/processed \
  --seed 42
```

Confirm:

- All six expected labels are present.
- Every row has the 17 ordered features plus `label`.
- The source indicators are mutually exclusive.
- Safety booleans are numeric after feature extraction.
- The fixed-seed split produces train, validation, and held-out test CSV files.
- No label appears in inference or Model Monitor feature input.

The split is 70% train, 15% validation, and 15% test. Preserve the held-out test set for evaluation; do not tune against it.

Before any production claim, replace or supplement synthetic records with representative real trajectories and independently reviewed human labels.
