# charts —— 数据层（回归测试集）

## 角色

存放**命主档案与解盘产出**。本质上是**回归测试集**——用来检验 `methodology/` 和 `prompts/` 的迭代效果。

不是命理档案管理系统：命主特定的解盘产出（`dayun_overview.md`、流年文档）是某次跑测的快照，可以重跑、可以丢弃、可以重写。真正沉淀的是 methodology 和 prompts。

## 命主索引

| chart_id | 内部代号 | 性别 | 出生年 | 日主 | 关键特征 | 已验证事件数 |
|---|---|---|---|---|---|---|
| chart_001 | xierui | 待补 | 待补 | 待补 | 待补 | 待补 |

## 命名规范

### 命主目录命名

- 使用 **`chart_xxx`** 编号，如 `chart_001`、`chart_002`
- **不使用真实姓名**作为路径
- 内部代号（用于沟通时叫名字）记录在该命主的 `yan_qian_shi.md` 头部

### 命主目录结构

```
chart_xxx/
├── yan_qian_shi.md             # 命主验前事（基本信息 + 反向推断的高能量事件 + 校验对话）
├── static_chart.json           # 软件排盘原始数据
├── dayun_overview.md           # 大运总览（AI 产出）
└── liunian/                    # 流年细化（AI 产出）
    └── dayun_xxx_liunian.md    # 一步大运对应一份流年细化文档
```

### 流年文件命名

格式：`dayun_{大运干支}_liunian.md`

示例：
- `dayun_bingwu_liunian.md` —— 丙午大运的流年细化
- `dayun_yisi_liunian.md` —— 乙巳大运的流年细化

## yan_qian_shi.md 规范

每个命主目录必须有 `yan_qian_shi.md`（验前事），它有三个职责：

1. **基本信息 + 边界声明**：命主代号、性别、出生年、日主、当前时间锚、覆盖年份范围、所跨大运段
2. **验前事推断**：基于命局结构反向推断的若干"高能量年"事件，按 [`prompts/yan_qian_shi.md`](../prompts/yan_qian_shi.md) 的规则产出
3. **校验对话 + 测试用例价值**：命主可在文档内逐条标注命中 / 部分命中 / 未命中；同时记录该命主在测试集中的价值（结构触发密度、覆盖大运段、特殊性等）

### Markdown 建议结构

```markdown
# chart_xxx · 验前事

> 内部代号：xxx
> 推演基础：static_chart.json + dayun_overview.md

## 0. 边界声明
（当前时间锚 / 覆盖年份范围 / 所跨大运段）

## 1. 高能量年候选清单
（按 prompts/yan_qian_shi.md 约束 2 的 8 项门槛筛选的候选表）

## 2. 验前事推断（逐年）
（4-8 年事件层面推断，每年附结构触发 / 命理链条 / 推断事件 / 可证伪点）

## 3. 整体形态
（覆盖年份的主线形态提炼）

## 4. 校验对话
（命中 / 部分命中 / 未命中标注表格）

## 5. 测试用例价值
（这个命主为什么入测试集？）
```

## AI 产出文件

`dayun_overview.md` 和 `liunian/*.md` 是 AI 跑出来的产出。这些产出可重跑、可丢弃，真正沉淀的是 `methodology/` 和 `prompts/`。

## 测试集使用方式

charts 是回归测试集，典型用法：

1. **新增命主入测试集**：选有代表性的命局（如某种格局、某种偏向）+ 已发生事件密度高的命主，建立 `chart_xxx/` 目录，填好 `yan_qian_shi.md` 和 `static_chart.json`。
2. **首次解盘**：跑 `prompts/full_analysis.md`，产出落到该命主目录。
3. **methodology / prompts 修订后回归**：在选定的若干 chart 上重跑，对比新旧 `dayun_overview.md` 的差异。
4. **更新事件命中度**：人工核对 AI 产出 vs `yan_qian_shi.md` 的验前事标注，必要时回流到 methodology 修订。

## 不做什么

- **不存 PII**：`yan_qian_shi.md` 不写真实姓名、身份证号、具体地址、联系方式。
- **不反向修改 methodology**：命主特定的发现，必须验证有普适性后才升级到 methodology。
- **不存历史版本**：AI 重跑后旧产出直接覆盖，不在文件名里加 `_v2`、`_v3`，依赖 git history。
