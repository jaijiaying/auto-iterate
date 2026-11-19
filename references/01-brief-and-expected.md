# Brief 模板与预期清单

Intent 完成后、首轮 Produce 前必须完成。Brief 是任务说明书；`expected_checklist` 是评测时逐条对照的**预期结果**。

## 通用规则

1. 先从用户请求与现有文件推断已知字段；只有无法推断且影响方向的关键缺口才询问
2. 从 `goal` / `target_style` / `done_when` **拆出**可观察的检查项，写入 `expected_checklist`
3. 每条检查项必须可判定 `met | unmet | partial | unverified`，禁止「更好看一点」这类不可验条目
4. 每条注明验证方式及是否为硬性项；仅在有必要时把 Brief 与清单写入 `runs/<session_id>/`，避免污染目标仓库

## 图片模式 Brief 模板

```markdown
# Brief (image)
- session_id: YYYYMMDD-HHMM
- source_image: <path>
- goal: <内容与用途>
- target_style: <视觉样式>
- lock: <不可变项，可空>
- style_skill: <下游画风 skill，可空>
- acceptance: <用户原话验收，可空>
- max_rounds: 5

## expected_checklist
- [ ] E1: ...
- [ ] E2: ...
- [ ] E3: ...
```

### 如何拆 expected_checklist（image）

从用户表述逐条抽出，通常覆盖：

| 来源 | 转成检查项示例 |
|------|----------------|
| `goal` 中的对象/数量/动作 | 「画面中有 2 个小孩且在上课」 |
| `goal` 中的文字 | 「黑板上可读中文『春天』」 |
| `target_style` 硬约束 | 「描边为纯黑细线」/「仅双色平涂」 |
| `lock` | 「两童脸型发型与输入图一致可辨」 |
| `acceptance` | 原句改写成可观察条件 |
| `style_skill` 验收 | 引用该 skill 的硬验收原文（缩写 + 指向） |

建议 4–8 条；超过 10 条则合并次要项。`hard_fail` 应对应清单中标为硬性的项。

## Skill 模式 Brief 模板

```markdown
# Brief (skill)
- session_id: YYYYMMDD-HHMM
- skill_path: <dir or SKILL.md>
- goal: <能力与行为>
- done_when: <达标标准>
- lock: <不可改约定，可空>
- base_skill: <参考 skill，可空>
- acceptance: <可空>
- max_rounds: 5

## smoke_cases（可空）
1. id: S1
   input: <提示或测图>
   expect: <期望现象>

## expected_checklist
- [ ] E1: ...
- [ ] E2: ...
```

### 如何拆 expected_checklist（skill）

| 来源 | 转成检查项示例 |
|------|----------------|
| `goal` | 「description 含双色教材触发词」 |
| `done_when` | 「入口加载链包含蓝色可见验收」 |
| 结构质量 | 「模式 A/B 分流互斥且可执行」 |
| `lock` | 「固定资产路径未被改名」 |
| `smoke_cases` | 「S1 试跑：蓝版可见、无偏灰」 |

## 清单冻结

- 首轮 Produce 前冻结清单
- 用户改目标时：更新 Brief + 清单，并注明是否重置轮次
- 模型不得为让 `pass` 更容易而私自删减硬性项；只能建议用户改 Brief

## 通用交付物模式 Brief

记录 `artifact_path_or_target`、`artifact_type`、`goal`、`done_when`、`lock`、`max_rounds` 和 `expected_checklist`。清单应对应可观察的内容、功能、格式或运行结果；验证方式写在每条旁边。新建产物可以用拟交付路径。详见 [30-artifact-loop.md](30-artifact-loop.md)。

## 功能模式 Brief

记录 `feature_name`、`repo_or_app`、`user_outcome`、`done_when`、`constraints`、`max_rounds` 和 `expected_checklist`。每条验收项写清用户动作/输入、预期行为及验证方法；包含关键失败路径和已有功能的回归范围。优先读取现有需求、测试和代码中的约定；只有验收方向无法推断时再问。详见 [40-feature-loop.md](40-feature-loop.md)。

## 指标优化模式 Brief

记录目标系统或流程、`metric`（定义与单位）、`baseline`、`target`、测量方法、工作负载或样本、不可退化的护栏指标、变更权限及 `max_rounds`。基线未知时第一步先测；测量环境不具备时说明限制，不编造数值。详见 [50-optimization-loop.md](50-optimization-loop.md)。
