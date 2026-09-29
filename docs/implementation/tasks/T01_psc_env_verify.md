# T01 — PSC Environment & Storage Verification

- **Status**: pending | **Owner**: Cathy | **Track**: stable | **Due**: Sep 29–30
- **Hypothesis tested**: none — pure engineering verification.
- **Scope**: environment, storage, raw-data staging, credentials **only**. Dataset/loader code belongs to T02; loader iteration is verified in T02 (local) and T03 (GPU chain). T01 and T02 may proceed **in parallel**; both must be complete before T03.

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
4. Credentials **without writing them anywhere tracked**: prefer environment variables only. If a local untracked config is used instead (e.g. `secrets.json` / `.env`), confirm it matches the `.gitignore` credential patterns (added 2026-09-29) before first use. If a starter cell demands keys inline, pass values at runtime only — never commit a notebook copy containing them.
5. `!nvidia-smi` → V100 visible in the notebook kernel.
6. Spot-check raw-data integrity against `DATA.md`: `cls_data/train/images` ≈ 431,550 files, `labels.txt` covers 8,631 classes, dev/test images and both pair files present. (Dataset/loader iteration is **not** a T01 requirement — that code is implemented in T02 and exercised in T03.)
7. Write a dummy checkpoint into `checkpoint_dir` from the kernel; restart the kernel; reload it and verify contents. Record the **actual persistent path** and which platform guarantee you rely on — a kernel restart alone does **not** prove cross-node persistence; note the node-change/re-allocation case as an unverified assumption to re-check when a new node is actually assigned.
8. Understand allocation reality: interactive/background jobs are bounded by PSC allocation limits. A background training run does **not** survive allocation expiry or node loss — plan checkpoints + resume (verified in T03), don't assume continuity.

## Acceptance

- [ ] `nvidia-smi` shows V100 inside the notebook kernel
- [ ] Raw data staged on `$LOCAL` and spot-checked against `DATA.md` counts (no loader iteration required here)
- [ ] `checkpoint_dir` writable; checkpoint readable **after a kernel restart**; actual persistent path documented together with the persistence guarantee relied on (cross-node check deferred to first real node change)
- [ ] Data re-download-to-`$LOCAL` procedure confirmed on (re)start of a session
- [ ] Kaggle + wandb auth work with keys sourced from env (preferred) or git-ignored untracked config; `git grep` for key patterns in the repo returns nothing
- [ ] Notes recorded: allocation limit, expected node behavior, resume strategy (feeds T03)

## Notes / current status

- Allocation confirmed 2026-09-29.
- Starter `$LOCAL` guidance verified in starter notebook cells 25/39/41/42 (2026-09-29).
- (fill as you run)
