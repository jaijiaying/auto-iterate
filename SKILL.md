---
name: auto-iterate
description: >-
  对开发中的功能、现有系统指标、图片、skill 或可编辑交付物进行有证据的多轮迭代。适用于用户要求反复实现、验证、诊断和修正直至达标的任务；普通一次性修改无需启用。
disable-model-invocation: true
---

# 自动迭代

先识别真正要改的对象，再用同一份验收清单逐轮改进。此 skill 编排迭代，不替代对象专用的创作、编辑或验证流程。

## 路由

读 [意图路由](references/00-intent-routing.md)，选一个 `iterate_mode`：

| 模式 | 主交付 |
|---|---|
| `image` | 图片或图像文件 |
| `skill` | `SKILL.md` 及配套资源 |
| `feature` | 可运行的软件功能、交互或端到端行为，可能跨多个文件 |
| `optimization` | 已有功能或流程的可测指标，如速度、稳定性、成本、准确率或可用性 |
| `artifact` | 其他可编辑交付物，如文档、代码、提示词、表格、演示稿、网页或设计文件 |

按主交付物而非所用工具分类。对象不明确且无法从上下文推断时才询问。用户改目标时更新 Brief；改主交付物时重新路由。

## 执行

1. 按 [Brief 与预期清单](references/01-brief-and-expected.md)记录对象、目标、约束、可观察的验收条件和轮次上限（默认最多 5 轮）。从现有上下文推断已知信息；只询问无法推断的关键缺口。
2. 每轮执行 `Plan → Produce → Verify → Evaluate → Diagnose/Finish`。产出方式见 [宿主对接](references/03-produce-host.md)，证据、停滞处理与版本恢复见 [轮次协议](references/02-round-protocol.md)。
3. 每轮实际检查产物，逐项记录 `expected / actual / met|partial|unmet|unverified`。验证方法应适合对象：看图、通读、渲染、运行测试、操作功能或检查实际输出。不能用“已修改”代替“已达标”。
4. 未达标时，针对失败项做最小充分修改，并回归检查已达标项。无需逐轮等待确认，除非用户要求。
5. 达标、达到轮次上限、用户停止，或继续执行需要未获授权的外部动作/显著成本时停止。交付最佳可用版本、验证证据与剩余问题；不得把未执行的验证说成通过。

## 模式细则

- `image`：读 [图片循环](references/10-image-loop.md)、[评测](references/11-image-evaluation-rubric.md)、[诊断](references/12-image-gap-analysis.md)。画风要求以用户指定的 style skill 为准。
- `skill`：读 [Skill 循环](references/20-skill-loop.md)、[评测](references/21-skill-evaluation-rubric.md)、[诊断](references/22-skill-gap-analysis.md)。试跑产物是证据，主交付是 skill 文件。
- `feature`：读 [功能循环](references/40-feature-loop.md)。以用户可观察的行为和验收场景为准，按需修改多个文件。
- `optimization`：读 [指标优化循环](references/50-optimization-loop.md)。先测基线，再按同一方法比较改动前后。
- `artifact`：读 [通用交付物循环](references/30-artifact-loop.md)。先确定具体文件类型及其专用工具/skill，再选择有效验证。

## 记录与边界

可在 `runs/<session_id>/` 保存 `intent.md`、`brief.md`、每轮产物与评测、`final.md`；若会污染目标仓库，可用任务临时目录。维护可恢复的 `best_so_far`，失败时不要默认交最后一轮；选择历史最佳版后要实际恢复或说明无法安全恢复。不得为了过关删改用户硬性条件；每轮修改均受原任务授权和工具权限约束，尤其是发布、发送、部署与其他外部写入。
