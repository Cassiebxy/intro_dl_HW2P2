# Piazza HW2P2 Staff Guidelines

## HW2P2 Description & FAQs

### HW2 – Part 2 (HW2P2) is Live!

HW2P2 is an **active Kaggle competition**. Please read the instructions below carefully before getting started.

### Kaggle Competition Link

👉 **Join using this exact link only:**  
https://www.kaggle.com/t/d025d75eb26d436da206a8e7aaa38235

> **Important:** You *must* join using this exact link (click or copy-paste). Other links will not work.

### Important Links

- HW Canvas Quiz
- All HW Canvas Materials
- Gradescope Submission

### Starter Materials

**For Those Using PyTorch:**
- Notebook
- Writeup

**For Those Using JAX:**
- Notebook
- Writeup

---

## Deadlines

### ✅ Checkpoint Deadline (Early Submission)

📅 **October 2, 2026 — 11:59 PM EST**

- **3% deduction** (your final score will be scaled by **0.97**) if you fail to make an early submission
- Requirements:
  - Make Kaggle submission
  - Achieve the **very low cutoff** (higher is better)
  - Complete the **HW 2 Canvas Quiz**
- Your **name must appear on the Kaggle leaderboard**

After this deadline, we will release **tentative performance cutoffs** for High / Medium / Low / Very Low.

### 📌 Evaluation Metric & Cutoffs

- **Evaluation Metric:**  
  `Final Kaggle Score = 0.5 × Classification Accuracy + 0.5 × (1 - Verification EER)`
- **Very Low Cutoff:** **0.80**
- **Low / Medium / High Cutoffs:** **TBD** (will be released after the checkpoint deadline).

### 🏁 On-Time Submission Deadline

📅 **October 9, 2026 — 11:59 PM EST**

- Kaggle submissions close and any submission after this deadline must be made to the **Slack Kaggle Competition (TBD)**
- You will then have the **following weekend (2 additional days)** to clean up and submit:
  - Code
  - Related files
- Final **Gradescope submission**

---

## ⚙️ Additional Guidelines & Tips

- **Parameter limit:** 30M (including ensembles)
- **Model constraints:** @302
- **Kaggle daily submission limit:** 10
- Read the **write-up carefully**
- Review **all Kaggle tabs** (Data, Rules, Discussion, etc.)
- **Tips for Arcface:** @303

---

## 🎓 Bootcamp

- **HW2 Bootcamp: Sept 19 2-5 pm EST in Rashid Auditorium, GHC 4401**
- Will be **recorded**, later available on **YouTube and MediaTech**

---

## 📢 Updates & Clarifications

- **TBD**

---

## ❓ Questions & Discussion

- All HW2P2 questions **must be asked on this designated Piazza thread**
- Please follow **Piazza etiquette (@25)**

---

## [Very Important] HW2P2 Model Implementation

*Endorsed by Instructor (Bradley Warren)*

For this homework, you are only allowed to use **CNN models**. Please **do not use** RNNs (GRUs/LSTMs), attention or Transformer modules, graph neural networks, or any pretrained encoder/decoder models.

You **must implement your own CNN from scratch**, meaning the model should be built directly using components from `torch.nn` (or similar), **without pretrained weights**, and **trained from scratch**. Directly using already-implemented CNN architectures (e.g., `torchvision.models.resnet50` or other models from `torchvision.models`) is **not allowed**.

---

## [HW2P2] Tips for Using Arcface

*Endorsed by Instructor (Ahmed Issah)*

A few tips for using Arcface as they seem to be common pitfalls:

1. Arcface contains trainable parameters, it keeps an embedding vector for each class.
2. Normalize the output embedding of your model and the embeddings of Arcface to unit vectors.
3. Update the embeddings of Arcface as well.
4. Don't label-smoothing/cutmix/mixup when training with Arcface.
