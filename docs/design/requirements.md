# HW2P2 Requirements & Constraints (summary)

> This is a working summary. The authoritative sources are `references/Piazza_HW2P2_Staff_Guidelines.md` (archived staff posts @301/@302/@303) and the official `starter/` notebook + writeups. When this summary and an official source disagree, the official source wins.

## Task

Two connected tasks sharing one CNN feature extractor:

1. **Classification**: predict identity over 8,631 known classes (112×112 RGB faces).
2. **Verification**: cosine similarity between learned embeddings for image pairs; identities may be unseen in training. Scored by EER.

## Metric

```
Final Kaggle Score = 0.5 × Classification Accuracy + 0.5 × (1 − Verification EER)
```

- Cutoffs: Very Low = **0.80** (checkpoint requirement); Low/Medium/High TBD, released after checkpoint deadline.
- Kaggle daily submission limit: **10**.

## Deadlines & submission checklist

| What | When | Notes |
| --- | --- | --- |
| Checkpoint (early) | Oct 2, 2026 11:59 PM EST | Miss ⇒ final score × 0.97 |
| Final Kaggle | Oct 9, 2026 11:59 PM EST | Weekend (2 days) after for Gradescope cleanup+submit |

Checkpoint requires ALL of (Oct 2 is a **best-effort** milestone — miss ⇒ −3%/×0.97, but it is not a gate that compresses the pipeline or truncates learning steps):
- [ ] Kaggle submission made (join via the exact link in `references/`)
- [ ] Score ≥ 0.80 cutoff (target, not a pipeline-compression gate)
- [ ] HW2 Canvas Quiz completed
- [ ] Name visible on the Kaggle leaderboard

## Submission integrity & data rules (hard — from starter Requirement Acknowledgement)

- **No external data or datasets at any stage** (starter acknowledgement rule 5). Only the provided `hw2p2_data/`.
- **Protected submission cells must not be modified.** The notebook's final submission cell(s) are DO-NOT-MODIFY; altering them (even to fix a discrepancy) is an **Academic Integrity Violation** (starter acknowledgement rule 3).
- **Final Gradescope `MODEL` must match the selected Kaggle submission model.** The model object assigned to `MODEL` in the final notebook must be the same model whose score was selected — no train-time-only or swapped-in variant (starter acknowledgement rule 3).
- Credentials (Kaggle API, wandb key) come from environment variables / untracked config — never committed, never pasted into a notebook copy that could be published.

## Model rules (instructor-endorsed)

- **CNN only.** No RNN/GRU/LSTM, no attention/Transformer, no GNN, no pretrained encoder/decoder.
- Implement from scratch with `torch.nn`-style components; train from scratch; **no pretrained weights**; no imported architectures (`torchvision.models.*` forbidden).
- **≤ 30M parameters**, ensembles included.
- ArcFace rules (@303):
  1. ArcFace has trainable per-class embedding parameters — keep them updated.
  2. Normalize model output embedding and ArcFace class embeddings to unit vectors.
  3. Update ArcFace embeddings during training.
  4. **No label smoothing / CutMix / Mixup with ArcFace.**

## Platform notes

- PSC V100 via course shared conda env (V100 only, per @254 policy); Kaggle/Colab also possible.
- Starter notebook supports Colab / Kaggle / PSC; backbone (cell 68) is a student TODO.

## Open items to watch

- Low/Med/High cutoffs TBD → only 80% is a hard target; rely on val combined score otherwise.
- Piazza @38 master index still has broken `@XX` links; use direct post links.
- Any rule ambiguity → ask staff on the designated HW2P2 Piazza thread BEFORE implementing; student answers are hints, not permissions.
