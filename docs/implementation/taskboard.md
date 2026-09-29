# Task Board

> Statuses: `pending / in_progress / blocked / done / dropped`. Update this file AND the task file when status changes.

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/codex/2026-09-29_planning_review.md`.

## Phase A — complete setup and verified baseline

| ID | Task | Status | Blocked-by | Deliverable | Actual result |
| --- | --- | --- | --- | --- | --- |
| T01 | PSC env verify | pending | — | GPU visible, env loaded, keys set | |
| T02 | Baseline 5-layer backbone | pending | — | backbone + cls layer, param count | |
| T03 | End-to-end smoke test | pending | T01, T02 | full chain incl. submission.csv generated | |
| T04 | Baseline training (~20 epochs) | pending | T03 | best checkpoint by val combined | |
| T05 | Optional early-cutoff attempt | pending (optional) | Verified training/inference readiness | Record attempt/result if ready; ≥0.80 is desirable | Canvas HW2 quiz completed, Cathy confirmed Sep 29; no score independently verified |

## Phase B — exploration (deadline Oct 9)

| ID | Task | Status | Blocked-by | Deliverable | Actual result |
| --- | --- | --- | --- | --- | --- |
| T06 | Custom residual CNN + CE | pending | Verified baseline; not early-cutoff success | screen + full comparison vs H1 | |
| T07 | ArcFace vs CE (same backbone) | pending | T06 | cls acc / EER compared | |
| T08 | Augmentation experiments | pending | Stable baseline selected | one-change-at-a-time results | |
| T09 | Verification flip-TTA | pending | T06/T07 | EER with/without TTA | |
| T10 | Final selection + final submissions | pending | At least one validated reproducible candidate; optional experiments completed or stopped | Kaggle final + Gradescope zip | |

## Rules

- T03 is the hard gate: no long training before the whole smoke chain is green.
- Every done task links its evidence file in `experiments/runs/`.
- `blocked` tasks state the blocker and the escalation taken (staff question? fallback?).
