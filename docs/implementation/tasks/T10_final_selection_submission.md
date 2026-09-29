# T10 — Final Selection, Kaggle Final & Gradescope

> **Cathy update — 2026-09-29 15:11 America/New_York:** Canvas HW2 quiz is completed (user-confirmed; score not independently checked). The Oct 2 early cutoff is a best-effort opportunity, not an internal completion gate. Preserve full setup, pipeline completion, correctness checks, resume validation, and learning steps even if 0.80 is not reached by Oct 2. Official deadlines and grading consequences remain unchanged. This decision supersedes earlier checkpoint-sprint/noon-target instructions; see `review/gpt6.1sol/2026-09-29_planning_review.md`.

- **Status**: pending | **Owner**: Cathy | **Track**: both | **Due**: Oct 8 selection; **Oct 9 final submit (daytime buffer)**; Oct 10–11 Gradescope | **Blocked-by**: at least one validated reproducible candidate; optional experiments completed or explicitly stopped

## Steps

1. **Select** the final model by val **combined** score (not best single metric); check stability, param count, training cost.
2. Final Kaggle submission: load frozen selected checkpoint explicitly → generate `submission.csv` → submit; keep ≥3 slots for the day.
3. Freeze final checkpoint dir; verify leaderboard selected score.
4. **Gradescope** (following weekend): follow notebook steps 1–7 — assign `MODEL`, fill `README` var, set credential env (not committed!), `NOTEBOOK_PATH`, additional files, generate zip, upload.
5. Preserve experiment history: no overwriting; final notebook path recorded.

## Acceptance

- [ ] Selection rationale in decision log (combined score table across candidates)
- [ ] Final selected model is the best validated reproducible candidate on val combined; if an early checkpoint submission exists, final should match or beat it (else explain in decision log). If no early submission was made (T05 skipped), do **not** compare against a nonexistent checkpoint score — validate format/inference correctness with an explicit-purpose submission slot if needed
- [ ] Gradescope zip generated; auto-grading result checked; submission timestamped
- [ ] `experiments/runs/` complete for every promoted run

## Notes / current status

- (fill as you run)
