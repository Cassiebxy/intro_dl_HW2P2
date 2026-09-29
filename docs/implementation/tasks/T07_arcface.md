# T07 — ArcFace vs CE (same backbone)

- **Status**: pending | **Owner**: Cathy | **Track**: exploration | **Due**: Oct 4–6 | **Blocked-by**: T06
- **Hypothesis**: H3 (see `docs/design/model_hypotheses.md`)

## Compliance checklist (implement ONLY after all boxes are sure)

- [ ] ArcFace per-class embedding matrix is a **trainable parameter** and receives updates
- [ ] Model output embedding L2-normalized to unit vector
- [ ] ArcFace class embeddings L2-normalized
- [ ] Cosine-similarity logits scaled by `s`; margin `m` applied correctly
- [ ] **No** label smoothing, **no** Mixup, **no** CutMix in this run's pipeline
- [ ] Anything ambiguous → staff question on HW2P2 Piazza thread BEFORE training

## Steps

1. Implement ArcFace head; unit-test: normalized norms ≈ 1, loss sanity on random data.
2. Same backbone as best-of-T06; same optimizer budget; screen short runs first (watch loss scale, EER trend).
3. Full comparison: CE vs ArcFace, cls acc and EER reported **separately**.
4. Tune `s`/`m` only if screen shows instability, one change at a time.

## Acceptance

- [ ] Compliance checklist green before any training
- [ ] CE vs ArcFace results table (cls acc, EER, combined) in `experiments/runs/`
- [ ] Conclusion written to H3 card + decision log

## Notes / current status

- (fill as you run)
