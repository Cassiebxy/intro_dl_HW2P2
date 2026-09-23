# intro_dl_HW2P2

Repository for CMU 11-785 **HW2P2 (Fall 2026)**.

## Competition

Kaggle competition page:

https://www.kaggle.com/competitions/hw-2-p-2-fall-2026-student-competition/overview

The course-provided **handin / starter package** is available from the Kaggle competition page above.  
This repository tracks the code, planning notes, and experiment records used for the project, but does not store the full official dataset.

## Repository structure

```text
intro_dl_HW2P2/
├── starter/        # course-provided handin / starter code
├── PLAN.md         # current project plan
├── README.md
├── experiments/    # experiment notes and logs
└── .gitignore
```

## Data

The official HW2P2 dataset is intentionally **not tracked by Git** because it contains a large number of image files.

Expected local layout:

```text
hw2p2_data/
├── cls_data/
│   ├── train/
│   │   ├── images/
│   │   └── labels.txt
│   ├── dev/
│   │   ├── images/
│   │   └── labels.txt
│   ├── test/
│   │   └── images/
│   ├── test_pairs.txt
│   └── val_pairs.txt
└── ver_data/
    └── *.jpg
```

Keep `hw2p2_data/` locally, on PSC, or in the Kaggle environment. Do not commit it to this repository.

## Project workflow

- Keep the original course starter / handin code under `starter/`.
- Keep planning and model ideas in `PLAN.md`.
- Record meaningful experiments under `experiments/`.
- Keep a reproducible baseline and submission pipeline.
- Track code and configuration changes with Git.
- Do not commit large datasets, model checkpoints, or generated artifacts.

## Large generated files

The following are ignored by default through `.gitignore`:

- `hw2p2_data/`
- model checkpoints such as `*.pt`, `*.pth`, and `*.ckpt`
- `wandb/`
- generated `submissions/`
- Jupyter notebook checkpoints
- Python cache files
