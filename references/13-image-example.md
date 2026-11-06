# 图片迭代示例（虚构）

## Intent

```markdown
## Intent
- iterate_mode: image
- confidence: high
- evidence: 用户提供输入图，要求改场景与样式直到画面过关
- not_the_other_because: 交付物是图，不是改 skill 文件
```

## 用户输入

- `source_image`: 两个小孩在公园的插图
- `goal`: 改成教室内上课，保留两孩身份与座位关系，黑板上写「春天」
- `target_style`: 专黑细线、马卡龙配色教材插画
- `lock`: 两个小孩的脸型与发型
- `max_rounds`: 5

## Round 1

**Plan** → 公园改教室；保留相对位置；套专黑细线+马卡龙；板书「春天」

**Produce** → `runs/20261105-1523/round-01.png`

```markdown
## Round 1 Evaluation
- score_goal: 3
- score_style: 4
- score_fidelity: 4
- score_quality: 3
- hard_fail: ["黑板文字不可读"]
- verdict: fail
- summary: 教室基本成立，板书乱码，右童手部粘连。
```

```markdown
## Round 1 Diagnosis
1. [P0] 黑板「春天」不可读 → 板书笔画混乱 → 仅重绘黑板文字
2. [P1] 右童右手粘连 → 袖口手部糊成一块 → 局部修手
- preserve_next: 两童脸型发型、教室大构图、专黑线与配色
- redraw_scope: local
```

## Round 2

**Produce** → `round-02.png` → `verdict: pass`（四项 ≥ 4，无 hard_fail）

**Final**：终稿 `round-02.png`，达标，共 2 轮
