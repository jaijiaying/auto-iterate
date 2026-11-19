# Skill 差距分析与修复策略

当 skill 模式 `verdict: fail` 时使用。把失败收敛为对 skill 文件的最小修改。每条诊断挂到具体 `E#` 与文件路径。

## 分析顺序

1. **P0** — `hard_fail`、清单硬性 `unmet`、分数 ≤ 2、回归项
2. **P1** — 仍 < 4 的维度与 `partial` 项
3. **P2** — 已 ≥ 4 的抛光（未达标前不做大范围润色）

## 问题分类 → 修复策略

| 类型 | 典型证据 | edit_scope | 策略 |
|------|----------|------------|------|
| 目标未覆盖 | `goal` 某能力无对应章节 | section | 增补专用小节或 reference，并在入口挂上 |
| 触发弱 | description 无 WHEN/关键词 | patch | 重写 description，保留准确 name |
| 步骤不可执行 | 「注意xxx」但无动作顺序 | section | 改成有序步骤 + 完成前检查 |
| 模式歧义 | A/B 模式边界不清 | section | 加分流表与互斥条件 |
| 入口与详规冲突 | SKILL 说读 02，02 又否定入口 | patch / section | 以 Brief 目标为准统一一处，删矛盾句 |
| 死链/漏载 | 链接文件不存在 | patch | 补文件或改链接；保证一层引用 |
| 过长臃肿 | SKILL.md 堆满细则 | restructure | 细则外置 references，入口只编排 |
| 试跑失败 | 出图/行为与规则声称不符 | section | 先改规则缺口；避免只改试跑提示词掩盖 skill 问题 |
| 过度修改 | 无关章节被重写 | patch | 回滚无关 diff，只留诊断相关改动 |

## 诊断字段

每条：

- `problem` / `evidence`（路径+章节）/ `priority` / `minimum_repair` / `locked_invariants`

整轮：

- `preserve_next`：已通过且勿动的部分
- `edit_scope`：`patch` | `section` | `restructure`

## 范围选择

- `patch`：一两句、description、单个清单项、死链
- `section`：单个 reference 或 SKILL 内一整节
- `restructure`：目录重划、入口/详规职责重绑；仅连续两轮 patch/section 无法消 P0，或结构性矛盾时使用

## 防振荡

- 同一规则缺口两轮无改进 → 换写法（示例、反例、检查清单），勿重复同义空话
- 修 A 引入新 hard_fail → 先撤回归再继续
- 已 ≥ 4 的维度非必要不改
- 试跑仅证明问题；主修对象仍是 skill，除非用户改判为 `image` 模式
