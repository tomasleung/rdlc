# BRD Style Standard v1.0

## 1. Purpose

This standard defines the reusable visual and HTML presentation rules for BC SPCA-style RDLC Business Requirements Documents. It controls presentation only. Business content must come from the confirmed shared understanding and the BRD Content Standard.

## 2. Design principles

The BRD must appear:

- governed rather than promotional;
- concise but complete;
- readable on screen and in print;
- visually consistent across RDLC products;
- accessible without colour alone;
- maintainable as one self-contained HTML file; and
- suitable for controlled PDF export.

Decoration must never compete with definitions, decisions, limitations, or approval status.

## 3. Document format

Default output:

- HTML5
- `lang="en-CA"` unless another language is confirmed
- UTF-8
- embedded CSS
- no JavaScript requirement
- no external font, stylesheet, image, or CDN dependency
- responsive browser layout
- Letter-size print layout by default
- A4 only when explicitly required

Use semantic elements such as `main`, `section`, `nav`, `table`, `thead`, and `tbody`.

## 4. Visual system

### 4.1 Colour tokens

Use CSS variables. Recommended defaults:

- Navy headings: `#18364a`
- Blue accent: `#176b87`
- Teal accent: `#2f7f78`
- Green approved/in scope: `#3f7652`
- Amber warning/deferred/limitation: `#a56616`
- Red risk/error: `#9b3d35`
- Body ink: `#24323b`
- Muted text: `#5d6c75`
- Borders: `#d8e0e4`
- Soft background: `#f3f7f8`
- White page: `#ffffff`

Colour is supplementary. Every status must also have a text label.

### 4.2 Typography

- Display headings: Georgia or Times New Roman fallback
- Body: Segoe UI or Arial fallback
- Code/formulas: Consolas or Courier New fallback
- Body size: approximately 10.5 pt for print
- Main title: approximately 28 pt
- Section heading: approximately 18 pt
- Subsection heading: approximately 13.5 pt
- Metadata and footnotes: 8.5–9 pt

Do not use more than these three font families.

### 4.3 Spacing

- Use generous cover-page whitespace.
- Keep paragraph spacing consistent.
- Place 7–11 mm above major section headings.
- Keep tables and cards visually close to their heading.
- Avoid dense blocks longer than six to eight lines without a subheading, list, table, or callout.

## 5. Cover page

The cover should contain:

- uppercase capability or domain label;
- document title;
- artifact type;
- RDLC phase and stage;
- status and version pill;
- one short decision-supported callout;
- author;
- created date;
- business decision owner;
- business definition owner;
- technical owner; and
- classification.

The cover must not imply approval unless approval has occurred.

## 6. Section hierarchy

Use numbered major sections:

- `01 · Executive summary`
- `02 · Business problem`
- etc.

Use unnumbered `Document metadata` and `Contents` before Section 01.

Each `h2` should have a restrained navy top rule. Use `h3` for subsections and `h4` sparingly.

## 7. Components

### 7.1 Metadata table

Use a two-column table with Field and Value. The first column should be about 29% of the width. Do not leave unknown values looking complete. Use Pending or To be assigned.

### 7.2 Callouts

Provide reusable classes:

- `.approved` or `.in-scope`: green
- `.warning`, `.deferred`, or `.limitation`: amber
- `.risk`: red
- `.note`: grey
- default informational callout: blue

Callouts should have a coloured left border, pale background, and explicit label.

### 7.3 Definition cards

Use cards for KPIs, metrics, and important governed definitions. Each card should contain, as applicable:

- name;
- definition;
- formula;
- grain;
- owner;
- interpretation; and
- validation note.

Use a two-column card grid on desktop and a one-column grid on narrow screens.

### 7.4 Tables

- Use rounded outer borders and light internal rules.
- Use dark navy headers with white text.
- Repeat table headers when printed where supported.
- Prevent individual rows from splitting across pages where possible.
- Keep cell paragraphs short.
- Do not use tables purely for page layout.
- Do not place large code blocks inside tables.

### 7.5 Formula and code blocks

Use `.formula` for a short business formula and `.code` for a field list or pseudocode. Formulas must remain readable in print and must not depend on syntax highlighting.

### 7.6 Status labels

Use rounded status pills for document status only. Do not turn every decision into a pill.

### 7.7 Approval table

Columns:

- Role
- Name
- Decision or status
- Date

Provide enough row height for review or signature annotation. Do not pre-populate approval decisions.

## 8. Print behaviour

The template must include:

```css
@page {
  size: Letter;
  margin: 15mm 14mm 17mm;
}
```

Include print rules to:

- remove browser-grey background;
- make the document full width;
- force a page break after the cover;
- preserve table headers;
- prevent cards, callouts, and table rows from splitting where practical;
- remove unnecessary hyperlink decoration; and
- display confidentiality and page numbering where the renderer supports margin boxes.

Because browser support for CSS margin boxes varies, include an in-document footer line as a fallback.

## 9. Responsive behaviour

Below approximately 720 px:

- reduce page padding;
- change card and cover metadata grids to one column;
- change a multi-column contents list to one column; and
- preserve table usability without reducing body text to unreadable sizes.

## 10. Accessibility

- Maintain sufficient colour contrast.
- Never use colour as the only meaning carrier.
- Use logical heading order.
- Add an accessible label to the table of contents.
- Use meaningful link text.
- Use table headers correctly.
- Avoid all-caps for long passages.
- Keep body line length near 60–90 characters where practical.
- Do not encode required information only in images.

## 11. HTML integrity

The output must:

- begin with `<!DOCTYPE html>`;
- contain one `html`, `head`, and `body` element;
- include a meaningful `title`;
- contain no unresolved placeholder tokens in an issued artifact;
- use unique section IDs;
- escape special characters in inserted content;
- avoid unsupported proprietary markup; and
- open without network access.

## 12. Placeholder convention

The reusable template uses uppercase double-curly placeholders, for example:

```text
{{DOCUMENT_TITLE}}
{{EXECUTIVE_SUMMARY_HTML}}
{{APPROVAL_ROWS_HTML}}
```

Rules:

- Scalar placeholders contain escaped text.
- Placeholders ending in `_HTML` contain validated HTML fragments.
- Optional sections should be removed cleanly when not applicable.
- Never leave a placeholder visible in the final artifact.
- Do not hard-code project-specific facts in the template.

## 13. File naming

Recommended pattern:

```text
BRD_<Capability_Short_Name>_v<major.minor>.html
```

Examples:

```text
BRD_Live_Capacity_Reporting_v1.0.html
BRD_Foster_Centre_Animal_Type_Utilization_v0.1.html
```

The file name, document title, version, and metadata must agree.

## 14. Reference usage

Prior BRDs may inform visual treatment, but the reusable template must not contain their project-specific content. In particular, do not hard-code:

- product names;
- business owners;
- metrics;
- source systems;
- dates;
- thresholds;
- mappings;
- refresh schedules;
- scope; or
- approval state.

## 15. Visual QA checklist

Before delivery confirm:

- cover content fits on one page;
- no heading is stranded at a page bottom;
- no important card or callout is split;
- tables fit the printable width;
- repeated table headers work where supported;
- text remains readable in grayscale;
- status and limitation labels are explicit;
- mobile layout is usable;
- all placeholders are resolved;
- footer, version, and classification agree with metadata; and
- PDF export does not clip borders, formulas, or approval rows.
