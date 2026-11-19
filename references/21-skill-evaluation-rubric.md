# 评测量表（Skill 模式）

仅当 `iterate_mode: skill` 时使用。先按 [02-round-protocol.md](02-round-protocol.md) 完成 **Expected vs Actual**（对照 `expected_checklist`），再打分。必须实际阅读改后的文件；有试跑则结合试跑证据。

## 五维分数

### score_goal_fit — 目标契合（0–5）

| 分 | 含义 |
|----|------|
| 5 | `goal` / `done_when` 全部被规则覆盖 |
| 4 | 主目标覆盖，仅次要场景未写清 |
| 3 | 部分覆盖，关键能力缺口明显 |
| 2 | 只沾边 |
| 0–1 | 与目标无关或方向相反 |

### score_clarity — 清晰可发现（0–5）

| 分 | 含义 |
|----|------|
| 5 | `description` 含 WHAT+WHEN；术语统一；结构好扫读 |
| 4 | 整体清楚，有 1 处含糊 |
| 3 | 能用但触发词弱或章节跳跃 |
| 2 | 难判断何时该用、步骤含混 |
| 0–1 | 无法作为 skill 使用 |

### score_executability — 可执行性（0–5）

| 分 | 含义 |
|----|------|
| 5 | 步骤可直接执行；模式分流、读写顺序、工具调用无歧义 |
| 4 | 可执行，偶有需常识补全处 |
| 3 | 关键步骤缺约束，执行会漂移 |
| 2 | 多处不可执行或互相打架 |
| 0–1 | 无法落地 |

### score_consistency — 内部一致（0–5）

| 分 | 含义 |
|----|------|
| 5 | 入口与 references 一致；无死链；与 `lock` 不冲突 |
| 4 | 基本一致，有轻微重复或可合并处 |
| 3 | 入口与详规矛盾，或路径/术语不一致 |
| 2 | 多处冲突 |
| 0–1 | 结构崩溃 |

### score_smoke — 试跑表现（0–5 或 n/a）

无 `smoke_cases` 时标 `n/a`，不参与达标。

| 分 | 含义 |
|----|------|
| 5 | 全部用例符合期望现象 |
| 4 | 主用例通过，次要用例外有可接受偏差 |
| 3 | 主用例部分失败 |
| 2 | 主用例大多失败 |
| 0–1 | 试跑证明规则失效 |

## 硬伤 hard_fail（任一即 fail）

- 清单硬性项 `unmet` 或 `done_when` 未满足
- 破坏 `lock`（资产路径、客户硬约束等）
- 上轮已 met 的规则项本轮回归
- `description` 缺失或无法发现/触发
- 入口要读的 reference 不存在或矛盾到无法执行
- 与宿主协议或安全边界冲突的错误指令
- `acceptance` 任一条未满足
- 有 `smoke_cases` 时主用例结果与 `goal` 相反

## 达标判定

```
pass  ⟺  hard_fail 为空
      ∧  清单硬性项全部 met
      ∧  score_goal_fit ≥ 4
      ∧  score_clarity ≥ 4
      ∧  score_executability ≥ 4
      ∧  score_consistency ≥ 4
      ∧  （若有 smoke_cases：score_smoke ≥ 4）
      ∧  done_when 与 acceptance 满足
```

同步更新 `best_so_far`（见 02）。

## 评测写法要求

- 先表后分：无 Expected vs Actual 表不得给 verdict
- 低分项引用**文件路径 + 章节/原文要点**
- 试跑失败写清：用例、现象、对应 E# / 规则缺口
- 禁止未读文件即宣称达标
