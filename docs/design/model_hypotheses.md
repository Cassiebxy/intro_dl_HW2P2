# Model Hypothesis Cards

> Format per the HW1→HW2 handover: fill all 8 lines before spending a full training budget. A candidate that can't be written clearly is not ready for GPU time.

## H1 — Starter 5-layer CNN + CE (baseline, stable track)

- **Data facts**: 112×112 RGB faces; 431K train images over 8,631 classes (~50/class); separate 12K-image verification pool with pair labels.
- **Baseline limitation**: shallow CNN with default augmentation; likely underfits a 8,631-way problem; verification relies on CE-learned features, which cluster classes but may not spread embeddings well.
- **Candidate design**: implement exactly the official starter FAQ backbone + cross-entropy; per-starter recommended ~20 epochs.
- **Mechanism**: establishes the reproducible floor and a valid submission pipeline; nothing more is claimed.
- **Compliance & cost**: trivially compliant; ~20 epochs on V100 (measure per-epoch wall-clock in T03/T04).
- **Minimal experiment**: full-8631-output, limited-step smoke (T03) before full run.
- **Promotion rule**: promote H1 as the stable baseline on **reproducibility, validation metrics, training curves, and measured budget** — not on clearing an early Kaggle cutoff. The Oct 2 checkpoint is a best-effort milestone, not the promotion gate.
- **Stop rule**: a 20-epoch val combined below the ~0.80 region is **not** automatically an engineering bug. First confirm curves/metrics/budget are healthy; a low score with healthy curves is a capacity signal about the shallow baseline, not necessarily a pipeline fault. Do not compress the pipeline or learning steps to chase the date.

## H2 — Custom residual CNN (ResNet-style, from scratch) + CE

- **Data facts**: same as H1; faces have strong spatial structure; aggressive early downsampling at 112×112 risks discarding identity cues (edges, texture).
- **Baseline limitation**: 5-layer CNN underfits capacity-wise; deeper plain CNNs are hard to optimize without residual paths.
- **Candidate design**: ResNet-style blocks implemented from scratch (identity shortcuts, projection when dims change), gentle early downsampling, 512-d embedding head, ≤30M params.
- **Mechanism**: residual connections allow depth → better features; embedding width tradeoff (512-d) balances verification quality vs head params (8,631-class head is param-heavy).
- **Compliance & cost**: compliant (CNN from scratch); estimate params before training; screen cost ≈ short 2–3 epoch runs on subset, record steps + wall-clock, not just epochs.
- **Minimal experiment**: 2–3 epoch screen vs H1 at same batch/steps; check train loss decreases and val moves.
- **Promotion rule**: val combined improves over H1 at equal budget → full-data confirmation.
- **Stop rule**: no gain over H1 at equal optimization budget → keep H1 backbone, invest in objective (H3) instead.

## H3 — Same backbone, ArcFace objective (representation learning)

- **Data facts**: verification is scored by EER on possibly unseen identities; classification accuracy ≠ good embedding space.
- **Baseline limitation**: softmax-CE geometry leaves embeddings loosely clustered; inter-class margins are not directly controlled.
- **Candidate design**: fixed backbone (best of H1/H2) + ArcFace head: unit-normalized embeddings and class weights, angular margin m and scale s, per-class embedding params trained.
- **Mechanism**: normalized angular margin explicitly pushes inter-class separation → smaller intra-class distance → lower EER.
- **Compliance & cost**: compliant if @303 rules strictly followed (trainable params, normalization, no LS/Mixup/CutMix); cost ≈ H2 run cost + head overhead.
- **Minimal experiment**: short screen; watch loss scale and EER trend; sanity-check cosine similarity distributions.
- **Promotion rule**: EER improves at same or better cls accuracy → promote.
- **Stop rule**: EER not improved or training unstable → debug margin/scale before abandoning; if still bad, keep CE result and move budget to augmentation/TTA.

## Parking lot (not yet cards)

- Augmentation variants (T08), verification flip-TTA (T09), SE/ConvNeXt-style blocks (rule-check first), alternative embedding dims.
