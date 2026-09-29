# T05 — Checkpoint Submission + Canvas Quiz (best-effort milestone)

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: **Oct 2 by NOON (best-effort)** | **Blocked-by**: T04
- **Framing**: Oct 2 is a best-effort milestone. Missing it costs the −3%/×0.97 checkpoint penalty, but it is **not** a hard gate that compresses the pipeline or truncates learning steps. Submit the best verified checkpoint available by noon; the Canvas quiz is required regardless of score.

## Steps

1. Load the best checkpoint via the submission path (explicit load, not "whatever is in memory").
2. Generate `submission.csv` with the DO-NOT-MODIFY cell; preview columns/rows.
3. Submit via Kaggle API with a meaningful message (e.g. `baseline_5cnn_ep{N}_seed{S}`).
4. Verify on the Kaggle leaderboard: score ≥ 0.80, **your name visible**, correct submission selected.
5. Freeze that checkpoint dir: never overwrite (add note in config).
6. **Complete the HW2 Canvas Quiz** (separate requirement!).
7. Record Kaggle score + submission timestamp in `experiments/runs/`.

## Acceptance

- [ ] Best verified checkpoint submitted by noon; score ≥ 0.80 is the target (best-effort, not a pipeline-compression gate)
- [ ] Name on leaderboard; submission message + timestamp recorded
- [ ] Canvas quiz submitted (required regardless of score)
- [ ] Checkpoint dir frozen; dry-run reproducibility note written
- [ ] ≥ 2 Kaggle slots left unused for same-day fixes

## Notes / current status

- (fill as you run)
