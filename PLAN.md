# HW2P2 Plan

## Current setup

- Keep course starter / handin code under `starter/`.
- Keep the official dataset outside Git tracking under local `hw2p2_data/`.
- Keep experiment notes under `experiments/`.
- Do not commit model checkpoints or other large generated artifacts.

## Immediate workflow

1. Confirm HW2P2 assignment rules, metric, deadlines, and starter-code constraints.
2. Run a minimal end-to-end smoke test:
   - load a small amount of data
   - forward / backward pass
   - save checkpoint
   - reload checkpoint
   - inference
   - generate a valid submission
3. Establish a reproducible baseline.
4. Record each meaningful experiment in `experiments/`.
5. Keep one stable submission path while separately testing new model hypotheses.

## Experiment record

For each run, record at minimum:

- hypothesis / main change
- code or notebook version
- full config and seed
- train/dev split
- epochs, batch size, and training steps
- best validation metric
- checkpoint used
- runtime / GPU constraints
- Kaggle result if submitted
- conclusion: keep, reject, or investigate further
