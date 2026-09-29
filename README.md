# intro_dl_HW2P2

Repository for CMU 11-785 **HW2P2 (Fall 2026)** — face classification + verification.
This repo is the **single source of truth** for the project: any human or AI reviewer should get full project context here (plan, design, tasks, experiment records, review comments).

## Competition & deadlines

- Kaggle competition: https://www.kaggle.com/competitions/hw-2-p-2-fall-2026-student-competition/overview
- Metric: `0.5 × Classification Accuracy + 0.5 × (1 − EER)`; checkpoint cutoff **80%**; Kaggle daily limit **10**.
- Checkpoint deadline: **Oct 2, 11:59 PM EST** (miss ⇒ −3%); Final: **Oct 9, 11:59 PM EST**.

## Repository map

```text
intro_dl_HW2P2/
├── README.md                      # you are here: project overview + navigation map
├── PLAN.md                        # master plan: timeline, two tracks, risks, definition of done
├── DATA.md                        # dataset description: structure, splits, shapes, path conventions
├── HW2P2_Milestone_Check.md       # status snapshot & knowledge-by-milestone notes
├── docs/
│   ├── design/
│   │   ├── requirements.md        # task / rules / constraints / metric / deadlines (summary + links to official)
│   │   ├── model_hypotheses.md    # model hypothesis cards (8-line format per candidate)
│   │   └── decision_log.md        # decision log: date | topic | decision | rationale | impact
│   ├── implementation/
│   │   ├── critical_path.md       # critical path & buffers: checkpoint sprint → exploration window
│   │   ├── taskboard.md           # task status board: ID | status | blocked-by | deliverable | actual result
│   │   └── tasks/                 # one file per task: goal / steps / acceptance / current status
│   │       ├── T01_psc_env_verify.md
│   │       ├── T02_baseline_backbone.md
│   │       ├── T03_smoke_test.md
│   │       ├── T04_baseline_train.md
│   │       ├── T05_kaggle_checkpoint_submit.md
│   │       ├── T06_resnet_ce.md
│   │       ├── T07_arcface.md
│   │       ├── T08_augmentation.md
│   │       ├── T09_verification_tta.md
│   │       └── T10_final_selection_submission.md
│   └── handover/
│       └── handover_fromhw1_tohw2.md   # HW1P2 → HW2P2 lessons transfer (Chinese)
├── experiments/
│   ├── README.md                  # experiment record template
│   └── runs/                      # one file per run: YYYY-MM-DD_HHMM_run-name.md
├── review/                        # AI / peer review comments
│   ├── TEMPLATE.md                # review comment template
│   ├── claude/                    # Claude (Code) reviews, date-prefixed filenames
│   ├── codex/                     # Codex reviews
│   ├── gpt/                       # GPT reviews, date-prefixed filenames
│   └── misc/                      # other AIs / classmates
├── references/
│   ├── Piazza_HW2P2_Staff_Guidelines.md   # archived official staff clarifications
│   └── piazza_snapshots/          # manual snapshots of HW2P2-related staff posts (note fetch dates)
├── starter/                       # official course starter + writeups (single source, do not edit)
└── .gitignore                     # excludes data, checkpoints, submissions, wandb, tool state
```

## Navigation for reviewers

| Want to know… | Read |
| --- | --- |
| What the assignment requires and forbids | `docs/design/requirements.md` → `references/` |
| Overall timeline and phase goals | `PLAN.md` |
| What must happen in what order, with buffers | `docs/implementation/critical_path.md` |
| Current progress per task | `docs/implementation/taskboard.md` → `tasks/` |
| Why a design choice was made | `docs/design/decision_log.md`, `docs/design/model_hypotheses.md` |
| Experiment evidence | `experiments/runs/` |
| Prior review feedback | `review/<ai>/` |

## Repo scope (this repo is public)

This repository is a **public** repo and holds **only** planning, records, and materials that are safe to publish: `PLAN.md`, `docs/`, `experiments/runs/`, `review/`, `references/`, `starter/`.

- **Working implementation is not published here.** Keep working notebook/code in a **private** repo or purely local Git; the assignment implementation should not be made public just because API keys were removed — a key-stripped notebook is still your graded solution.
- **Credentials** (Kaggle API, wandb key) come from environment variables or a local **untracked** config; never paste them into a notebook copy that might be committed or shared.
- Do not assume that deleting a key makes a notebook safe to push; the starter's submission/credential cells are protected and must stay unmodified.

## Data

The official HW2P2 dataset is intentionally **not tracked by Git** (too large). See `DATA.md` for the expected local layout and path conventions. Keep `hw2p2_data/` locally, on PSC `$LOCAL`, or in the Kaggle environment — never commit it.

## Red lines (never commit)

- API keys / tokens (wandb key, Kaggle API token) and any credential in notebook copies.
- `hw2p2_data/`, the raw competition zip, model checkpoints (`*.pt` / `*.pth` / `*.ckpt`), generated submissions, `wandb/`.
- Working notebook/implementation that should stay private; this repo is **public** — keep private notes and graded code out of it.

## Project workflow

- Official starter materials stay in `starter/` untouched; working notebook/code live in a private/local Git (not this public repo).
- Planning lives in `PLAN.md`; per-task detail in `docs/implementation/tasks/`.
- Record every meaningful experiment under `experiments/runs/`.
- Review comments (AI or human) go under `review/`, filenames prefixed with the date.
- Track planning/config changes with Git; never overwrite the checkpoint dir tied to a committed submission.
