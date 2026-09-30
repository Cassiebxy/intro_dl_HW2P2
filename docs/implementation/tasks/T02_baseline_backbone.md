# T02 — Starter Pipeline Completion + Baseline 5-Layer CNN

- **Status**: done (local CPU verification; PSC GPU check deferred to T03) | **Owner**: Cathy | **Track**: stable | **Due**: Sep 29–30
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

- [x] Every TODO in the inventory above implemented — no `NotImplementedError` left in the run path (cells 51/56/59/68/70/72 filled via `scripts/apply_starter_edits.py`; local runner executes the full chain)
- [x] Backbone + head per FAQ, no `torchvision.models`, no pretrained weights (plain `nn` conv blocks; `torchinfo` fallback only for CPU summary)
- [x] Param count recorded: **15,127,031** params (≤ 30M); feature dim 1024 after `AdaptiveAvgPool2d((1,1))` + `Flatten`; head `Linear(1024, 8631)`
- [x] Full cls + ver loaders yield batches; forward + full train/val/test chain works on local CPU (PSC GPU check in T03)
- [x] Frozen baseline recipe written down — see table below; T04 reuses verbatim
- [x] `verification_metrics` percent units verified by `scripts/sanity_metrics.py` (runs the actual notebook cell 72 source): perfect separation → EER=0.0000 / AUC=100.0000 / ACC=100.0000; pos_min 0.7 > neg_max 0.25; reversed scores → AUC=0.0000; TPRs in percent; noisy case EER=17.17 / ACC=84.00 / AUC=93.52

### Frozen baseline recipe (T04 reuses verbatim)

| Item | Value |
| --- | --- |
| Model | Starter 5-layer CNN (`Network`): conv 3→64 k7 s4, 128/256/512/1024 k3 s2, pad=k//2, BN+ReLU, AdaptiveAvgPool→Flatten, Linear 1024→**8631**; 15,127,031 params |
| Loss | `nn.CrossEntropyLoss()` |
| Optimizer | SGD lr=0.01, momentum=0.9, weight_decay=1e-4 |
| Scheduler | `CosineAnnealingLR(T_max=config["epochs"])` — `HW2P2_EPOCHS` = total budget, fixed across resume segments (contract in decision log 2026-09-30) |
| Batch size | 64 (env `HW2P2_BATCH_SIZE`) |
| Epochs | 2 default for early-submission recipe (env `HW2P2_EPOCHS`) |
| Image size / transform | 112×112; Resize→ToTensor→ToDtype(scale)→Normalize([0.5]×3) |
| Augmentation | train only: `RandomHorizontalFlip(p=0.5)` — intentionally minimal, T08 tests richer sets |
| Precision | AMP fp16 autocast + GradScaler on CUDA; CPU fallback via nullcontext/CPU scaler in local runner only |
| Seed | 42 (`HW2P2_SEED`); `torch.manual_seed` in config cell before init/shuffle/aug |

## Notes / current status

- 2026-09-30: implemented in working notebook `HW2P2_Student.ipynb` (private repo `idl_HW2P2`, regenerate with `scripts/apply_starter_edits.py`, starter untouched). Local CPU smoke via `scripts/run_notebook_local.py`: fresh 2-epoch run combined 32.4519% → 38.3523%; resume-after-completion skips retraining and regenerates artifacts (PASS). Determinism: identical scores across runs with seed 42. Scheduler/resume contract recorded in `docs/design/decision_log.md`. PSC GPU + real-data check = T03.
