# T01 — PSC Environment Verification

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 29
- **Hypothesis tested**: none — pure engineering verification.

## Steps

1. SSH into Bridges2; request a compute node (`交互式` per course PSC guide @254/@299).
2. Load Anaconda module; activate the **course-provided shared conda env** (do not build your own first).
3. Launch Jupyter; run the starter notebook's PSC setup cells.
4. Confirm data path: check `/local/hw2p2_data` exists on compute node, or copy/point to local `hw2p2_data/`.
5. Set `config['data_root']`, `config['checkpoint_dir']` (writable, persistent across node restarts).
6. Configure wandb key + Kaggle API key (never commit them).
7. Run `!nvidia-smi` → V100 visible; run one batch through a DataLoader.

## Acceptance

- [ ] `nvidia-smi` shows V100 inside the notebook kernel
- [ ] cls + ver DataLoaders yield a batch with expected shapes
- [ ] checkpoint dir writable and survives node switch (path documented)
- [ ] wandb + Kaggle auth working, keys NOT in any committed file

## Notes / current status

- Allocation confirmed 2026-09-29.
- (fill as you run)
