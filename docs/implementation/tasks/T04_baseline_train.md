# T04 — Baseline Full Training (~20 epochs)

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 30–Oct 1 | **Blocked-by**: T03

## Steps

1. Config: full `num_classes=8631`, `epochs=20` (starter recommendation for early submission), batch 64 → increase if V100 memory allows.
2. Launch as background run on PSC; wandb on; log per-epoch: train loss/acc, val cls acc, val EER, combined score, wall-clock.
3. Save best-by-combined checkpoint each epoch; keep ALL epoch checkpoints until done.
4. If per-epoch × 20 exceeds the time before Oct 2, do **not** compress the pipeline or truncate learning just to hit the date. Submit whatever verified checkpoint exists as a best-effort milestone (Oct 2 is best-effort, not a hard gate); the full pipeline and honest learning steps take priority.

## Acceptance

- [ ] Training curves sane (loss ↓, acc ↑, no divergence)
- [ ] Best val combined recorded + checkpoint path noted
- [ ] Run record file in `experiments/runs/` with config + seed + metrics table
- [ ] Val combined recorded; if below the ~0.80 checkpoint region, note it as a best-effort checkpoint target — not a reason to compress the pipeline or truncate learning

## Notes / current status

- (fill as you run)
