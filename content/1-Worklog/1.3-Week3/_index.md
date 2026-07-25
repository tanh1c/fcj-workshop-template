---
title: "Week 3: Dataset Design and Trajectory Log Simulation"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

## 15/06/2026 - 21/06/2026

**Work mode:** Individual implementation combined with group-based learning and discussion.
**Program:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** No fixed mentor assigned; work was self-managed and supported by documentation, tutorials, and peer discussion.

## Objective

Design the raw trajectory log format and generate representative samples that can support feature engineering and supervised model training.

## Context

The model cannot score agent behavior without structured evidence. This week translated abstract risk concepts into fields that could be stored, processed, and converted into ML features.

## AWS Learning Focus

This week focused on the data foundation required before using AWS ML services. I studied how logs and labels should be prepared so they can later be processed by SageMaker and stored in S3.

- **JSONL log format:** Studied why JSONL is useful for event-style logs: each line is one record, easy to append, easy to inspect, and suitable for batch processing.
- **Raw data design for S3:** Reviewed how raw files should remain close to their original form so later processing steps are reproducible.
- **Schema consistency:** Studied why every trajectory record should contain consistent fields such as files read, files modified, commands run, test status, lint status, and final summary.
- **Feature planning:** Identified which raw fields could later become tabular ML features: number of files read, number of files modified, command count, diff size, tool count, and latency.
- **Safety signal planning:** Studied how rule-based indicators can become model features, including sensitive-file access, destructive commands, network commands, and unsupported success claims.
- **AWS connection:** Prepared the dataset structure so it could later be uploaded to S3 and used as SageMaker Processing input.

This week was mainly about preparing the data correctly before using AWS managed services, because a poor raw schema would make later processing and training harder.

## Daily Breakdown

| Date | Work performed |
|---|---|
| 15/06/2026 | Defined JSONL as the raw trajectory log format and reviewed how one line should represent one agent run. |
| 16/06/2026 | Designed fields for files_read, files_modified, commands_run, test/lint status, diff size, and final_summary. |
| 17/06/2026 | Added safety-related signals such as touched_sensitive_files, used_network_command, and destructive_command_detected. |
| 18/06/2026 | Generated local sample trajectories for safe, failed, risky, and require-review scenarios. |
| 19/06/2026 | Checked combined trajectory data and labels to confirm the dataset could support supervised learning. |
| 20/06/2026 - 21/06/2026 | Captured dataset screenshots and linked the full JSONL evidence file for readability. |


## Technical Activities

- Designed JSONL as the raw data format because it is simple, append-friendly, and suitable for one-record-per-agent-run storage.
- Created fields for task description, files_read, files_modified, commands_run, tests_passed, lint_passed, diff_lines_added, diff_lines_deleted, touched_sensitive_files, used_network_command, destructive_command_detected, and final_summary.
- Defined representative labels such as safe, failed, risky, and hallucinated_success to reflect both quality and safety problems.
- Generated local sample runs that include normal fixes, missing test evidence, sensitive file access, risky command attempts, and large unrelated diffs.

## Deliverables

- **Raw JSONL schema defined.**
- **Synthetic trajectory samples generated.**
- **Labeling rules documented.**
- **Dataset ready for S3 upload.**

## Challenge and Solution

**Challenge:** The generated dataset had to be realistic enough for a demo but simple enough to explain in a workshop.

**Solution:** The schema focused on high-signal fields that are easy to understand: file scope, command safety, test/lint evidence, sensitive access, and diff size.

## Project Relevance

This week contributed to the final MVP by strengthening the path from **AI coding-agent behavior evidence** to an **AWS-based risk scoring workflow**. The work helped ensure that the final workshop is not only a conceptual explanation, but also follows the actual implementation sequence used in the project.

## Evidence Screenshots and Files

![Trajectory JSONL sample screenshot](/images/worklog/week03-trajectory-jsonl.png)

![Dataset generation folder](/images/worklog/week03-dataset-folder.png)

The screenshot of the JSONL sample is difficult to read at full size, so the full raw trajectory evidence is linked directly here: [week03-sample-trajectories.jsonl](/images/worklog/week03-sample-trajectories.jsonl).

## Evidence and References Studied

- [JSON Lines format](https://jsonlines.org/)
- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

---

[Previous](/1-worklog/1.2-week2/) | [Back to Worklog](/1-worklog/) | [Next](/1-worklog/1.4-week4/)
