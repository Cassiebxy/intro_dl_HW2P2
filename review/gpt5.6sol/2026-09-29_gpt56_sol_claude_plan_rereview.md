# Review: Claude Planning Revision Re-review — GPT-5.6 Sol — 2026-09-29

- **Reviewed object**: repository head `e94faa43c15d0206f5d2793f0387f093bd516ea8`; substantive planning state comes from `f6afb6ced2e66906229e2c84e3a6316b86de02ff` / Claude revision `8a86f5d965cecd3ea6d6e0b7321c6468c1c07b10`.
- **Reviewer model/context**: GPT-5.6 Sol, MLE review of current `PLAN.md`, `README.md`, `DATA.md`, requirements, hypotheses, critical path, taskboard, T01–T10, starter notebook interfaces/TODOs, and archived staff constraints.
- **Overall verdict**: **substantially improved and directionally approved, with remaining execution-contract concerns**. The project can move toward implementation after a small Phase-A cleanup; the model family does not need another redesign.

## What is now correct

The Claude revision fixes the central structural problems from the earlier GPT review:

- PSC data is now staged on node-local `$LOCAL`; checkpoints/code are persistent.
- T02 correctly treats the starter as a **pipeline scaffold**, not merely a missing CNN backbone.
- T03 correctly preserves the **8,631-class output space** and limits work by batches/steps.
- Reload integrity and resumed optimization are separated.
- Official hard rules now explicitly include no external data, immutable submission cells, and final-`MODEL` integrity.
- Public planning/records are separated from private/local graded implementation.
- Oct 2 remains an official graded checkpoint but is no longer used internally as a reason to skip correctness work.
- Residual/augmentation exploration is gated on a verified baseline rather than Kaggle cutoff success.
- Final submission no longer requires every optional experiment to succeed.

The model direction remains appropriate:

```text
starter 5-layer CNN + CE
→ custom residual CNN + CE
→ selective augmentation / ArcFace
→ optional verification refinement
→ best reproducible final candidate
```

That is compatible with the assignment's CNN-from-scratch constraints and is a sensible risk-adjusted plan.

## Writeup / official-constraint alignment

The current plan is broadly aligned with the writeup-derived/starter/staff requirements already captured in the repository:

- face classification + verification;
- 8,631-class classifier output;
- CNN only, from scratch, no pretrained weights / imported ready-made CNN;
- ≤30M parameter budget;
- no external data;
- checkpoint requirements and Kaggle daily limit;
- protected submission path left unchanged;
- final Gradescope `MODEL` must correspond to the selected Kaggle model;
- ArcFace parameters train, model/class embeddings normalize, and ArcFace is not combined with label smoothing / Mixup / CutMix;
- final Kaggle and code-package deadlines are represented.

I did not directly re-extract the binary PDF through the GitHub connector in this pass. Compliance was cross-checked against the current starter notebook, archived staff rules, and writeup-derived facts already recorded in the repo. No direct contradiction with those official constraints was found, subject to the findings below.

## Findings

| # | Severity | Location | Finding | Recommended change | Status |
| --- | --- | --- | --- | --- | --- |
| 1 | **critical** | T01 vs T02 | **Dependency inversion:** T01 requires full cls/ver DataLoaders to be built and iterated, while T02 is the task that implements those datasets/loaders. A clean run cannot complete T01 before T02. | Make T01 environment/storage/raw-data/auth only. Move loader-yield validation into T02 acceptance or T03. T01/T02 may proceed in parallel; both gate T03. | open |
| 2 | **critical** | T01 / README / `.gitignore` | T01 suggests `.env` / `secrets.json` as git-ignored local credential files, but current `.gitignore` ignores neither. | Add explicit credential-file ignore patterns before using them, or use process environment variables only. The current text is unsafe as written. | open |
| 3 | **major** | T02 TODO inventory | `MODEL = model` is included in baseline pipeline completion. It is a final packaging assignment, not a prerequisite for training/smoke, and the final `MODEL` must correspond to the selected trained submission model. | Remove cell 98 from T02. Handle `MODEL` only in T10 / Gradescope packaging after final model freeze. | open |
| 4 | **major** | T03 acceptance | “Resume ≥1 step and verify loss keeps decreasing” is not a valid deterministic resume test. Stochastic mini-batch loss can rise even when resume is correct. | Verify model/optimizer/scheduler state restoration, LR/epoch/global-step continuity, finite gradients/loss, parameter update after resumed `optimizer.step()`, and AMP scaler state if used. Use multi-batch/fixed-batch loss trend only as diagnostic. | open |
| 5 | **major** | H1 / metrics | H1 says local “val combined below the ~0.80 region,” but starter validation metrics are computed in **percent units (0–100)**. | Use `~80.0%` for local validation; reserve `0.80` for Kaggle scale. Encode units explicitly in experiment logs. | open |
| 6 | **major** | T04 vs critical path | T04 still says “launch as background run on PSC,” although the revised critical path correctly says background execution does not survive allocation expiry/node loss. It also says keep **all** epoch checkpoints. | Describe a resumable run within the active allocation. Persist `last.pth` + `best_combined.pth` (optionally best-cls/best-EER); keep all epoch metrics, not every epoch weight file. | open |
| 7 | **major** | T02/T04 baseline recipe | Exact baseline hyperparameters are not yet frozen: momentum, weight decay, scheduler parameters, augmentation set, seed, etc. | Before T04, record one authoritative H1 config and reuse it verbatim for the reference run. Avoid hidden/default hyperparameters. | open |
| 8 | **major** | H2 + T06 | The residual screen still effectively requires early H1 outperformance to promote. A deeper model can warm up more slowly. “Same optimizer/schedule as H1” is useful for an ablation but not necessarily a competent final comparison. | Use 2–3 epoch screens for feasibility only: shape/OOM/throughput/finite gradients/loss movement. Separate equal-recipe architecture ablation from reasonably tuned per-model comparison. | open |
| 9 | **major** | H3 + T07 | ArcFace promotion still requires better EER with same/better classification accuracy, which can reject a model with a better actual 50/50 combined score. Train/inference behavior is also underspecified. | Select/promote on **combined validation score**, report both components, and define label-dependent train logits vs label-free classification inference, `out`/`feats` contract, optimized parameters, and checkpoint contents. | open |
| 10 | **major** | T07 parameter budget | ArcFace adds a trainable per-class embedding matrix, but T07 does not explicitly recount the parameter budget. | Count all trainable candidate parameters—backbone + classifier/ArcFace head + auxiliary trainable heads—before training; document any staff clarification if budget accounting differs. | open |
| 11 | **major** | T09 / immutable submission path | Verification-only TTA is not yet proven compatible with the immutable official submission cell, which invokes the same model interface for classification and verification. | Design TTA through the allowed model/interface without changing protected code. Normalize original/flip embeddings separately → fuse → normalize. Verify classification output is unchanged if that is the claim; otherwise reevaluate both metrics. | open |
| 12 | **medium** | PLAN Phase B | PLAN says B1–B4 are optional but also calls baseline → residual → ArcFace the “fixed route.” | Rename it to the **default exploration order if pursued**. Only final selection/submission is mandatory. | open |
| 13 | **medium** | PLAN experiment/Kaggle rule | “Only spend a Kaggle slot when val combined exceeds the current best submission” is too rigid: first format validation, reproducibility checks, and corrected inference can justify a slot. | Keep the 10/day budget, but allow explicit-purpose exceptions and record why each slot was used. | open |
| 14 | **medium** | DATA.md | Tree text still says `test_pairs.txt` is “same format” as labeled `val_pairs.txt`, while test pairs are unlabeled. | State explicit schemas: val = `imgA imgB label`; test = `imgA imgB`. Phrase the classifier as an 8,631-output model rather than implying test labels exist. | open |
| 15 | **medium** | T02 transforms | T02 treats augmentation as something that must be filled before the engineering pipeline is complete. That can accidentally mix modeling changes into H1. | Require transforms to be runnable; allow the baseline augmentation block to be intentionally minimal/empty and document it. Test augmentation cleanly in T08. | open |
| 16 | **medium** | optional-task dependencies | T07 is blocked by T06 and T09 by T06/T07 even though B1–B4 are declared optional. | T07 should require a selected validated CE backbone (H1 or H2); T09 should require a frozen validated candidate plus compatible inference path. | open |

## Priority before execution

No further architecture brainstorming is needed. Before the first serious PSC run, resolve **1–7**. Findings 8–16 should be fixed before their corresponding Phase-B experiments.

Recommended immediate dependency graph:

```text
T01 environment + $LOCAL + persistent storage + auth
        ╲
         ├── T02 starter pipeline + H1 config
        ╱
T01 + T02 complete
        ↓
T03 full-8631 limited-step smoke
    + fixed-input reload
    + true optimizer/scheduler resume
    + immutable submission generation
        ↓
T04 measured full-data baseline
```

## MLE assessment

The revised planning is **reasonable, substantially more executable, and mostly aligned with the official writeup/starter rules**. Remaining risk is now concentrated in execution details—task dependencies, metric units, resume semantics, exact baseline config, and experiment attribution—not in the overall model choice.

The best next move is to clean those contracts and start Phase A, rather than adding more model families or another broad planning layer.

## Scope limitations

- No training, benchmark, PSC execution, Kaggle submission, or Gradescope package was run.
- No claim is made that a particular ResNet depth, 512-d embedding, ArcFace, augmentation, or TTA will improve the score.
- Live Piazza was not re-fetched in this pass; the repo's archived staff rules were used.
- The binary writeup PDF was not directly text-extracted in this pass; compliance was assessed against the starter and writeup-derived constraints already recorded in the repo.
