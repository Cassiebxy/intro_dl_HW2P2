# T04 — Baseline Full Training (~20 epochs)

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 30–Oct 1 | **Blocked-by**: T03

## Steps

1. Config: full `num_classes=8631`, `epochs=20` (starter recommendation for early submission), batch 64 → increase if V100 memory allows.
2. Launch as background run on PSC; wandb on; log per-epoch: train loss/acc, val cls acc, val EER, combined score, wall-clock.
3. Save best-by-combined checkpoint each epoch; keep ALL epoch checkpoints until done.
4. If per-epoch × 20 exceeds remaining time to Oct 2 noon: cut epochs (e.g. 10) rather than risk the deadline — cutoff-safe > optimal.

## Acceptance

- [ ] Training curves sane (loss ↓, acc ↑, no divergence)
- [ ] Best val combined recorded + checkpoint path noted
- [ ] Run record file in `experiments/runs/` with config + seed + metrics table
- [ ] Val combined comfortably ≥ 0.80 target, or explicit fallback decision logged

## Notes / current status

- (fill as you run)
