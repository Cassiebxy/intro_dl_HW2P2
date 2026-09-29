# T08 — Augmentation Experiments

- **Status**: pending | **Owner**: Cathy | **Track**: exploration | **Due**: Oct 5–7 (parallel with T07) | **Blocked-by**: T05
- **Hypothesis**: augmentation as regularization; identity-preserving transforms only. One change at a time.

## Rules

- **Never** combine Mixup/CutMix/label-smoothing with an ArcFace run (@303).
- Keep a fixed baseline transform set; each experiment changes exactly one element.
- Short screen first (same budget as H1 screen), promote only with val evidence.

## Candidate ladder (each = one experiment)

1. horizontal flip (face identity-preserving) — likely free win
2. mild random crop / translation (small)
3. photometric jitter (brightness/contrast, mild)
4. mild rotation (small degrees)
5. (drop if it hurts) anything that destroys identity cues

## Acceptance

- [ ] Each candidate a separate run record with val cls acc / EER / combined vs fixed baseline
- [ ] Final augmentation set documented in config
- [ ] Decision log entry: which augmentations kept, which rejected, why

## Notes / current status

- (fill as you run)
