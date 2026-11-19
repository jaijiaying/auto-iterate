# Skill 迭代循环

仅当 `iterate_mode: skill` 时使用。迭代对象是 skill 文件。先完成 [01-brief-and-expected.md](01-brief-and-expected.md)。

## Brief 字段

| 字段 | 必填 | 含义 |
|------|------|------|
| `skill_path` | 是 | 目标 skill 目录或 `SKILL.md` |
| `goal` | 是 | 应具备的能力与行为 |
| `done_when` | 是 | 达标标准 |
| `expected_checklist` | 是 | 由 goal/done_when 拆出的预期项 |
| `acceptance` | 否 | 更细清单 |
| `max_rounds` | 否 | 默认 5 |
| `base_skill` | 否 | 结构参考 |
| `smoke_cases` | 否 | 试跑用例 |
| `lock` | 否 | 不可改约定 |
| `confirm_each_round` | 否 | true 时每轮确认 |

从零新建时：`skill_path` 指向将创建目录；首轮搭骨架。

## Round 流程

### 1. Plan

- 对应哪些 E# / 上轮 Diagnosis
- preserve 的章节与 `lock`
- 是否跑哪些 smoke

### 2. Produce

按 [03-produce-host.md](03-produce-host.md)：

- 最小改 `SKILL.md` / `references/*` 等
- 写 `round-NN-changelog.md`
- 需要时跑 smoke → `round-NN-smoke/`

### 3. Evaluate

1. 通读改动后的关键文件（+ 试跑证据）
2. 填 Expected vs Actual（[02-round-protocol.md](02-round-protocol.md)）
3. 按 [21-skill-evaluation-rubric.md](21-skill-evaluation-rubric.md) 打分

```markdown
## Round N Evaluation
### Expected vs Actual
| id | expected | actual | status |
|----|----------|--------|--------|
| E1 | ... | 路径+章节事实 | met/partial/unmet |

- score_goal_fit / clarity / executability / consistency: 0-5
- score_smoke: 0-5 | n/a
- hard_fail: []
- verdict: pass | fail
- summary: 一句话
- best_so_far: round-XX
```

达标：无 hard_fail；四维 ≥ 4；有 smoke 则 smoke ≥ 4；清单硬性项全 met；`done_when` / `acceptance` 满足。

### 4. Diagnose（fail）

按 [22-skill-gap-analysis.md](22-skill-gap-analysis.md)，挂到具体 E# 与文件路径。

```markdown
## Round N Diagnosis
1. [P0] E# → 证据（路径/章节）→ 最小改法
- preserve_next: ...
- edit_scope: patch | section | restructure
```

### 5. Update / Decide

更新 `best_so_far`；短报告；Final 时按轮次协议恢复并交付最佳轮次的文件状态；无法安全恢复时给出可恢复版本的位置与步骤。

## 终稿

1. skill 路径 + 变更摘要（最佳轮）
2. 是否达标、试跑结论、剩余 unmet
3. `final.md`
