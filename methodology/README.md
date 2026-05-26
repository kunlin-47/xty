# methodology —— 知识层

## 角色

存放**可复用、跨任务共享**的八字命理知识。被 `prompts/` 按需引用，被人作为学习/查阅资料。

methodology 是仓库的**核心资产之一**，演化要谨慎、要有积累——所有 prompts 和 charts 都建立在它之上。

## 文件清单

| 文件 | 性质 | 主要受众 |
|---|---|---|
| `basics.md` | 概念解释 + 推理逻辑（散文式） | 人为主，AI 也可读 |
| `lookup_tables.md` | 结构化硬数据（表格/矩阵） | AI 必查，人参考 |
| `sanyuan_jiuyun.md` | 三元九运专题 | 人 + AI |
| `zhuyun_overview.md` | 助运总论（三层叠加通用框架） | 人 + AI |
| `shuzi_zhuyun.md` | 数字助运（标杆样例） | 人 + AI |
| `yanse_zhuyun.md` | 颜色助运 | 人 + AI |
| `fangwei_zhuyun.md` | 方位助运（不含贵人方位） | 人 + AI |

> 助运系列以 `zhuyun_overview.md` 为入口，各专题文件应用同一套"用神 + 五行映射 + 时空加权"三层框架。新增助运专题（贵人方位 / 材质 / 饮食 / 时辰等）应保持同样命名与结构。

## 关键边界规则

为避免内容重复和职责混乱，**严格执行以下分工**：

### 规则 1：硬数据归 `lookup_tables.md`

凡是**结构化、可查表、有确定答案**的内容，全部放在 `lookup_tables.md`。包括但不限于：

- 十天干 / 十二地支基础属性表（阴阳、五行、所属时辰）
- 地支藏干（本气/中气/余气）
- 十神推导矩阵（生我/同我/我生/我克/克我 × 阴阳同异 → 偏/正）
- 地支刑冲合害（六冲、三刑、六合、三合、半合、六害）
- 长生十二宫表
- 其他可枚举的对照表

### 规则 2：语义和推理逻辑归 `basics.md`

凡是**需要解释、判断、权衡**的内容，放在 `basics.md`。包括但不限于：

- 概念定义（日主、月令、用神、格局等）
- 五行 / 十神在现实中的含义和翻译
- 身强身弱判断的逻辑
- 用神判断的多步推理
- 合冲刑害的"剧烈程度"和"现实体感"
- 十神的现实角色（同辈、合伙人、规则、伴侣等）

### 规则 3：跨主题专题独立成文

如三元九运（`sanyuan_jiuyun.md`）这种相对独立、自成体系的内容，单独成文。未来如有类似主题（如神煞专题、调候用神专题），同样独立。

**助运系列特殊约定**：所有助运维度（数字 / 颜色 / 方位 / 贵人 / 材质…）共享 `zhuyun_overview.md` 中的"三层叠加框架"，各维度专题以 `zhuyun_overview.md` 为入口，文件命名为 `{维度拼音}_zhuyun.md`（数字 → `shuzi_zhuyun.md`，颜色 → `yanse_zhuyun.md`，方位 → `fangwei_zhuyun.md`）。新增助运专题应保持同一命名与结构。硬映射表（五行 ↔ 颜色 / 方位 / 数字）仍归 `lookup_tables.md`。

### 规则 4：禁止排盘公式

本仓库的工作流是"软件排盘 + AI 解读"，所以**不收录**：

- 年上起月法、日上起时法
- 起运公式（出生日至节气天数 ÷ 3）
- 节气换月规则的具体计算

这些信息由排盘软件负责，AI 不需要自己算。

## 演化机制

methodology 的修订要**保守 + 可追溯**：

1. **修订动机要明确**：每次 commit message 必须说明"为什么改"——是修正错误、补充缺漏、还是吸收实测发现。
2. **跨命主验证**：如果某条规则的修订是基于命主特定的发现，需要在 commit message 或 commit body 中说明在哪些 chart 上验证过。
3. **不引入流派分歧**：当传统命理多个流派对同一问题答案不一致时，选定一派作为主线，并在文末注明"本仓库采用 X 派 / X 经典作为主线"。

## 引用规范

`prompts/` 中引用 methodology 文件时，使用相对路径：

```
methodology/lookup_tables.md
methodology/basics.md
methodology/sanyuan_jiuyun.md
methodology/zhuyun_overview.md
methodology/shuzi_zhuyun.md
methodology/yanse_zhuyun.md
methodology/fangwei_zhuyun.md
```

不要在 prompts 中复制 methodology 的内容，始终通过引用，避免双份维护。
