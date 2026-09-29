# Task Board

> Statuses: `pending / in_progress / blocked / done / dropped`. Update this file AND the task file when status changes.

## Phase A — checkpoint sprint (deadline Oct 2)

| ID | Task | Status | Blocked-by | Deliverable | Actual result |
| --- | --- | --- | --- | --- | --- |
| T01 | PSC env verify | pending | — | GPU visible, env loaded, keys set | |
| T02 | Baseline 5-layer backbone | pending | — | backbone + cls layer, param count | |
| T03 | End-to-end smoke test | pending | T01, T02 | full chain incl. submission.csv generated | |
| T04 | Baseline training (~20 epochs) | pending | T03 | best checkpoint by val combined | |
| T05 | Kaggle checkpoint submit + Canvas quiz | pending | T04 | Kaggle ≥ 80, quiz done, name on LB | |

## Phase B — exploration (deadline Oct 9)

| ID | Task | Status | Blocked-by | Deliverable | Actual result |
| --- | --- | --- | --- | --- | --- |
| T06 | Custom residual CNN + CE | pending | T05 | screen + full comparison vs H1 | |
| T07 | ArcFace vs CE (same backbone) | pending | T06 | cls acc / EER compared | |
| T08 | Augmentation experiments | pending | T05 | one-change-at-a-time results | |
| T09 | Verification flip-TTA | pending | T06/T07 | EER with/without TTA | |
| T10 | Final selection + final submissions | pending | T06–T09 | Kaggle final + Gradescope zip | |

## Rules

- T03 is the hard gate: no long training before the whole smoke chain is green.
- Every done task links its evidence file in `experiments/runs/`.
- `blocked` tasks state the blocker and the escalation taken (staff question? fallback?).
