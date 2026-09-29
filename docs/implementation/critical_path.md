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

## Baseline track (stable) — Oct 2 checkpoint = best-effort milestone

```text
Sep 29        T01 PSC env + storage verify ──► T02 pipeline completion + baseline ──► T03 smoke test
              (must finish today; ⚠️ PSC queue wait: use the wait for T02/T03 code work, not background runs)
Sep 30        T04 baseline training (≈20 epochs reference budget, measured per-epoch)
              ⚠️ if per-epoch × 20 > remaining time → submit best available as best-effort;
                 do NOT compress pipeline or truncate learning to hit Oct 2
Oct 1         T04 finish + best-checkpoint selection; dry-run submission.csv locally
Oct 2 morning T05 Kaggle submission (best-effort, target by NOON)
              ⚠️ buffer: leaderboard queue, upload failures, daily-limit count
```

**Pipeline hard gate**: T03 fully green before T04 long training. No exceptions. Oct 2 itself is best-effort, not a hard gate.

## Calendar anchors

- **Oct 2:** optional early-cutoff opportunity. Try only when ready; no mandatory noon target and no requirement to stop necessary work to submit.
- **Oct 9:** official on-time Kaggle deadline; preserve a submission buffer.
- **Oct 11:** official on-time Gradescope code-package deadline per starter/writeup; preflight packaging earlier.
- Sources label deadlines EST; confirm course-platform countdown before execution. Slack dates differ across materials and are not the planning baseline.

```text
(reference schedule — enter after verified baseline T04 exists; Oct 2 checkpoint runs in parallel as best-effort)
Oct 2 evening  T06 start ResNet CE screen (subset, 2–3 epochs, steps+wall-clock recorded)
Oct 3–4        T06 full-data confirmation vs H1 baseline
               T07 ArcFace screen (same backbone, short runs)
Oct 5–6        T07 ArcFace full comparison; T08 augmentation (parallel, one change at a time)
Oct 7          T06/T07/T08 results review; pick candidate
Oct 7–8        T09 verification TTA evaluation (EER only)
Oct 8          T10 final selection; Oct 9 daytime = submission buffer
              ⚠️ Oct 9: final Kaggle deadline; only T10 submission allowed
Oct 10–11      Gradescope zip (notebook steps 1–7), cleanup, submit
```

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
