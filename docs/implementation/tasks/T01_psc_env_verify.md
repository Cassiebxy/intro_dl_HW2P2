# T01 — PSC Environment & Storage Verification

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 29–30
- **Hypothesis tested**: none — pure engineering verification.

## Two storage zones (do not mix them up)

| Zone | Path | Lifetime | Holds |
| --- | --- | --- | --- |
| Node-local staging | `$LOCAL/hw2p2_data` (PSC: `/local/...`) | **Temporary** — wiped on node change / allocation end | Dataset only |
| Persistent home | e.g. `/jet/home/<user>/hw2p2/...` | Survives nodes & reboots | Code, notebooks, **checkpoints**, logs, any re-download cache |

Per starter (cells 25/39/41): data **must** live on `$LOCAL` to avoid shared-filesystem I/O bottlenecks (hours/epoch otherwise). Consequences to accept, not fight:

- Every new node ⇒ re-run the data download cell into `$LOCAL` (add it as a first cell in every session).
- `config['data_root']` points at `$LOCAL/hw2p2_data`; `config['checkpoint_dir']` points at the persistent home path — never at `$LOCAL`.

## Steps

1. SSH Bridges2 → request a compute node → load the course shared conda env → launch Jupyter on the GPU node.
2. Run the starter's `$LOCAL` data download cell; confirm `cls_data/` + `ver_data/` + pair files exist under `$LOCAL/hw2p2_data`.
3. Set `config['data_root']` = `$LOCAL/hw2p2_data`; `config['checkpoint_dir']` = persistent path (create it).
4. Credentials **without writing them anywhere tracked**: Kaggle API + wandb key from environment variables or a local untracked config (e.g. `secrets.json` / `.env`, git-ignored). If a starter cell demands keys inline, pass values at runtime only — never commit a notebook copy containing them.
5. `!nvidia-smi` → V100 visible in the notebook kernel.
6. Build the **full** cls train/val datasets + loaders and the ver val loader (not a subset) — iterate one batch, check shapes/labels.
7. Write a dummy checkpoint into `checkpoint_dir` from the kernel; restart the kernel; reload it and verify contents.
8. Understand allocation reality: interactive/background jobs are bounded by PSC allocation limits. A background training run does **not** survive allocation expiry or node loss — plan checkpoints + resume (verified in T03), don't assume continuity.

## Acceptance

- [ ] `nvidia-smi` shows V100 inside the notebook kernel
- [ ] Full cls train/val + ver val DataLoaders yield batches with expected shapes and label ranges
- [ ] `checkpoint_dir` writable; checkpoint readable **after a kernel restart** (path documented in this file)
- [ ] Data re-download-to-`$LOCAL` procedure confirmed on (re)start of a session
- [ ] Kaggle + wandb auth work with keys sourced from env/untracked config; `git grep` for key patterns in the repo returns nothing
- [ ] Notes recorded: allocation limit, expected node behavior, resume strategy (feeds T03)

## Notes / current status

- Allocation confirmed 2026-09-29.
- Starter `$LOCAL` guidance verified in starter notebook cells 25/39/41/42 (2026-09-29).
- (fill as you run)
