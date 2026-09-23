# intro_dl_HW2P2

Repository for CMU 11-785 HW2P2.

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

The official HW2P2 dataset is intentionally **not tracked by Git**.

Expected local layout:

```text
hw2p2_data/
├── cls_data/
│   ├── train/
│   ├── dev/
│   └── test/
└── ver_data/
```

Keep `hw2p2_data/` locally (or on PSC/Kaggle) and do not commit it to this repository.

## Large generated files

Model checkpoints, W&B artifacts, notebook checkpoints, and generated submissions are ignored by default.
