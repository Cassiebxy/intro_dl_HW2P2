# Critical Path & Buffers

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/codex/2026-09-29_planning_review.md`.

## Engineering dependencies

```text
Environment and persistent storage readiness
  + complete starter TODOs (not only backbone)
→ limited-step, full-label-space smoke test
→ save / fresh-session reload / resume validation
→ measured full-data baseline training
→ frozen checkpoint and verified inference/submission path
→ readiness-based model exploration
→ final model freeze
→ final Kaggle and Gradescope submission
```

No long training before the engineering smoke/resume checks pass. Do not omit these checks to meet the early cutoff.

## Calendar anchors

- **Oct 2:** optional early-cutoff opportunity. Try only when ready; no mandatory noon target and no requirement to stop necessary work to submit.
- **Oct 9:** official on-time Kaggle deadline; preserve a submission buffer.
- **Oct 11:** official on-time Gradescope code-package deadline per starter/writeup; preflight packaging earlier.
- Sources label deadlines EST; confirm course-platform countdown before execution. Slack dates differ across materials and are not the planning baseline.

## Readiness and budget

- Canvas HW2 quiz is already completed, per Cathy's Sep 29 confirmation; it does not depend on model training.
- Reaching early 0.80 is not a prerequisite for residual-model exploration. A verified baseline and sufficient understanding are.
- Estimate full-data epoch time including validation, plus data staging, checkpointing, queue and inference costs.
- Do not turn a short subset epoch into a full-data timing estimate without accounting for the difference.
- Reserve time for final training, inference and packaging; make later experiments optional when budgets are exhausted.
- Freeze a valid candidate even if ArcFace, extra augmentation or TTA are unfinished or unhelpful.
- Preserve committed checkpoint directories and record code/config/run identity.
- PSC fallback must be based on actual platform availability; background execution does not extend a GPU allocation.

## Pending technical revision

Use the GPT and Codex reviews to revise individual tasks. The previous date-by-date checkpoint sprint and arbitrary 50% Phase-A GPU allocation are superseded by the readiness-based schedule above. No replacement epoch-time or GPU-hour estimate is claimed until measured.
