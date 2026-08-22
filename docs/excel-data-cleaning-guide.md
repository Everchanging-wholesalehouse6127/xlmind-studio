# Excel Data Cleaning Guide: A Practical Workflow for Messy Spreadsheets

Most spreadsheet work does not fail on the analysis. It fails earlier, on data that looks fine until you try to use it: a SUMIF that returns zero, a lookup that misses half the rows, a pivot table that lists "Istanbul", "istanbul " and "ISTANBUL" as three separate cities.

This guide describes a repeatable workflow for cleaning data in Microsoft Excel — the order to do things in, what to check at each step, and the mistakes that quietly corrupt a dataset. It is written for people who receive exports from other systems and have to make them usable.

## Contents

- [Why spreadsheet data gets messy](#why-spreadsheet-data-gets-messy)
- [The workflow in order](#the-workflow-in-order)
- [Before you change anything](#before-you-change-anything)
- [Fixing the text layer](#fixing-the-text-layer)
- [Fixing the type layer](#fixing-the-type-layer)
- [Fixing the structure layer](#fixing-the-structure-layer)
- [Fixing the meaning layer](#fixing-the-meaning-layer)
- [Validating the result](#validating-the-result)
- [Automating repetitive cleanup](#automating-repetitive-cleanup)
- [Common mistakes](#common-mistakes)
- [A repeatable checklist](#a-repeatable-checklist)

## Why spreadsheet data gets messy

Messy data is rarely anyone's fault. It is a by-product of how data travels:

- **Export formatting.** Systems export numbers, dates and IDs as text so nothing is lost in transit. Excel then treats them as text too.
- **Encoding mismatches.** A UTF-8 file opened as a legacy code page turns accented and non-Latin characters into sequences like `Ã¼` or `Å`.
- **Free-text entry.** Anywhere a human types a value, you will eventually get trailing spaces, mixed capitalization, and three spellings of the same thing.
- **Copy between workbooks.** Merged cells, hidden columns and layout formatting travel with the data and break sorting and filtering.
- **Regional settings.** `03/04/2026` is 3 April in one locale and 4 March in another. Decimal commas and decimal points do the same thing to numbers.
- **Layout built for reading, not for analysis.** Repeated header rows, blank spacer rows, subtotal rows mixed in with data, and one column per month instead of one row per observation.

Knowing the cause matters, because it tells you whether to fix the spreadsheet or fix the export.

## The workflow in order

Order matters more than people expect. Removing duplicates before you have trimmed spaces will leave duplicates behind, because `"ACME "` and `"ACME"` are not equal. Converting text to numbers before you have stripped thousands separators can produce silent errors.

A sequence that avoids most of those traps:

| # | Step | Why it comes here |
|---|---|---|
| 0 | Copy the raw data | You will need something to compare against |
| 1 | Profile the table | Decide what actually needs fixing |
| 2 | Fix structure | Unmerge, remove spacer rows, one header row |
| 3 | Trim and clean text | Everything downstream compares text |
| 4 | Repair broken characters | Before any matching or grouping |
| 5 | Standardize capitalization | Before deduplication and grouping |
| 6 | Convert types | Numbers, dates, and codes that must stay text |
| 7 | Split and combine columns | Now that values are predictable |
| 8 | Handle blanks | You can now tell "empty" from "looked empty" |
| 9 | Remove duplicates | Only once values are truly comparable |
| 10 | Reconcile categories | Merge variants of the same label |
| 11 | Range and outlier checks | Catch what survived |
| 12 | Validate | Prove the cleaning did not lose anything |

## Before you change anything

### Keep an untouched copy of the original

Put the raw export on its own worksheet and never edit it. Every cleaning step should either write to a new sheet or be reversible. When a number looks wrong three weeks later, the raw sheet is the only thing that lets you find out whether the source was wrong or your cleaning was.

If the file is large, keep the original export file itself and note its name and date in a small "source" sheet: where it came from, when, and which filters were applied at export time.

### Profile the table first

Cleaning without profiling means guessing. Before touching anything, answer these for each column:

- How many rows, and how many are actually filled?
- How many distinct values? (A "Country" column with 340 distinct values has a problem.)
- What data type does Excel think it is, and what should it be?
- What are the shortest and longest text values? (Extremes usually reveal the anomalies.)
- What are the minimum and maximum for numeric and date columns?

A fast manual version: select a column and read the status-bar count, then use `=COUNTA()`, `=COUNTBLANK()` and a quick pivot on distinct values. Ten minutes here saves an hour of rework.

### Fix structure before content

Analysis tools expect a rectangle: one header row, one record per row, no gaps, no merged cells.

- Unmerge every merged cell and fill the resulting blanks downward.
- Delete blank spacer rows and columns.
- Remove repeated header rows that arrived from a paginated export.
- Move subtotal rows out of the data; recalculate them later from clean data.
- Collapse multi-row headers into a single row of clear, unique column names.

## Fixing the text layer

### Remove unnecessary spaces

Leading, trailing and doubled spaces are the single most common cause of failed lookups, because they are invisible.

| Before | After |
|---|---|
| `" ACME Ltd "` | `"ACME Ltd"` |
| `"North  Region"` | `"North Region"` |

`TRIM()` removes leading and trailing spaces and collapses internal runs to one. It does **not** remove non-breaking spaces (character 160), which are extremely common in data copied from web pages and PDFs. For those, substitute character 160 with a normal space first:

    =TRIM(SUBSTITUTE(A2, CHAR(160), " "))

A quick diagnostic: `=LEN(A2)-LEN(TRIM(A2))` tells you how many stray spaces a cell is carrying.

### Repair broken or corrupted-looking characters

Text like `Ã‡ELÄ°K` or `Ãœrÿn` is an encoding mismatch, not corrupt data — the bytes are intact, they were just decoded with the wrong character set. Two options, in order of preference:

1. **Re-import correctly.** Use Data → From Text/CSV and set File Origin to UTF-8. This fixes every column at once and is always better than patching text.
2. **Substitute the known pairs.** If re-importing is impossible, build a small mapping table of broken sequence → correct character and apply nested `SUBSTITUTE()` calls or a lookup.

`CLEAN()` is a different tool: it removes non-printable control characters (line feeds inside cells, tabs) that break exports and CSV round-trips. It does not fix encoding.

### Standardize capitalization

Excel lookups and pivot grouping are case-insensitive, but `EXACT()`, many database imports and most downstream systems are not. Pick one convention per column and apply it.

`UPPER()`, `LOWER()` and `PROPER()` cover the basics. Be careful with `PROPER()`: it turns `"ACME LTD"` into `"Acme Ltd"` and `"o'brien"` into `"O'Brien"` — usually right — but it also turns `"IBAN"` into `"Iban"` and `"McDonald"` into `"Mcdonald"`. For columns that contain acronyms or names with internal capitals, apply a correction list after the case change.

## Fixing the type layer

### Numbers stored as text

The symptoms are familiar: values left-aligned by default, a green triangle in the corner, `SUM()` returning zero, and `VLOOKUP` failing between a text `"1024"` and a numeric `1024`.

Before converting, remove what makes them non-numeric:

- thousands separators that do not match your locale (`1,234.56` vs `1.234,56`)
- currency symbols and unit suffixes (`€`, `USD`, `kg`)
- trailing minus signs (`1234-`) used by some accounting exports
- non-breaking spaces used as thousands separators

Then convert. `VALUE()` works when the string already matches your locale. Text to Columns with the right decimal and thousands settings handles locale mismatches. Paste Special → Add with an empty cell converts a whole range in place.

Always confirm afterwards with `=COUNT(range)` against `=COUNTA(range)`. If the two differ, some cells are still text.

### Dates

Dates are the most dangerous column type, because a wrong date is still a valid date — nothing errors out.

- `03/04/2026` is ambiguous. Establish the source convention before converting anything, ideally from the exporting system rather than by inspecting the data.
- `=ISNUMBER(A2)` is the fastest way to see whether Excel has a real date or a text string that looks like one.
- Text to Columns lets you declare the incoming order explicitly (DMY, MDY, YMD). This is more reliable than `DATEVALUE()`, which follows your machine's regional settings.
- Two-digit years are interpreted by a cut-off rule that varies. Convert them to four digits explicitly rather than trusting the default.
- After conversion, check the minimum and maximum. A date in 1899 or 2087 means the parse went wrong.

Store dates as real date values and control the display with number formatting. Never store a date as formatted text.

### Codes that must stay text

Product codes, postal codes, national IDs, phone numbers and account numbers look numeric but are not. Convert them to numbers and `007412` silently becomes `7412`, `+90 212...` loses its prefix, and a 19-digit ID loses precision because Excel stores only 15 significant digits.

The rule: if you would never add two values together, it is not a number.

Set the column to Text format **before** pasting the data, or import through Data → From Text/CSV and mark those columns as Text. If leading zeros are already gone and you know the required length, pad them back with `=TEXT(A2,"000000")` — but treat that as a repair, not a habit.

## Fixing the structure layer

### Split and combine text

Splitting is straightforward when there is a consistent delimiter — Text to Columns, or `TEXTSPLIT()` in current versions of Excel. It gets harder when the delimiter is inconsistent (`"Doe, John"` and `"John Doe"` in the same column) or when the pattern is positional rather than delimited.

Two practices make it survivable:

1. Split into new columns rather than in place, so the original stays visible while you check.
2. Count the parts first. `=LEN(A2)-LEN(SUBSTITUTE(A2,",",""))+1` tells you how many pieces each row will produce. If most rows give 2 and a few give 3, those few are the ones that will break.

For combining, `TEXTJOIN()` handles delimiters and skips blanks cleanly, which is what you usually want when building a full address or a composite key.

### Blanks and missing values

"Blank" is not one thing. Distinguish between:

- a genuinely empty cell,
- a cell containing an empty string `""` returned by a formula,
- a cell containing only spaces,
- a placeholder such as `N/A`, `-`, `NULL`, `0` or `unknown`.

They behave differently: `COUNTBLANK()` counts empty strings, `COUNTA()` does not; `ISBLANK()` is false for a cell containing `""`. Decide per column whether blank means "not applicable", "not yet known" or "zero", and encode that decision consistently. Filling every blank with `0` is a common and expensive mistake — it turns missing data into a real value and quietly biases every average.

Excel's Go To Special → Blanks is the fastest way to select and inspect them all at once.

### Duplicates

Decide what "duplicate" means before you remove anything. Two rows with the same customer name are not necessarily duplicates; two rows with the same invoice number almost certainly are.

- Define the key columns explicitly — the combination that must be unique.
- **Flag before you delete.** Add a helper column with `=COUNTIFS(...)` on the key columns, sort by it, and look at what you are about to lose. Deleting first and asking later is not recoverable.
- Clean text before deduplicating, or near-duplicates will survive.
- Watch for rows that are duplicated on the key but differ elsewhere — those are conflicts, not duplicates, and need a rule for which record wins.

## Fixing the meaning layer

### Inconsistent categories

The same real-world thing recorded several ways is the hardest problem in this guide, because no function detects it for you.

| Recorded as | Should be |
|---|---|
| `Ltd`, `Ltd.`, `LTD`, `Limited` | one form |
| `DE`, `Germany`, `Deutschland` | one form |
| `Paid`, `paid`, `PAID `, `Payment received` | one form |

The practical approach is a pivot table or `UNIQUE()` list of every distinct value in the column, sorted alphabetically so variants sit next to each other, plus a small mapping table of variant → canonical value. Apply the mapping with a lookup, keep the mapping table in the workbook, and reuse it next month. The mapping table is the deliverable, not the cleaned column.

### Unexpected values and outliers

Rules that catch most real problems:

- Numeric columns: check minimum, maximum, and count of negatives. A negative quantity or a unit price of 999,999 is usually a data-entry error.
- Date columns: check for dates before the business started and after today.
- Text columns: check the shortest and longest values. A two-character company name and a 300-character one are both worth reading.
- Percentages: check that they are all on the same scale. A column mixing `0.15` and `15` will produce nonsense.
- Conditional formatting with a simple rule is often faster than a formula for spotting these visually.

### Compare against a reference list

Much of what looks like cleaning is really reconciliation: does every product code in this export exist in the product master? Which customers are in last month's file but not this month's?

`COUNTIF()` against the reference range answers "does it exist" for a single list. `XLOOKUP()` or `MATCH()` answer "what does it map to". For a two-way comparison, run the check in both directions — items missing from list A and items missing from list B are different problems with different causes.

Trim and standardize both lists before comparing. Most reported "missing" items are really spacing or capitalization differences.

## Validating the result

Cleaning is not finished when the data looks right. It is finished when you can show that nothing was lost or invented.

- **Row count.** Compare against the raw sheet. If rows disappeared, know exactly which ones and why.
- **Control totals.** Sum the key numeric columns before and after. They should match unless you deliberately removed rows.
- **Type check.** `=COUNT()` versus `=COUNTA()` on every column that should be fully numeric.
- **Distinct counts.** Category columns should have fewer distinct values after cleaning, and the new count should be a number you can explain.
- **Error scan.** No `#N/A`, `#VALUE!`, `#REF!` or `#DIV/0!` left in the output.
- **Spot check.** Pick five rows at random, including the first and last, and trace them back to the raw sheet by eye.

Record the results of these checks in the workbook. Next month, they become the regression test.

## Automating repetitive cleanup

Most of the steps above are not intellectually difficult. They are just repetitive — the same trim, the same case fix, the same date repair, on the same export, every month. That repetition is where errors and lost hours come from.

There are three normal ways to remove it:

- **Power Query**, built into Excel, records import and transformation steps and replays them when the source refreshes. It is the right answer for a stable, recurring import.
- **Templates and helper-column blocks** you copy forward, when the transformation is small and the source changes shape often.
- **A dedicated add-in**, when the operations are one-off in structure but repeated across many different files.

[XLMind Studio](https://xlmindstudio.com/en/features?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=data_cleaning_guide_inline) is a Windows desktop add-in for Microsoft Excel built around that third case. It provides 150+ tools on two ribbon tabs, including dedicated commands for the operations in this guide: trimming and cleaning text, repairing broken characters, cleaning header rows, unmerging and filling, deleting blanks, changing case, converting text to numbers, repairing apostrophe numbers and leading zeros, fixing and checking dates, splitting and extracting text, highlighting duplicates and unique values, comparing two lists, and profiling a table before you start. Everything runs on the workbook open in Excel — your Excel data stays on your computer for XLMind Studio's local processing.

It is one option among several. The point of the workflow is the workflow; tooling only decides how long each step takes.

## Common mistakes

- **Cleaning in place with no copy of the original.** The single most expensive habit in this list.
- **Removing duplicates before standardizing text.** Guarantees survivors.
- **Converting ID and code columns to numbers.** Leading zeros and long identifiers are destroyed silently.
- **Trusting the default date interpretation.** Ambiguous dates parse without error and are wrong half the time.
- **Filling blanks with zero.** Turns unknown into a value and biases every calculation downstream.
- **Using `PROPER()` on columns containing acronyms.** `IBAN` becomes `Iban`.
- **Fixing symptoms instead of the export.** If the same problem appears every month, change the export or the import settings.
- **Cleaning a copy of the data and then analysing the original.** Label sheets clearly: `raw`, `clean`, `report`.
- **No validation step.** Without control totals you cannot tell cleaning from corruption.

## A repeatable checklist

- [ ] Raw export saved untouched, with source and date recorded
- [ ] Table profiled: row counts, distinct counts, types, ranges
- [ ] Structure fixed: single header row, no merged cells, no spacer or subtotal rows
- [ ] Spaces trimmed, including non-breaking spaces
- [ ] Non-printable characters removed
- [ ] Encoding problems fixed at import where possible
- [ ] Capitalization standardized per column
- [ ] Numbers converted from text and verified with `COUNT` vs `COUNTA`
- [ ] Dates converted with an explicit order and range-checked
- [ ] Code-like columns kept as text with leading zeros intact
- [ ] Columns split or combined, with part counts verified
- [ ] Blanks classified and handled per column, not globally
- [ ] Duplicate key defined, duplicates flagged, then removed
- [ ] Category variants mapped to canonical values, mapping table saved
- [ ] Range and outlier checks run on numeric and date columns
- [ ] Lists reconciled against reference data in both directions
- [ ] Row count and control totals reconciled against raw
- [ ] No error values remain
- [ ] Five rows spot-checked by hand

## Where to go next

- [Excel Dashboard Guide](excel-dashboard-guide.md) — what to do with the data once it is clean
- [Excel Productivity Checklist](excel-productivity-checklist.md) — 30 ways to cut repetitive spreadsheet work
- [Feature overview](features.md) — the XLMind Studio tool groups, by category

---

If you want to try the cleanup tools described above on your own data, XLMind Studio offers a 7-day free trial with all features available.

[Explore XLMind Studio tools](https://xlmindstudio.com/en/features?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=data_cleaning_guide_features) · [Download the free trial](https://xlmindstudio.com/en/download?utm_source=github&utm_medium=referral&utm_campaign=github_public_repo&utm_content=data_cleaning_guide_download)
