# xty —— 八字解盘 AI 系统

## 仓库定位

这个仓库是一个**可复用的八字解盘 AI 系统**，不是命理档案管理系统。

- 核心资产是 `methodology/`（通用知识）和 `prompts/`（AI 指令模板），它们决定整个系统的解盘质量。
- `charts/` 里的命主数据是**回归测试集 / 验证素材**——用来检验 methodology 和 prompts 的迭代效果，不是档案本体。
- 命主特定的解盘产出（`dayun_overview.md`、流年文档）是某次跑测的快照，可以重跑、可以丢弃。

> **心智模型**：methodology + prompts 是产品，charts 是测试用例。

## 目录结构

```
xty/
├── README.md                # 本文件
│
├── methodology/             # 知识层：可复用的概念、推理逻辑、查询数据
│   ├── README.md
│   ├── basics.md            # 概念解释和推理逻辑（语义层）
│   ├── lookup_tables.md     # 结构化硬数据（藏干、十神矩阵、刑冲合等）
│   └── sanyuan_jiuyun.md    # 三元九运
│
├── prompts/                 # 指令层：自包含的 AI 解盘指令
│   ├── README.md
│   ├── full_analysis.md     # 完整解盘
│   ├── yearly_focus.md      # 流年聚焦
│   └── event_calibration.md # 已发生事件校验
│
└── charts/                  # 数据层：命主档案与解盘产出（回归测试集）
    ├── README.md
    └── chart_xxx/           # 编号化命名，不使用真名
        ├── profile.md
        ├── static_chart.json
        ├── dayun_overview.md
        └── liunian/
```

## 三层职责

| 层 | 内容 | 变动频率 | 谁读 |
|---|---|---|---|
| `methodology/` | 知识与推理规则 | 演进式修订 | 人 + AI |
| `prompts/` | 任务级完整指令 | 跟随任务定义 | AI（必读） |
| `charts/` | 命主数据与产出 | 一人一份，可重跑 | AI（输入参考）+ 人（验证） |

**约束**：`charts/` 里的内容不反向污染 `methodology/`。命主特定的发现，验证后升级为通用规则才能写进 `methodology/`。

## 工作流

一次完整解盘按以下顺序触发：

1. **排盘**：使用专业软件完成排盘，输出落地到 `charts/{chart_id}/static_chart.json`。**本仓库不让 AI 排盘**——AI 排盘运算量大、准确度不可控。
2. **AI 解盘**：调用 `prompts/full_analysis.md`，AI 按 prompt 内置的工作流：
   - 读取 `prompts/full_analysis.md`（指令）
   - 按需引用 `methodology/lookup_tables.md`（查表）
   - 按需引用 `methodology/basics.md`（推理逻辑）
   - 读取 `charts/{chart_id}/static_chart.json`（命主数据）
   - 读取 `charts/{chart_id}/profile.md`（已发生事件，用于校验）
3. **产出**：AI 输出落地到 `charts/{chart_id}/dayun_overview.md` 或 `liunian/*.md`，文件头部带 frontmatter（详见 `prompts/README.md`）。
4. **验证**：人工对照 `profile.md` 中的已发生事件，更新命中度。

## 关键约束

1. **不让 AI 排盘**：所有命盘必须由专业排盘软件完成。AI 只做解读。
2. **职责边界**：硬数据归 `methodology/lookup_tables.md`；语义/推理归 `methodology/basics.md`；任务流程和输出格式归 `prompts/{task}.md`。
3. **命主命名**：`charts/` 下使用编号或化名（如 `chart_001`），真实姓名不进入路径。
4. **frontmatter**：所有 AI 产出文件头部必须带 frontmatter，至少包含 `prompt`、`model`、`generated_at` 三个字段（详细规范见 `prompts/README.md`）。

## 现状与迁移路线

当前仓库已有以下历史文件，**骨架阶段保持不动**，后续按计划迁移：

| 当前位置 | 目标位置 | 处理方式 |
|---|---|---|
| `bazi/bazi_basics.md` | `methodology/basics.md` + `methodology/sanyuan_jiuyun.md` | 拆分迁移 |
| `bazi/xierui/static_chart.json` | `charts/chart_001/static_chart.json` | 改名迁移 |
| `bazi/xierui/dayun_overview.md` | `charts/chart_001/dayun_overview.md` | 改名 + 加 frontmatter |
| `bazi/xierui/dayun_*_liunian.md` | `charts/chart_001/liunian/dayun_NN_*.md` | 改名（编号前缀）+ 加 frontmatter |
| `reference/shuzi_zhuyun.md` | 待定 | 不在本架构范围内，单独决策 |

迁移按以下 PR 顺序推进：

- **PR-1（本次）**：建立目录骨架与 4 份 README，不动现有文件。
- **PR-2**：迁移 `bazi/xierui/` → `charts/chart_001/`，新增 `profile.md`，给现有产出补 frontmatter。
- **PR-3**：拆分 `bazi/bazi_basics.md` → `methodology/basics.md` + `methodology/sanyuan_jiuyun.md`，移除其中的硬数据。
- **PR-4**：新建 `methodology/lookup_tables.md`（藏干、十神矩阵、刑冲合等结构化数据）。
- **PR-5**：新建 `prompts/full_analysis.md` 等任务指令。

## 远期路线（暂不实施，留作记录）

- **回归测试机制**：methodology / prompts 修订后在所有 chart 上重跑，对比新旧产出差异。
- **methodology 演化日志**：`methodology/calibration_log.md` 记录哪条规则在哪些命主上验证或失败。
- **流年文档拆分策略**：单步大运文档超过阈值（如 30KB）时按年拆分。
- **多 LLM 适配**：当前 prompt 默认 Markdown 风格；切换其他模型时再考虑提示工程差异。
- **schema 校验与自动化**：为 `static_chart.json` 定义 JSON Schema，CI 跑校验。
