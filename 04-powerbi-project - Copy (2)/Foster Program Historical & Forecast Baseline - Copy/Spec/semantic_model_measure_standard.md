# Semantic Model Architecture Standard: KPI & Measure Governance

## 1. Objective & Principle
To maintain a high-trust, self-service-ready semantic model, all DAX measures must strictly separate **analytical business logic** from **report presentation/UI glue**. Developers and AI assistants must follow this specification when adding or modifying measures.

* **Signal vs. Noise:** Business users and ad-hoc analysts must only encounter decision-grade numeric KPIs at the primary folder level.
* **No Uncurated Display Junk:** Hex codes, font rules, dynamic SVG strings, and text-based visual conclusions must never sit at the root level of a business domain folder.

---

## 2. Folder Hierarchy & Naming Standard

Measures must reside in dedicated measure containers (e.g., `KPI`) structured by **Functional Business Domain**, utilizing a standardized subfolder for presentation logic.

```text
KPI
└── <Functional Domain>                  <-- Primary Domain (Business Metrics Only)
    ├── [Measure Name]                   <-- Pure numeric/analytical KPI
    ├── [Measure Name %]                 <-- Formatted ratio/variance
    └── _Formatting & Display            <-- Subfolder (Always prefixed with underscore)
        ├── [Metric] Color               <-- Conditional formatting (Hex / RGB)
        ├── [Metric] Font Color          <-- Dynamic font contrast logic
        ├── [Metric] Executive Summary   <-- Dynamic text / chart subtitle strings
        └── [Metric] KPI Icon            <-- SVG / UNICHAR display indicators
```

### Reference Implementation (Seasonal Operations Domain)

```text
KPI
└── Seasonal Operations
    ├── Monthly Foster Participation
    ├── Monthly Participation
    ├── Planning Priority
    ├── Seasonal Benchmark Variance %
    └── _Formatting & Display
        ├── Planning Priority Color
        ├── Seasonal Heatmap Font Color
        ├── Seasonality Heatmap Color
        └── Seasonality Executive Conclusion
```

---

## 3. Classification & Placement Rules

| Classification | Output Type | Target Location | `IsHidden` Rule | Examples |
| :--- | :--- | :--- | :--- | :--- |
| **Domain KPI** | Numeric (Integer, Decimal, Currency, Percentage) | `<Domain>` root folder | `False` (Visible) | `Monthly Foster Participation`, `Seasonal Benchmark Variance %` |
| **Base Measure** | Numeric (Raw aggregations) | `_Base Measures` | `True` (Hidden) | `_Count_FosterCases`, `_Sum_ActiveDays` |
| **UI Formatting** | String (Hex code, CSS, RGB) | `<Domain>\_Formatting & Display` | `True` (or Visible if needed for thin report) | `Planning Priority Color`, `Seasonality Heatmap Color` |
| **Dynamic Title / Text** | String (Text output) | `<Domain>\_Formatting & Display` | Optional (Thin report local measure preferred) | `Seasonality Executive Conclusion` |

---

## 4. DAX Definition & Property Constraints

When writing or generating DAX for measures, apply the following properties:

### 1. Business KPI (`Seasonal Benchmark Variance %`)
* **Display Folder:** `Seasonal Operations`
* **Format String:** `0.0%;-0.0%;0.0%`
* **Description:** Explicitly state the business definition, numerator, and denominator.
* **Pattern:** Always use `DIVIDE()` with safe zero handling.

```dax
Seasonal Benchmark Variance % = 
VAR _Current = [Monthly Participation]
VAR _Benchmark = [Seasonal Benchmark]
RETURN
    DIVIDE( _Current - _Benchmark, _Benchmark, BLANK() )
```

### 2. UI Color Measure (`Planning Priority Color`)
* **Display Folder:** `Seasonal Operations\_Formatting & Display`
* **Format String:** Text / Unformatted
* **Description:** Internal conditional formatting rule for heatmap and table visuals.
* **Pattern:** Return strict 6-character hex strings with `#` prefix (e.g., `"#D83B01"`, `"#107C41"`, `"#F3C11B"`).

```dax
Planning Priority Color = 
SWITCH(
    SELECTEDVALUE( 'Planning Dimension'[PriorityLevel] ),
    "Critical", "#D83B01",
    "Medium",   "#F3C11B",
    "Low",      "#107C41",
    BLANK()
)
```

### 3. Dynamic Visual Narrative (`Seasonality Executive Conclusion`)
* **Display Folder:** `Seasonal Operations\_Formatting & Display`
* **Format String:** Text / Unformatted
* **Description:** Dynamic summary block for visual subtitles and executive cards.

```dax
Seasonality Executive Conclusion = 
VAR _Variance = [Seasonal Benchmark Variance %]
RETURN
    IF(
        ISBLANK(_Variance),
        "Insufficient data to evaluate current seasonal trend.",
        "Operations are currently tracking " & 
        FORMAT(_Variance, "+0.0%;-0.0%;0.0%") & 
        " against historical seasonal benchmarks."
    )
```

---

## 5. Developer & AI Assistant Checklist

Before opening a pull request or saving changes to the model:
- [ ] Is the primary folder named after a clear business process/domain?
- [ ] Are all hex codes, font rules, and titles nested inside `_Formatting & Display`?
- [ ] Does every core metric have a defined `FormatString`?
- [ ] Are technical base measures (`SUM`, `COUNT`) hidden from end users?
- [ ] If this dynamic title is only used on one visual in one report, has it been placed in the thin report rather than the core model?