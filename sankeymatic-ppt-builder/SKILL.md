---
name: sankeymatic-ppt-builder
description: Guide a novice user from Sankey diagram requirements to paste-ready SankeyMATIC Inputs code and PPT-ready output. Use when the user wants to create, restyle, troubleshoot, or export SankeyMATIC diagrams; build multiple comparable Sankey small-multiples; normalize flows to percentages; assign consistent country colors; generate Source-Value-Target data; or fix label, layout, SVG, PNG, and PowerPoint issues. Ask for the missing design and data parameters in a staged conversation, then provide code that can be copied directly into SankeyMATIC. When the user is dissatisfied with the rendering or asks where a setting lives, verify and follow current official SankeyMATIC documentation and give full menu breadcrumbs from the root section.
---

# SankeyMATIC PPT Builder

## Purpose

Guide a user who may know nothing about SankeyMATIC from requirements to a usable chart. Keep the interaction practical: collect only the missing information, generate paste-ready code, then coach the user through SankeyMATIC settings with exact navigation paths.

Read these references when needed:
- `references/interaction-workflow.md` for the staged conversation and decision logic.
- `references/sankeymatic-ui.md` whenever the user asks how to change a setting, export, troubleshoot, or fix an unsatisfactory diagram.
- `references/code-patterns.md` when generating or validating SankeyMATIC Inputs code.
- `references/source-provenance.md` when distinguishing official SankeyMATIC behavior from PowerPoint compatibility guidance and workflow/design recommendations.

## Core behavior

1. Treat the user as a beginner unless they demonstrate otherwise.
2. Do not dump every possible setting at once. Ask for requirements in small batches following `references/interaction-workflow.md`.
3. Do not re-ask information already supplied in the conversation.
4. When enough information is known, generate the chart code immediately instead of continuing to interview the user.
5. Make every generated Inputs block directly copyable into SankeyMATIC.
6. When the user asks only for code, output only the requested code blocks plus minimal labels such as `Sankey 1`.
7. **Official-doc-first is mandatory for every operational answer.** Before telling the user how to operate SankeyMATIC, where a setting is located, what parameter to change, how to export, how to troubleshoot, or before proactively giving any UI/operation recommendation, first verify the relevant control or behavior against current official SankeyMATIC documentation. Open the official Build interface and/or the relevant official Manual page before composing the answer. Do not answer from memory, prior conversation, screenshots alone, or bundled reference files without this current official check. Prefer `sankeymatic.com` official pages; use the official SankeyMATIC GitHub repository only as a secondary source.
8. After the official check, consult `references/sankeymatic-ui.md` for this workflow's tested recommendations. If the official Manual and the current official Build interface use different control names or hierarchy, treat the current Build interface as authoritative for the breadcrumb and note the discrepancy only if it helps the user.
9. Always explain UI changes as a full breadcrumb from the root, for example: `Labels → Sizes → Base Size`, not merely “change Base Size.” Never invent a breadcrumb.
10. If current official pages cannot be accessed, say that the current UI path could not be verified and avoid presenting an exact menu path as fact. Ask the user for a screenshot or provide only the behavior that is already confirmed by the bundled official references.

## Data rules

- Basic flow syntax: `Source [Value] Target`.
- Values must be positive integers or decimals and must not contain commas or unit symbols inside brackets.
- For a two-stage chart with no middle nodes, every flow must go directly from a left/source node to a right/target node. Never introduce an intermediate node unless the user explicitly requests one.
- If the user wants percentage labels, normalize each Sankey independently so the total of all flow values in that Sankey equals 100. Before giving the UI path, verify the current official Build interface. As of the current verified Build UI, use `Labels → Number Format → Suffix` and enter `%`; the older official Manual describes this as `Labels & Units → Units → Suffix`, so never reuse the older breadcrumb without checking the live Build page first.
- If the user provides absolute data but wants percentages, calculate each flow as `flow / total flow × 100`. Preserve the relative widths.
- If the user asks for random demo data, create noticeably different flow sizes, not near-equal values. For percentage diagrams, make the generated flows sum exactly to 100 per diagram.
- If the user requests `Others`, use the visible label exactly as `Others`; never add words such as `Africa`, `Destinations`, `Countries`, or similar unless the user asks.

## Preserve raw flows; default ordering and ranking

Preserve the user's original flow structure by default. **Do not merge, collapse, group, bucket, or reclassify small Sources, Targets, or flows unless the user explicitly asks for aggregation/grouping** (for example, “Top 5 targets + Others”, “merge Hong Kong into China”, or “put small destinations into Others”). A small value by itself is never a reason to alter the raw flow structure.

Apply descending ordering by default unless the user explicitly asks to preserve the supplied order or requests another ranking rule. Sorting changes only the presentation order; it must not change node identities, flow values, or the original source-to-target relationships.

1. Start from the raw user-supplied flows. If the user did **not** request aggregation, keep every original Source, Target, and flow exactly as supplied.
2. If the user **did** explicitly request an aggregation/grouping rule, apply only that requested transformation first, then rank the resulting nodes.
3. Compute each Source node total as the sum of all outgoing flows and sort Source nodes from largest to smallest.
4. Compute each Target node total as the sum of all incoming flows and sort named Target nodes from largest to smallest.
5. If an `Others` node already exists in the supplied data or was explicitly requested/created by the user, keep Target-side `Others` last by default, even when its total is larger than a named Target. If the user explicitly asks for strict numeric order including `Others`, sort it numerically instead.
6. If the user explicitly asks for “Top N targets + Others”, rank destinations by total incoming value across all Sources, keep the Top N named Targets, aggregate every remaining destination into `Others`, then recompute totals and apply the ordering rules above. Never apply this Top-N reduction automatically.
7. Within each Source, emit its flow lines in the same global Target order so the code reflects one consistent target ranking.
8. For percentage charts, validate the final flows and ensure the whole Sankey sums to exactly 100 before sending code. If aggregation was requested, validate after aggregation as well.
9. Never create fake or zero-value flows solely to force node positions.

When giving the user UI instructions to preserve this code order in SankeyMATIC, first perform the mandatory live official-doc check. If the current official Build interface still provides an exact-input-order option, instruct the user to enable it using the current verified breadcrumb. Do not hard-code a stale path without verification.

For two-stage charts, design the flow-line sequence so first appearances of Target nodes follow the global Target ranking whenever possible: list the largest Source first; within that Source, list flows in global Target order; then list remaining Sources in descending Source-total order, always using the same Target order.

## Country color system

Build a global color mapping across all diagrams before writing any code so a repeated country keeps the same color in every chart in the same role.

### Ask for colors before applying a palette

Do not assume that Source must be green or Target must be blue for every user. During requirements collection, ask the user for:
- Source-side main color.
- Target-side main color.
- Preferably the exact RGB values, e.g. `RGB(60,95,102)`, or HEX values, e.g. `#3C5F66`.
- Whether the assistant should derive multiple shades from each main color. Default: yes.

If the user gives RGB, convert it to HEX for SankeyMATIC code. If the user gives HEX, preserve it and optionally report the equivalent RGB when useful.

### Fallback defaults when the user gives no color preference

Use these user-approved defaults only as a fallback, not as a SankeyMATIC official standard:
- Source/flow-out main color: RGB(60,95,102), HEX `#3C5F66`.
- Target/flow-to main color: RGB(112,127,167), HEX `#707FA7`.
- Source countries use a green-gray family derived from the Source main color.
- Target countries use a blue family derived from the Target main color.
- Use visibly separated shades within each family; avoid tiny RGB differences that become indistinguishable in PowerPoint.
- Keep `Others` on each side in the lightest shade of that side's family unless the user requests otherwise.

Fallback source palette, darkest to lightest:
`#1E353A`, `#2C4A50`, `#3C5F66`, `#5F7F85`, `#86A1A5`, `#B5C8CB`.

Fallback target palette, darkest to lightest:
`#3F4A70`, `#56658F`, `#707FA7`, `#8F9BC0`, `#B1B9D1`, `#D2D7E5`.

When deriving a custom palette from user-supplied main colors, preserve the hue family while changing lightness enough to make adjacent countries clearly distinguishable. Keep the main color as one of the middle/darker shades rather than making every shade a tiny variation around it.

If a country repeats across multiple Sankeys, assign its shade once and reuse it everywhere. If the same country appears as a source in one chart and a target in another, ask which rule takes priority: country identity or source/target role. Do not silently violate both requirements.

## Flow color rule

When flows should inherit the Source-side palette while Target nodes keep their own palette, use the user-selected colors. Example using the fallback colors:

```text
:SourceCountry #3C5F66 >>
:TargetCountry #707FA7
SourceCountry [42] TargetCountry
```

Use `>>` on each source node declaration. Do not add `<<` to target nodes unless the user explicitly wants flows to inherit target colors. Never assume green/blue if the user supplied a different palette.

Recommend `Flow Opacity` around `0.45–0.55`; use `0.50` as the default for a clean PPT style unless the user's chart is unusually dense or sparse.

## Duplicate visible names on opposite sides

SankeyMATIC treats identical node names as the same node. If the user requires a visible `Others` on both source and target sides of the same diagram, use two technically distinct strings while keeping the visible label identical. Preferred pattern:

- Source: `Others`
- Target: `Others‌` where the target version contains an invisible U+200C ZERO WIDTH NON-JOINER after the word.

Keep that exact invisible character in every target declaration and flow. Warn the user not to retype that target label manually. If the invisible character is stripped by their browser/editor, explain that SankeyMATIC has no display-alias mechanism and ask whether a visibly distinct target label is acceptable.

## Multiple Sankeys on one PowerPoint slide

For small-multiple layouts such as four Sankeys in one row:

- Generate each Sankey separately rather than merging all four into one SankeyMATIC diagram unless the user explicitly wants a single connected network.
- Use identical canvas dimensions and comparable layout parameters for all diagrams.
- If the user wants flow thicknesses comparable across diagrams, use the official Diagram Scale guidance; see `references/sankeymatic-ui.md`.
- If every chart is normalized to 100%, the total height basis is naturally comparable when other layout settings are held constant.

## Output contract for code generation

For each Sankey:

1. Put source node color declarations first.
2. Put target node color declarations second.
3. Add a blank line.
4. Put direct flow lines after the color declarations.
5. Keep the code syntactically minimal and directly pasteable.
6. Never include markdown commentary inside the code block except optional `//` comments that the user specifically wants.
7. Validate totals before sending. For percentage charts, explicitly verify the sum equals 100.
8. Validate that no middle node exists when the user requested a two-stage chart.
9. Validate that repeated countries use the same assigned color across all code blocks.

## Optional Excel handoff

If the user requests an Excel source file, create one worksheet per Sankey and use exactly these columns in this order:

`Source | Value | Target`

If percentage mode is requested, store percentage values in `Value`. If the user also wants original absolute values preserved, ask whether to add a separate raw-data workbook/sheet rather than silently adding extra columns.

## Troubleshooting and export

When the user reports a problem, do not guess. **First open and verify the relevant current official SankeyMATIC documentation or Build interface. Only after that** use `references/sankeymatic-ui.md` for workflow-specific recommendations. Give a verified root-to-control path and a specific value/range to try.

Separate provenance clearly: SankeyMATIC syntax/UI capabilities come from official SankeyMATIC documentation; PowerPoint rendering/compression behavior should be grounded in Microsoft documentation when available; palette choices, slide dimensions, opacity starting points, and PNG-vs-SVG workflow preferences are workflow recommendations. Never describe a workflow recommendation as an official SankeyMATIC requirement. See `references/source-provenance.md`.

For PowerPoint delivery:
- Prefer high-resolution PNG when SVG text renders incorrectly in PowerPoint.
- Prefer SVG when the user needs true vector output and the labels render correctly.
- If SVG looks correct in a browser but left-side labels overlap only in PowerPoint, treat it as an Office SVG text-rendering issue rather than a flow-data problem. Offer the tested options in `references/sankeymatic-ui.md` in order of least effort.

