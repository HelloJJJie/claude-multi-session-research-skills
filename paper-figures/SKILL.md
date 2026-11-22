---
name: paper-figures
description: Make or check a data figure for a paper. Decide what the figure must argue before choosing a chart. Render at the final printed size with SciencePlots' IEEE style. Encode groups redundantly so the figure survives grayscale. State in the caption what every band, bar and line is. Close the loop by reading the rendered PNG before export. Use when writing a figure script for a paper, when asked to "画图 / 出图 / 论文配图 / make the figure", or when reviewing a draft's figures (paper-review §4). Not for diagrams, flowcharts or interactive dashboards.
---

# Paper figures

A figure is a claim. The reader sees it before the text and remembers it longer, so every
mark has to be traceable to data, the way the numbers in the text are. Load `paper-review`
alongside this when you are checking someone else's figure.

## 0. Before plotting: the argument and the data

1. **Write down the one sentence the figure must make the reader believe.** If you have two
   sentences, make two panels or two figures. If you cannot write one, the figure is not
   ready.
2. **Look at the data it plots**: n per group, how skewed it is, outliers, the range.
   The chart type follows from these facts, not from habit.
3. **Read the figure's numbers from the result files the paper cites**, never from a copy
   or a summary. The script names its input files and asserts what it expects (row count,
   excluded groups). Evidence: a figure script that asserted `len(rows)==288` and "no
   excluded corpus in any row" made a reviewer's denominator check a one-line read.
4. **An example must be placed in its distribution before anyone sees it.** When a figure
   shows one unit (a recording, a speaker, a window) as an illustration, report to the user
   where that unit sits: the rank or percentile of the effect it shows among all units of
   its kind. Choose it by a stated rule, and say in the caption when the panel is an
   excerpt. Evidence: an illustration chosen by a positional rule happened to show the
   largest error and the strongest correction in its recording; the writer checked only
   when the user asked whether it was representative.

## 1. Choosing the chart

| data | use | do not use |
|---|---|---|
| a category plus a value, n < 10 per group | strip / dot plot of every point | mean bar |
| a category plus a value, n ≥ 10 | box or violin with the points overlaid | a bare mean bar |
| two continuous variables | scatter, with ρ or r and n in the text | a line connecting unordered points |
| an ordered x (k, time, dose) against a value | line with its interval as a band | bars |
| a summary over units that differ a lot | mean **plus** the spread over units (min–max band or the points) | the mean alone |
| a matrix | heat map with a labelled colour bar, viridis or a diverging map | rainbow/jet, 3-D |
| shares of a whole | sorted horizontal bars | pie |

- Never use two y-axes for different quantities: the apparent agreement is set by how you
  scale them. Use two panels with a shared x-axis.
- A truncated y-axis on a proportion needs a stated baseline (chance, or 0) or a visible
  break.
- **A mean over heterogeneous units hides the unit that matters.** Evidence: a panel
  plotting a method's mean cost over 19 sources showed "no cost from k = 8", while 7 of the
  19 still lost and the worst lost 0.166. The fix was a worst-to-best band.

## 2. Style: SciencePlots, at the final size

```python
import matplotlib; matplotlib.use("Agg")
import matplotlib.pyplot as plt, scienceplots   # pip: SciencePlots (pin the version)
plt.style.use(["science", "ieee", "no-latex"])  # no-latex: no LaTeX install needed
plt.rcParams.update({"font.serif": ["Times New Roman", "TeX Gyre Termes", "Times"],
                     "pdf.fonttype": 42, "ps.fonttype": 42})   # editable, embedded fonts
fig, ax = plt.subplots(figsize=(3.39, 2.4))     # final width in inches; see table
```

| venue | one column | two columns | text in the figure |
|---|---|---|---|
| IEEE / ICASSP / Interspeech (spconf) | 3.39–3.5 in | 7.16 in | 8–9 pt; never below 6 pt |
| Nature family | 3.5 in (89 mm) | 7.2 in (183 mm) | 5–7 pt |

- **Set `figsize` to the printed size and never scale the figure in LaTeX** (always
  `width=\columnwidth` for a figure made at column width). Scaling changes every font size.
- The `ieee` style cycles black/red/blue/green **with** solid/dashed/dotted/dash-dot line
  styles, so lines stay distinguishable in grayscale. Keep that pairing. For scatter plots,
  vary the marker too (circle vs. square), not only the colour.
- Match the paper's body font (Times-like for IEEE).
- **LaTeX text rendering:** with a TeX installation (`latex`, `dvipng`, `type1cm`,
  `type1ec`), use `["science", "ieee"]`. Labels and maths are then typeset by LaTeX and
  match the paper. Without LaTeX, use `"no-latex"`, otherwise drawing fails with "latex
  could not be found".
  - Put the TeX binaries on `PATH` inside the script's environment, e.g. prepend
    `/Library/TeX/texbin` on macOS. A shell started before the install does not have it.
  - With `usetex` on, every label is LaTeX: escape `%` as `\%`, `_` as `\_`, `&` as `\&`.
    An unescaped `%` comments out the rest of the label without any error, so read the
    rendered PNG.
  - One figure script uses one mode for all its figures. Do not mix LaTeX-rendered and
    mathtext figures in one paper.
- Vector output: PDF for the paper, plus a PNG at 200–300 dpi for checking. Never JPEG.
- A colour that encodes a value gets a labelled colour bar. The same group has the same
  colour and marker in every panel and every figure of the paper.

### Colour by role, and keep groups apart without colour

- **Assign colour by role, not by order.** The paper's own method gets one saturated
  anchor hue (e.g. deep blue `#0F4D92`). Compared methods get a second, contrasting family.
  Baselines, reference regions and bands get neutral greys (`#CFCECE`, `#767676`). Use at
  most one highlight colour per figure, for a single call-out. Never separate two groups by
  red vs. green alone: colour-blind readers cannot tell them apart.
- **Ordered variants of one method get lightness steps of one hue.** Ablations ("add one
  component at a time") and increasing k run from light to dark, darkest = most complete.
  A different hue per variant hides the order.
- **Bars get a black edge (0.5–1 pt at column width) and, where fills are close, a hatch**
  (`/`, `\\`, `.`). The `ieee` line/marker cycling does not apply to bars, so without
  edges and hatches, bars with similar fills merge in grayscale.
- **Keep the legend off the data.** In a multi-panel figure use one shared legend beside or
  above the panels, or a dedicated panel with `ax.set_axis_off()` that holds only the
  legend. Evidence: moving a figure's legend from above the axes to its side recovered
  vertical space and stopped it crowding the top markers.
- **Print values on bars only when there are few bars**, and only when the script reads
  them from the same result file the text cites. A hand-typed label is a second copy of the
  number, and it goes stale the same way a copied number in the text does.

## 3. The caption carries the statistics

Write the caption in the form the venue's exemplars use (`paper-writing` §3). In speech
venues that is usually a descriptive phrase naming what is shown, followed by the reading
keys; the takeaway belongs in the text, and the table's row order tells the story (the
reference first, the proposed method last and set apart). Anything readable directly from
the figure or table is not repeated. When the caption names what every colour and line
style means, drop the legend from the figure.

A figure that introduces a phenomenon can follow the overview-plus-zoom layout: one panel
over a stretch of data with the instants of interest boxed, and each boxed instant enlarged
below.

The caption, not only the text, must say:
- what each band, bar or whisker is (95% CI over *what unit*, SD, min–max over *which* units);
- n, or the number of cells, speakers or sources behind each mark;
- every reference line (chance, criterion, threshold) and where its value comes from;
- that a panel is exploratory or was added after the run, when it was.

Evidence: a post-hoc panel was labelled "added after the run" in the body text only. A
reader who looks at the figure first never sees the label.

Report every bin you binned, or say why you chose the ones you show. Evidence: a text
reported three of five bins, and an omitted bin was the one showing the effect vanishing at
the criterion.

## 4. Close the loop: read the rendered figure

A script that ran without error has not been checked. Before export:

1. Render a PNG at the final size (150–200 dpi) and **open it with Read**.
2. Go through the list item by item. "Looks fine" is not a check.
   - missing glyphs or boxes (minus sign, ±, Δ, Greek letters, CJK);
   - text clipped at the edges; tick labels colliding;
   - legend or annotations covering data;
   - panel labels (a)(b) aligned and in one style; panels not overlapping;
   - groups still separable in grayscale (`img.convert("L")` and look again);
   - no data, error bar or extreme point cut off by the axis limits;
   - the same variable in the same colour, marker and scale across panels.
3. Fix, re-render, re-read. Export the vector file only after a clean pass.
4. Re-run the figure script whenever its input export changes. A stale figure next to
   updated text is the figure version of a stale number.

## Provenance

Adapted on 2026-11-11 from `Haojae/scipilot-figure-skill` (MIT, commit 43098dd). From it
came the argument-first selection, the pitfall list, the caption rules and the
render-and-read loop. The CJK-journal material, plotly and the eight-step ceremony were
not adopted. The style is `garrettj403/SciencePlots` (MIT, v2.2.2, commit b9b1695). The
evidence lines come from figure findings in paper reviews in this skill set's home project.

Rewritten in our own words on 2026-11-14 from `ChenLiu-1996/figures4papers`
(`scientific-figure-making/`, CC BY-NC 4.0, commit 0ddfbd3). Five rules came from it: role
colours, lightness steps of one hue, bar edges and hatches, a separate legend, and on-bar
values. The two hex anchors are from its palette. That repository's rules are its author's
house style, inferred from his own scripts, not tested for readability. These were not
adopted, because they conflict with §1–2:
- sans-serif fonts (IEEE body text is Times);
- 16–24 pt text on very large canvases, scaled down in LaTeX;
- y-limits set to min − SD;
- x-tick labels hidden in favour of the legend;
- red-vs.-green method encoding.

## Maintaining this skill

Patch it in the session where a figure went out with a problem this text should have
caught (`skill-maintenance`). Project facts belong in the project layer: which export
feeds which figure, and the paper's column width.
