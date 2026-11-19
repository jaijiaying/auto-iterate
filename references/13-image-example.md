# 图片迭代示例（虚构）

## Intent

- iterate_mode: image / confidence: high
- evidence: 提供输入图并要求改场景与样式直到画面过关
- not_the_other_because: 交付物是图

## Brief（节选）

- source_image: 公园双童图
- goal: 改成教室上课；保留两孩身份与座位；黑板写「春天」
- target_style: 专黑细线 + 马卡龙教材风
- lock: 两童脸型发型
- expected_checklist:
  - E1 教室场景（课桌/黑板）可见
  - E2 两童可辨且相对位置保留
  - E3 黑板「春天」清晰可读
  - E4 描边为专黑细线、配色马卡龙
  - E5 脸型发型相对输入可辨（lock）

## Round 1

Produce → `round-01.png`

| id | status | actual |
|----|--------|--------|
| E1 | met | 有课桌黑板 |
| E2 | met | 两童座位关系保留 |
| E3 | unmet | 板书乱码 |
| E4 | met | 线色基本符合 |
| E5 | met | 脸型可辨 |

verdict: fail（E3 hard_fail）；手部粘连 → quality 3  
Diagnosis: P0 重绘板书；P1 局部修手；scope=local

## Round 2

局部修 → E3/质量过关 → 清单全 met → pass  
Final: `round-02.png`，best_round=2
