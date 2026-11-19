# 产出对接（宿主工具）

本 skill 只编排循环；真正出图/改文件依赖宿主能力。按可用性选择，不编造工具名之外的 API。

## 图片模式 Produce

### 优先级

1. 用户指定了 `style_skill` → **先读并遵循该 skill**，再调用其要求的生图/编辑方式
2. 宿主提供 `GenerateImage` 或项目既有生图协议 → 按协议调用
3. 仅有编辑能力 → 以当前底图做局部/区域编辑
4. 均不可用 → 停止循环，告知用户缺少图像工具，保留 Brief 与已完成轮次

### 提示词组织（无 style_skill 时）

按序写入，避免空泛形容词堆叠：

1. 本轮 Plan 的改造点（来自 Diagnosis 或首轮 goal）
2. `target_style` 硬约束（逐条）
3. `lock` / `preserve_next`（逐条「保持…」）
4. `redraw_scope`：local / region / full 及区域描述
5. 与本轮相关的 `expected_checklist` 硬性项

### 底图策略

| 轮次 | 默认底图 | 例外 |
|------|----------|------|
| 1 | `source_image` | 用户要求从零生成且无内容保真需求 |
| ≥2 | 上轮产物 | Diagnosis 要求 `full` 重绘时可用 `source_image` 或上轮图作内容参考 |

### 禁止

- 未看 Brief / 清单就出图
- 跳过评测连续出多张「碰运气」
- 把路径写进面向模型的风格散文里导致污染（按宿主协议放置参考图字段）

## Skill 模式 Produce

### 优先级

1. 读取目标 `skill_path` 现状（及 Brief 指定的 `base_skill` 结构，若有）
2. 按 Diagnosis 的 `edit_scope` 改文件
3. 需要行为证据时再跑 `smoke_cases`

### 试跑（smoke）

- 试跑前：确认本轮规则改动已保存
- 试跑中：按用例 input 调用下游流程；记录图路径与现象到 `round-NN-smoke/`
- 试跑后：现象只用于评测；**不要**为了让试跑好看而绕过该改的规则（先改 skill，再试跑）

### 禁止

- 大面积重写无关章节冒充迭代
- 未读入口与 references 就改
- 用「提示词临场发挥」掩盖 skill 缺口却宣称 skill 已达标

## 通用交付物模式 Produce

先识别文件格式，使用其专用工具或 skill 制作；验证方式见 [30-artifact-loop.md](30-artifact-loop.md)。不能把文件写入成功视为验收通过。保留上一轮可回退版本，并遵守原任务的外部写入权限。

## 功能模式 Produce

先检查项目结构、相关实现、现有验收标准和测试入口。按 [40-feature-loop.md](40-feature-loop.md)实现最小可用改动并在每轮运行对应验证。不要用新增的、只复述实现的测试冒充功能通过；无法运行时记录原因与未验证项。

## 指标优化模式 Produce

先取得可复现的基线，再做一次可归因的改动，使用同一测量方法复测目标和护栏指标。详细比较与停止规则见 [50-optimization-loop.md](50-optimization-loop.md)。
