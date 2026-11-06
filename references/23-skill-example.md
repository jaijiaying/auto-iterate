# Skill 迭代示例（虚构）

## Intent

```markdown
## Intent
- iterate_mode: skill
- confidence: high
- evidence: 「把双色 skill 的 description 和验收规则补到能稳定触发，试跑教室场景要过」
- not_the_other_because: 主交付是改 SKILL/references；试跑图只作验证
```

## 用户输入

- `skill_path`: `../shiyun-duotone-skill-kfc`
- `goal`: 明确双色触发词；补「蓝色必须可见」验收；模式 A/B 分流不歧义
- `done_when`: description 含 WHAT+WHEN；验收含蓝色可见硬伤；用 1 个教室用例试跑无明显偏色
- `smoke_cases`: [教室双童上课，期望双色平涂且蓝可见]
- `max_rounds`: 5

## Round 1

**Plan** → 重写 description；在视觉边界 reference 加蓝色可见 hard 条；入口挂验收

**Produce** → 改 `SKILL.md` + `references/03-...md`；changelog 写入 `round-01-changelog.md`；试跑 1 张

```markdown
## Round 1 Evaluation
- score_goal_fit: 4
- score_clarity: 3
- score_executability: 4
- score_consistency: 3
- score_smoke: 3
- hard_fail: ["入口未要求读取含蓝色验收的 reference"]
- verdict: fail
- summary: 规则写了蓝色，但入口加载路径漏挂；试跑仍偏灰。
```

```markdown
## Round 1 Diagnosis
1. [P0] 入口漏载验收文档 → SKILL 规则加载节无 03 → 补加载并发读
2. [P1] description 触发词偏少 → 缺「双色教材/K80」→ 补 WHEN 关键词
3. [P1] 试跑偏灰 → 提示未强制蓝可见 → 在生成规则加必写句
- preserve_next: 已有模式 A/B 定义、资产表
- edit_scope: patch
```

## Round 2

**Produce** → 补入口加载与 description；强化生成必写句 → 再试跑

```markdown
## Round 2 Evaluation
- score_goal_fit: 5
- score_clarity: 4
- score_executability: 4
- score_consistency: 5
- score_smoke: 4
- hard_fail: []
- verdict: pass
- summary: 加载链闭合，试跑蓝可见，done_when 满足。
```

**Final**：skill 已达标；主交付为文件变更；试跑图仅作附件；共 2 轮
