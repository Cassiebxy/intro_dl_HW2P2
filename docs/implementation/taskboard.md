# Task Board

> Statuses: `pending / in_progress / blocked / done / dropped`. Update this file AND the task file when status changes.

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/codex/2026-09-29_planning_review.md`.

## Phase A — complete setup and verified baseline (Oct 2 checkpoint = best-effort milestone)

| ID | Task | Status | Blocked-by | Deliverable | Actual result |
| --- | --- | --- | --- | --- | --- |
| T01 | PSC env + storage verify | pending | — | V100, shared env, `$LOCAL` data + persistent ckpt dir, DataLoader + kernel-reload reload checks | |
| T02 | Starter pipeline completion + baseline CNN | pending | — | full runnable pipeline (datasets/loaders/backbone/head/criterion/optimizer/scheduler/metrics), param count ≤30M | |
| T03 | End-to-end smoke test | pending | T01, T02 | full chain incl. submission.csv generated (8631 output, limited steps) | |
| T04 | Baseline training (~20 epochs, measured) | pending | T03 | best checkpoint by val combined | |
| T05 | Optional early-cutoff attempt (Kaggle) | pending (optional) | T04 + verified training/inference readiness | Record attempt/result if ready; ≥0.80 desirable, name on LB | Canvas HW2 quiz completed, Cathy confirmed Sep 29; no score independently verified |

## Phase B — exploration (deadline Oct 9)

| ID | Task | Status | Blocked-by | Deliverable | Actual result |
| --- | --- | --- | --- | --- | --- |
| T06 | Custom residual CNN + CE | pending | T04 verified baseline; not early-cutoff success | screen + full comparison vs H1 | |
| T07 | ArcFace vs CE (same backbone) | pending | T06 | cls acc / EER compared | |
| T08 | Augmentation experiments | pending | T04 stable baseline selected | one-change-at-a-time results | |
| T09 | Verification flip-TTA | pending | T06/T07 | EER with/without TTA | |
| T10 | Final selection + final submissions | pending | At least one validated reproducible candidate; optional experiments completed or stopped | Kaggle final + Gradescope zip | |

## Rules

- T03 is the hard **pipeline** gate: no long training before the whole smoke chain is green.
- **Oct 2 is a best-effort milestone, not a hard gate.** The full pipeline and honest learning steps are never compressed to hit that date; exploration (T06/T08) is blocked by a verified baseline (T04), not by the checkpoint submission (T05).
- Every done task links its evidence file in `experiments/runs/`.
- `blocked` tasks state the blocker and the escalation taken (staff question? fallback?).
