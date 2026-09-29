# T10 — Final Selection, Kaggle Final & Gradescope

- **Status**: pending | **Owner**: Cathy | **Track**: both | **Due**: Oct 8 selection; **Oct 9 final submit (daytime buffer)**; Oct 10–11 Gradescope | **Blocked-by**: the selected final model — T06–T09 if explored, otherwise the verified baseline (T04) if exploration is skipped

## Steps

1. **Select** the final model by val **combined** score (not best single metric); check stability, param count, training cost.
2. Final Kaggle submission: load frozen selected checkpoint explicitly → generate `submission.csv` → submit; keep ≥3 slots for the day.
3. Freeze final checkpoint dir; verify leaderboard selected score.
4. **Gradescope** (following weekend): follow notebook steps 1–7 — assign `MODEL`, fill `README` var, set credential env (not committed!), `NOTEBOOK_PATH`, additional files, generate zip, upload.
5. Preserve experiment history: no overwriting; final notebook path recorded.

## Acceptance

- [ ] Selection rationale in decision log (combined score table across candidates)
- [ ] Final model chosen by **val combined score**, not by having to beat the checkpoint version; the checkpoint is a best-effort milestone, not a bar the final must clear
- [ ] Final `MODEL` in the Gradescope notebook matches the selected Kaggle submission model (no train-time-only variant left behind)
- [ ] Gradescope zip generated; auto-grading result checked; submission timestamped
- [ ] `experiments/runs/` complete for every promoted run

## Notes / current status

- (fill as you run)
