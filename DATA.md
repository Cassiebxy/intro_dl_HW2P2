# HW2P2 Dataset Notes

> Verified locally on 2026-09-29 against `hw2p2_data/` (3.9 GB extracted; raw zip ~2.7 GB, re-downloadable from Kaggle).
> The dataset is **never committed to Git** (see `.gitignore`).

## Layout

```text
hw2p2_data/
├── cls_data/                     # classification task
│   ├── train/
│   │   ├── images/               # 431,550 images (train_XXXXXX.jpg)
│   │   └── labels.txt            # "<image_file> <class_id>" per line, 8,631 classes
│   ├── dev/
│   │   ├── images/               # 43,097 images (dev_XXXXXX.jpg)
│   │   └── labels.txt            # same format
│   └── test/
│       └── images/               # 43,097 images, NO labels (Kaggle test set)
├── ver_data/                     # verification task images (12,000 .jpg, opaque names)
├── val_pairs.txt                 # 1,000 lines: "<imgA> <imgB> <0|1>" (same/different identity)
└── test_pairs.txt                # 5,000 lines, same format (Kaggle test set)
```

## Facts

- Images: RGB, **112 × 112** (per assignment).
- Classification: **8,631 identities** in train labels; ~50 images/identity on average.
- Verification: pairs over `ver_data/` images; label `1` = same identity, `0` = different.
- The classification test set and verification test pairs have no labels — Kaggle leaderboard is the only feedback there.
- Verification may involve identities unseen during training: learn an embedding, not just a classifier.

## Path conventions (two zones — do not mix)

- **Local machine**: `hw2p2_data/` next to the repo; point `config['data_root']` here.
- **PSC node-local staging**: dataset **must** live on `$LOCAL/hw2p2_data` (per starter cells 25/39/41) to avoid shared-FS I/O bottlenecks. `$LOCAL` is **temporary** — wiped on node change / allocation end, so re-download the dataset into `$LOCAL` at the start of each new-node session. `config['data_root']` = `$LOCAL/hw2p2_data`.
- **PSC persistent**: checkpoints, code, notebooks, logs live on the persistent home path (e.g. `/jet/home/<user>/hw2p2/...`), **never** on `$LOCAL`. `config['checkpoint_dir']` points here — must be writable, survive a kernel restart, and **never be reused by a new run** once its score is committed.
- The raw zip `hw-2-p-2-fall-2026-student-competition.zip` is redundant once extracted; both stay out of Git.

## Notes

- The classification label space is fixed at **8,631 classes** for train, dev, and test. `config['num_classes']` must stay 8,631 so the classifier can score the full dev/test labels — shrink a smoke test by limiting **steps/batches/samples**, not by reducing the label space (see T03).
- Pair files are consumed by the starter's `ImagePairDataset`; EER is computed by the starter's `valid_epoch_ver`.
