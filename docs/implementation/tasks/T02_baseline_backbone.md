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

> `MODEL = model` (cell 98) is **not** a T02 TODO: it is a final packaging assignment. It is set in T10, after the final model is frozen, pointing at the selected trained model. Validate `out`/`feats` interfaces here; do not assign `MODEL` before a baseline exists.

## Steps

1. Implement transforms, cls/ver datasets and loaders (full label space, no subset). The baseline augmentation block may be **intentionally minimal/empty** (e.g. resize+normalize only) — document that choice explicitly; augmentation is a modeling change tested cleanly in T08, so do not smuggle it into H1 here.
2. Implement the 5-layer CNN backbone per starter FAQ + `cls_layer` → 8,631 logits; keep the `forward(x, return_feats)` contract intact.
3. Implement criterion (CE), optimizer (SGD+momentum+w.d.), scheduler. **Freeze the exact baseline recipe — no hidden/library-default parameters**: lr, momentum, weight_decay, scheduler type+params, batch size, epochs, augmentation set, seed, loss. Write every value into one authoritative config block; T04 reuses it verbatim.
4. Implement `verification_metrics` (EER/AUC/TNR/ACC) used by `valid_epoch_ver`, matching the starter's expected return keys and **percent units (0–100)** — see the metric-units contract in `docs/design/requirements.md`.
5. `summary(model, (3,112,112))`: check feature shape and param count — **must stay ≤ 30M** (≪ 30M for the 5-layer baseline).

## Acceptance

- [ ] Every TODO in the inventory above implemented — no `NotImplementedError` left in the run path
- [ ] Backbone + head per FAQ, no `torchvision.models`, no pretrained weights
- [ ] `summary()` output recorded here (layers, feature dim, params) with param count ≤ 30M
- [ ] Full cls + ver loaders yield batches; one forward pass on CPU/MPS works locally (PSC GPU check in T03)
- [ ] Frozen baseline recipe written down for T04 to reuse verbatim — every value explicit (lr, momentum, weight_decay, scheduler params, augmentation set, seed), no implicit defaults
- [ ] `verification_metrics` returns the starter's expected keys in percent units; deterministic sanity checks pass: both positive and negative pairs present, same-identity pairs score higher similarity than different-identity pairs, perfectly separated scores give EER ≈ 0

## Notes / current status

- (fill as you run)
