# External/OOD Hugo Workshop Update Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Reconcile the bilingual Hugo report with the completed AWS ML/MLOps workflow and add the local-only 40-sample External/OOD pilot without overstating AWS execution or production model quality.

**Architecture:** Keep the existing Hugo content tree, routes, front matter, images, and Learn theme. Edit existing English/Vietnamese page pairs in place: preserve the split between managed governance in `us-east-1`, historical local-artifact serving in `ap-southeast-1`, and the independent local External/OOD branch. Use Markdown tables and the existing `{{% notice warning %}}` shortcode for evidence limitations; do not copy raw trajectories or create new content pages.

**Tech Stack:** Hugo Extended, Markdown, Hugo Learn theme, TOML, Git, Python standard library for optional parity checks.

## Global Constraints

- Modify only `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS_Report\fcj-workshop-template`.
- Treat `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS` as read-only source material.
- Do not call AWS or run Processing, Training, HPO, Pipeline, Endpoint, Model Monitor, Lambda, API Gateway, or CloudWatch operations.
- Preserve all existing user changes: baseline status is 78 modified files and 27 untracked files before this update.
- Do not clean, reset, restore, rename, delete, commit, or push any existing file.
- Do not add dependencies, change the theme, change routes, or change navigation weights.
- Keep every English/Vietnamese pair factually equivalent while allowing natural wording.
- Do not publish raw public trajectories, full annotation packages, credentials, account identifiers, full ARNs, private bucket names, inactive API URLs, or personal filesystem paths in Hugo content.
- Do not invent metrics, evidence, screenshots, resource names, or run results.
- Synthetic held-out macro F1 and risky recall `1.00` are workflow evidence, not production-quality estimates.
- External/OOD is a local-only diagnostic: 40 public trajectories, frozen 17-feature model, no retraining, no threshold tuning, no SageMaker call, and no AWS lifecycle mutation.
- External labels are multi-agent/AI-assisted, not human ground truth.
- Registry versions `/1` and `/2` remain `Completed` and `PendingManualApproval`; neither was approved or deployed.
- Historical serving used the earlier local XGBoost artifact in `ap-southeast-1`; it was not a Registry deployment.
- The Pipeline ends at conditional registration and contains no Endpoint deployment step.
- Workflow CI uses Hugo Extended `0.134.3`; report any local-version-related build failure instead of changing theme or config.
- Do not create commits, despite the generic planning workflow recommending frequent commits, because the user explicitly prohibited committing unless separately requested.

## File Structure

### Read-only sources

- `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS\README.md` — concise completion status and External/OOD summary.
- `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS\report\final_report_vi.md` — canonical narrative, risks, results, limitations, and conclusion.
- `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS\report\architecture.md` — current External/OOD local branch and split-Region description.
- `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS\report\demo_evidence.md` — canonical AWS evidence, cleanup state, External/OOD evidence, and hashes.
- `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS\report\demo_script.md` — evidence-first video sequence and cleanup rules.
- `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS\report\external_eval\external_pilot_report.json` — highest-priority External/OOD metrics.
- `C:\Users\LG\Desktop\Study Material\AI\25-05-2026\AWS\report\external_eval\false_negatives.jsonl` — redacted risky false-negative evidence only.

### Existing Hugo files to modify

- `content/_index.md`, `content/_index.vi.md` — short completed-project summary and highlighted External/OOD result.
- `content/2-Proposal/_index.md`, `content/2-Proposal/_index.vi.md` — implemented deliverables, external diagnostic, risks, limitations, mitigation, and future work.
- `content/5-Workshop/5.1-Workshop-overview/_index.md`, `_index.vi.md` — three-track architecture summary and completion boundary.
- `content/5-Workshop/5.3-S3-vpc/5.3.1-create-gwe/_index.md`, `_index.vi.md` — AWS managed-data path plus separate local External/OOD path.
- `content/5-Workshop/5.3-S3-vpc/5.3.2-test-gwe/_index.md`, `_index.vi.md` — governance/serving boundary and explicit non-interference from External/OOD.
- `content/5-Workshop/5.6-Cleanup/_index.md`, `_index.vi.md` — full External/OOD Generalization Pilot results subsection; retain route and title.
- `content/5-Workshop/5.7-Deploy-endpoint/_index.md`, `_index.vi.md` — confirm external evaluation did not register, approve, or deploy anything.
- `content/5-Workshop/5.9-End-to-end-demo/_index.md`, `_index.vi.md` — evidence-first External/OOD review before optional live serving.
- `content/5-Workshop/5.10-Monitoring-cost-control/_index.md`, `_index.vi.md` — external pilot creates no AWS monitoring or paid resources.
- `content/5-Workshop/5.11-Cleanup/_index.md`, `_index.vi.md` — no AWS cleanup applies to the external pilot; do not copy raw data into Hugo.
- `content/6-Self-evaluation/_index.md`, `_index.vi.md` — remove stale local-fallback/future-extension claims and reflect completed governance, monitoring, cost control, and honest generalization testing.
- `content/7-Feedback/_index.md`, `_index.vi.md` — remove stale incomplete-MLOps wording and update reflection/future expectations.
- `content/1-Worklog/1.8-Week8/_index.md`, `_index.vi.md` — add 24 July report reconciliation and local External/OOD pilot without claiming an AWS rerun.

### Existing files to inspect but modify only if a contradiction is found during implementation

- `content/5-Workshop/5.3-S3-vpc/_index.md`, `_index.vi.md` — existing split-Region architecture landing page.
- `content/5-Workshop/_index.md`, `_index.vi.md` — Workshop navigation and summary.

---

### Task 1: Reconcile Home and Proposal

**Files:**
- Modify: `content/_index.md:25-30`
- Modify: `content/_index.vi.md:25-30`
- Modify: `content/2-Proposal/_index.md:28-202`
- Modify: `content/2-Proposal/_index.vi.md:28-202`

**Interfaces:**
- Consumes: canonical completion and External/OOD facts from `README.md`, `final_report_vi.md`, and `external_pilot_report.json`.
- Produces: concise report entry point and project proposal narrative used by later Workshop pages.

- [ ] **Step 1: Replace the stale Home project paragraph in both languages**

Use a two-paragraph maximum summary containing these exact facts:

```text
Completed AWS workflow: S3, Processing, managed XGBoost Training, held-out Evaluation,
Experiments/HPO, Pipeline, Registry, historical short-lived serving, Data Capture,
Model Monitor, and CloudWatch acceptance.

External/OOD diagnostic: frozen model evaluated locally on 40 pinned public trajectories;
synthetic macro F1 1.00 fell to external macro F1 0.1212. No retraining or AWS call.
```

End by directing readers to Workshop and Self-Assessment; retain current front matter, student table, and report navigation.

- [ ] **Step 2: Add the External/OOD pilot to Proposal implemented scope and deliverables**

Add one scope item and one deliverable that state:

```text
40 public trajectories sampled 20 + 20 at pinned revisions with seed 42;
multi-agent/AI-assisted A/B annotation plus adjudication;
frozen 17-feature model evaluated locally with no retraining, threshold tuning, or AWS call.
```

- [ ] **Step 3: Add accepted External/OOD evidence to the Proposal result table**

Add one row with:

```text
40 samples; A/B full-axis agreement 3/40 = 7.5%; 37 adjudicated;
final labels failed=28, safe=10, risky=2;
accuracy 0.0500; macro F1 0.1212; risky recall 0.5000; risky FNR 0.5000.
```

- [ ] **Step 4: Update Proposal risks and future work**

Add or replace rows so both languages cover:

```text
Observed distribution shift: synthetic macro F1 1.00 versus external 0.1212.
Label uncertainty: AI-assisted labels, 7.5% full-axis agreement.
Statistical uncertainty: only two risky samples; Wilson 95% [0.0945, 0.9055].
Mitigation: larger representative dataset, independent human labeling, parser review,
then governed retraining/evaluation—not immediate threshold tuning.
```

- [ ] **Step 5: Run targeted factual checks**

Run from Hugo root:

```bash
python -c "from pathlib import Path; p=[Path('content/_index.md'),Path('content/_index.vi.md'),Path('content/2-Proposal/_index.md'),Path('content/2-Proposal/_index.vi.md')]; s='\n'.join(x.read_text(encoding='utf-8') for x in p); assert '0.1212' in s and '7.5%' in s and 'External/OOD' in s"
```

Expected: exit code `0` with no output.

---

### Task 2: Add the Separate External/OOD Architecture Branch

**Files:**
- Modify: `content/5-Workshop/5.1-Workshop-overview/_index.md:28-52`
- Modify: `content/5-Workshop/5.1-Workshop-overview/_index.vi.md:28-52`
- Modify: `content/5-Workshop/5.3-S3-vpc/5.3.1-create-gwe/_index.md:13-41`
- Modify: `content/5-Workshop/5.3-S3-vpc/5.3.1-create-gwe/_index.vi.md:13-41`
- Modify: `content/5-Workshop/5.3-S3-vpc/5.3.2-test-gwe/_index.md:9-44`
- Modify: `content/5-Workshop/5.3-S3-vpc/5.3.2-test-gwe/_index.vi.md:9-44`
- Modify: `content/5-Workshop/5.7-Deploy-endpoint/_index.md:48-52`
- Modify: `content/5-Workshop/5.7-Deploy-endpoint/_index.vi.md:48-52`

**Interfaces:**
- Consumes: AWS split-Region boundary from `demo_evidence.md` and local External/OOD branch from `architecture.md:125-148`.
- Produces: consistent three-track architecture used by results, demo, monitoring, and reflection pages.

- [ ] **Step 1: Extend Workshop Overview completed scope**

Add External/OOD as the ninth completed evidence track:

```text
A local-only 40-sample External/OOD pilot evaluated the frozen managed model without
retraining, threshold tuning, SageMaker calls, or changes to Pipeline, Registry,
Endpoint, or Model Monitor baseline.
```

Add a warning notice in both languages using the existing syntax:

```markdown
{{% notice warning %}}
External labels are multi-agent/AI-assisted rather than human ground truth, and only two samples are risky. This is an OOD diagnostic, not a production benchmark.
{{% /notice %}}
```

- [ ] **Step 2: Add a separate text architecture branch to 5.3.1**

After the managed path, add:

```text
Pinned public trajectories
  -> bounded sampling: 20 Nebius + 20 OpenHands, seed 42
  -> sanitized canonical records
  -> independent annotators A/B
  -> adjudication
  -> frozen 17-feature model, local evaluation only
  -> external_pilot_report.json + redacted false_negatives.jsonl
```

State that external source flags remain zero and the branch does not enter S3, SageMaker Processing, Training, HPO, Pipeline, Registry, serving, or Model Monitor.

- [ ] **Step 3: Protect the governance and serving boundary in 5.3.2**

Add one concise paragraph:

```text
The External/OOD pilot did not change the gate, create a Registry version, approve a
package, or deploy an Endpoint. Historical serving still used the earlier local artifact.
```

- [ ] **Step 4: Protect the Pipeline boundary in 5.7**

Append one sentence to accepted governance evidence saying External/OOD metrics were not fed into `CheckRiskyRecall`, did not trigger registration, and are evidence for future data/model review only.

- [ ] **Step 5: Check architecture language for forbidden implications**

Search only the modified architecture pages with the dedicated search tool for claims matching:

```text
External.*(Pipeline|retrain|Endpoint|Registry)
```

Manually verify every match is negative (`did not`, `does not`, `no`). Also verify no new full ARN, account ID, private bucket, or inactive API URL was introduced.

---

### Task 3: Add Full External/OOD Generalization Results

**Files:**
- Modify: `content/5-Workshop/5.6-Cleanup/_index.md:53-60`
- Modify: `content/5-Workshop/5.6-Cleanup/_index.vi.md:53-60`

**Interfaces:**
- Consumes: exact values from `external_pilot_report.json` and redacted fields from `false_negatives.jsonl`.
- Produces: canonical bilingual Hugo results page referenced by Home, Proposal, demo, Self-Assessment, and Feedback.

- [ ] **Step 1: Insert `External/OOD Generalization Pilot` after accepted AWS evidence**

Describe the goal as testing the unchanged frozen 17-feature model outside the synthetic training distribution. Include this source table:

```markdown
| Public source | Pinned revision | Selected |
|---|---|---:|
| `nebius/SWE-agent-trajectories` | `68195a1450865274106246d0d0296a1d6807b88e` | 20 |
| `nebius/SWE-rebench-openhands-trajectories` | `35455389ab51bf5e2306bfd436ef72d0f98bf882` | 20 |
```

State seed `42`, total `40`, and that raw public trajectories remain gitignored and are not copied into Hugo.

- [ ] **Step 2: Add annotation coverage and final labels**

Use a table containing:

```text
Selected 40
A/B full-axis agreement 3/40 = 7.5%
Adjudicated 37
Excluded 0
Pending 0
Final labels failed=28, safe=10, risky=2
```

Immediately add a warning notice saying the labels are multi-agent/AI-assisted, not human ground truth.

- [ ] **Step 3: Add overall frozen-model metrics**

Use this exact table:

```markdown
| Metric | Result |
|---|---:|
| Accuracy | `0.0500` |
| Macro F1 | `0.1212` |
| Risky precision | `1.0000` |
| Risky recall | `0.5000` |
| Risky F1 | `0.6667` |
| Risky false-negative rate | `0.5000` |
| Risky recall Wilson 95% interval | `[0.0945, 0.9055]` |
| Risky false negatives | `1` |
```

Add a warning that only two labels are risky, so the confidence interval is wide.

- [ ] **Step 4: Add per-source and rule-only baseline tables**

Use these exact rows:

```markdown
| Scope | Samples | Accuracy | Macro F1 | Risky recall | Risky FNR |
|---|---:|---:|---:|---:|---:|
| Nebius | 20 | `0.1000` | `0.1889` | `1.0000` | `0.0000` |
| OpenHands | 20 | `0.0000` | `0.0000` | `0.0000` | `1.0000` |
| Rule-only baseline | 40 | `0.0250` | `0.1111` | `0.5000` | `0.5000` |
```

Mention the source-only internal result only as `diagnostic_only` on 183 internal rows: accuracy `0.2077`, macro F1 `0.1105`; never label it an external benchmark.

- [ ] **Step 5: Add one redacted risky false negative**

Publish only:

```text
Source: OpenHands
True label: risky
Predicted label: require_review
Risky probability: approximately 0.00156
Evidence: redacted record retained in report/external_eval/false_negatives.jsonl
```

Do not include the sample ID, task text, patch, raw trajectory, full event stream, or personal path.

- [ ] **Step 6: Add execution boundary and conclusion**

Use a warning notice stating:

```text
Local only. Frozen model. No retraining. No threshold tuning. No SageMaker call.
No external row entered the full AWS Pipeline or changed Endpoint, Registry, Pipeline,
or Model Monitor baseline.
```

Conclude that synthetic macro F1 `1.00` falling to external macro F1 `0.1212` demonstrates distribution shift/generalization gap. State that the 40-sample, two-risky-label, AI-assisted pilot is an OOD diagnostic—not a production benchmark—and requires a larger human-labeled dataset before retraining or production-quality assessment.

- [ ] **Step 7: Verify exact machine-readable metrics**

Run from the read-only source repository without writing files:

```bash
python -c "import json, pathlib; d=json.loads(pathlib.Path('report/external_eval/external_pilot_report.json').read_text()); assert d['coverage']=={'selected':40,'accepted':3,'adjudicated':37,'excluded':0,'a_b_agreement':0.075}; assert round(d['overall']['six_class']['macro_f1'],4)==0.1212; assert d['overall']['risky_vs_rest']['recall']==0.5; assert d['overall']['risky_vs_rest']['false_negative']==1; assert d['frozen_model']['retrained'] is False and d['frozen_model']['threshold_tuned'] is False"
```

Expected: exit code `0` with no output.

---

### Task 4: Update Demo, Monitoring, and Cleanup Narrative

**Files:**
- Modify: `content/5-Workshop/5.9-End-to-end-demo/_index.md:9-53`
- Modify: `content/5-Workshop/5.9-End-to-end-demo/_index.vi.md:9-53`
- Modify: `content/5-Workshop/5.10-Monitoring-cost-control/_index.md:29-60`
- Modify: `content/5-Workshop/5.10-Monitoring-cost-control/_index.vi.md:29-60`
- Modify: `content/5-Workshop/5.11-Cleanup/_index.md:50-64`
- Modify: `content/5-Workshop/5.11-Cleanup/_index.vi.md:50-64`

**Interfaces:**
- Consumes: evidence-first sequence from `demo_script.md` and cleanup state from `demo_evidence.md`.
- Produces: safe demo flow that does not rerun paid AWS jobs and clearly separates local OOD evidence from optional serving.

- [ ] **Step 1: Add External/OOD evidence review before optional live serving**

In 5.9, add a local evidence sequence:

```text
1. Open report/external_eval/external_pilot_report.json at coverage, overall, and by_source.
2. Open report/external_eval/false_negatives.jsonl and show only the redacted record.
3. Explain synthetic macro F1 1.00 versus external macro F1 0.1212.
4. State no retraining, threshold tuning, SageMaker call, or AWS Pipeline execution occurred.
```

Do not instruct the reader to regenerate annotations, fetch public rows, or run local evaluation during the video.

- [ ] **Step 2: Preserve the evidence-first live demo boundary**

Keep the existing rule that Processing, Training, HPO, Pipeline, and Model Monitor are not rerun. Keep optional serving limited to one short-lived `ml.t2.medium` Endpoint, Lambda/API, 1–2 requests, and immediate cleanup after explicit authorization.

- [ ] **Step 3: Add monitoring non-interference**

In 5.10, state that External/OOD produced local JSON/JSONL reports only and created no Data Capture, Model Monitor, CloudWatch, Endpoint, or other AWS resource. Do not alter accepted AWS metrics: baseline 854 rows/17 features, `CompletedWithViolations`, two drift features, two type limitations, 101 data metrics, dashboard, and seven actions-disabled alarms cleaned after acceptance.

- [ ] **Step 4: Add cleanup non-applicability and data handling**

In 5.11, state that the External/OOD pilot requires no AWS cleanup. Raw public trajectories remain outside Hugo and gitignored; only concise aggregate metrics and a redacted false-negative summary are published. Preserve the existing AWS absence checklist and retained-evidence list.

- [ ] **Step 5: Verify no executable AWS expansion was added**

Review the diff for these three page pairs and confirm no new command contains `--start`, `--upsert`, endpoint deploy arguments, `aws`, role ARN, account ID, or concrete bucket URI.

---

### Task 5: Rewrite Stale Self-Evaluation and Feedback Claims

**Files:**
- Modify: `content/6-Self-evaluation/_index.md:13-54`
- Modify: `content/6-Self-evaluation/_index.vi.md:13-54`
- Modify: `content/7-Feedback/_index.md:24-55`
- Modify: `content/7-Feedback/_index.vi.md:24-55`

**Interfaces:**
- Consumes: completed AWS lifecycle and observed External/OOD limitations from Tasks 1–4.
- Produces: factual personal reflection and conclusion without stale “future extension” wording.

- [ ] **Step 1: Correct the Self-Assessment project history**

Replace the claim that local XGBoost remained the fallback because Training quota was unavailable. State instead:

```text
Training quota was unavailable in ap-southeast-1, so an earlier local artifact supported
historical serving. After ml.m5.large quota was approved in us-east-1, managed Training,
Evaluation, HPO, Pipeline, Registry, and Model Monitor acceptance were completed there.
```

- [ ] **Step 2: Replace stale Self-Assessment strengths**

Strengths must include:

```text
Full AWS ML/MLOps workflow execution.
Conditional registration and PendingManualApproval governance.
Data Capture, Model Monitor, CloudWatch metrics/dashboard/alarm acceptance.
Short-lived paid resources and verified cleanup.
Honest local External/OOD test exposing a 1.00 -> 0.1212 generalization gap.
Human review and hard rules remain authoritative.
```

Remove wording that calls Training, Pipeline, Registry, or Model Monitor unimplemented future extensions.

- [ ] **Step 3: Update observed limitations and improvements**

Use evidence-based limitations: 40 external samples, only two risky, AI-assisted labels with 7.5% full-axis agreement, parser/default-value uncertainty, and scikit-learn `0.24.1` to `1.8.0` compatibility warning. Improvement path: larger human-labeled data, pinned runtime, calibration/cost-sensitive learning, and reviewed release after manual approval.

- [ ] **Step 4: Correct Feedback completed-project narrative**

Replace the local-only MVP description and “future extensions” paragraph with a concise reflection on completed managed AWS workflow, governance, monitoring, cost control, and the external diagnostic that challenged perfect synthetic metrics.

- [ ] **Step 5: Update Feedback future expectations**

Do not say “complete MLOps components when limits allow.” State future work as representative human-labeled data, framework adapters, calibrated thresholds, canary/rollback, and a separate reviewed deployment pipeline after Registry approval.

- [ ] **Step 6: Search for stale claims in the modified pages**

Use the dedicated search tool for:

```text
future extensions|local XGBoost training had to be used as a fallback|mọi AWS MLOps component|when AWS account limits allow|khi giới hạn tài khoản AWS cho phép
```

Expected: no stale match in Self-Assessment or Feedback except historical context that explicitly explains the later managed completion.

---

### Task 6: Reconcile Week 8 Chronology

**Files:**
- Modify: `content/1-Worklog/1.8-Week8/_index.md:15-86`
- Modify: `content/1-Worklog/1.8-Week8/_index.vi.md:15-86`

**Interfaces:**
- Consumes: completed local External/OOD source documents dated as current project truth and the existing Week 8 chronology.
- Produces: factual worklog through 24/07/2026 without claiming future work or paid AWS reruns.

- [ ] **Step 1: Advance Week 8 status to 24/07/2026**

Change the status statement so completed entries cover 20–24 July and 25–26 July remain planned.

- [ ] **Step 2: Replace the 24–26 July combined planned row**

Use separate rows:

```text
24/07/2026 — Completed final-report reconciliation; completed the bounded local
External/OOD pilot with multi-agent A/B annotation, adjudication, frozen-model
evaluation, and report/demo updates. No paid AWS job or serving resource was run.

25/07/2026–26/07/2026 — Planned reviewer feedback and documentation corrections only.
```

- [ ] **Step 3: Add a concise External/OOD evidence subsection**

Include only:

```text
20 + 20 pinned public trajectories, seed 42.
3/40 full-axis A/B agreement; 37 adjudicated; no excluded/pending.
External macro F1 0.1212; risky recall/FNR 0.5000/0.5000.
Frozen model, no retraining, no threshold tuning, no AWS call.
```

Do not add raw events, annotation packages, personal paths, source hashes, or new images.

- [ ] **Step 4: Update deliverables through 24/07/2026**

Add final-report reconciliation, local External/OOD pilot, multi-agent annotation/adjudication, frozen-model evaluation, and demo update. Preserve the statement that the four SVG cards summarize accepted AWS evidence and are not Console screenshots.

- [ ] **Step 5: Verify chronology and parity**

Run:

```bash
python -c "from pathlib import Path; en=Path('content/1-Worklog/1.8-Week8/_index.md').read_text(encoding='utf-8'); vi=Path('content/1-Worklog/1.8-Week8/_index.vi.md').read_text(encoding='utf-8'); required=['24/07/2026','0.1212','7.5%']; assert all(x in en and x in vi for x in required); assert '25/07/2026' in en and '25/07/2026' in vi"
```

Expected: exit code `0` with no output.

---

### Task 7: Full Content, Build, Git, and Bilingual Verification

**Files:**
- Verify all files modified in Tasks 1–6.
- Do not modify source repository files, theme, config, workflow, or generated `public/` intentionally.

**Interfaces:**
- Consumes: all bilingual content changes.
- Produces: validated Hugo build and an exact final report of changes, evidence gaps, contradictions, and operational safety.

- [ ] **Step 1: Search required External/OOD facts**

Use the dedicated content-search tool rather than shell `grep` for:

```text
0.1212|7.5%|External/OOD|multi-agent|AI-assisted|0.0945|0.9055
```

Expected: matches in both English and Vietnamese pages for Home/Proposal where concise, Workshop results/demo, Self-Assessment/Feedback where relevant, and Week 8.

- [ ] **Step 2: Search forbidden or misleading claims**

Search all `content/**/*.md` for claims that imply:

```text
managed AWS workflow is pending;
external data was used to retrain;
external data entered the full AWS Pipeline;
external annotations are human ground truth;
external macro F1 exceeds 0.1212;
Registry packages were approved or deployed;
Pipeline deployed an Endpoint.
```

Inspect every match in context. Rewrite only actual contradictions; preserve explicit negations and historical chronology.

- [ ] **Step 3: Run a bilingual fact-parity check**

Run from Hugo root:

```bash
python -c "from pathlib import Path; pairs=[('content/_index.md','content/_index.vi.md'),('content/2-Proposal/_index.md','content/2-Proposal/_index.vi.md'),('content/5-Workshop/5.1-Workshop-overview/_index.md','content/5-Workshop/5.1-Workshop-overview/_index.vi.md'),('content/5-Workshop/5.3-S3-vpc/5.3.1-create-gwe/_index.md','content/5-Workshop/5.3-S3-vpc/5.3.1-create-gwe/_index.vi.md'),('content/5-Workshop/5.3-S3-vpc/5.3.2-test-gwe/_index.md','content/5-Workshop/5.3-S3-vpc/5.3.2-test-gwe/_index.vi.md'),('content/5-Workshop/5.6-Cleanup/_index.md','content/5-Workshop/5.6-Cleanup/_index.vi.md'),('content/5-Workshop/5.7-Deploy-endpoint/_index.md','content/5-Workshop/5.7-Deploy-endpoint/_index.vi.md'),('content/5-Workshop/5.9-End-to-end-demo/_index.md','content/5-Workshop/5.9-End-to-end-demo/_index.vi.md'),('content/5-Workshop/5.10-Monitoring-cost-control/_index.md','content/5-Workshop/5.10-Monitoring-cost-control/_index.vi.md'),('content/5-Workshop/5.11-Cleanup/_index.md','content/5-Workshop/5.11-Cleanup/_index.vi.md'),('content/6-Self-evaluation/_index.md','content/6-Self-evaluation/_index.vi.md'),('content/7-Feedback/_index.md','content/7-Feedback/_index.vi.md'),('content/1-Worklog/1.8-Week8/_index.md','content/1-Worklog/1.8-Week8/_index.vi.md')]; facts=['0.1212','7.5%','0.5000']; bad=[]; [(bad.append((a,b,f)) if ((f in Path(a).read_text(encoding='utf-8')) != (f in Path(b).read_text(encoding='utf-8'))) else None) for a,b in pairs for f in facts]; assert not bad,bad; print('PASS: key metric parity')"
```

Expected: `PASS: key metric parity`.

- [ ] **Step 4: Build with local Hugo**

Run:

```bash
hugo --minify
```

Expected: exit code `0`. Record the local Hugo version in the final response. Workflow target is Extended `0.134.3`; if a version-related error occurs, report it and do not modify the theme/config to compensate.

- [ ] **Step 5: Run Git whitespace validation**

Run:

```bash
git diff --check
```

Expected: exit code `0`. If it reports pre-existing errors outside target files, distinguish those from this task and do not overwrite unrelated user changes.

- [ ] **Step 6: Record final repository state without changing it**

Run:

```bash
git status --short
git diff --stat
```

Report the output accurately, noting that the repository already contained extensive modified and untracked work before this update.

- [ ] **Step 7: Review the final diff by bilingual pair**

Use `git diff --` with only the Task 1–6 target paths. Verify:

```text
front matter unchanged;
weights/routes unchanged;
EN/VI facts match;
no raw trajectories or annotation package copied;
no account ID/full ARN/private bucket/inactive API URL added;
no AWS command executed;
no source repository file changed;
no commit or push performed.
```

- [ ] **Step 8: Report manual evidence gaps**

Report that no new screenshot is required for External/OOD because the canonical evidence is the local JSON report and redacted false-negative file. If the final video/report needs visual proof, list these manual views only:

```text
external_pilot_report.json: coverage, overall risky_vs_rest, by_source;
false_negatives.jsonl: one redacted record;
existing AWS Console evidence for completed managed lifecycle;
existing post-cleanup absence evidence.
```

Do not fabricate or create screenshots.

## Self-Review Results

- **Spec coverage:** All requested page groups, External/OOD metrics, source revisions, annotation coverage, per-source results, baseline, false negative, demo flow, monitoring, cleanup, Self-Assessment, Feedback, Week 8, and verification commands are assigned to Tasks 1–7.
- **Placeholder scan:** No `TBD`, `TODO`, “implement later,” unspecified test, or invented URL remains in the plan.
- **Consistency:** Every task uses `External/OOD`, `40`, `20 + 20`, seed `42`, macro F1 `0.1212`, risky recall/FNR `0.5000`, AI-assisted labels, frozen model, no retraining, no threshold tuning, and no AWS call consistently.
- **Known source contradiction:** `report/architecture.md:26-29` and its text fallback visually connect `PendingManualApproval` to a short-lived Endpoint, but `report/demo_evidence.md:25,56,192,229,359,447` confirms Registry `/1` and `/2` were never deployed and historical serving used the earlier local artifact. Hugo must preserve the already-correct separated tracks and must not copy that misleading edge.
- **Git safety:** The pre-existing 78 modified and 27 untracked files are treated as user work; no reset, cleanup, commit, or push is permitted.
