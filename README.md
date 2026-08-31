# SankeyMATIC PPT Builder

帮助新手从需求梳理、数据整理到 SankeyMATIC 绘图与 PowerPoint 交付，生成可直接粘贴的 Inputs 代码，并提供适合放入 PPT 的导出与排错建议。

Guides beginners from requirements and data preparation to SankeyMATIC diagrams and PowerPoint delivery, with paste-ready Inputs code and practical export and troubleshooting guidance.

## 当前状态 / Current status

- 版本 / Version: `v0.0`
- 阶段 / Stage: 早期原型 / Early prototype
- 案例基础 / Evidence: 目前来自一个真实工作案例 / Currently derived from one real-world use case
- 结构校验 / Structural validation: 已通过 Codex `skill-creator` 的基础结构校验 / Passed the basic Codex `skill-creator` structural validator
- 尚未完成 / Not completed: 系统化行为测试、回归测试和跨环境验证 / Systematic behavioral, regression, and cross-environment testing

这个版本用于保存当前可用的原型，不应被视为已经成熟或适用于所有 Sankey 图场景的正式版本。

This release preserves the current working prototype. It should not yet be treated as a mature solution for every Sankey-diagram workflow.

## 主要能力 / What it does

- 分阶段向新手收集数据、版式、颜色和输出要求，避免一次询问过多参数。
- 生成可直接粘贴到 SankeyMATIC 的 `Source [Value] Target` Inputs 代码。
- 支持百分比归一化、节点排序、Top N + Others 和多图一致配色。
- 为一页 PPT 中的多个 Sankey 小图保持可比较的尺寸和视觉规则。
- 在给出界面操作路径前核对当前 SankeyMATIC 官方界面或文档。
- 针对 SVG、PNG、标签、布局和 PowerPoint 兼容问题提供排错路径。

English summary:

- Collects data, layout, color, and output requirements in beginner-friendly stages.
- Produces paste-ready `Source [Value] Target` input for SankeyMATIC.
- Supports percentage normalization, ordering, Top N + Others, and consistent colors across multiple diagrams.
- Helps keep Sankey small multiples visually comparable on a PowerPoint slide.
- Verifies current official SankeyMATIC controls before giving UI instructions.
- Provides troubleshooting guidance for SVG, PNG, labels, layout, and PowerPoint compatibility.

## 使用方式 / Usage

将 `sankeymatic-ppt-builder` 文件夹安装到兼容 Agent 的 Skills 目录，然后明确调用该 Skill。示例：

```text
$sankeymatic-ppt-builder
我有一组国家到目的地的流向数据，请帮我生成四张配色一致、适合放在同一页 PPT 中的 Sankey 图。
```

Install the `sankeymatic-ppt-builder` folder in a compatible agent's Skills directory, then invoke it explicitly. Example:

```text
$sankeymatic-ppt-builder
Turn these country-to-destination flows into four consistently colored Sankey diagrams for one PowerPoint slide.
```

## 仓库结构 / Repository structure

```text
sankeymatic-ppt-builder/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── code-patterns.md
    ├── interaction-workflow.md
    ├── sankeymatic-ui.md
    └── source-provenance.md
```

- `SKILL.md`: Skill 的入口、行为规则和输出约定。
- `references/interaction-workflow.md`: 分阶段对话和决策流程。
- `references/sankeymatic-ui.md`: 界面、导出与常见问题处理建议。
- `references/code-patterns.md`: SankeyMATIC Inputs 代码模式。
- `references/source-provenance.md`: 官方事实、PowerPoint 兼容建议与工作流经验之间的来源边界。

## 限制与安全 / Limitations and safety

- 使用者仍需核对原始数据、计算结果、标签和来源说明。
- Skill 不是 SankeyMATIC 官方产品，也不代表 SankeyMATIC 官方建议。
- SankeyMATIC 界面可能变化，因此具体菜单路径应在使用时重新核实。
- 将图表用于正式汇报前，应检查 PowerPoint 中的字体、标签、清晰度和压缩效果。
- 仓库目前未声明开源许可证；公开可见不等于自动授予复制、修改或再发布权利。

- Users must still verify source data, calculations, labels, and attribution.
- This Skill is not an official SankeyMATIC product or endorsement.
- SankeyMATIC's interface may change, so menu paths should be rechecked when used.
- Check fonts, labels, clarity, and compression in PowerPoint before formal delivery.
- No open-source license is currently declared; public visibility alone does not grant reuse, modification, or redistribution rights.
