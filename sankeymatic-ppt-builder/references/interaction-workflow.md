# Interaction Workflow

Use this as the default conversation order. Ask only for missing items and keep each round short.

## Stage 1 — Understand the deliverable

Ask:
- How many Sankey diagrams are needed?
- Is each chart two-stage only (`Source → Target`) or are middle nodes needed?
- Is the final use PowerPoint, web, print, or another destination?
- If PowerPoint, how are the charts arranged (for example, four horizontally on one 16:9 slide)?

If the user already provided these, skip this stage.

## Stage 2 — Define the nodes

Ask:
- How many source nodes per chart?
- How many target nodes per chart?
- Should either side include `Others`? Do not create or populate `Others` automatically; only do so if it already exists in the data or the user explicitly requests aggregation/grouping.
- Are country/node names supplied by the user, researched from authoritative sources, or randomly generated for a demo?

If the user asks for researched countries or current trade destinations, use web research and prefer authoritative trade databases, government statistics, company filings, or established multilateral sources. State the trade/product scope because rankings change by commodity and HS code.

## Stage 3 — Define values

Ask:
- Absolute values or percentages?
- If percentages, should each chart sum to 100%? Default: yes.
- Should values be user-supplied or generated?
- If generated, should flow widths show strong, medium, or subtle differences? Default: strong enough to be visually obvious.

Rules:
- Percentage mode: sum every flow in a chart to exactly 100.
- Random demo mode: use a few large, medium, and small flows; avoid uniform distributions.
- Preserve the raw flow structure by default. Never merge, bucket, or collapse small flows merely because they are small. Aggregate only when the user explicitly requests a rule such as “Top N + Others” or a named geographic merge.
- By default, rank Source nodes by total outgoing flow and Target countries by total incoming flow, both largest to smallest. Default to keeping an existing or explicitly requested `Others` last unless the user explicitly requests strict numeric ranking including `Others`.
- Within each Source, order flows according to the same global Target ranking.
- Only preserve the user's supplied order when the user explicitly asks to keep the original order or specifies another ordering rule.

## Stage 4 — Define color logic

Ask only what is not known. For a new user, ask color preference explicitly instead of assuming the fallback palette:
- What main color should the Source/flow-out side use? Ask for RGB or HEX if available.
- What main color should the Target/flow-to side use? Ask for RGB or HEX if available.
- Should the assistant derive several clearly separated shades from each main color? Default: yes.
- Should repeated countries keep the same color across all charts? Default: yes.
- Should flows inherit the source or target color? Default: source.

Use a compact prompt such as:
“你希望 Source 和 Target 分别用什么主色？最好给我 RGB（例如 R60 G95 B102）或 HEX；如果没有指定，我可以使用默认 Source RGB(60,95,102) / `#3C5F66`，Target RGB(112,127,167) / `#707FA7`，并自动生成同色系的深浅色。”

Fallback colors only when the user has no preference:
- Source main: RGB(60,95,102), `#3C5F66`.
- Target main: RGB(112,127,167), `#707FA7`.

These are user-approved workflow defaults, not SankeyMATIC official colors. Use visibly different shades derived from the chosen main colors and assign colors globally before generating code.

If a country changes roles across charts (source in one, target in another), ask whether country identity or role color has priority.

## Stage 5 — Generate paste-ready code

Produce one code block per Sankey. Verify:
- Node declarations use valid HEX.
- Source declarations include `>>` when flows inherit source colors.
- Targets do not include `<<` unless specifically requested.
- Flow lines are direct `Source [Value] Target` for two-stage charts.
- Totals equal 100 in percentage mode.
- `Others` is exactly `Others` visually.
- Repeated countries keep the same assigned color.
- Preserve every raw Source → Target relationship unless the user explicitly requested aggregation/grouping.
- Source declarations and flow groups follow descending Source-total order by default.
- Named Targets follow descending Target-total order by default; an existing or explicitly requested Target-side `Others` is last unless the user requests strict numeric sorting.
- Source and Target first appearances in the Inputs code follow the precomputed descending ranking by default, and flow lines within each Source follow the same Target order.

If the user wants only code, do not add tutorial prose.

## Stage 6 — Guide SankeyMATIC settings

**Before offering even the minimum setup sequence, first open the current official SankeyMATIC Build page and the relevant Manual page. Do not rely on remembered paths or this file alone.**

If this is the user's first chart, after the official check offer only the minimum setup sequence using the exact live control names. Current verified examples are:

1. `Inputs` → paste code.
2. If deterministic largest-to-smallest ordering was requested: `Inputs → Arrange the diagram → Using the exact input order`.
3. Click `Preview`.
4. For percentage values: `Labels → Number Format → Suffix` → enter `%`.
5. For UI-driven source coloring if code does not already force it: `Flows → Default Flow Colors → each flow's Source`.
6. `Flows → Opacity` → start around `0.50` as a workflow recommendation.
7. Adjust layout only if needed.
8. Export PNG or SVG according to the final-use requirements.

If the live Build page differs from these examples, follow the live Build page. Do not give every available parameter unless the user asks.

## Stage 7 — Iterate from screenshots or symptoms

If the user dislikes the result, ask for either:
- a screenshot, or
- a precise symptom such as “labels overlap,” “flows too thin,” “too much white space,” “colors too similar,” “four charts do not line up,” or “PowerPoint changes the SVG.”

Then **first verify the relevant current official SankeyMATIC Build/Manual controls**, and only afterward use `sankeymatic-ui.md` for tested workflow recommendations. Give one or two changes at a time, each with a verified full root breadcrumb and a target value/range.
