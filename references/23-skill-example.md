# Skill 迭代示例（虚构）

## Intent

- iterate_mode: skill / confidence: high
- evidence: 要补 description 与验收，试跑只作验证
- not_the_other_because: 主交付是改 skill 文件

## Brief（节选）

- skill_path: `../shiyun-duotone-skill-kfc`
- goal: 明确双色触发；补「蓝色必须可见」；A/B 分流清晰
- done_when: description 含 WHAT+WHEN；加载链含蓝色验收；教室用例试跑蓝可见
- smoke_cases: S1 教室双童
- expected_checklist:
  - E1 description 含双色/教材类触发词
  - E2 入口加载含蓝色可见验收文档
  - E3 生成规则含「蓝色必须可见」必写句
  - E4 A/B 分流互斥可执行
  - E5 S1 试跑蓝可见、无明显偏灰

## Round 1

改了验收文但入口漏挂 → E2 unmet → fail  
Diagnosis: patch 补加载；补触发词；强化必写句

## Round 2

加载链闭合 + 试跑过 → 清单全 met → pass  
Final: skill 变更达标；试跑图仅附件；best_round=2
