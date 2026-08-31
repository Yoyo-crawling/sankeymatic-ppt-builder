# SankeyMATIC Code Patterns

## Basic direct flow

```text
DRC [45] China
```

## Node color

```text
:DRC #3C5F66
:China #707FA7
```

## Source node controls outgoing flow color

```text
:DRC #3C5F66 >>
:China #707FA7
DRC [45] China
```

## Target node controls incoming flow color

Use only when requested:

```text
:China #707FA7 <<
DRC [45] China
```

## Individual flow color and opacity

```text
DRC [45] China #3C5F66.5
```

Prefer node inheritance for repeated styling because it is easier to maintain.

## Two-stage percentage example

All flow values sum to 100:

```text
:DRC #1E353A >>
:Zambia #3C5F66 >>
:South Africa #86A1A5 >>
:Others #B5C8CB >>

:China #3F4A70
:India #707FA7
:Netherlands #B1B9D1
:Others‌ #D2D7E5

DRC [30] China
DRC [8] India
DRC [4] Netherlands
DRC [2] Others‌
Zambia [18] China
Zambia [5] India
Zambia [4] Netherlands
Zambia [2] Others‌
South Africa [8] China
South Africa [6] India
South Africa [3] Netherlands
South Africa [2] Others‌
Others [3] China
Others [2] India
Others [2] Netherlands
Others [1] Others‌
```

Total = 100.

## Percentage conversion

When the user supplies absolute flows `v1...vn`, compute:

`percentage_i = vi / SUM(v1...vn) × 100`

Use enough decimals to make the total exactly 100. If rounding creates 99.9 or 100.1, adjust the largest flow by the rounding residual rather than leaving an inconsistent total.

## Cross-flow/random demo generation

When a demo should show different line thicknesses:
- Include at least one dominant flow around 20–35%.
- Include several medium flows around 7–18%.
- Include several small flows around 1–6%.
- Do not force every source to connect to every target unless the user requests a full matrix.
- Mix connections so adjacent sources do not all terminate at the same target; this produces visible crossings and variety.

## Global country-color map

First ask for Source and Target main colors. Accept RGB or HEX and convert RGB to HEX for the code. If the user does not specify colors, fall back to Source RGB(60,95,102) / `#3C5F66` and Target RGB(112,127,167) / `#707FA7`. These fallback colors are workflow defaults, not official SankeyMATIC colors.

Create one mapping before writing multiple code blocks, for example using the fallback palette:

```text
SOURCE ROLE
DRC          #1E353A
Zambia       #3C5F66
South Africa #86A1A5
Guinea       #5F7F85
Others       #B5C8CB

TARGET ROLE
China        #3F4A70
India        #707FA7
France       #8F9BC0
Netherlands  #B1B9D1
Others       #D2D7E5
```

Reuse the exact mapping in every chart.

## Default descending node order pattern

Use largest-to-smallest ordering by default unless the user explicitly requests another order. **Preserve the raw flow structure by default**: do not merge, group, bucket, or collapse small flows merely to simplify the chart. Calculate node totals from the raw flows unless the user explicitly requested an aggregation rule.

- Source rank = total outgoing flow per Source, descending.
- Target rank = total incoming flow per named Target, descending.
- Keep an existing or explicitly requested Target-side `Others` last by default.
- Only when the user explicitly asks for Top N Targets + `Others`, select the Top N by aggregated incoming value across all Sources, fold the remainder into `Others`, then rerank.
- Within each Source, emit flow lines in the same global Target order.
- Percentage charts must sum to exactly 100. If an explicit aggregation was performed, revalidate after aggregation.
- Before telling the user how to lock this order in SankeyMATIC, verify the current official Build page and use the current exact-input-order control if it still exists.

Example where Source totals rank `A > B > C` and Target totals rank `X > Y > Z > Others`:

```text
:A #123456 >>
:B #345678 >>
:C #56789A >>

:X #234567
:Y #456789
:Z #6789AB
:Others #ABCDEF

A [35] X
A [12] Y
A [8] Z
A [5] Others
B [15] X
B [8] Y
B [4] Z
B [3] Others
C [5] X
C [3] Y
C [1] Z
C [1] Others
```

Do not add zero or fake flows to force ordering. If first-appearance constraints prevent the exact Target order, use node dragging in SankeyMATIC after checking the current official guidance.

