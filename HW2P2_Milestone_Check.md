# HW2P2 Milestone Check

## 0. Purpose

> Status snapshot for HW2P2 as of September 23, 2026.
>
> This file records current progress, current understanding, open questions, remaining work, and the CNN / course knowledge needed at each project milestone.
>
> Official assignment requirements and Piazza staff clarifications are kept separately in the official starter materials and `references/Piazza_HW2P2_Staff_Guidelines.md`. Brainstorming and planning notes in this file should not be treated as official course policy.

## 1. Deadline

- **Checkpoint deadline:** October 2, 2026, 11:59 PM EST.
  - Missing the required early submission results in a 3% deduction / final score scaled by 0.97.
- **Final Kaggle deadline:** October 9, 2026, 11:59 PM EST.
  - Final code / related-file submission to Gradescope follows after the Kaggle deadline.
- **Kaggle submission requirements:**
  - Make a Kaggle submission before the checkpoint deadline.
  - Achieve at least the **Very Low cutoff: 0.80**.
  - Complete the **HW2 Canvas Quiz**.
  - My name must appear on the Kaggle leaderboard.
  - Kaggle daily submission limit: **10**.
  - Final Kaggle score:
    `0.5 × Classification Accuracy + 0.5 × (1 - Verification EER)`.

## 2. Current Status

- **Repo/starter code:**
  - GitHub repo is set up.
  - PyTorch starter notebook and official writeups are stored under `starter/`.
  - Official Piazza staff information is stored separately under `references/`.
  - `PLAN.md`, `README.md`, `.gitignore`, and `experiments/` are initialized.

- **Dataset:**
  - Official `hw2p2_data/` is available locally.
  - Dataset is intentionally excluded from GitHub because of its size.
  - Classification and verification data structures have been identified.

- **Baseline:**
  - Official starter baseline is available.
  - No verified end-to-end baseline training result has been recorded yet.
  - No baseline Kaggle score has been recorded yet.

- **Kaggle submission:**
  - Competition and submission requirements are understood.
  - No confirmed HW2P2 Kaggle submission has been recorded yet.

- **PSC/GPU setup:**
  - HW2P2-specific training environment, data paths, checkpoint paths, and full-run workflow still need to be verified before a long training run.

- **What has already been completed:**
  - Repository organization.
  - Dataset exclusion through `.gitignore`.
  - Starter materials archived.
  - Piazza staff guidelines archived separately.
  - Assignment rules, parameter limit, allowed model family, deadlines, and evaluation metric reviewed.
  - Initial model brainstorming started.

## 3. What I Know About the Assignment

- **Task:**
  - Two connected tasks:
    1. Face classification on known identities.
    2. Face verification using learned face embeddings for identities that may be unseen during training.
  - The same CNN feature extractor connects classification and verification.

- **Input:**
  - Face images with resolution **112 × 112**.
  - Classification dataset contains **8,631 identities**.
  - Verification uses image pairs.

- **Output:**
  - Classification: predicted identity.
  - Verification: similarity score between two face embeddings.
  - Final Kaggle submission combines both tasks.

- **Evaluation metric:**
  - Classification: accuracy.
  - Verification: Equal Error Rate (EER), where lower is better.
  - Combined:
    `0.5 × Accuracy + 0.5 × (1 - EER)`.

- **Required / prohibited methods:**
  - Maximum **30M parameters**, including ensembles.
  - CNN only.
  - CNN must be implemented from scratch using components such as `torch.nn`.
  - Must train from scratch.
  - No pretrained weights.
  - No directly imported ready-made CNN models such as `torchvision.models.resnet50`.
  - No RNN / GRU / LSTM.
  - No attention / Transformer.
  - No GNN.
  - No pretrained encoder / decoder.
  - If using ArcFace:
    - ArcFace parameters must be trained.
    - Model embeddings and ArcFace class embeddings must be normalized.
    - Do not combine ArcFace training with label smoothing, Mixup, or CutMix.

- **Minimum requirements:**
  - First priority is a complete end-to-end pipeline that can train, save/load a checkpoint, run classification and verification inference, generate the unified submission, and cross the **0.80 checkpoint cutoff**.

## 4. Current Brainstorming

- **Ideas currently being considered:**
  - Use the official starter CNN as the pipeline baseline.
  - Build a stronger custom residual CNN rather than only tuning the starter CNN.
  - Avoid overly aggressive early downsampling on 112 × 112 face images.
  - Start serious architecture exploration with a ResNet-style CNN implemented from scratch.
  - Use approximately **512-dimensional face embeddings** as an initial candidate.
  - Compare standard Cross-Entropy training with ArcFace while keeping the backbone fixed.
  - Explore identity-preserving augmentations such as horizontal flip and mild crop / photometric changes.
  - Consider verification-time TTA later if the core model is already strong.

- **Things not decided yet:**
  - Exact residual architecture:
    - ResNet18-like
    - ResNet34-like
    - SE-ResNet-like
    - other custom residual variants
  - Exact embedding dimension.
  - Exact channel widths / stage depths.
  - CE vs ArcFace training schedule.
  - Optimizer and learning-rate scheduler.
  - Exact augmentation pipeline.
  - ArcFace margin / scale hyperparameters.
  - Whether more advanced CNN families such as ConvNeXt-like designs are worth the added complexity.

- **Things I want to experiment with:**
  1. Official starter baseline.
  2. Custom residual CNN + CE.
  3. Same residual CNN + ArcFace.
  4. Identity-preserving augmentation changes.
  5. Embedding dimension / parameter-budget tradeoff.
  6. Verification inference improvements such as flip TTA after the core model is stable.

## 5. Current Problems / Unknowns

- **Concepts I don't understand yet:**
  - Need a focused knowledge check on:
    - residual blocks / ResNet design,
    - face embeddings,
    - why classification accuracy and verification quality can diverge,
    - ArcFace,
    - cosine similarity,
    - EER.
  - Need to decide which concepts must be learned before implementation versus learned while implementing.

- **Code/pipeline parts I don't understand:**
  - Need to walk through the starter notebook end-to-end.
  - Need to identify exactly where:
    - the feature embedding is produced,
    - the classification head is attached,
    - verification embeddings are extracted,
    - EER is computed,
    - checkpoints are selected,
    - the final unified Kaggle CSV is generated.

- **Infrastructure problems:**
  - Large dataset cannot be stored directly in GitHub; solved by keeping `hw2p2_data/` outside Git tracking.
  - Need to verify data paths and full training workflow on the chosen GPU environment.
  - Need to verify checkpoint storage and recovery before long runs.

- **Other uncertainties:**
  - High / Medium / Low Kaggle cutoffs have not been released yet.
  - Exact architecture and experiment budget have not been finalized.
  - Need to balance learning the CNN material with reaching the checkpoint quickly.

## 6. Estimated Work Remaining

- **Must do:**
  - Understand the starter notebook sufficiently to modify it safely.
  - Run a small end-to-end smoke test.
  - Train and validate the official baseline.
  - Verify checkpoint save/load.
  - Verify classification inference.
  - Verify verification inference and EER.
  - Generate a valid unified Kaggle submission.
  - Submit to Kaggle and cross the 0.80 checkpoint cutoff.
  - Complete the HW2 Canvas Quiz.
  - Build and evaluate at least one stronger custom CNN.
  - Keep experiment records.
  - Prepare final Kaggle and Gradescope submissions.

- **Nice to have:**
  - Strong custom residual CNN.
  - CE vs ArcFace controlled comparison.
  - Better augmentation pipeline.
  - Parameter-budget optimization.
  - Reliable validation-based model selection using both classification accuracy and verification EER.

- **Optional experiments:**
  - SE blocks.
  - ConvNeXt-like CNN blocks.
  - Alternative embedding dimensions.
  - Additional margin-based losses.
  - Test-time horizontal-flip augmentation.
  - Small inference / similarity refinements.

## 7. Knowledge Needed by Milestone

> The goal is **not** to finish all CNN material before starting HW2P2. Learn the concepts that are necessary for the next milestone, then apply them immediately.

### Milestone 0 — Understand and Run the Starter Pipeline

**Goal**
- Understand the starter notebook well enough to run a small end-to-end smoke test.
- Do not redesign the model yet.

**Knowledge needed before / during this milestone**
- Basic image tensor structure: `[batch, channels, height, width]`
- What a convolution layer does
- Kernel size, stride, padding
- How convolution changes spatial dimensions
- Channels / feature maps
- ReLU
- Batch normalization
- Pooling / adaptive average pooling
- Linear classification layer
- Forward pass
- Cross-entropy loss
- Basic training loop:
  - forward
  - loss
  - backward
  - optimizer step
- Train vs evaluation mode
- Checkpoint save / load

**Learning depth needed**
- Functional understanding is enough.
- I do not need to derive every convolution formula or fully understand advanced CNN architectures yet.

**Deliverable**
- Starter code can run.
- Small training / validation smoke test succeeds.
- Checkpoint can be saved and loaded.
- Classification and verification inference paths can execute.

### Milestone 1 — Produce the First Kaggle Baseline

**Goal**
- Train the official / minimally modified baseline.
- Generate a valid unified Kaggle submission.
- Reach the checkpoint requirement: Very Low cutoff = 0.80.

**Additional knowledge needed**
- Data loading and batching
- Image normalization
- Basic image augmentation
- Learning rate
- Batch size
- Epoch
- Overfitting vs underfitting
- Train loss vs validation loss
- Classification accuracy
- Face embedding:
  - what the embedding represents
  - where it is extracted from the CNN
- L2 normalization
- Cosine similarity
- Basic idea of verification
- EER at a conceptual level

**Important**
- I do **not** need to fully understand ArcFace or advanced residual architectures before getting the first Kaggle submission.

**Deliverable**
- Baseline validation results recorded.
- Classification accuracy recorded.
- Verification EER recorded.
- Unified submission successfully uploaded to Kaggle.
- Checkpoint cutoff achieved.

### Milestone 2 — Build a Stronger CNN Backbone

**Goal**
- Replace the simple starter CNN with a stronger custom residual CNN.

**Knowledge needed before / during this milestone**
- Why deeper CNNs can be difficult to optimize
- Vanishing / exploding gradients at a conceptual level
- Residual connections
- Residual blocks
- Identity shortcuts
- Projection shortcuts when dimensions change
- Spatial downsampling
- Channel expansion
- Receptive field
- Global average pooling
- Parameter counting
- Relationship between:
  - network depth
  - network width
  - computation
  - parameter count
- Why aggressive early downsampling may lose information

**Primary architecture topic**
- ResNet-style CNN

**Possible later topics**
- SE blocks, if confirmed compatible with course rules
- ConvNeXt-style CNN blocks

**Learning depth needed**
- Must understand the architecture well enough to implement it from scratch.
- Do not need to memorize canonical ResNet configurations.

**Deliverable**
- Custom residual CNN under the 30M parameter limit.
- Controlled comparison:
  - starter CNN + CE
  - residual CNN + CE
- Determine whether the backbone improves classification and/or verification.

### Milestone 3 — Improve the Face Embedding Objective

**Goal**
- Improve verification performance without confusing it with architecture changes.

**Knowledge needed**
- Difference between:
  - classification objective
  - representation learning objective
- Why high classification accuracy does not necessarily mean good verification
- Intra-class similarity
- Inter-class similarity
- Margin
- Normalized embeddings
- Cosine similarity in embedding space
- Standard Softmax Cross-Entropy geometry
- Margin-based Softmax losses
- ArcFace:
  - normalized feature vectors
  - normalized class weights
  - angular margin
  - scale parameter
  - ArcFace trainable parameters
- Piazza-specific ArcFace restrictions:
  - no label smoothing
  - no Mixup
  - no CutMix

**Useful but not immediately required**
- Triplet loss
- N-pair loss
- CosFace
- Circle Loss
- Supervised Contrastive Loss

**Learning depth needed**
- Understand ArcFace well enough to implement and debug it.
- Other metric-learning losses can initially remain conceptual.

**Deliverable**
- Same backbone compared under:
  - Cross-Entropy
  - ArcFace
- Classification Accuracy and Verification EER compared separately.
- Determine whether ArcFace improves embedding quality.

### Milestone 4 — Tune Augmentation and Generalization

**Goal**
- Improve robustness without changing everything at once.

**Knowledge needed**
- Data augmentation as regularization
- Identity-preserving transformations
- Horizontal flipping
- Random cropping / translation
- Brightness / contrast changes
- Mild rotation
- Why overly aggressive augmentation can destroy useful identity information
- Difference between:
  - geometric augmentation
  - photometric augmentation
  - label-mixing augmentation
- Mixup
- CutMix

**Important**
- Mixup / CutMix should be treated separately from the ArcFace training path because Piazza staff explicitly advises against using them with ArcFace.

**Deliverable**
- Controlled augmentation experiments.
- Determine which augmentations improve validation performance rather than simply increasing training difficulty.

### Milestone 5 — Verification-Specific Improvement

**Goal**
- Improve EER using a stable trained backbone.

**Knowledge needed**
- Embedding normalization
- Cosine similarity
- Euclidean distance at a conceptual level
- Similarity score distributions
- Positive vs negative pairs
- ROC curve
- False Acceptance Rate (FAR)
- False Rejection Rate (FRR)
- Equal Error Rate (EER)
- Verification threshold
- Test-time augmentation (TTA)

**Possible experiment**
- Original-image embedding
- Horizontally flipped-image embedding
- Average the embeddings
- Normalize
- Compute cosine similarity

**Deliverable**
- Verification pipeline validated independently of classification.
- Any TTA / inference improvement evaluated using EER.

### Milestone 6 — Final Model Selection and Submission

**Goal**
- Select the strongest reproducible model rather than the model with the best single metric.

**Knowledge needed**
- Model selection
- Validation leakage
- Reproducibility
- Checkpoint selection
- Tradeoff between classification accuracy and verification EER
- Combined Kaggle metric
- Basic experiment comparison

**Need to evaluate**
- Classification Accuracy
- Verification EER
- Combined validation score
- Parameter count
- Training cost
- Stability across runs

**Deliverable**
- Final checkpoint selected.
- Final Kaggle submission generated.
- Code cleaned for Gradescope.
- Experiment history preserved.

### Milestone 7 — Learning Priority for Time Planning

#### Learn first — needed immediately
1. Convolution basics
2. Tensor / feature-map shapes
3. Stride, padding, channels
4. BatchNorm + ReLU
5. Pooling
6. Cross-entropy
7. Basic PyTorch training loop
8. Face embedding concept
9. Cosine similarity
10. Basic EER concept

#### Learn after the first baseline works
1. Residual connections
2. ResNet architecture
3. Receptive field / downsampling strategy
4. Parameter counting
5. Better image augmentation

#### Learn after a strong CE backbone exists
1. Embedding geometry
2. ArcFace
3. Margin-based losses
4. Intra-class / inter-class separation

#### Learn only if needed later
1. Triplet Loss
2. N-Pair Loss
3. Circle Loss
4. Supervised Contrastive Loss
5. ConvNeXt details
6. More advanced inference / TTA

### Recommended Study / Implementation Order

```text
Learn basic CNN concepts
        ↓
Understand + run starter
        ↓
Get first Kaggle baseline
        ↓
Learn residual connections / ResNet
        ↓
Build stronger CE backbone
        ↓
Learn embedding geometry / ArcFace
        ↓
Improve verification objective
        ↓
Tune augmentation
        ↓
Verification-specific refinements / TTA
        ↓
Final model selection and submission
```
