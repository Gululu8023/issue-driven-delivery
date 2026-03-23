# Issue Driven Delivery

[中文说明](./README.zh-CN.md)

Current version: `0.1.0`

`issue-driven-delivery` is a Codex skill for delivery rounds that need a fixed workflow:

1. confirm or refine a plan,
2. write the plan into a repository-local `todolist`,
3. split the work into ordered `issues`,
4. pause for user review when requested,
5. implement in issue order,
6. update progress and review status after each issue,
7. report by default only after the whole issue list is complete.

This README is the human-facing overview for GitHub visitors. The actual agent instructions live in [SKILL.md](./SKILL.md).

## What This Skill Does

This skill turns one delivery round into a documented and traceable workflow rooted in the repository itself.

It is designed for cases where the user already has a plan, wants to co-create a plan with Codex, or wants Codex to help consolidate an incomplete plan before execution starts.

## When To Use

Use this skill when you want Codex to:

- discuss and settle the plan before coding,
- create a repository-local work plan under `.work-plans/`,
- produce both a narrative `todolist` and an executable `issues.csv`,
- wait for plan review before implementation when requested,
- execute issues in order and keep tracking artifacts updated,
- finish with a single final report instead of frequent phase summaries.

## When Not To Use

This skill is not meant for:

- ad-hoc one-off coding tasks that do not need plan artifacts,
- workflows that intentionally skip planning and issue tracking,
- delivery modes where issues are executed in parallel without ordered tracking,
- teams that want status summaries after every phase by default.

## Workflow

The skill follows this default flow:

1. Confirm or refine the plan with the user.
2. Create `.work-plans/` at the repository root.
3. Write the plan to `.work-plans/<topic>-todolist.md`.
4. Split the plan into `.work-plans/<topic>-issues.csv`.
5. Stop at the documentation stage if the user wants to review the plan first.
6. After approval, execute issues in order.
7. After each issue, update tracking artifacts before moving to the next one.
8. Mark review as passed only after code, documentation, and verification are aligned.
9. Give a single consolidated report after all issues are complete unless the user explicitly asks for interim updates.

## Artifact Conventions

By default, all delivery artifacts for the round live under `.work-plans/`.

Recommended filenames:

- `.work-plans/<topic>-todolist.md`
- `.work-plans/<topic>-issues.csv`

The `todolist` is the planning document. It should capture:

- goal and scope,
- confirmed baseline,
- exclusions,
- current gaps or open questions,
- phased plan,
- acceptance criteria,
- execution order.

The `issues.csv` is the execution tracker. Each row should at least cover:

- `issue_id`,
- `task_title`,
- `task_content`,
- `status`,
- `progress_pct`,
- `done_criteria`,
- `review_criteria`,
- `dependencies`,
- `affected_paths`,
- `notes`.

For the field-level checklist and update discipline, see [references/artifact-checklist.md](./references/artifact-checklist.md).

## Execution Discipline

This skill assumes a few operating rules:

- Plan sources are flexible: the user may provide the plan directly, co-create it with Codex, or ask Codex to consolidate it.
- Do not enter implementation before the plan is settled.
- Do not mark progress based on intent; only update progress from completed facts.
- Keep issue order aligned with the real implementation order when the user asks for sequential execution.
- Update tracking files before starting the next issue.
- Keep `notes` short and evidence-oriented.

## Prompt Examples

Example requests that should trigger this skill:

- `Use $issue-driven-delivery to discuss the plan with me first, then write the todolist and issues.`
- `Use $issue-driven-delivery for this request, but stop after the plan so I can review it.`
- `I already have a plan. Use $issue-driven-delivery to turn it into a todolist and issues.`
- `Use $issue-driven-delivery to continue the issues in .work-plans/ and report only after everything is done.`

## Repository Layout

```text
issue-driven-delivery/
├── CHANGELOG.md
├── SKILL.md
├── README.md
├── README.zh-CN.md
├── agents/
│   └── openai.yaml
└── references/
    └── artifact-checklist.md
```

## Notes

- [SKILL.md](./SKILL.md) is the runtime contract for Codex.
- [CHANGELOG.md](./CHANGELOG.md) tracks released versions of the skill.
- [references/artifact-checklist.md](./references/artifact-checklist.md) contains the detailed checklist for `todolist` and `issues.csv`.
- `agents/openai.yaml` contains UI metadata for environments that surface skills in a picker or command palette.

## Versioning

This skill currently starts at `0.1.0` as the initial public release.

Versioning is expected to happen at the repository level through Git tags or GitHub Releases, while [CHANGELOG.md](./CHANGELOG.md) records the human-readable release history.
