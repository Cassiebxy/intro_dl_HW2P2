# T06 — Custom Residual CNN + CE

- **Status**: pending | **Owner**: Cathy | **Track**: exploration | **Due**: Oct 3–4 | **Blocked-by**: T05
- **Hypothesis**: H2 (see `docs/design/model_hypotheses.md`)

## Steps

1. Design ResNet-style blocks from scratch: identity shortcuts; projection shortcut when channels/resolution change; gentle early downsampling on 112×112 (first stages keep spatial resolution relatively high).
2. 512-d embedding head; count params with `summary()` — ≤30M including the 8,631-class head.
3. **Screen** (cheap): subset or few epochs vs H1 at equal steps; record steps, effective batch, samples seen, wall-clock — not just epochs.
4. **Promote per H2 rules**; if promoted, run full-data confirmation with the same optimizer/schedule as H1 for a controlled comparison.
5. Log screen + confirmation in `experiments/runs/`.

## Acceptance

- [ ] Param count ≤ 30M recorded pre-training
- [ ] Screen result: improvement at equal optimization budget vs H1? (yes/no + evidence)
- [ ] If confirmed: cls acc / EER / combined compared to H1, same-budget
- [ ] Decision (keep/stop) written back to H2 card and decision log

## Notes / current status

- (fill as you run)
