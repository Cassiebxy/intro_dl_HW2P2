# T02 — Baseline 5-Layer CNN Backbone

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 29
- **Hypothesis**: H1 (see `docs/design/model_hypotheses.md`)

## Steps

1. Read starter notebook FAQ for the required 5-layer CNN description (cell 67/68).
2. Implement `Network.backbone` per FAQ: conv layers + BN + ReLU + pooling, ending in a feature vector; no imported architectures.
3. Implement `cls_layer` → 8,631 logits; keep `forward(x, return_feats)` contract intact.
4. `summary(model, (3,112,112))`: check feature shape and param count (must be ≪ 30M).

## Acceptance

- [ ] Backbone + head implemented per FAQ, no `torchvision.models`
- [ ] `summary()` output recorded in this file (layers, feature dim, params)
- [ ] One forward pass on CPU/MPS works locally (PSC GPU check in T03)

## Notes / current status

- (fill as you run)
