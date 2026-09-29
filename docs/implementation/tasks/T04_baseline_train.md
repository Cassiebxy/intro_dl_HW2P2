# T04 — Baseline Full Training (~20 epochs)

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/gpt6.1sol/2026-09-29_planning_review.md`.

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 30–Oct 1 | **Blocked-by**: T03

## Steps

1. Config: full `num_classes=8631`, `epochs=20` (starter recommendation for early submission), batch 64 → increase if V100 memory allows. Use the **frozen T02 baseline recipe verbatim** (lr, momentum, weight_decay, scheduler, augmentation set, seed) — no hidden defaults.
2. Run **within the active PSC allocation**, with checkpoint + resume for continuity across node loss — do NOT rely on background execution surviving allocation expiry (see critical_path "Risks"). wandb on; log per-epoch: train loss/acc, val cls acc **pct**, val EER **pct**, `combined_pct`, wall-clock (units contract in `docs/design/requirements.md`).
3. Persist `last.pth` + `best_combined.pth` each epoch (optionally `best_cls` / `best_eer`); keep **per-epoch metrics for every epoch** in the run record, but not every epoch's weight file — full retention is unnecessary unless quota justifies it.
4. Choose duration from measured convergence and available compute. Do not cut necessary training or engineering steps solely to meet Oct 2; missing the early cutoff is acceptable.

## Acceptance

- [ ] Training curves sane (loss ↓, acc ↑, no divergence)
- [ ] Best val combined recorded (percent units) + checkpoint path noted
- [ ] Run record file in `experiments/runs/` with config + seed + metrics table
- [ ] Measured per-epoch wall-clock (train + validation) and total budget recorded — this feeds the T05–T09 budget decision
- [ ] Actual validation metrics and next-step decision documented; early-cutoff success is not required

## Notes / current status

- (fill as you run)
