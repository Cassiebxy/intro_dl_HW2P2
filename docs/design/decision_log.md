# Decision Log

> Append-only. One entry per meaningful decision: what was decided, why, and what it affects. Reversals get a new entry referencing the old one.

| Date | Topic | Decision | Rationale | Impact |
| --- | --- | --- | --- | --- |
| 2026-09-29 | Repo restructure | Restructure repo into `docs/{design,implementation,handover}`, `experiments/runs`, `review/{claude,codex,misc}`, `references/piazza_snapshots`, `DATA.md`; master plan into `PLAN.md` | Make the repo the single source of truth for all AI reviewers; separate design / implementation / evidence / review | All future docs follow this layout; `starter/` stays untouched |
| 2026-09-29 | Duplicate cleanup | Deleted local `write-up/` (byte-identical to `starter/`); `.remember/` added to `.gitignore` | Verified identical via shasum; avoid two copies drifting | `starter/` is the only copy of official materials |
| 2026-09-29 | Training platform | Use PSC V100 (course shared conda env) as primary compute; allocation confirmed | Allocation confirmed; V100 policy per @254 | Long runs target PSC; Kaggle/Colab as fallback |
| 2026-09-29 | Track strategy | Two tracks: stable (checkpoint ≥0.80 by Oct 2) + exploration (stronger model by Oct 9) | HW1P2 lesson: committing early to one model family's local tuning wasted exploration time | Phase A and B scheduled in `critical_path.md` |
| 2026-09-29 | Checkpoint model choice | Checkpoint uses starter 5-layer CNN + CE (H1), not a custom model | Fastest compliant path to 0.80; submission pipeline gets validated early | T01–T05 before any architecture exploration |
| 2026-09-29 | Submission timing | Checkpoint submission target: Oct 2 **noon**, not last hour | Buffer for training finish, upload, leaderboard queue; daily limit 10 | T05 acceptance criterion |
| 2026-09-29 | Canvas HW2 quiz | Completed, confirmed directly by Cathy at 15:11 America/New_York; no score independently verified | User status update | Remove quiz from pending work; preserve distinction between completion and verified full marks |
| 2026-09-29 | Early-cutoff priority (supersedes checkpoint sprint/noon/model-gate decisions above) | Oct 2 / 0.80 is best effort, not a required internal gate; full setup, validation and learning take priority | Cathy explicitly accepts missing early cutoff rather than compressing or omitting steps | PLAN, critical path, taskboard and T05 updated; official grading rules unchanged; detailed technical review remains pending |
| 2026-09-29 | Starter pipeline scope | Verified against starter notebook: datasets/loaders, criterion/optimizer/scheduler, verification metrics are all `NotImplementedError`, not just the backbone | Reviewer finding confirmed by inspecting starter cells 51/56/59/68/70/72 | T02 expanded to pipeline completion; T03 smoke keeps 8,631 output |
| 2026-09-29 | PSC storage zones | Dataset on node-local `$LOCAL` (temporary, per starter cells 25/39/41); checkpoints/code on persistent home path | Shared-FS I/O is hours/epoch; `$LOCAL` wiped on node change | T01 acceptance; DATA.md path conventions |
| 2026-09-29 | Credentials & repo scope | Public repo holds only planning/records/allowed materials; working code via local/private git; keys via env/untracked config, never in notebook or committed file | Starter cell 104 demands inline keys; repo is public → leak risk | README, requirements checklist, T01 |
