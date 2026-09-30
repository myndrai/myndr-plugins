---
name: update-sheet
description: Make exact, range-precise edits to a Google Sheets spreadsheet — values, formulas, and new rows or columns — showing before and after. Use when asked to fill in, fix, append to, or restructure a spreadsheet.
---

# Update a sheet

Every write names its exact range, and the user sees the cells before and after
before it happens. A write to the wrong range silently destroys data.

Tools from this plugin's `gsheets` server:

- Read: `gsheets__get_spreadsheet`, `gsheets__get_values`
- Changes the spreadsheet: `gsheets__update_values`, `gsheets__update_formulas`,
  `gsheets__insert_dimension`, `gsheets__update_spreadsheet`

If the tools take a `myndr_account` argument, use the account the user named.

## Steps

1. **Locate.** The spreadsheet id from the link (after `/spreadsheets/d/`). Get
   tab titles and numeric `sheetId`s with `gsheets__get_spreadsheet`, using
   `fields: ["sheets.properties"]`.
2. **Read the target first.** `gsheets__get_values` on the exact range you will
   write, for example `'Q3'!C2:C14`. To append, read the key column to find the
   first empty row; never assume it.
3. **Show before → after** as a small table of cell, current value and new value.
   Wait for the user's yes. Non-empty cells being overwritten are listed
   explicitly.
4. **Write.**
   - **Plain values:** `gsheets__update_values` with `range` and `values` as a
     2-D array whose shape exactly matches the range (rows × columns). Use
     strings for text and numbers for numbers. `""` clears a cell; `null` leaves
     it unchanged.
   - **Formulas:** `gsheets__update_formulas` with `formulas` as a 2-D array of
     strings starting with `=`. Never put formulas through `gsheets__update_values`.
   - **New rows or columns:** `gsheets__insert_dimension` with the numeric
     `sheetId`, `dimension` `ROWS` or `COLUMNS`, 0-based `startIndex`
     (inclusive) and `endIndex` (exclusive), and `inheritFromBefore` to keep
     formatting. Then write the values.
   - **Formatting or structure nothing above covers:**
     `gsheets__update_spreadsheet` with `requests`. Show the request JSON to the
     user first.
5. **Verify.** Re-read the written range with `gsheets__get_values` and confirm
   it matches. Report any cell that differs.

## Rules

- One confirmation covers one described change. A new change needs a new yes.
- Never write outside the range the user approved, and never "tidy" neighbouring
  cells.
- Never overwrite a formula cell with a value unless the user asked for exactly
  that.
- Row and column numbers in A1 notation are 1-based; `gsheets__insert_dimension`
  indices are 0-based. Double-check the conversion.
- On any error, stop and report it; do not retry with a different range.

## Output

```
<Spreadsheet> — '<Tab>'!<range>
Cell   Before        After
C2     1200          1350
…
Written and verified: <n> cells.
```
