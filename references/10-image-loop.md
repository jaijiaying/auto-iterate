# 图片迭代循环

仅当 `iterate_mode: image` 时使用。

## Brief 字段

| 字段 | 必填 | 含义 |
|------|------|------|
| `source_image` | 是 | 输入图 |
| `goal` | 是 | 内容与用途目标 |
| `target_style` | 是 | 最终视觉样式 |
| `acceptance` | 否 | 自定义达标条件 |
| `max_rounds` | 否 | 默认 5 |
| `lock` | 否 | 不可变内容 |
| `style_skill` | 否 | 下游画风 skill；有则 Produce/评测遵循它 |

## Round 流程

### 1. Plan

- 本轮要修的问题（首轮写主改造点）
- 本轮不改的锁定项
- 预期可见变化

### 2. Produce

- 调用图像生成/编辑工具，或按 `style_skill` 协议出图
- 首轮以 `source_image` 为内容基础；后续轮默认以上轮候选为底图（诊断要求整幅重绘除外）
- 最小充分修改；结果记为 `runs/<session_id>/round-NN.png`（或工具返回可引用路径）

### 3. Evaluate

实际看图，按 [11-image-evaluation-rubric.md](11-image-evaluation-rubric.md)：

```markdown
## Round N Evaluation
- score_goal: 0-5
- score_style: 0-5
- score_fidelity: 0-5
- score_quality: 0-5
- hard_fail: []
- verdict: pass | fail
- summary: 一句话总评
```

默认达标：无 `hard_fail`，四项均 ≥ 4，且 `acceptance`（若有）全满足。

### 4. Diagnose（fail 时）

按 [12-image-gap-analysis.md](12-image-gap-analysis.md)：

```markdown
## Round N Diagnosis
1. [P0] 问题 → 证据 → 下一轮最小修复动作
- preserve_next: ...
- redraw_scope: local | region | full
```

## 终稿输出

1. 终稿或当前最佳图路径
2. 是否达标、轮次摘要
3. 未达标时的遗留问题
