# SankeyMATIC UI and Troubleshooting

## Official references

Use these official pages first:
- Manual home / entering data / diagram size: https://sankeymatic.com/manual/
- Labels & Units: https://sankeymatic.com/manual/labels-units.html
- Colors: https://sankeymatic.com/manual/colors.html
- Exporting: https://sankeymatic.com/manual/exporting-publishing.html
- Syntax: https://sankeymatic.com/manual/syntax.html
- Imbalances: https://sankeymatic.com/manual/imbalances.html
- Scaling diagrams for comparison: https://sankeymatic.com/manual/scaling.html
- Miscellaneous / reverse graph: https://sankeymatic.com/manual/misc.html
- Build interface: https://sankeymatic.com/build/

Exact control names can change. **For every operational recommendation, setting path, export instruction, or troubleshooting step, verify the relevant current official page first, even if the control is already described below.** This file is a workflow reference, not a substitute for the live official check. Do not answer from memory.

## Mandatory official-check sequence before any UI advice

Use this order every time the user asks how to operate SankeyMATIC, where a setting lives, what value to choose, how to export, or how to fix a rendering problem:

1. Open the current official SankeyMATIC Build page: https://sankeymatic.com/build/ .
2. Open the relevant official Manual page for the feature.
3. Confirm the live control name and hierarchy.
4. If the Manual and Build page differ, use the **current Build interface** for the breadcrumb because that is what the user must click.
5. Only then give workflow-specific recommended values. Clearly distinguish official capability/path from recommendation.

If official pages are unavailable, do not fabricate an exact breadcrumb. Say verification is unavailable and ask for a screenshot if an exact path is needed.

## How to explain any UI setting

Always provide the complete path from a root section using the current verified Build wording. Current examples verified from the official Build page include:
- `Labels → Number Format → Suffix` → enter `%`.
- `Labels → Sizes → Base Size`.
- `Flows → Default Flow Colors → each flow's Source` when using the UI's source-based coloring.
- `Flows → Opacity` → then give a recommended value only after confirming the control exists.
- `Diagram Size & Background → Width / Height / Margins`.

Never say only “change Base Size,” and never reuse an old breadcrumb simply because an older Manual page still uses it.


## Largest-to-smallest node ordering

Official SankeyMATIC guidance confirms that the Build page has `Arrange the diagram` with `Automatically` and `Using the exact input order`, and the Manual's miscellaneous section states that exact-input-order mode makes nodes appear in the same order they are listed in the data. The official Manual also says nodes can be dragged for precise repositioning.

When the user requests descending order:
1. Calculate Source totals and sort Sources descending before generating code.
2. Calculate Target totals and sort named Targets descending; keep `Others` last by default unless the user requests strict numeric ordering including `Others`.
3. Write the first Source's flows in the desired Target order, then repeat the same Target order for each remaining Source.
4. After verifying the current live Build page, instruct: `Inputs → Arrange the diagram → Using the exact input order`.
5. If the graph topology prevents perfect ordering from first appearances, do not invent fake flows. After an official check, tell the user to drag the affected node(s) to the final position.

Official sources:
- https://sankeymatic.com/build/
- https://sankeymatic.com/manual/misc.html

## Preserve descending input order

The workflow default is to preserve raw flows and rank Sources and named Targets by their computed totals from largest to smallest before code generation. Do not aggregate merely to simplify the chart. If the user explicitly requested aggregation, rank the post-aggregation nodes. Keep an existing or explicitly requested Target-side `Others` last unless the user asks otherwise.

When the user asks how to make SankeyMATIC display that ranking, follow the mandatory official-check sequence first. If the current Build interface still exposes an exact-input-order arrangement option, instruct the user to enable it using the exact current breadcrumb. Do not rely on a saved breadcrumb in this file because the Build UI can change.

Do not add zero-value or fake flows merely to control visual ordering.

## First-time setup

1. Go to the SankeyMATIC Build page.
2. Open `Inputs`.
3. Paste the complete code block.
4. Click `Preview`.
5. Verify the diagram has only the intended stages.

Official syntax is `Source [Number] Target` and node declarations can use `:Node #Color`, with `>>` or `<<` to make flows inherit a node's color.

## Percentage node labels

Use percentage values in the Inputs themselves when the user wants percentage node totals.

Then, after verifying the live Build page, use the current controls:
- `Labels → Number Format → Prefix` → leave blank.
- `Labels → Number Format → Suffix` → `%`.
- `Labels → Show Values` → ON.

The older official Labels & Units Manual describes Prefix/Suffix under `Labels & Units → Units`; treat that as historical/manual terminology when it conflicts with the live Build UI. SankeyMATIC adds the flows at each node, so a source label and target label display the sum of their connected percentage flows.

## Labels: size, font, and readability

The official manual confirms label font size, color, face, and weight are configurable. The current Build interface may expose more nested controls than the manual text.

When the user needs to change them, give the exact live breadcrumb, for example if present:
- `Labels → Sizes → Base Size`.
- `Labels → Sizes → Magnify → Names`.
- `Labels → Sizes → Magnify → Values`.
- `Labels → Sizes → Line Spacing`.
- `Labels → Font`.
- `Labels → Show Names / Show Values → Style`.

If those labels differ in the current interface, verify the Build page first and use the current names.

Start with:
- Base size: 13–15 for a small-multiple PPT.
- Name/value magnification: 100%.
- Font face: sans-serif.
- Weight: Normal.
- Increase line spacing only when the browser rendering itself is too tight.

Do not keep changing SankeyMATIC label spacing when the SVG looks correct in a browser but breaks only after insertion into PowerPoint; that indicates an Office SVG rendering problem.

## Colors

First use the Source and Target main colors chosen by the user. If the user has not chosen colors, use the documented fallback defaults in `SKILL.md`. Do not present those fallback colors as SankeyMATIC official colors.

For source-colored flows:
- Code source nodes as `:Country #HEX >>`.
- Code target nodes as `:Country #HEX` without `<<`.

Global UI fallback:
- `Flows → Default Flow Colors → each flow's Source`.
- `Flows → Opacity` → `0.45–0.55`; start at `0.50` as a workflow recommendation.

Official behavior: flows are more transparent than nodes by default, so the same HEX appears lighter in the flow.

## Canvas size and margins

The manual advises adjusting Diagram Width and Height to fit the communication goal.

For four Sankeys in one horizontal 16:9 slide, keep every Sankey at the same canvas size. A practical starting point is roughly:
- Width: 300–320 px.
- Height: 360–390 px.

If the current UI exposes margins as `Margins: Left / Right / Top / Bot`:
- Start at 5–10 px each.
- Increase left/right to 10–15 px if labels are long.
- Keep margins identical across the four charts whenever possible.

Do not merge four independent small-multiple Sankeys into one Build diagram just to place them side-by-side. Generate four separate charts and align them in PowerPoint.

## Compare flow thickness across multiple charts

Use the official `Layout Options → Diagram Scale` readout when true cross-chart thickness comparison matters. Make the reported amount-per-pixel scale match across diagrams.

If each diagram is normalized to 100% and all layout settings/canvas heights are identical, they already share a natural common total basis, but verify Diagram Scale when rigorous visual comparison is required.

## Export PNG for PowerPoint

SankeyMATIC officially supports multiple PNG resolutions. The default export scale is 2x, and the Export controls expose higher 4x and 6x options.

For PowerPoint:
1. Find the `Export` controls above the diagram.
2. Expand the additional size options if necessary (`+ more...` in the documented interface).
3. Choose the high-resolution 6x/Huge option for final delivery when file size is acceptable.
4. Export `.PNG image`.

For a 310 × 380 canvas, 6x yields approximately 1860 × 2280 pixels.

Use PNG when:
- PowerPoint misrenders SVG text.
- The chart will be displayed, not edited as vector text.
- High-resolution raster output is acceptable.

## Export SVG

Use SVG when the user needs vector scaling and PowerPoint renders it correctly.

If SVG is visually correct in a browser but left-side source labels overlap or the name and `%` collide only in PowerPoint, diagnose this as an Office SVG text-rendering compatibility issue, especially when right-side target labels remain correct.

Troubleshooting order:
1. Verify by moving the first-stage labels to the opposite side in SankeyMATIC. If the problem disappears, the issue is label anchoring/rendering rather than data.
2. If that placement violates the desired layout, prefer high-resolution PNG for PowerPoint.
3. If vector output is mandatory, convert SVG text to vector outlines using a vector editor. Figma's web app can export SVG with outlined text; Illustrator or Inkscape can also convert text to paths if available.
4. Do not repeatedly adjust line spacing when only PowerPoint is broken and the browser SVG is correct.

## Hide parts of the diagram for external editing

Official export guidance:
- Labels only: set `Nodes → Opacity` to `0.0`, `Nodes → Border` to `0`, and `Flows → Opacity` to `0.0`.
- No labels: in the current Build UI, turn both `Labels → Show Names` and `Labels → Show Values` OFF (verify live UI first).
- Nodes only: turn label display off and set `Flows → Opacity` to 0.
- Flows only: turn label display off, set `Nodes → Opacity` to 0, and `Nodes → Border` to 0.

Use these only if the user wants to reconstruct labels or layers in PowerPoint/another editor.

## Common symptom → action map

### Too much empty white space
- Reduce `Margins` first.
- Then adjust Diagram Width/Height.
- Keep all small-multiple charts on the same settings.

### Flows all look too similar
- This is usually a data issue, not a styling issue.
- Make values more differentiated or use a more realistic distribution.
- Do not fake thickness by independently scaling individual flows.

### Country colors too similar
- Reassign shades using larger lightness steps within the same hue family.
- Preserve the global country-color map across all charts.

### Flow color is blue but should be green/source-colored
- Check that source declarations include `>>`.
- Check that target declarations do not include `<<`.
- Check `Colors → Flow Colors` if no explicit inheritance is in the code.

### Two `Others` nodes collapse into one
- SankeyMATIC uses node name as identity.
- Use visually identical but technically distinct strings (`Others` and `Others‌` with U+200C) and keep the exact hidden character when pasting.

### Percent labels are wrong
- Confirm the flow values in that diagram sum to 100.
- Verify the live Build UI, then confirm `Labels → Number Format → Suffix` is `%`.
- Do not merely add `%` to absolute values.
