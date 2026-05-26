# xty —— 八字解盘 AI 系统

## 仓库定位

这个仓库是一个 **Claude Skill**：`bazi-analysis`。

- 入口文件是 `SKILL.md`（Claude 加载时读它）；本 README 是**开发者文档**
- 核心资产：`methodology/`（通用知识）+ `prompts/`（任务级指令模板）
- `charts/` 是回归测试集，验证 methodology / prompts 的迭代效果
- 本仓库只做"解盘"，不做"排盘"——排盘由专业软件完成

> **心智模型**：methodology + prompts 是产品，charts 是测试用例。

## 目录结构

```
xty/
├── SKILL.md                 # Claude Skill 入口（含 frontmatter）
├── README.md                # 开发者文档（本文件）
│
├── methodology/             # 知识层：可复用的概念、推理逻辑、查询数据
│   ├── README.md
│   ├── basics.md            # 概念解释和推理逻辑（语义层）
│   ├── lookup_tables.md     # 结构化硬数据（藏干、十神矩阵、刑冲合等）
│   ├── sanyuan_jiuyun.md    # 三元九运
│   └── shuzi_zhuyun.md      # 数字助运
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
| `methodology/` | 知识与推理规则 | 演进式修订 | 人 + Claude |
| `prompts/` | 任务级完整指令 | 跟随任务定义 | Claude（必读） |
| `charts/` | 命主数据与产出 | 一人一份，可重跑 | Claude（输入参考）+ 人（验证） |

**约束**：`charts/` 里的内容不反向污染 `methodology/`。命主特定的发现，验证后升级为通用规则才能写进 `methodology/`。

## 入口分工

| 文件 | 给谁 | 职责 |
|---|---|---|
| `SKILL.md` | Claude（加载到 context） | 触发条件、工作流、知识资源引用、治幻觉约束 |
| `README.md` | 开发者 / 你自己 | 项目说明、目录、迁移历史、远期路线 |

Claude 解盘时只看 `SKILL.md` + 它引用的文件；开发者维护时看 README。两者职责不重叠。

## 下一步推进路线

（已完成的不再列出，详见 git history）

- 新建 `prompts/full_analysis.md` 等任务指令。

## 远期路线（暂不实施，留作记录）

- **回归测试机制**：methodology / prompts 修订后在所有 chart 上重跑，对比新旧产出差异。
- **methodology 演化日志**：`methodology/calibration_log.md` 记录哪条规则在哪些命主上验证或失败。
- **流年文档拆分策略**：单步大运文档超过阈值（如 30KB）时按年拆分。
- **schema 校验与自动化**：为 `static_chart.json` 定义 JSON Schema，CI 跑校验。
