# Visual Naming & Grouping Convention

Guideline for naming and organizing visuals in the Selection pane, so any
visual can be identified at a glance instead of showing generic labels like
"Text box", "Shape", or "Button". Applies to every page built in
`Foster Dashboard.Report` going forward.

## Why this exists

By default, PBIR visuals show up in the Selection pane named after their
visual type ("Text box", "Shape", "Button"), repeated once per instance with
no indication of which section or card they belong to. Once a page has more
than a handful of visuals this becomes unusable for review or edits. The fix
is two PBIR mechanisms, confirmed by inspecting a manually-renamed/grouped
visual in this report:

1. **Per-visual rename** — set `visualContainerObjects.title.text` to a
   descriptive label. This drives the Selection-pane name even when
   `title.show` is left off/false, so it never has to appear on the canvas.

   ```json
   "title": [{
     "properties": {
       "text": { "expr": { "Literal": { "Value": "'Executive Briefing — Narrative Card'" } } }
     }
   }]
   ```

2. **Grouping** — a Selection-pane group is its own visual-container entry
   with a `visualGroup` object instead of a `visual` object; member visuals
   get a `parentGroupName` pointing at the group's `name`, and their
   `position` becomes relative to the group's origin instead of the page.

   ```json
   // group container's visual.json
   { "name": "<20hexId>", "visualGroup": { "displayName": "Executive Briefing", "groupMode": "ScaleMode" } }

   // member visual.json
   { "name": "<20hexId>", "parentGroupName": "<groupContainerId>", "position": { "x": 0, "y": 0, ... }, "visual": { ... } }
   ```

## Rule 0 — Group at creation time, not as a retrofit

When asked to build a new visual (a KPI card, a chart, a badge — anything),
place it into its section's group **as part of that same task**, not as a
follow-up cleanup pass:

1. **Section already has a group on this page** → add the new visual as a
   member: give it `parentGroupName` set to the group container's `name`,
   and position it **relative to the group's origin** (subtract the group
   container's absolute `x`/`y` from the visual's intended absolute page
   position). If the new visual's bounds fall outside the group container's
   current `position` box, grow the group container's `height`/`width` to
   re-enclose all members.
2. **Section has no group yet on this page** → create the group container
   first, sized to bound the new visual(s), then add the visual(s) as
   members. Confirmed schema (from an existing group container in this
   report):

   ```json
   // group container — its own visual.json, NO "visual" key
   {
     "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.12.0/schema.json",
     "name": "<20hexId>",
     "position": { "x": 40, "y": 366, "z": 12000, "height": 120, "width": 1840, "tabOrder": 12 },
     "visualGroup": { "displayName": "KPI Scorecards", "groupMode": "ScaleMode" }
   }
   ```

   ```json
   // member visual.json — position is relative to the group's (x, y) above
   {
     "name": "<20hexId>",
     "parentGroupName": "<groupContainerId>",
     "position": { "x": 0, "y": 0, "z": 0, "height": 120, "width": 352, "tabOrder": 0 },
     "visual": { "...": "..." }
   }
   ```

3. **Naming a KPI card specifically** (the recurring case): "build me KPI N"
   always means: create/reuse the `KPI Scorecards` group per the Rule 1
   table, name the card `<N> — <Content>` per Rule 2, and set its position
   relative to that group. Never leave a new KPI card as a bare page-level
   visual outside the group.

This makes grouping a zero-cost side effect of normal building instead of a
separate task someone has to remember to ask for.

## Rule 1 — One group per logical section

Every named section of a page (as defined by the mock / page spec, not by
visual type) gets one Selection-pane group. Do not nest groups within
groups unless a section itself has clearly separable sub-cards that are also
independently useful to hide/move as a unit.

Group `displayName` = the section name as it appears in the page spec /
mock, Title Case, no abbreviations:

| Page | Section | Group name |
|---|---|---|
| Director Summary | Header bar + top nav | `Header & Navigation` |
| Director Summary | Executive briefing narrative + status | `Executive Briefing` |
| Director Summary | 5 KPI scorecards | `KPI Scorecards` |
| Director Summary | Q1 growth trend card | `Q1 — Growth Trend` |
| Director Summary | Q2 seasonality card | `Q2 — Seasonality` |
| Director Summary | Q3 recent trend card | `Q3 — Recent Trend` |
| Director Summary | Q4 forecast card | `Q4 — Forecast Outlook` |
| Director Summary | Key findings footer | `Key Findings & Conclusion` |

Use the same pattern (`<Section Name>` as it reads in that page's spec doc)
for Growth Analysis, Operations Planning, and Methodology once built.

## Rule 2 — Every visual gets a descriptive `title.text` name

Inside a group, name each member by **role**, not by visual type. The group
already supplies the section context in the Selection-pane tree, so don't
repeat the section name inside the child name unless the child would be
ambiguous without it (e.g. multiple KPI cards sitting outside any group).

Pattern: `<Role>` (short, Title Case, 2–5 words). For elements that repeat
in a numbered sequence (KPI cards, nav buttons, table columns), prefix with
the number so Selection-pane order and reading order match:

| Element | Name pattern | Example |
|---|---|---|
| Card / textbox container | `<Content> Card` | `Narrative Card`, `Status Card` |
| Static label text | `<Label> Label` | `Program Status Label` |
| Dynamic (measure-bound) value | `<Measure Purpose> (Live)` | `Program Status Value (Live)` |
| Decorative badge/pill background | `<Badge> Badge BG` | `Eyebrow Badge BG`, `Period Badge BG` |
| Badge text (static) | `<Badge> Badge Label` | `Eyebrow Badge Label` |
| Badge text (dynamic) | `<Badge> Badge Label (Live)` | `Period Badge Label (Live)` |
| Background/divider shape | `<Purpose> Background` | `Header Bar Background` |
| Numbered repeating card | `<N> — <Content>` | `1 — Historical Foster Animals`, `2 — Program Growth` |
| Nav button | `Nav — <Page Name>` | `Nav — Director Summary`, `Nav — Growth Analysis` |
| Chart | `<Question> Chart` | `Q1 Trend Chart` |

Mark any text run bound to a DAX measure (via the dynamic `values` /
`propertyIdentifier` mechanism) with the `(Live)` suffix — it's the fastest
way to tell, at a glance in the Selection pane, which text will move when
the model refreshes versus which is hand-typed and static.

## Rule 3 — Practical limits

- Keep names under ~40 characters — the Selection pane truncates long names,
  and a truncated name defeats the point.
- Use an em dash (`—`) as the only separator inside a name (never a colon or
  slash) — matches the pattern already used in this report's page titles.
- Numbers for repeating elements are always Arabic numerals with no leading
  zero (`1`, `2`, `3` — not `01`, `First`).
- Never rely on `title.show: true` just to get a name — keep the title
  hidden unless the mock actually calls for an on-canvas title.

## Retrofitting existing visuals

One-time backlog only — everything built before Rule 0 existed. As of this
writing that's the Director Summary page: visuals are named (done) but not
yet grouped into `Header & Navigation`, `Executive Briefing` (only 2 of its
5 members are currently grouped), and `KPI Scorecards`. Do this as one
reviewable batch, since regrouping changes every affected visual's
`position` to group-relative coordinates. Every visual built *after* this
backlog pass is grouped inline per Rule 0 — there should never be a second
retrofit needed.

## Reference worked example (Executive Briefing, once retrofitted)

```
▾ Executive Briefing                         (group)
    Narrative Card                            (textbox: title + Executive Summary/Forecast Narrative (Live))
    Status Card                               (textbox: Program Status (Live) + Program Status Description (Live))
    Eyebrow Badge BG                          (shape)
    Eyebrow Badge Label                       (shape text, static)
    Period Badge BG                           (shape)
    Period Badge Label (Live)                 (textbox, dynamic)
```
