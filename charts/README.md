# charts —— 数据层（回归测试集）

## 角色

存放**命主档案与解盘产出**。本质上是**回归测试集**——用来检验 `methodology/` 和 `prompts/` 的迭代效果。

不是命理档案管理系统：命主特定的解盘产出（`dayun_overview.md`、流年文档）是某次跑测的快照，可以重跑、可以丢弃、可以重写。真正沉淀的是 methodology 和 prompts。

## 命主索引

| chart_id | 内部代号 | 性别 | 出生年 | 日主 | 关键特征 | 已验证事件数 |
|---|---|---|---|---|---|---|
| _（待迁移）_ | xierui | 女 | 1992 | 丙火 | 待补 | 待补 |

> 本骨架阶段命主目录尚未建立。`bazi/xierui/` 将在后续 PR 中迁移到 `charts/chart_001/`。

## 命名规范

### 命主目录命名

- 使用 **`chart_xxx`** 编号，如 `chart_001`、`chart_002`
- **不使用真实姓名**作为路径
- 内部代号（用于团队沟通时叫名字）记录在该命主的 `profile.md` frontmatter 的 `codename` 字段

### 命主目录结构

```
chart_xxx/
├── profile.md                  # 命主档案（基本信息 + 已发生事件 + 测试价值标注）
├── static_chart.json           # 软件排盘原始数据
├── dayun_overview.md           # 大运总览（AI 产出）
└── liunian/                    # 流年细化（AI 产出）
    ├── dayun_NN_xxx.md         # 文件名带步数前缀（NN = 01-08），便于排序
    └── ...
```

### 流年文件命名

格式：`dayun_{两位编号}_{大运干支}.md`

示例：
- `dayun_06_bingwu.md` —— 第 6 步大运（丙午）
- `dayun_07_yisi.md` —— 第 7 步大运（乙巳）

带编号前缀的好处：
- 文件按时间顺序自然排序
- 一眼看出是命主一生中的第几步大运

## profile.md 规范

每个命主目录必须有 `profile.md`，它有两个职责：

1. **基本信息**：命主代号、性别、出生年、日主、关键命局特征
2. **测试用例价值**：标注这个命主在测试集中的价值（已发生事件数、命局特殊性等）

### Frontmatter 模板

```yaml
---
chart_id: chart_001
codename: xierui                  # 内部沟通用代号
gender: female
birth_year: 1992
day_master: 丙火
key_features:
  - 伤官见官
  - 日主偏弱
  - 月令食神
verified_events: 5                # 已验证的人生事件数（测试集价值指标）
last_updated: 2026-01-15
---
```

### Markdown 正文建议结构

```markdown
# chart_001 —— 命主档案

## 基本信息
（出生信息、家庭背景、当前状态等。注意：不要写真名、身份证号等强 PII。）

## 命局速览
（四柱、日主、月令、大运起步年龄、当前所处大运等关键信息）

## 已发生事件清单
| 年份 | 年龄 | 事件 | 类别 |
|---|---|---|---|
| 2018 | 26 | xxx | 事业 |
| ... | ... | ... | ... |

## 测试用例价值
（这个命主为什么入测试集？例如：日主极弱+伤官见官的典型例、已发生事件密度高、跨多步大运可观察等）
```

## AI 产出文件规范

`dayun_overview.md` 和 `liunian/*.md` 是 AI 跑出来的产出，**必须带 frontmatter**。frontmatter 字段定义见 [`prompts/README.md`](../prompts/README.md#frontmatter-规范)。

最低要求 3 个字段：

```yaml
---
prompt: prompts/full_analysis.md
model: claude-sonnet-4.5
generated_at: 2026-01-15
---
```

## 测试集使用方式

charts 是回归测试集，典型用法：

1. **新增命主入测试集**：选有代表性的命局（如某种格局、某种偏向）+ 已发生事件密度高的命主，建立 `chart_xxx/` 目录，填好 `profile.md` 和 `static_chart.json`。
2. **首次解盘**：跑 `prompts/full_analysis.md`，产出落到该命主目录。
3. **methodology / prompts 修订后回归**：在选定的若干 chart 上重跑，对比新旧 `dayun_overview.md` 的差异。
4. **更新事件命中度**：人工核对 AI 产出 vs `profile.md` 的已发生事件，更新 `profile.md` 的 `verified_events` 字段，必要时回流到 methodology 修订。

## 不做什么

- **不存 PII**：`profile.md` 不写真实姓名、身份证号、具体地址、联系方式。
- **不反向修改 methodology**：命主特定的发现，必须验证有普适性后才升级到 methodology。
- **不存历史版本**：AI 重跑后旧产出直接覆盖，不在文件名里加 `_v2`、`_v3`，依赖 git history。
