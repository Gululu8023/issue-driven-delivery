# Issue Driven Delivery

[English](./README.md)

当前版本：`0.1.0`

`issue-driven-delivery` 是一个面向 Codex 的交付流程 skill，用来把一轮需求交付收敛为固定流程：

1. 先确认或补齐方案，
2. 再把方案写入仓库内的 `todolist`，
3. 再拆成按顺序执行的 `issues`，
4. 用户要求先审核时先停在文档阶段，
5. 审核通过后按 issue 顺序实现，
6. 每完成一条 issue 就更新进度和 review 状态，
7. 默认等全部 issue 完成后再统一汇报。

这份 README 是给 GitHub 访客看的入口说明，真正给 agent 执行的规则在 [SKILL.md](./SKILL.md)。

## 这个 Skill 做什么

这个 skill 的目标，是把一轮交付变成“有文档、有顺序、有追踪”的仓库内流程。

它适用于三类情况：用户已经有方案，用户希望和 Codex 共创方案，或者方案还不完整、需要 Codex 先帮忙整理收口。

## 适用场景

当你希望 Codex：

- 编码前先把方案讨论清楚，
- 在仓库根目录创建 `.work-plans/`，
- 同时产出叙述型 `todolist` 和可执行 `issues.csv`，
- 用户要求先审核时先停在方案阶段，
- 后续严格按 issue 顺序推进并持续更新追踪文件，
- 默认只在全部完成后做一次统一汇报，

就适合用这个 skill。

## 不适用场景

这个 skill 不适合：

- 不需要计划文档的一次性小改动，
- 明确跳过规划和 issue 跟踪的交付方式，
- 需要大规模并行执行、而不是顺序推进的流程，
- 默认要求每个阶段都单独汇报的团队习惯。

## 流程

默认流程如下：

1. 先和用户确认或补齐方案。
2. 在仓库根目录创建 `.work-plans/`。
3. 把方案写成 `.work-plans/<topic>-todolist.md`。
4. 把方案拆成 `.work-plans/<topic>-issues.csv`。
5. 如果用户要求先审核方案，就停在文档阶段等待确认。
6. 审核通过后，按 issue 顺序执行。
7. 每完成一条 issue，先更新跟踪文件，再进入下一条。
8. 只有当代码、文档、验证结果都对齐后，review 才能标记通过。
9. 除非用户明确要求中途汇报，否则默认等全部 issue 完成后统一汇报。

## 产物约定

默认情况下，本轮交付相关文件都放在 `.work-plans/` 下。

推荐命名：

- `.work-plans/<topic>-todolist.md`
- `.work-plans/<topic>-issues.csv`

`todolist` 是规划文档，通常应包含：

- 目标与范围，
- 已确认基线，
- 排除项，
- 当前差异或待收口问题，
- 分阶段计划，
- 验收标准，
- 执行顺序。

`issues.csv` 是执行跟踪表，每一行至少应覆盖：

- `issue_id`，
- `task_title`，
- `task_content`，
- `status`，
- `progress_pct`，
- `done_criteria`，
- `review_criteria`，
- `dependencies`，
- `affected_paths`，
- `notes`。

字段级检查项和更新纪律，见 [references/artifact-checklist.md](./references/artifact-checklist.md)。

## 执行纪律

这个 skill 默认遵循以下规则：

- 方案来源可以是用户直接提供、与 Codex 共创，或由 Codex 按需整理。
- 方案没有定稿前，不进入实现阶段。
- 进度只能基于已经完成的事实更新，不能按意图预支。
- 如果用户要求顺序推进，issue 行顺序要和真实实施顺序一致。
- 每完成一条 issue，必须先更新追踪文件，再开始下一条。
- `notes` 保持简短，以证据为主。

## 触发示例

下面这类请求适合触发这个 skill：

- `用 $issue-driven-delivery 先和我一起把方案讨论清楚，再写 todolist 和 issues`
- `按 $issue-driven-delivery 流程推进这轮需求，但先停在方案阶段给我审核`
- `我已经有方案了，用 $issue-driven-delivery 把它落成 todolist 和 issues`
- `用 $issue-driven-delivery 继续执行 .work-plans/ 里的 issue，全部完成后再统一汇报`

## 目录结构

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

## 说明

- [SKILL.md](./SKILL.md) 是给 Codex 执行时读取的运行规则。
- [CHANGELOG.md](./CHANGELOG.md) 用来记录这个 skill 的已发布版本变更。
- [references/artifact-checklist.md](./references/artifact-checklist.md) 负责补充 `todolist` 和 `issues.csv` 的细项检查清单。
- `agents/openai.yaml` 是供支持技能列表或命令面板的环境读取的 UI 元数据。

## 版本说明

这个 skill 当前以 `0.1.0` 作为首次公开发布版本。

版本号建议通过仓库级别的 Git tag 或 GitHub Release 管理，[CHANGELOG.md](./CHANGELOG.md) 则负责记录人类可读的版本变更历史。
