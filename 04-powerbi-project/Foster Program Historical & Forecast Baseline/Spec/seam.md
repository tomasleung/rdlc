ere is a clean Markdown engineering specification you can copy directly into your project repo, coding assistant instructions (e.g., Cursor/Copilot rules), or developer onboarding wiki.Semantic Model Architecture Standard: KPI & Measure Governance1. Objective & PrincipleTo maintain a high-trust, self-service-ready semantic model, all DAX measures must strictly separate analytical business logic from report presentation/UI glue. Developers and AI assistants must follow this specification when adding or modifying measures.Signal vs. Noise: Business users and ad-hoc analysts must only encounter decision-grade numeric KPIs at the primary folder level.No Uncurated Display Junk: Hex codes, font rules, dynamic SVG strings, and text-based visual conclusions must never sit at the root level of a business domain folder.2. Folder Hierarchy & Naming StandardMeasures must reside in dedicated measure containers (e.g., KPI) structured by Functional Business Domain, utilizing a standardized subfolder for presentation logic.PlaintextKPI
└── <Functional Domain>                  <-- Primary Domain (Business Metrics Only)
    ├── [Measure Name]                   <-- Pure numeric/analytical KPI
    ├── [Measure Name %]                 <-- Formatted ratio/variance
    └── _Formatting & Display            <-- Subfolder (Always prefixed with underscore)
        ├── [Metric] Color               <-- Conditional formatting (Hex / RGB)
        ├── [Metric] Font Color          <-- Dynamic font contrast logic
        ├── [Metric] Executive Summary   <-- Dynamic text / chart subtitle strings
        └── [Metric] KPI Icon            <-- SVG / UNICHAR display indicators
Reference Implementation (Seasonal Operations Domain)PlaintextKPI
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
3. Classification & Placement RulesClassificationOutput TypeTarget LocationIsHidden RuleExamplesDomain KPINumeric (Integer, Decimal, Currency, Percentage)<Domain> root folderFalse (Visible)Monthly Foster Participation, Seasonal Benchmark Variance %Base MeasureNumeric (Raw aggregations)_Base MeasuresTrue (Hidden)_Count_FosterCases, _Sum_ActiveDaysUI FormattingString (Hex code, CSS, RGB)<Domain>\_Formatting & DisplayTrue (or Visible if needed for thin report)Planning Priority Color, Seasonality Heatmap ColorDynamic Title / TextString (Text output)<Domain>\_Formatting & DisplayOptional (Thin report local measure preferred)Seasonality Executive Conclusion4. DAX Definition & Property ConstraintsWhen writing or generating DAX for measures, apply the following properties:1. Business KPI (Seasonal Benchmark Variance %)Display Folder: Seasonal OperationsFormat String: 0.0%;-0.0%;0.0%Description: Explicitly state the business definition, numerator, and denominator.Pattern: Always use DIVIDE() with safe zero handling.2. UI Color Measure (Planning Priority Color)Display Folder: Seasonal Operations\_Formatting & DisplayFormat String: Text / UnformattedDescription: Internal conditional formatting rule for heatmap and table visuals.Pattern: Return strict 6-character hex strings with # prefix (e.g., "#D83B01", "#107C41", "#F3C11B").5. Developer & AI Assistant ChecklistBefore opening a pull request or saving changes to the model:[ ] Is the primary folder named after a clear business process/domain?[ ] Are all hex codes, font rules, and titles nested inside _Formatting & Display?[ ] Does every core metric have a defined FormatString?[ ] Are technical base measures (SUM, COUNT) hidden from end users?[ ] If this dynamic title is only used on one visual in one report, has it been placed in the thin report rather than the core model?