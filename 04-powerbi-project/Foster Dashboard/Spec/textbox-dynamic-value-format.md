# Textbox Dynamic Value — Correct PBIR Format

How to wire a `textbox` visual's dynamic (measure-bound) text runs so Power BI
Desktop treats them as real, click-to-edit values — not just correctly
*rendered* text. Confirmed by comparing a hand-authored value against one
Desktop generated natively (via the "+Value" → natural-language flow) for
the same measure.

## The problem this fixes

A `textbox` dynamic value has two independent things that can each be
right or wrong:

1. **Does it render the correct live data?** — controlled by the DAX
   expression tree in `objects.values[].properties.expr.expr`.
2. **Does Desktop's UI recognize it as an existing bound value?** — i.e.
   clicking the value in the canvas opens "this is bound to `<field>`,
   click to change it" instead of the blank "Create a dynamic value..."
   panel.

A **bare `Measure` expression** satisfies (1) but not (2):

```json
// ❌ Renders correctly, but Desktop's click-to-edit UI won't recognize it
"expr": { "Measure": { "Expression": { "SourceRef": { "Entity": "KPI" } }, "Property": "Historical Fostered Animals" } }
```

Desktop never writes a bare `Measure` expression into a textbox `values`
entry — even for a plain, already-existing measure with no aggregation
involved, it always wraps it in a `Min` aggregation over a **subquery**.
Matching that exact shape is what satisfies (2).

## The correct shape (confirmed for measures)

```json
{
  "properties": {
    "expr": {
      "expr": {
        "Min": {
          "Expression": {
            "Column": {
              "Expression": {
                "Subquery": {
                  "Query": {
                    "Version": 2,
                    "From": [
                      { "Name": "k", "Entity": "KPI", "Type": 0 }
                    ],
                    "Select": [
                      {
                        "Measure": {
                          "Expression": { "SourceRef": { "Source": "k" } },
                          "Property": "Historical Fostered Animals"
                        },
                        "Name": "KPI.Historical Fostered Animals"
                      }
                    ]
                  }
                }
              },
              "Property": "KPI.Historical Fostered Animals"
            }
          },
          "IncludeAllTypes": 1
        },
        "Annotations": {
          "NaturalLanguage": {
            "version": 1,
            "kind": "NaturalLanguage",
            "annotation": { "name": "Value", "utterance": "historical fostered animals" }
          }
        }
      }
    }
  },
  "selector": { "id": "Value" }
}
```

### The grammar, field by field

| Field | Rule |
|---|---|
| `Min` | Always `Min` as the outer aggregator, regardless of the measure's own aggregation type or data type (text, date, number all use `Min`). It's a wrapper convention, not a real aggregation choice. |
| `Column.Expression.Subquery.Query` | A full mini query object, not a direct field reference. `Version: 2` always. |
| `Query.From[0]` | `{ "Name": "<alias>", "Entity": "<TableName>", "Type": 0 }`. Alias is arbitrary (Desktop used `"k"` for `KPI` — short, lowercase, first letter(s) of the table); pick a short lowercase alias per table. |
| `Query.Select[0].Measure` | The actual field reference — `Expression.SourceRef.Source` points at the `From` alias (not `Entity` — this is a subquery-scoped alias reference, different from the `Entity` form used elsewhere in PBIR). |
| `Query.Select[0].Name` | Must be exactly `"<TableName>.<MeasureName>"` — this is also what `Column.Property` (outside the subquery) must match. |
| `Column.Property` | Same string as `Query.Select[0].Name` above — the subquery's output column is referenced back by this name. |
| `IncludeAllTypes` | Always `1`. |
| `Annotations.NaturalLanguage.annotation.name` | Must equal the `selector.id` used for this value (e.g. `"Value"`, `"Value 2"`). |
| `Annotations.NaturalLanguage.annotation.utterance` | The human-readable phrase describing the value — Desktop fills this with whatever the user typed into "How would you calculate this value". When authoring by hand, use a plain lowercase description of the measure (e.g. `"historical fostered animals"`), not a `Table.Measure` identifier. |

### Selector `id` naming convention

Desktop names the first dynamic value in a textbox **`Value`** (no number),
then `Value 2`, `Value 3`, `Value 4`... for subsequent ones **within that
same visual**. Match this so hand-authored visuals are indistinguishable
from native ones:

- 1st value in a textbox → `"Value"`
- 2nd → `"Value 2"`
- 3rd → `"Value 3"`, etc.

(Earlier work in this report used `"Value 1"`, `"Value 2"` — functionally
fine, since the id is only referenced by the matching `textRuns[].value.selector`
in the same visual, but new textboxes should follow Desktop's own
`Value` / `Value 2` / `Value 3` pattern.)

## Referencing the value from a paragraph run

Unchanged from the general dynamic-textbox mechanism — a `textRuns[].value`
object pointing at the `values` entry by selector id:

```json
{
  "value": {
    "propertyIdentifier": { "objectName": "values", "propertyName": "expr" },
    "selector": { "id": "Value" }
  },
  "textStyle": { "fontFamily": "Segoe UI Semibold", "fontSize": "22px", "color": "#4F46E5" }
}
```

## Scope of this confirmation

Verified for a **measure** reference. A dynamic value bound to a plain
**column** (not a measure) has not been confirmed against a Desktop-native
example yet — the older, simpler `Min(Column)` pattern in
`powerbi-report-authoring`'s `textbox.md` (aggregating a raw column
directly, no subquery) may still be correct for that case, or it may also
need the subquery wrapper. Verify against a Desktop-generated example
before assuming either way, the same method used to confirm this doc:
create the value via Desktop's own "+Value" UI, inspect the resulting
`visual.json`, then match its shape exactly.

## When to use this vs. a plain static run

Only wrap a value this way when it must be **live** — i.e. it should move
when the model refreshes (a measure's current value, a dynamic label). A
plain caption/heading (e.g. a KPI card's title text) should stay a normal
string `textRuns[].value`, per [visual-naming-convention.md](visual-naming-convention.md)'s
`(Live)` suffix rule — that suffix exists specifically to flag which runs
use this dynamic-value mechanism.
