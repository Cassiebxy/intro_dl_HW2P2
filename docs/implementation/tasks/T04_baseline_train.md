# T04 — Baseline Full Training (~20 epochs)

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/codex/2026-09-29_planning_review.md`.

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 30–Oct 1 | **Blocked-by**: T03

## Steps

1. Config: full `num_classes=8631`, `epochs=20` (starter recommendation for early submission), batch 64 → increase if V100 memory allows.
2. Launch as background run on PSC; wandb on; log per-epoch: train loss/acc, val cls acc, val EER, combined score, wall-clock.
3. Save best-by-combined checkpoint each epoch; keep ALL epoch checkpoints until done.
4. Choose duration from measured convergence and available compute. Do not cut necessary training or engineering steps solely to meet Oct 2; missing the early cutoff is acceptable.

## Acceptance

- [ ] Training curves sane (loss ↓, acc ↑, no divergence)
- [ ] Best val combined recorded + checkpoint path noted
- [ ] Run record file in `experiments/runs/` with config + seed + metrics table
- [ ] Actual validation metrics and next-step decision documented; early-cutoff success is not required

## Notes / current status

- (fill as you run)
