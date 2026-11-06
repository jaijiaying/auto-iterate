---
name: auto-iterate
description: >-
  自动迭代编排器：先根据用户前期意图识别模式，再进入图片迭代或 skill 迭代。
  图片迭代对输入图按目标与样式多轮「产出→评测→诊断→再改」直至达标；
  skill 迭代对 SKILL.md / references 等多轮「改写→评测→诊断→再改」直至达标。
  在用户要求自动迭代、多轮评测修图、迭代改 skill、调试 skill、达标为止时使用。
disable-model-invocation: true
---

# 自动迭代（图片 / Skill）

本 skill 是迭代编排器，不代替具体画风 skill。每次调用先做**意图识别**，再进入唯一模式，禁止混跑。

## 执行总序

```
1. Intent   识别 iterate_mode = image | skill
2. Brief    按模式补齐输入契约（缺项先问）
3. Loop     该模式的 产出 → 评测 → 诊断 → 再迭代
4. Final    输出终稿 / 当前最佳 + 轮次摘要
```

未写出 `iterate_mode` 与判定依据前，不得开始 Produce。

## Step 0 — 意图识别

读取 [references/00-intent-routing.md](references/00-intent-routing.md)，根据用户**本轮及前期表述**判定模式。

输出固定结构后再继续：

```markdown
## Intent
- iterate_mode: image | skill
- confidence: high | medium | low
- evidence: 引用用户原话或关键信号（1–3 条）
- not_the_other_because: 为何不是另一模式（一句话）
```

规则摘要：

| 模式 | 迭代对象 | 典型说法 |
|------|----------|----------|
| `image` | 图片像素结果 | 改图、修图、出图到达标、按样式迭代画面 |
| `skill` | skill 文本与规则文件 | 写/改 skill、调试 SKILL.md、补 references、把 skill 打磨达标 |

易混：

- 「用某某 skill 把这张图改到过关」→ **image**（skill 是工具，图是迭代对象）
- 「把某某 skill 改到能稳定出对的图」→ **skill**（skill 文件是迭代对象；可用试跑图作评测证据）

`confidence: low` 或信号冲突时：用一句话向用户确认模式，得到答复后再进入 Brief。用户也可显式指定：`模式：图片迭代` / `模式：skill 迭代`。

会话中途仅当用户明确要求切换模式时才重跑 Intent；否则锁定首轮模式。

## Step 1 — Brief（按模式）

### 模式 A：`image`

必填：`source_image`、`goal`、`target_style`  
可选：`acceptance`、`max_rounds`（默认 5）、`lock`、`style_skill`

详见 [references/10-image-loop.md](references/10-image-loop.md)。

### 模式 B：`skill`

必填：`skill_path`（目标 skill 目录或 `SKILL.md`）、`goal`（skill 应具备的能力/行为）、`done_when`（达标标准）  
可选：`acceptance`、`max_rounds`（默认 5）、`base_skill`（可参考的现有 skill）、`smoke_cases`（试跑用例：提示词/测图）

详见 [references/20-skill-loop.md](references/20-skill-loop.md)。

## Step 2 — 核心循环（两模式共用骨架）

```
Round N:
  1. Plan
  2. Produce   # image→出图；skill→改 skill 文件（可附带试跑）
  3. Evaluate  # 看实际产物，禁止只凭自称达标
  4. Decide    # pass 结束；fail 则 Diagnose → N+1；达上限结束
```

- 禁止跳过评测连跑多轮
- 下一轮 Plan 必须消费上轮 Diagnosis
- 默认最小充分修改，已通过项写入 preserve

### 模式路由

| `iterate_mode` | 循环细则 | 评测 | 诊断 | 示例 |
|----------------|----------|------|------|------|
| `image` | [10-image-loop.md](references/10-image-loop.md) | [11-image-evaluation-rubric.md](references/11-image-evaluation-rubric.md) | [12-image-gap-analysis.md](references/12-image-gap-analysis.md) | [13-image-example.md](references/13-image-example.md) |
| `skill` | [20-skill-loop.md](references/20-skill-loop.md) | [21-skill-evaluation-rubric.md](references/21-skill-evaluation-rubric.md) | [22-skill-gap-analysis.md](references/22-skill-gap-analysis.md) | [23-skill-example.md](references/23-skill-example.md) |

进入循环前，只加载当前模式对应的 references。

## 停止条件

1. `verdict: pass`
2. 已完成 `max_rounds`
3. 用户要求停止或改目标（改目标则更新 Brief；改模式则重跑 Intent）

## 会话落盘（建议）

```
runs/<session_id>/
  intent.md
  brief.md
  round-01.*          # image: png + eval；skill: diff 摘要 + eval（+ 可选试跑图）
  ...
  final.md
```

## 输出给用户

1. 判定的 `iterate_mode`
2. 终稿（图路径，或 skill 文件变更摘要）
3. 是否达标、轮次摘要
4. 未达标时的遗留问题与建议决策点

## 边界

- 一次会话一个主模式；需要两种结果时分两次意图清晰的任务跑
- 不代替具体画风 skill 的视觉规则；image 模式下画风细节以 `style_skill` 为准
- skill 模式下试跑出图是**评测证据**，不是主交付物（除非用户同时要图）
- 评测必须基于实际产物（看图 / 读改后的 skill 全文）
