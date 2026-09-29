# HW2P2 Master Plan (2026-09-29 → 2026-10-11)

> This file is the single source of truth for the HW2P2 project plan.
> Official requirements live in `references/` and `starter/`; per-task details live in `docs/implementation/tasks/`.

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/gpt6.1sol/2026-09-29_planning_review.md`.

## Context

- **Course**: CMU 11-785 HW2P2 (Fall 2026) — face classification (8,631 identities, 112×112 images) + verification (embedding pairs).
- **Metric**: `0.5 × Classification Accuracy + 0.5 × (1 − EER)`; checkpoint requires Kaggle score ≥ **80%** (Kaggle uses the 0–1 fraction scale; local validation runs on percent units — see the units contract in `docs/design/requirements.md`).
- **Hard constraints** (see `references/Piazza_HW2P2_Staff_Guidelines.md`):
  - CNN only, ≤ **30M parameters** (including ensembles), implemented from scratch, trained from scratch.
  - No pretrained weights, no RNN/GRU/LSTM, no attention/Transformer, no GNN, no pretrained encoder/decoder.
  - ArcFace rules: its parameters must be trained; embeddings and class weights normalized; **never** combine with label smoothing / Mixup / CutMix.
- **Deadlines**: Checkpoint **Oct 2, 11:59 PM EST** (miss ⇒ −3%); Final **Oct 9, 11:59 PM EST**; Kaggle daily limit **10**.
- **Status as of 2026-09-29**: PSC V100 allocation confirmed; data available locally; **nothing run yet** — no smoke test, no baseline training, no Kaggle submission.
- **Key finding (verified against starter notebook)**: the starter is a scaffold, not a turnkey baseline. Beyond the 5-layer CNN backbone (cell 68), datasets/loaders (cls + ver), criterion/optimizer/scheduler (cell 70), and verification metrics (cell 72) are all `NotImplementedError`/TODO. Phase A therefore includes a **pipeline completion** stage (T02), not just a backbone.

## Two parallel tracks (from HW1→HW2 handover principle)

| Track | Goal | Window |
| --- | --- | --- |
| Stable track | A complete, correct, reproducible pipeline and verified baseline; checkpoint by Oct 2 as **best-effort** (miss ⇒ −3% penalty, not a pipeline-compression gate) | Sep 29 → Oct 2; readiness-driven |
| Exploration track | Optional stronger residual CNN + ArcFace + augmentation + TTA after a verified baseline, budget/val-driven | Baseline ready → Oct 9 |

## Phase A — Complete setup and reproducible baseline (Oct 2 checkpoint = best-effort milestone)

### A1. PSC environment & storage verification (T01, first thing)
- SSH Bridges2 → request compute node → load the course shared conda env → launch Jupyter on the GPU node.
- Two storage zones: dataset on node-local `$LOCAL/hw2p2_data` (re-download per node), checkpoints/code on the **persistent** home path; `checkpoint_dir` writable and reloadable after a kernel restart.
- Credentials via environment variables (preferred) or a git-ignored untracked config — never written into a notebook or committed file.
- **Scope**: env/storage/raw-data/credentials only — loader code belongs to T02; loader iteration is verified in T02/T03. T01 ∥ T02 in parallel; both gate T03.
- **Done when**: `nvidia-smi` shows V100, raw data staged on `$LOCAL` and spot-checked against `DATA.md`, checkpoint survives a kernel restart (persistent path documented).

### A2. Complete the starter pipeline + baseline backbone (T02)
- Fill every starter TODO: transforms, cls/ver datasets + loaders, 5-layer CNN backbone + classification head (8,631 logits), criterion, optimizer, scheduler, verification metrics; verify param count ≤30M with `summary(model, (3,112,112))`. Baseline augmentation intentionally minimal/empty and documented — augmentation is tested in T08.
- **Freeze the exact baseline recipe** (lr, momentum, weight_decay, scheduler params, augmentation set, seed, batch size, epochs) in one authoritative config; T04 reuses it verbatim. `MODEL = model` (cell 98) is **not** a T02 TODO — it is set in T10 after the final model freeze.
- Implement only — no long training yet.

### A3. End-to-end smoke test (T03, before any long training)

```text
full 8,631-class output, limited samples/batches/steps (not a reduced label space)
→ forward/backward → save checkpoint (incl. AMP scaler state if used)
→ fresh kernel reload, eval-mode fixed-input outputs match within tolerance (integrity)
→ resume ≥1 step: optimizer/scheduler state restored, counters continue,
   finite loss/grads, params actually update (correctness — not "loss decreased")
→ classification inference → verification EER (percent units)
→ submission.csv via the official immutable cell
```

- This exposes all engineering issues (data paths, kernel state, checkpointing, resume, submission format) before GPU time is spent on long training.

### A4. Baseline training (T04, start immediately after smoke test)
- Starter recommends **~20 epochs as a reference budget**; batch_size 64, increase if V100 memory allows.
- Record per epoch: train/val cls acc, ver EER, combined score; select best checkpoint by combined score.
- Estimate per-epoch wall-clock first (full-data, including validation); if it cannot fit before Oct 2, do **not** compress the pipeline or truncate learning to chase the date. Oct 2 is best-effort — submit the best verified checkpoint available and keep the full pipeline honest. 20 epochs is a reference budget, not a score guarantee.

### A5. Optional early-cutoff attempt (T05, when ready — earlier is nicer, no mandatory noon target)
- When the verified pipeline and trained model are ready, use the DO-NOT-MODIFY cells to generate and submit `submission.csv`; confirm the leaderboard shows your name; score ≥ 0.80 (Kaggle fraction scale) is the target, not a pipeline-compression gate. Missing it does not block the project or justify skipping steps.
- **Canvas HW2 Quiz: completed**, confirmed by Cathy on Sep 29; no quiz score independently verified.
- Keep buffer for training completion, download/upload, and leaderboard queue; the final Kaggle and Gradescope submissions remain project deliverables.

## Phase B — Exploration track (readiness-driven; original dates below are provisional)

> B1–B4 are **optional explorations**, entered only when a verified baseline exists (T04) and justified by validation results + remaining time budget — not mandatory phases. **Default exploration order if pursued**: baseline → residual CNN → ArcFace (only B5 final selection is mandatory).

1. **B1 Custom residual CNN + CE** (T06, Oct 3–4): ResNet-style from scratch, 512-d embeddings, ≤30M params; avoid aggressive early downsampling on 112×112; short screens record steps and wall-clock, not just epochs; controlled comparison vs starter backbone.
2. **B2 ArcFace comparison** (T07, Oct 4–6): same backbone, CE vs ArcFace; cls acc and EER reported separately; strictly follow staff ArcFace rules.
3. **B3 Augmentation** (T08, Oct 5–7, parallel with B2): identity-preserving only (flip, mild crop, photometric); change one thing at a time.
4. **B4 Verification TTA** (T09, Oct 7–8): flip embedding → average → normalize → cosine; evaluate by EER alone.
5. **B5 Final selection & submission** (T10, Oct 8–9): select by combined val score (not best single metric); reserve Oct 9 daytime as submission buffer; after Oct 9 produce the Gradescope zip (notebook steps 1–7).

## Experiment record (every run records at least)

- Hypothesis + the single main change; code/notebook version; full config and seed
- train/dev split; epochs, batch size, and **training steps / samples seen**
- best val combined score and its checkpoint; runtime / GPU constraints; Kaggle result if submitted
- conclusion: keep / reject / investigate + next step
- One authoritative config entry point; the checkpoint dir of a committed submission is **never overwritten** — reproductions get new run names
- Kaggle 10/day: prefer to spend a slot only when val combined beats the current best submission; **explicit-purpose exceptions are allowed** (first format/system validation, reproducibility check, corrected inference) — record the stated purpose and result for every slot used

## Risks and fallbacks

Phase A contract fixes (gpt5.6sol findings 1–7, gpt6.1sol R1–R4) are applied to T01–T04/T10/PLAN/taskboard on 2026-09-29. Phase B findings (gpt6.1sol R5–R11, gpt5.6sol #8–16) remain proposed and must be resolved before their corresponding tasks.

| Risk | Fallback |
| --- | --- |
| PSC queue / node wait | Queue time is used for T02/T03 code work. A background run does **not** survive PSC allocation limits or node loss — rely on checkpoint + resume (verified in T03), not on background continuity |
| Baseline cannot reach 0.80 before Oct 2 | Oct 2 is best-effort: keep completing and validating the pipeline, submit the best verified checkpoint available, record the missed opportunity and continue. Do not compress required steps to chase the cutoff |
| V100 OOM | Reduce batch_size / workers; record it, don't brute-force |
| Low/Med/High cutoffs still TBD | 0.80 is the checkpoint target, not a hard gate that compresses the pipeline; exploration decisions based on val combined score |
| ArcFace rule violations | Checklist against @301 before implementing; ask staff when ambiguous |

## Verification (definition of done)

- **Phase A**: setup, complete pipeline, smoke/resume checks, baseline measurements, and a reproducible inference path are verified; checkpoint submitted by Oct 2 as best-effort (score ≥ 0.80 is a target, not a pipeline-compression gate). Canvas quiz is user-confirmed complete.
- **Phase B**: every experiment has a record in `experiments/runs/`; final model is the best validated reproducible candidate by val combined score (percent units); compare against an early checkpoint submission **only if one exists** (otherwise explain in the decision log).
- **Final**: Gradescope zip generated and auto-grading passes; final `MODEL` matches the selected Kaggle submission model.
