# Experiments

Use this directory for experiment logs and comparisons. `runs/` holds **one file per run**.

## Run file naming

```text
runs/YYYY-MM-DD_HHMM_<short-run-name>.md
```

## Run record template (minimum)

```markdown
# <run name>
- Date / duration (wall-clock):
- Hypothesis / single main change:
- Code/notebook version + config (commit hash or file copy): 
- Seed:
- Split / subset used: epochs, batch size, **steps seen**, samples seen
- Metrics: train loss/acc, best val cls acc, best val EER, **best val combined** + epoch
- Checkpoint path (frozen?):
- Kaggle submission? (score, timestamp, slot used):
- Device / GPU / memory notes:
- Conclusion: keep / reject / investigate + next step
```

## Rules

- The checkpoint directory tied to a committed Kaggle submission is never overwritten; reproductions get new run names.
- No checkpoints, weights, or large artifacts committed here — text notes only.
- Distinguish evidence levels: smoke / screen / full-validation / Kaggle. Never label a screen as final evidence.
