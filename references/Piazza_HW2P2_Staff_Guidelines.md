# HW2P2 Piazza Staff Guidelines & Clarifications

> Source: CMU 11-785 HW2P2 Piazza posts/screenshots shared on September 23, 2026.
>
> Use this file as a quick reference only. If Piazza is updated later, the newest instructor/TA clarification should take precedence.

## Competition and starter materials

- HW2P2 is an active Kaggle competition.
- Join the Kaggle competition using the exact invitation link:
  - https://www.kaggle.com/t/d025d75eb26d436da206a8e7aaa38235
- Piazza explicitly warns that the exact invitation link must be used; other links may not work.
- Starter materials are provided for both:
  - PyTorch: Notebook + Writeup
  - JAX: Notebook + Writeup
- Also review all Kaggle tabs, including Data, Rules, and Discussion.

## Core model constraints

- **Parameter limit: 30M total parameters, including ensembles.**
- Only **CNN models** are allowed for HW2P2.
- The CNN must be implemented **from scratch** using low-level framework components such as `torch.nn` (or similar).
- The model must be trained **from scratch**.
- **Pretrained weights are not allowed.**
- Direct use of already-implemented CNN architectures such as `torchvision.models.resnet50` or other `torchvision.models` models is **not allowed**.
- The following are not allowed:
  - RNNs, including GRUs/LSTMs
  - attention modules
  - Transformers
  - graph neural networks
  - pretrained encoder/decoder models
- Piazza model-implementation clarification: **@302**.

## Kaggle limits and evaluation

- Kaggle daily submission limit: **10 submissions/day**.
- Final Kaggle score:

```text
Final Kaggle Score
= 0.5 × Classification Accuracy
+ 0.5 × (1 - Verification EER)
```

- **Very Low cutoff: 0.80**
- Low / Medium / High cutoffs: **TBD**, to be released after the checkpoint deadline.

## Checkpoint / early submission

**Deadline:** October 2, 2026 — 11:59 PM EST

Failing to make the required early submission causes a **3% deduction**: the final score is scaled by **0.97**.

Checkpoint requirements:

1. Make a Kaggle submission.
2. Achieve the **Very Low cutoff** or higher.
3. Complete the **HW2 Canvas Quiz**.
4. Ensure your **name appears on the Kaggle leaderboard**.

After the checkpoint deadline, tentative High / Medium / Low / Very Low performance cutoffs will be released.

## On-time submission

**Deadline:** October 9, 2026 — 11:59 PM EST

- The regular Kaggle competition closes for on-time submissions at this deadline.
- Any submission after this deadline must be made to the Slack Kaggle Competition once it becomes available.
- After the Kaggle deadline, students have the following weekend (2 additional days) to clean up and submit:
  - code
  - related files
- A final Gradescope submission is also required.

## ArcFace staff tips

Piazza ArcFace guidance: **@303**.

1. **ArcFace has trainable parameters.**
   - It maintains an embedding / class-weight vector for each class.
2. **Normalize both sides.**
   - Normalize the model's output embedding to a unit vector.
   - Normalize ArcFace's class embeddings / weights to unit vectors.
3. **Update the ArcFace embeddings during training.**
   - They are trainable parameters and must be included in optimization.
4. **Do not use label smoothing, CutMix, or Mixup when training with ArcFace.**

## Bootcamp

- HW2 Bootcamp: **September 19, 2026, 2:00–5:00 PM EST**
- Location: **Rashid Auditorium, GHC 4401**
- The session is recorded and will later be available on **YouTube** and **MediaTech**.

## Questions and future clarifications

- All HW2P2 questions should be asked on the designated Piazza thread.
- Follow Piazza etiquette guidance (**@25**).
- The Piazza “Updates & Clarifications” section was marked **TBD** in the provided screenshot.
- Always re-check Piazza before major implementation decisions or full training runs in case new staff clarifications are posted.

## Practical pre-run checklist

Before launching a major experiment, confirm:

- [ ] Total parameters are under 30M, including any ensemble components.
- [ ] Architecture is CNN-only and implemented from scratch.
- [ ] No pretrained weights or ready-made model implementations are used.
- [ ] The experiment is compatible with the current loss function.
- [ ] If using ArcFace, embeddings/weights are normalized and ArcFace parameters are optimized.
- [ ] If using ArcFace, label smoothing / Mixup / CutMix are disabled.
- [ ] The experiment plan accounts for the 10-submission/day Kaggle limit.
- [ ] The latest Piazza staff clarifications have been checked.
