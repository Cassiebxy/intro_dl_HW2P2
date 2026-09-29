# Review Comment Template

> Copy per review event. Filename: `YYYY-MM-DD_<topic>.md` inside the reviewer's folder (`claude/`, `codex/`, `misc/`).

```markdown
# Review: <topic> — <reviewer name> — YYYY-MM-DD

- **Reviewed object**: file/commit/task (e.g. `PLAN.md @ commit abc123`, `T07 compliance checklist`)
- **Reviewer model/context**: (e.g. Claude Opus, ChatGPT Codex, classmate)
- **Overall verdict**: approve / approve-with-nits / concerns / reject

## Findings (severity ordered)

| # | Severity | Location | Issue | Suggested fix | Status (accepted/rejected/waved) |
| --- | --- | --- | --- | --- | --- |
| 1 | critical/major/minor | file:section | ... | ... | ... |

## What this review does NOT cover

- (scope limits — e.g. "did not run code", "rules not re-verified against Piazza")

## Follow-ups created
- [ ] task/decision-log entries spawned by this review
```

## Conventions

- One review event = one file; re-reviews reference it.
- Reviewer folder = which AI/human produced it. Cross-AI comparison: same topic, same date prefix, different folders.
- **Status updates are Cathy's**: accept/reject each finding explicitly; rejected with one-line reason.
- Reviews of rule compliance must cite official sources (`references/`, staff posts); opinions labeled as opinions.
