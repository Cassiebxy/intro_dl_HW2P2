# T03 — End-to-End Smoke Test (HARD GATE)

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 29–30 | **Blocked-by**: T01, T02
- **Hypothesis tested**: none — engineering gate. **No long training may start until this is fully green.**

## Steps

1. Config: small `num_classes` subset (e.g. 200), `epochs: 1–2`, batch_size 64.
2. Train one epoch: loss decreases, no NaN.
3. Save checkpoint mid/after training; **restart kernel**; reload checkpoint; weights match.
4. Classification inference on dev batch → sensible predictions.
5. Verification inference: embeddings → cosine → EER computed by `valid_epoch_ver` runs.
6. Generate `submission.csv` via the DO-NOT-MODIFY cell; inspect format (columns, row count vs `test_pairs.txt` / cls test).
7. Record wall-clock per epoch for T04 planning.

## Acceptance

- [ ] All 6 chain steps green (check each)
- [ ] Reloaded checkpoint gives identical outputs to pre-save model
- [ ] `submission.csv` generated with correct format, no NaN/Inf
- [ ] Per-epoch wall-clock recorded here: ____ s/epoch
- [ ] Evidence file created in `experiments/runs/`

## Notes / current status

- (fill as you run)
