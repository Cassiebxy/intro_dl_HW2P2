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

Checkpoint requires ALL of:
- [ ] Kaggle submission made (join via the exact link in `references/`)
- [ ] Score ≥ 0.80 cutoff
- [x] HW2 Canvas Quiz completed — Cathy confirmed Sep 29; score not independently verified
- [ ] Name visible on the Kaggle leaderboard

> **Planning priority (not course policy):** Cathy accepts missing the early cutoff. Do not compress setup, correctness checks or learning to reach 0.80 by Oct 2; the official requirements and consequences above remain unchanged.

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
