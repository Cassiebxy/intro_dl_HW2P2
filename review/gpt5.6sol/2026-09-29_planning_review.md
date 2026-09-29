# Review: HW2P2 Planning & Execution Framework — GPT-5.6 Sol — 2026-09-29

- **Reviewed object**: `README.md`, `PLAN.md`, `HW2P2_Milestone_Check.md`, `docs/design/*`, `docs/implementation/*`, `references/Piazza_HW2P2_Staff_Guidelines.md`, and the PyTorch starter notebook at commit `3c0fc609d8f55db2130cff479ac1ef10d4624bc5`
- **Reviewer model/context**: GPT-5.6 Sol; review based on the repository planning/docs plus direct inspection of the PyTorch starter notebook cells and archived staff rules.
- **Overall verdict**: **direction approved, major execution revisions needed before implementation**.

## Executive summary

The high-level strategy is sound and should be retained:

1. Establish a reproducible checkpoint-safe baseline first.
2. After crossing the 0.80 checkpoint cutoff, move to a stronger custom residual CNN.
3. Improve the training objective / generalization only after a strong CE backbone exists.
4. Apply verification-specific inference improvements last.

The main issue is not model-family choice. The current plan understates how much of the official starter notebook remains incomplete and therefore makes Phase A look more linear than it actually is. Several experiment rules in Phase B also need tightening so short screens do not accidentally reject promising deeper models.

## Findings (severity ordered)

| # | Severity | Location | Issue | Suggested fix | Status |
| --- | --- | --- | --- | --- | --- |
| 1 | **critical** | `T02_baseline_backbone.md`, `T03_smoke_test.md` | The starter has substantially more TODOs than the backbone. Classification datasets/loaders, verification datasets/loaders, augmentation, verification metrics (ROC/EER/AUC/ACC), loss, optimizer, and scheduler are also incomplete. T03 cannot run after only T01+T02. | Add an explicit **Starter Pipeline Completion** stage before the end-to-end smoke test, or expand T02 into multiple implementation tasks covering every required TODO. | pending Qwen/Claude response |
| 2 | **critical** | `T01_psc_env_verify.md`, `DATA.md` | PSC data-path assumptions do not match the starter guidance. The notebook explicitly instructs PSC users to download/extract the dataset to the current GPU node's `$LOCAL` storage to avoid shared-filesystem I/O bottlenecks. `$LOCAL` is temporary and disappears when the node changes. | Use `$LOCAL/hw2p2_data` for training data; keep checkpoints/code under persistent `/jet/home/<user>/...`. Make node-local data re-download part of T01. | pending Qwen/Claude response |
| 3 | **critical** | `T03_smoke_test.md` | The proposed `num_classes=200` smoke conflicts with the starter's validation path. A 200-logit classifier cannot compute CE against full dev labels 0–8630, and the starter asserts train/val class lists match. | Keep the classifier output at **8631 classes** for the primary smoke test and reduce runtime by limiting batches/samples/steps instead of shrinking the label space. | pending Qwen/Claude response |
| 4 | **major** | `PLAN.md`, `T04_baseline_train.md` | Local validation and Kaggle score scales are mixed. Starter validation metrics are percentages (0–100) and compute `0.5 * cls_acc + 0.5 * (100 - EER)`; Kaggle is reported 0–1. | Use explicit units everywhere: local validation target `80.0%`; Kaggle target `0.80`. | pending Qwen/Claude response |
| 5 | **major** | `README.md`, workflow | The repo claims to be the single source of truth, but working notebook copies are expected to live only on PSC/Jupyter. Important implementation changes could therefore exist outside Git history. | Keep `starter/` immutable, but track a working notebook or source directory (for example `notebooks/HW2P2_working.ipynb` or `src/`) while continuing to exclude credentials, data, checkpoints, and submissions. | pending Qwen/Claude response |
| 6 | **major** | `T04_baseline_train.md` | “20 epochs” is correctly taken from the starter as an early-submission recommendation, but the actual baseline training recipe is still unspecified because criterion/optimizer/scheduler are TODOs. | Before the long run, freeze and record one baseline recipe: CE, SGD+momentum, weight decay, cosine schedule, AMP, batch size, and exact augmentation set. Treat 20 epochs as a reference budget rather than a mandatory stopping point. | pending Qwen/Claude response |
| 7 | **major** | `T01`, `T03`, `critical_path.md` | “Background training” does not solve PSC allocation limits. The starter example requests an 8-hour interactive job, and node-local data disappears after moving nodes. Resume behavior is therefore part of the critical path. | Make **resume verification** part of the smoke test: save and reload model, optimizer, scheduler, epoch, and metrics from persistent storage, then continue training for at least one step/mini-epoch. | pending Qwen/Claude response |
| 8 | **major** | `T04_baseline_train.md` | “Keep ALL epoch checkpoints until done” is unnecessary and may waste persistent quota; the starter already maintains last/best checkpoints. | Keep `last` and `best_combined` as required; optionally keep `best_cls` and best verification checkpoint. Preserve all metrics, not all weights. | pending Qwen/Claude response |
| 9 | **major** | `H2`, `T06_resnet_ce.md` | The current 2–3 epoch screen can incorrectly reject a deeper residual model. This also conflicts with the HW1→HW2 handover note that short screens cannot automatically reject slow-starting models. | Use short screens only for feasibility: OOM, throughput, shape bugs, loss decrease, gradient/training stability. Promotion to a full comparison should not require beating H1 after 2–3 epochs. | pending Qwen/Claude response |
| 10 | **major** | `T06_resnet_ce.md` | “Same optimizer/schedule as H1” is not automatically a fair architecture comparison. A deeper residual CNN may need a different LR/warmup/schedule to be trained competently. | Separate **architecture feasibility** from **training-recipe tuning**. Compare under a documented compute budget, but allow a reasonable model-specific recipe once the architecture is shown to train correctly. | pending Qwen/Claude response |
| 11 | **major** | Phase B ordering | The plan jumps from ResNet CE directly to ArcFace without an explicit stage for building a strong CE training recipe. | Recommended order: **strong residual CE backbone → training/generalization/augmentation tuning → ArcFace controlled comparison → verification TTA**. | pending Qwen/Claude response |
| 12 | **major** | `H3`, `T07_arcface.md` | ArcFace promotion is framed as “EER improves at same or better classification accuracy.” That can reject a model that improves the actual 50/50 combined objective. | Use **combined validation score** as the promotion criterion while always reporting classification accuracy and EER separately. | pending Qwen/Claude response |
| 13 | **major** | `T07_arcface.md` | ArcFace is not a drop-in replacement for the starter's current `criterion(logits, labels)` path. It changes the relationship among embeddings, class weights, labels, training logits, and final classification inference. | Before implementation, define an explicit ArcFace train/inference contract: where normalized embeddings are produced, how label-dependent margin logits are computed, how the classification head is represented at inference, and which parameters are optimized/checkpointed. | pending Qwen/Claude response |
| 14 | **major** | `requirements.md`, `PLAN.md` | Several starter acknowledgement rules are not yet prominent in the planning docs: no external data at any stage, required submission cells must not be modified, and the final `MODEL` must match the best Kaggle submission. | Add these to the hard constraints/red-line checklist before coding begins. | pending Qwen/Claude response |
| 15 | **medium** | `T08_augmentation.md` | Augmentation is allowed to start immediately after T05, even before the residual backbone is stabilized. Results on the shallow checkpoint model may not transfer cleanly. | First establish the residual CE backbone; then tune augmentation on that stable backbone. Carry only compatible transforms into ArcFace experiments. | pending Qwen/Claude response |
| 16 | **medium** | model parking lot | SE blocks introduce a rule ambiguity because squeeze-and-excitation is commonly described as channel attention, while the assignment prohibits attention modules. | Do not spend implementation time on SE unless staff explicitly confirms it is allowed. Keep ConvNeXt-like ideas low priority until the simpler residual route is exhausted. | pending Qwen/Claude response |
| 17 | **medium** | `T09_verification_tta.md` | Flip-TTA recipe should be specified more precisely. | Prefer: L2-normalize original embedding and flipped embedding separately → average/sum → L2-normalize fused embedding → cosine similarity. Evaluate against the same frozen checkpoint. | pending Qwen/Claude response |

## Recommended revised top-level flow

```text
A0  rules/repo safety
    └─ immutable starter + tracked working implementation + hard-rule checklist

A1  PSC/environment/data
    └─ V100 + shared env + $LOCAL data + persistent checkpoint path

A2  complete starter pipeline
    └─ transforms
       → cls datasets/loaders
       → ver datasets/loaders
       → 5-layer CNN
       → CE / SGD / scheduler
       → verification metrics

A3  end-to-end smoke
    └─ 8631-class output
       → limited batches/steps
       → train
       → cls validation
       → verification/EER
       → save
       → kernel/session reload
       → resume
       → inference
       → submission.csv

A4  checkpoint baseline
    └─ full-data baseline
       → submit once validation is plausibly cutoff-safe
       → freeze first Kaggle >= 0.80 checkpoint

B1  strong residual CNN + CE
B2  training recipe + augmentation
B3  ArcFace controlled comparison
B4  verification TTA
B5  final model selection + Kaggle + Gradescope
```

## Model-direction assessment

- **Keep ResNet-style CNN as the main exploration family.** It is a good match for the assignment, 112×112 images, from-scratch requirement, parameter budget, and available time.
- A ResNet18-like or ResNet34-like custom implementation is a better next step than immediately pursuing ConvNeXt-like or SE variants.
- Do not optimize for architectural novelty. A well-trained residual CNN with a strong CE recipe and sensible augmentation is a more valuable reference before adding ArcFace.
- ArcFace remains a reasonable later experiment because verification is explicitly scored, but it should be treated as a controlled objective/inference redesign rather than a simple loss swap.

## What this review does NOT cover

- No training run was executed; there are currently no experiment logs or Kaggle results to validate modeling assumptions.
- The binary PDF writeups were not text-parsed through the GitHub connector in this review. Rule checks were cross-referenced against the PyTorch starter notebook's requirement acknowledgement and the archived Piazza staff guidelines.
- No claim is made yet about the best residual depth, embedding dimension, ArcFace margin/scale, or final hyperparameters; those require empirical results.

## Follow-ups proposed

- [ ] Rework T01–T05 so Phase A reflects every actual starter TODO.
- [ ] Fix the PSC `$LOCAL` data-path documentation.
- [ ] Replace the reduced-class primary smoke test with an 8631-output limited-step smoke test.
- [ ] Add explicit resume-training verification.
- [ ] Define and freeze a baseline training recipe before T04.
- [ ] Rewrite T06 screen criteria so short runs test feasibility rather than final model quality.
- [ ] Add a strong-CE training/generalization stage before ArcFace.
- [ ] Define ArcFace's training/inference interface before coding it.
- [ ] Add missing starter acknowledgement rules to the hard constraints.
