# T02 — Starter Pipeline Completion + Baseline 5-Layer CNN

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 29–30
- **Hypothesis**: H1 (see `docs/design/model_hypotheses.md`)
- **Scope note**: the starter notebook is a scaffold, not a turnkey baseline. The official 5-layer CNN (cell 68) is only **one** of many `TODO`/`NotImplementedError` cells. T02 = make the whole pipeline runnable end-to-end. No new model family here — only the starter's own baseline.

## Starter TODO inventory (all must be filled for the pipeline to run)

| Starter cell | What to implement |
| --- | --- |
| `create_transforms` (51) | augmentation transforms + `Normalize` mean/std |
| cls datasets (56) | train / val / test `Dataset`s; class lists must match (`assert`) |
| cls loaders (56) | train / val / test `DataLoader`s |
| ver datasets (59) | val / test pair datasets |
| ver loaders (59) | val / test pair loaders |
| `Network` (68) | 5-layer CNN backbone per FAQ + `cls_layer` head → **8,631** logits |
| `criterion` (70) | cross-entropy loss |
| `optimizer` (70) | SGD + momentum + weight decay (baseline recipe) |
| scheduler (70/83) | LR scheduler wired into the train loop |
| `verification_metrics` (72) | EER, AUC, TNR, pos/neg counts, ACC from labels+scores |
| `MODEL = model` (98) | point submission cell at the trained model |

## Steps

1. Implement transforms, cls/ver datasets and loaders (full label space, no subset).
2. Implement the 5-layer CNN backbone per starter FAQ + `cls_layer` → 8,631 logits; keep the `forward(x, return_feats)` contract intact.
3. Implement criterion (CE), optimizer (SGD+momentum+w.d.), scheduler; record exact values in the config for T04.
4. Implement `verification_metrics` (EER/AUC/TNR/ACC) used by `valid_epoch_ver`.
5. `summary(model, (3,112,112))`: check feature shape and param count — **must stay ≤ 30M** (≪ 30M for the 5-layer baseline).

## Acceptance

- [ ] Every TODO in the inventory above implemented — no `NotImplementedError` left in the run path
- [ ] Backbone + head per FAQ, no `torchvision.models`, no pretrained weights
- [ ] `summary()` output recorded here (layers, feature dim, params) with param count ≤ 30M
- [ ] Full cls + ver loaders yield batches; one forward pass on CPU/MPS works locally (PSC GPU check in T03)
- [ ] Baseline recipe (CE, optimizer, scheduler, transforms) written down for T04 to reuse verbatim

## Notes / current status

- (fill as you run)
