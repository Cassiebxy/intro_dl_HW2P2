# T06 — Custom Residual CNN + CE

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/gpt6.1sol/2026-09-29_planning_review.md`.

- **Status**: pending | **Owner**: Cathy | **Track**: exploration | **Due**: Oct 3–4 | **Blocked-by**: verified baseline and pipeline readiness; not T05/cutoff success
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
