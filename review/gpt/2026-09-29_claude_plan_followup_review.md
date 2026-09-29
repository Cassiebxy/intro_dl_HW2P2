# Review: Claude planning revision and writeup compliance — 2026-09-29

- **Reviewed object:** `main @ f6afb6ced2e66906229e2c84e3a6316b86de02ff`, including Claude revision `8a86f5d965cecd3ea6d6e0b7321c6468c1c07b10` and the subsequent merge.
- **Reviewer:** Codex (GPT-6), MLE planning review; stored in `review/gpt/` at Cathy's explicit request.
- **Verdict:** overall direction approved; fix the Phase-A acceptance criteria and merge inconsistencies before treating the plan as an execution contract. Remaining modeling details can be resolved before their respective optional experiments.
- **Scope of this commit:** adds this review only. Findings are proposals, not implemented fixes or accepted user decisions.

## What was inspected

Current README, PLAN, DATA; all three design documents; critical_path, taskboard and T01–T10; experiments/README; review/TEMPLATE; archived Piazza staff guidelines; the previous GPT review.

Official materials: PyTorch writeup, especially pp. 1–4 (schedule, collaboration, constraints, checklist), pp. 19–20 (ArcFace), pp. 22–26 (verification, scoring, submission); PyTorch starter cells 3, 51/56/59/68/70/72, 74–85, 89, 97–98 and protected final submission cell. Previously extracted source text was used; current repository blob identities for the PyTorch writeup and notebook remain unchanged (PDF `f1e52bcfd108891c43805300d92b616f4e75251d`, notebook `6b7687388fc89d7bc0bc45395e20a3c6bd7bb732`). The JAX writeup was previously inspected for deadline discrepancies; no new JAX implementation review was performed.

Sources: [official PyTorch writeup](../../starter/HW2P2_Writeup_F26.pdf), [starter notebook](../../starter/F26%20HW2P2%20Student%20Starter%20Notebook_final.ipynb), [archived staff rules](../../references/Piazza_HW2P2_Staff_Guidelines.md).

No code execution, PSC allocation verification, training, Kaggle submission or Gradescope grading was performed. Live Piazza/Kaggle rule changes were not rechecked. Pending statuses and empty experiment records do not establish runtime readiness.

## Improvements confirmed

- T02 now covers the full starter pipeline, rather than just the backbone; this matches the writeup p. 4 checklist.
- T03 retains 8,631 outputs and limits workload; reload and resumed optimization are now distinct checks.
- T01/DATA separate node-local dataset staging from persistent checkpoints and acknowledge allocation limits.
- T05 and the main priority notice correctly distinguish Cathy's optional early-cutoff priority from official checkpoint grading requirements.
- Requirements now cover external data, protected submission code, and matching the final MODEL to the selected Kaggle model.
- Working implementation has a local/private Git destination, avoiding both credential exposure and public graded-code sharing. This is consistent with writeup p. 2's own-code/no-code-sharing policy.
- T08 no longer depends on early submission success; optional exploration and final fallback are stated explicitly.

These are meaningful improvements. Another architecture-planning round is unnecessary.

## Findings, ordered by execution relevance

All findings below are **pending Cathy/Claude disposition**. Severity describes execution impact; these are not claims of an already committed academic-integrity violation.

### R1 — Major: merge restored obsolete schedule requirements

**Location:** critical_path, “Baseline track”; PLAN A5 and Phase-B DoD; T10 acceptance.

critical_path says T01–T03 “must finish today” and targets NOON, while its own calendar says there is no mandatory noon target. PLAN also compares final validation against a “checkpoint version”; T10 compares against a checkpoint score, even though T05 may be skipped.

**Fix:** mark all date-by-date windows explicitly illustrative and conditional on readiness. Remove “must finish today”; keep any early upload buffer advisory. Compare final candidates to the best validated reproducible baseline/candidate; compare against an early submission only if one exists. Preserve the append-only decision log's historical entries with their existing supersession notice.

This is a user-priority consistency issue, not a change to official deadlines. Official Oct 2 / Oct 9 / Oct 11 dates remain (writeup p. 1).

### R2 — Major: T01 depends on implementation that belongs to T02

**Location:** T01 steps 6/acceptance; T02; taskboard.

T01 requires full loaders before T02 has implemented them. The taskboard lists both unblocked, which is possible for parallel preparation but does not justify “T01 first, fully done, then T02.”

**Fix:** split T01 into environment/storage readiness and a loader integration check after T02's dataset/loader portion. Alternatively explicitly allow T01 and T02 to overlap, and require both complete before T03. No new task family is necessary. Record actual paths and the persistent-storage guarantee; a kernel restart alone does not demonstrate cross-node persistence.

Writeup p. 4 lists environment/data preparation before architecture and training; this sequencing refinement is an engineering recommendation.

### R3 — Major: one-step loss decrease is not a reliable resume test

**Location:** T03 steps 2, 5–6 and acceptance.

Loss need not decrease between different mini-batches or after one resumed step. A correct stochastic optimizer can fail that criterion. “Identical output” also needs defined eval mode and numerical tolerance, since training-mode BN/dropout changes outputs.

**Fix:** on fixed inputs in eval mode, compare pre-save/reloaded outputs within a documented tolerance. Verify model, optimizer and scheduler state restoration, epoch/global-step progression, finite loss/gradients, a real optimizer step and parameter updates. With AMP, include GradScaler state. The starter cell 83 omits scaler state; cell 85 must also resume from the restored counter rather than restart the schedule. A small fixed-batch overfit check may test learning separately; monotonic mini-batch loss should not be the hard gate.

For mid-epoch resume, specify either saved sampling position/RNG state or an explicit restart-of-epoch policy. Do not claim bitwise uninterrupted-training equivalence from a single resumed step.

Writeup p. 4 requires save/load capability; the stronger resume checks are engineering recommendations.

### R4 — Major: metric units and sanity validation still need an explicit contract

**Location:** requirements Metric; T02 metrics; T04/experiment records; H1's “~0.80 region.”

The writeup p. 24 uses fractions for accuracy and EER. Starter cell 85 computes combined score on a percentage scale. Mixing 0.80 and 80.0 can select the wrong checkpoint even when all code runs.

**Fix:** define local `cls_acc_pct`, `eer_pct`, `combined_pct = 0.5*cls_acc_pct + 0.5*(100-eer_pct)`; convert to fractions explicitly when needed. Compare validation scores with validation scores, not a Kaggle score from a different split. Add small deterministic metric sanity checks: both positive/negative pairs present, higher similarity means same identity, perfect separated scores give near-zero EER. Use the expected starter return keys and units. Smoke EER from an untrained model is only evidence that the metric path works.

### R5 — Major before T06: short-run performance still gates residual exploration

**Location:** H2 promotion/stop rules and T06 step 4/acceptance.

The updated tasks still require H2 to beat H1 under a short/equal-budget screen before full confirmation. A deeper CNN's slower early convergence is not sufficient evidence to reject it. Identical optimizer/schedule is useful for one controlled ablation but is not proof of a competently tuned architecture comparison.

**Fix:** short screens assess shape, memory, throughput, finite gradients, loss trend and feasibility. Allocate a bounded confirmation budget when feasible. Document both any identical-recipe comparison and reasonable model-specific LR/schedule tuning. Stop on evidence of failure or exhausted budget; do not conclude “H2 is worse” from 2–3 epochs alone.

This is experimental-design advice, not an official architecture requirement.

### R6 — Major before T07: ArcFace inference and promotion remain underspecified

**Location:** H3 and T07.

H3 still requires better EER with unchanged/better accuracy; this can reject a higher combined score. T07 does not define how label-dependent training logits become label-free inference logits.

**Fix:** promote on combined validation score, reporting both components. Define normalized embeddings/class weights, target-only training margin, label-free classification inference without target margin, the `out`/`feats` interface, and all optimized/checkpointed head parameters. Include the class head in the 30M count. A model can improve combined score despite a small accuracy tradeoff.

ArcFace is compatible in principle with writeup pp. 19–20 and archived @303 if its parameters train, embeddings/weights normalize, and LS/Mixup/CutMix are excluded.

### R7 — Major before T09: TTA must work through the immutable submission interface

**Location:** T09 and PLAN B4.

“Average then normalize” differs from separately normalizing each view before fusion. More importantly, protected cell 89 calls the same model for classification and verification; a custom inference helper cannot simply replace that protected cell. “Classification unaffected, skip re-eval” is therefore premature.

**Fix:** if TTA is attempted, separately normalize original/flipped embeddings, fuse, normalize again and compare the same frozen checkpoint. Define a model/interface path compatible with official `out`/`feats` inference and final MODEL packaging without modifying protected cells. Verify classification outputs remain unchanged if that is the intent; otherwise evaluate both metrics. Confirm parameter-budget accounting and submission/Gradescope compatibility before adoption. Drop TTA if compatibility cannot be established within budget.

Normalization order is a modeling recommendation, not an official mandated recipe. Protected-code integrity is an official requirement (starter cell 3).

### R8 — Medium: optional tasks still have mandatory model dependencies

**Location:** T07 blocked-by T06; T09 blocked-by T06/T07; taskboard.

If residual exploration is skipped, ArcFace could still use H1, and TTA could still use the frozen H1 candidate. Requiring T06/T07 conflicts with the stated optionality.

**Fix:** T07 needs a selected, validated CE backbone; T09 needs a frozen validated candidate and an approved compatible inference path. Record skipped tasks as dropped with a reason. T10 already has an appropriate “at least one valid candidate” prerequisite.

### R9 — Medium: data schema and staging documentation need small corrections

**Location:** DATA layout and T01 staging text.

DATA says test_pairs has “same format” as labelled val_pairs, then says it has no labels. Document two filenames per test row and three fields per validation row. Verify with the actual files before implementation.

Node-local placement is the requirement; downloading again from Kaggle on every node is one staging method, not the only one. Copying/extracting an allowed persistent dataset cache into the new node's $LOCAL is also a practical option, subject to quota and platform rules. Keep checkpoints outside $LOCAL.

### R10 — Medium: distinguish official rules from project choices

**Location:** requirements checklist; README Repo scope; T02 inventory.

Writeup p. 1 requires FULL score on quiz MCQs. Completion is correctly user-confirmed, but the summary checklist should still state that full-score requirement explicitly without marking it verified.

README calls “submission/credential cells” protected. The starter explicitly marks submission cells DO NOT MODIFY; do not extend that status to every credential setup cell without source support. Environment-variable setup is appropriate where editable.

`MODEL=model` (cell 98) is a final packaging assignment, not a pipeline TODO required before a baseline has been trained. Validate `out`/`feats` in T02 and set final MODEL to the frozen selected model in T10.

No external data is permitted even though writeup p. 4 has generic “additional data” language; the explicit starter acknowledgement restriction governs.

### R11 — Medium: checkpoint retention, submission preflight and slots

**Location:** T04 “keep ALL epoch checkpoints”; PLAN B5 and experiment-record rules; T10.

Keep `last` and `best_combined`, plus optional component-best checkpoints and frozen submitted artifacts. Record all metrics; retaining every epoch's weights is unnecessary unless justified by quota/budget.

critical_path correctly asks for early packaging preflight, but PLAN/T10 still emphasize first packaging after Oct 9. Test the export/package/model contract before the final deadline, then regenerate with final artifacts. The first verified baseline submission may be worthwhile for format/system validation even without beating a previous score; daily slots are a resource policy, not a strict validation-improvement gate.

## Assessment of the earlier GPT review

Most earlier engineering concerns were valid, and this push addresses its central pipeline/storage/label-space issues. Three earlier recommendations need the user's newer policy/context:
- Crossing 0.80 before residual exploration is superseded by Cathy's optional-cutoff decision.
- Track working code in local/private Git; do not interpret “track notebook/src” as permission to publish a graded solution in this public repo.
- A strong CE recipe is useful before ArcFace, but separate augmentation tuning need not become another mandatory phase. Record the baseline transforms/recipe and change one main factor per controlled experiment.

The earlier review remains a historical document; this file is the current follow-up, not an overwrite.

## Next step by repository structure

1. In `docs/implementation/tasks/`, resolve R1–R4 sufficiently to define Phase-A acceptance. Update taskboard and append consequential decisions to `docs/design/decision_log.md`. Other findings can be handled before their optional task.
2. Establish the private/local working implementation and its commit/config identity. Read the starter acknowledgement and writeup checklist; keep `starter/` unchanged.
3. Execute T01 environment/storage preparation while implementing the T02 dataset/loader portion; integrate loader verification. Finish T02's baseline CNN, CE recipe and verified metrics.
4. Execute T03 limited-step smoke, fixed-input reload and resumed-optimization checks. Generate the full official CSV in the unmodified submission path; limited training does not mean truncating final test predictions.
5. Store each actual run/evidence in `experiments/runs/`, and update taskboard plus task notes with evidence links.
6. Only after T03 passes, execute measured T04 full-data baseline; retain its best-combined checkpoint and measure training + validation cost. T05 is optional when ready. Then decide which T06–T09 experiments fit the budget; final T10 remains required.

This does not authorize executing training or submitting coursework in this review turn.
