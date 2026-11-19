# 图片迭代循环

仅当 `iterate_mode: image` 时使用。先完成 [01-brief-and-expected.md](01-brief-and-expected.md)。

## Brief 字段

| 字段 | 必填 | 含义 |
|------|------|------|
| `source_image` | 是 | 输入图 |
| `goal` | 是 | 内容与用途目标 |
| `target_style` | 是 | 最终视觉样式 |
| `expected_checklist` | 是 | 由 goal/style/lock 拆出的预期项 |
| `acceptance` | 否 | 用户原话验收 |
| `max_rounds` | 否 | 默认 5 |
| `lock` | 否 | 不可变内容 |
| `style_skill` | 否 | 下游画风 skill |
| `confirm_each_round` | 否 | true 时每轮等待用户确认 |

## Round 流程

### 1. Plan

- 本轮要修：首轮=主改造点；其后=上轮 Diagnosis
- preserve / lock
- 预期可见变化（对应哪些 E# 将变为 met）

### 2. Produce

按 [03-produce-host.md](03-produce-host.md)。落盘 `round-NN.png`（或记录工具返回路径）。

### 3. Evaluate

1. 实际看图
2. 填 [02-round-protocol.md](02-round-protocol.md) 的 Expected vs Actual 表
3. 按 [11-image-evaluation-rubric.md](11-image-evaluation-rubric.md) 打分

```markdown
## Round N Evaluation
### Expected vs Actual
| id | expected | actual | status |
|----|----------|--------|--------|
| E1 | ... | ... | met/partial/unmet |

- score_goal: 0-5
- score_style: 0-5
- score_fidelity: 0-5
- score_quality: 0-5
- hard_fail: []
- verdict: pass | fail
- summary: 一句话
- best_so_far: round-XX (scores ...)
```

达标：无 `hard_fail`，四维 ≥ 4，清单硬性项全 `met`，`acceptance`（若有）满足。

### 4. Diagnose（fail）

按 [12-image-gap-analysis.md](12-image-gap-analysis.md)，优先对应 `unmet` 的 E#。

```markdown
## Round N Diagnosis
1. [P0] E# 问题 → 证据 → 最小修复
- preserve_next: ...
- redraw_scope: local | region | full
```

### 5. Update / Decide

更新 `best_so_far`；输出短轮次报告；pass 或达上限则 Final。

## 终稿

1. 达标稿或 `best_so_far` 路径
2. 是否达标、轮次、剩余 unmet
3. `final.md`
