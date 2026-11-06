# Skill 迭代循环

仅当 `iterate_mode: skill` 时使用。迭代对象是 skill 文件本身，不是最终业务图。

## Brief 字段

| 字段 | 必填 | 含义 |
|------|------|------|
| `skill_path` | 是 | 目标 skill 目录或 `SKILL.md` 路径 |
| `goal` | 是 | skill 应具备的能力、触发场景、行为约束 |
| `done_when` | 是 | 达标标准（规则完整性、可执行性、试跑表现等） |
| `acceptance` | 否 | 更细的检查清单 |
| `max_rounds` | 否 | 默认 5 |
| `base_skill` | 否 | 可对照的现有 skill（只参考结构/写法，不抄无关业务） |
| `smoke_cases` | 否 | 试跑用例列表（文案提示、测图路径、期望现象） |
| `lock` | 否 | 不可改动的约定（客户名、固定资产路径、宿主协议等） |

若从零新建：`skill_path` 指向将创建的目录；首轮 Produce 负责搭骨架。

## Round 流程

### 1. Plan

- 本轮要补/改的规则缺口（对应上轮诊断或 Brief）
- 本轮不改的 `lock` 与已通过章节
- 是否需要试跑、跑哪些 `smoke_cases`

### 2. Produce

允许的改动范围（最小充分）：

- `SKILL.md`（frontmatter `name` / `description`、流程、边界）
- `references/*`、必要的 `assets` 说明、辅助脚本说明
- 不擅自改无关项目文件

产出要求：

- 写出本轮变更摘要（改了哪些文件、解决哪条诊断）
- 若 Brief 含 `smoke_cases` 或评测需要行为证据：按用例调用下游生图/流程做试跑，结果作为评测附件，**不是**本模式主交付

落盘建议：

```
runs/<session_id>/round-NN-changelog.md
runs/<session_id>/round-NN-smoke/   # 可选试跑图与笔记
```

### 3. Evaluate

通读改后的 skill 关键文件（必要时对照试跑），按 [21-skill-evaluation-rubric.md](21-skill-evaluation-rubric.md)：

```markdown
## Round N Evaluation
- score_goal_fit: 0-5
- score_clarity: 0-5
- score_executability: 0-5
- score_consistency: 0-5
- score_smoke: 0-5 | n/a
- hard_fail: []
- verdict: pass | fail
- summary: 一句话总评
```

默认达标：无 `hard_fail`；`goal_fit` / `clarity` / `executability` / `consistency` 均 ≥ 4；若有 `smoke_cases` 则 `score_smoke` ≥ 4；且 `done_when` / `acceptance` 满足。

### 4. Diagnose（fail 时）

按 [22-skill-gap-analysis.md](22-skill-gap-analysis.md)：

```markdown
## Round N Diagnosis
1. [P0] 问题 → 证据（文件路径/章节） → 下一轮最小改法
- preserve_next: ...
- edit_scope: patch | section | restructure
```

## 终稿输出

1. skill 路径与本轮次变更摘要（可列关键 diff 要点）
2. 是否达标、轮次摘要、试跑结论（若有）
3. 未达标时的规则缺口与建议决策点
