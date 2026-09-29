# HW2P2 Master Plan (2026-09-29 → 2026-10-09)

> This file is the single source of truth for the HW2P2 project plan.
> Official requirements live in `references/` and `starter/`; per-task details live in `docs/implementation/tasks/`.

## Context

- **Course**: CMU 11-785 HW2P2 (Fall 2026) — face classification (8,631 identities, 112×112 images) + verification (embedding pairs).
- **Metric**: `0.5 × Classification Accuracy + 0.5 × (1 − EER)`; checkpoint requires Kaggle score ≥ **80%**.
- **Hard constraints** (see `references/Piazza_HW2P2_Staff_Guidelines.md`):
  - CNN only, ≤ **30M parameters** (including ensembles), implemented from scratch, trained from scratch.
  - No pretrained weights, no RNN/GRU/LSTM, no attention/Transformer, no GNN, no pretrained encoder/decoder.
  - ArcFace rules: its parameters must be trained; embeddings and class weights normalized; **never** combine with label smoothing / Mixup / CutMix.
- **Deadlines**: Checkpoint **Oct 2, 11:59 PM EST** (miss ⇒ −3%); Final **Oct 9, 11:59 PM EST**; Kaggle daily limit **10**.
- **Status as of 2026-09-29**: PSC V100 allocation confirmed; data available locally; **nothing run yet** — no smoke test, no baseline training, no Kaggle submission.
- **Key finding**: the starter notebook's backbone (cell 68) is a TODO — even the official baseline requires implementing a 5-layer CNN per the FAQ.

## Two parallel tracks (from HW1→HW2 handover principle)

| Track | Goal | Window |
| --- | --- | --- |
| Stable track | A committed checkpoint version ≥ 0.80, always reproducible | Sep 29 → Oct 2 |
| Exploration track | Stronger residual CNN + ArcFace + augmentation + TTA | Oct 2 evening → Oct 9 |

## Phase A — Checkpoint sprint (countdown from today)

### A1. PSC environment verification (T01, first thing)
- SSH Bridges2 → request compute node → load the course shared conda env → launch Jupyter.
- Confirm data path (shared `/local/hw2p2_data` or own copy), writable checkpoint dir, resumable runs.
- Configure wandb API key and Kaggle API key (notebook setup steps).
- **Done when**: `nvidia-smi` shows V100 and data loaders produce batches.

### A2. Implement baseline backbone (T02)
- Implement the 5-layer CNN per the starter FAQ (cell 68 TODO) + classification layer; verify shapes and param count with `summary(model, (3,112,112))`.
- Implement only — no optimization yet.

### A3. End-to-end smoke test (T03, before any long training)

```text
small subset (reduced num_classes + 1–2 epochs)
→ forward/backward → save checkpoint → reload checkpoint
→ classification inference → verification EER → generate submission.csv
```

- This exposes all engineering issues (data paths, kernel state, checkpointing, submission format) before GPU time is spent on long training.

### A4. Baseline training (T04, start immediately after smoke test)
- Starter recommends **20 epochs for the early submission**; batch_size 64, increase if V100 memory allows.
- Record per epoch: train/val cls acc, ver EER, combined score; select best checkpoint by combined score.
- Estimate per-epoch wall-clock first; if it cannot fit before Oct 2, trade epochs for a "cutoff-safe" score — the goal is crossing 80, not maximizing yet.

### A5. Checkpoint submission (T05, submit by Oct 2 **noon**, not the last hour)
- Use the DO-NOT-MODIFY cells to generate and submit `submission.csv`; confirm the leaderboard shows the name and score ≥ 80.
- **Complete the HW2 Canvas Quiz** (a checkpoint requirement).
- Keep buffer for training completion, download/upload, and leaderboard queue.

## Phase B — Exploration track (Oct 2 evening → Oct 9)

1. **B1 Custom residual CNN + CE** (T06, Oct 3–4): ResNet-style from scratch, 512-d embeddings, ≤30M params; avoid aggressive early downsampling on 112×112; short screens record steps and wall-clock, not just epochs; controlled comparison vs starter backbone.
2. **B2 ArcFace comparison** (T07, Oct 4–6): same backbone, CE vs ArcFace; cls acc and EER reported separately; strictly follow staff ArcFace rules.
3. **B3 Augmentation** (T08, Oct 5–7, parallel with B2): identity-preserving only (flip, mild crop, photometric); change one thing at a time.
4. **B4 Verification TTA** (T09, Oct 7–8): flip embedding → average → normalize → cosine; evaluate by EER alone.
5. **B5 Final selection & submission** (T10, Oct 8–9): select by combined val score (not best single metric); reserve Oct 9 daytime as submission buffer; after Oct 9 produce the Gradescope zip (notebook steps 1–7).

## Experiment hygiene (every run records at least)

- Hypothesis + the single main change; config; seed; best val combined score and its checkpoint; runtime / GPU constraints; conclusion (keep / reject / investigate).
- One authoritative config entry point; the checkpoint dir of a committed submission is **never overwritten** — reproductions get new run names.
- Kaggle 10/day: only spend a submission slot when validation combined score is higher than the current best submission.

## Risks and fallbacks

| Risk | Fallback |
| --- | --- |
| PSC queue / node wait | Long runs go to background; use waiting time for T02/T03 code work |
| 20 epochs won't fit before Oct 2 | Fewer epochs for a cutoff-safe score; submission pipeline already validated, can submit any time |
| V100 OOM | Reduce batch_size / workers; record it, don't brute-force |
| Low/Med/High cutoffs still TBD | Treat 80% as the only hard target; exploration decisions based on val combined score |
| ArcFace rule violations | Checklist against @301 before implementing; ask staff when ambiguous |

## Experiment record (every run records at least)

- hypothesis / main change; code or notebook version; full config and seed
- train/dev split; epochs, batch size, and training steps
- best validation metric and checkpoint used
- runtime / GPU constraints; Kaggle result if submitted
- conclusion: keep, reject, or investigate further

## Verification (definition of done)

- **Phase A**: full smoke-test chain passes; Kaggle leaderboard shows ≥ 80 with own name; Canvas quiz submitted.
- **Phase B**: every experiment has a record in `experiments/runs/`; final submission's val combined score ≥ checkpoint version.
- **Final**: Gradescope zip generated and auto-grading passes.
