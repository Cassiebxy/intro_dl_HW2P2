# T09 — Verification Flip-TTA

- **Status**: pending | **Owner**: Cathy | **Track**: exploration | **Due**: Oct 7–8 | **Blocked-by**: T06 or T07 (whichever backbone is final)

## Steps

1. With the frozen final backbone, evaluate verification on val pairs **without** TTA → baseline EER.
2. TTA path: original embedding + horizontal-flip embedding → average → L2-normalize → cosine similarity.
3. Compare EER with/without TTA on **val pairs only**; classification unaffected (skip re-eval).
4. Cost note: TTA doubles verification inference time — record it.

## Acceptance

- [ ] EER(no TTA) and EER(TTA) recorded from the same checkpoint
- [ ] Improvement (or not) written to `experiments/runs/` + decision log
- [ ] If adopted, the exact inference path is in the final submission code

## Notes / current status

- (fill as you run)
