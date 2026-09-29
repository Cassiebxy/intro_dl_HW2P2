# Critical Path & Buffers

> Times are wall-clock targets, EST. The path is "as-early-as-possible"; every ⚠️ is a known place the schedule breaks.

## Baseline track (stable) — Oct 2 checkpoint = best-effort milestone

```text
Sep 29        T01 PSC env + storage verify ──► T02 pipeline completion + baseline ──► T03 smoke test
              (must finish today; ⚠️ PSC queue wait: use the wait for T02/T03 code work, not background runs)
Sep 30        T04 baseline training (≈20 epochs reference budget, measured per-epoch)
              ⚠️ if per-epoch × 20 > remaining time → submit best available as best-effort;
                 do NOT compress pipeline or truncate learning to hit Oct 2
Oct 1         T04 finish + best-checkpoint selection; dry-run submission.csv locally
Oct 2 morning T05 Kaggle submission (best-effort, target by NOON) + Canvas quiz
              ⚠️ buffer: leaderboard queue, upload failures, daily-limit count
```

**Pipeline hard gate**: T03 fully green before T04 long training. No exceptions. Oct 2 itself is best-effort, not a hard gate.

## Exploration track — deadline Oct 9 11:59 PM EST

```text
(enter after verified baseline T04 exists; Oct 2 checkpoint runs in parallel as best-effort)
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

## Budget rules

- **Kaggle slots (10/day)**: slot only spent when val combined > current committed best. Reserve ≥2 slots on Oct 2 and ≥3 on Oct 9.
- **GPU**: Phase A budget ≤ 50% of available PSC hours through Oct 2; exploration uses the rest.
- **Never overwrite** the checkpoint dir tied to a committed submission.
- **Stop rules**: any task blocked >4h without progress → escalate to staff Piazza question or fallback in `PLAN.md` risk table.

## Timeline sketch

```text
Sep29 ──┬─ T01 ──► T02 ──► T03 ─┐
        │         (env+code, no GPU)
Sep30 ──┴─ T04 training ────► Oct1 checkpoint pick ──► Oct2 AM T05 SUBMIT ✅
Oct2 PM ───► T06 screen ──► Oct3-4 T06 full ──► Oct4-6 T07 ──► Oct5-7 T08 ∥
Oct7 ──► review ──► Oct7-8 T09 ──► Oct8 T10 pick ──► Oct9 T10 SUBMIT ✅ ──► Oct10-11 Gradescope
```
