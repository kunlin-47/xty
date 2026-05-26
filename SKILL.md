---
name: bazi-analysis
description: >
  中国传统四柱八字专业解盘技能。当用户提供已排好盘的八字命局信息
  （年柱、月柱、日柱、时柱）或询问八字、四柱、十神、大运、流年、
  用神、格局、命局分析等相关内容时触发。本技能不做排盘——所有
  命盘必须由用户用专业排盘软件完成后输入，本技能只负责解读。
---

# 八字解盘 Skill

## 触发后的工作流

1. **确认输入**：用户已经用专业排盘软件得到命局四柱、起运、大运序列等。如果用户还没有这些信息，先告知"请先用排盘软件得到完整命局再来"，**不要自己排盘**。

2. **根据任务读对应 prompt**：
   - 完整解盘 → 读 `prompts/full_analysis.md`
   - 流年聚焦 → 读 `prompts/yearly_focus.md`
   - 已发生事件校验 → 读 `prompts/event_calibration.md`

3. **按需引用知识资源**：
   - 概念与推理逻辑 → `methodology/basics.md`
   - 结构化硬数据（藏干、十神矩阵、刑冲合、长生十二宫等）→ `methodology/lookup_tables.md`
   - 三元九运专题 → `methodology/sanyuan_jiuyun.md`
   - 数字助运专题 → `methodology/shuzi_zhuyun.md`

4. **如有命主历史档案**：读 `charts/{chart_id}/profile.md` 拿已发生事件做命中度校验。

## 关键约束（治幻觉）

1. 涉及具体地支组合、藏干、十神归属、长生十二宫等结构化判断时，**必须查 `methodology/lookup_tables.md`**，不允许凭记忆作答。

2. 同一数字在不同体系含义不同（如"9"在后天洛书 = 火，在天干配数 = 水）。涉及数字助运时，必须按 `methodology/shuzi_zhuyun.md` 的"用神 + 洛书 + 九运"三层叠加优先级判定，不能用其他体系（包括"数字能量学"两位数磁场表）作主判断。

## 工程细节

见 `README.md`。
