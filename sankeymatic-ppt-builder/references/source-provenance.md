# Source and Provenance Rules

Use this reference to keep official product behavior separate from workflow recommendations.

## 1. SankeyMATIC Official

Treat the following as official only when supported by current SankeyMATIC pages:
- Input syntax such as `Source [Value] Target`.
- Node color declarations such as `:Node #HEX`.
- `>>` / `<<` flow-color inheritance behavior.
- Labels, units, colors, opacity, margins, diagram size, scaling, and export controls that appear in the current official manual/build interface.

Primary sources:
- https://sankeymatic.com/manual/
- https://sankeymatic.com/manual/syntax.html
- https://sankeymatic.com/manual/colors.html
- https://sankeymatic.com/manual/labels-units.html
- https://sankeymatic.com/manual/exporting-publishing.html
- https://sankeymatic.com/manual/scaling.html
- https://sankeymatic.com/build/

**Operational-answer rule:** before any instruction, parameter recommendation, troubleshooting step, or export guidance, verify the relevant current official page first. Do this even when the path is already written in the Skill. If the official Manual and the live official Build interface differ, use the live Build interface for the current breadcrumb. If official pages cannot be accessed, do not fabricate an exact path.

## 2. Microsoft / PowerPoint Official

When discussing PowerPoint-specific behavior such as image compression, SVG rendering support, or Office image settings, prefer current Microsoft documentation. Do not attribute PowerPoint behavior to SankeyMATIC.

## 3. Workflow Recommendation

Treat the following as recommendations rather than official SankeyMATIC requirements unless an official source explicitly says otherwise:
- Source/Target palette choices.
- The fallback Source RGB(60,95,102) and Target RGB(112,127,167).
- Derived shade spacing for country differentiation.
- Flow Opacity starting point around 0.50.
- Suggested Sankey canvas sizes for four charts on a 16:9 slide.
- Preference for separate Sankeys rather than combining independent small multiples.
- Preference for high-resolution PNG when PowerPoint misrenders SVG text.

When the user asks “is this official?”, answer with the category above. Never present a workflow recommendation as an official SankeyMATIC rule.
