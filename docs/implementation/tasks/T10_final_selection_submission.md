# T10 — Final Selection, Kaggle Final & Gradescope

- **Status**: pending | **Owner**: Cathy | **Track**: both | **Due**: Oct 8 selection; **Oct 9 final submit (daytime buffer)**; Oct 10–11 Gradescope | **Blocked-by**: T06–T09

## Steps

1. **Select** the final model by val **combined** score (not best single metric); check stability, param count, training cost.
2. Final Kaggle submission: load frozen selected checkpoint explicitly → generate `submission.csv` → submit; keep ≥3 slots for the day.
3. Freeze final checkpoint dir; verify leaderboard selected score.
4. **Gradescope** (following weekend): follow notebook steps 1–7 — assign `MODEL`, fill `README` var, set credential env (not committed!), `NOTEBOOK_PATH`, additional files, generate zip, upload.
5. Preserve experiment history: no overwriting; final notebook path recorded.

## Acceptance

- [ ] Selection rationale in decision log (combined score table across candidates)
- [ ] Kaggle final selected score ≥ checkpoint score (else explain)
- [ ] Gradescope zip generated; auto-grading result checked; submission timestamped
- [ ] `experiments/runs/` complete for every promoted run

## Notes / current status

- (fill as you run)
