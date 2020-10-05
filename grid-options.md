# 4. GridOptions

- **Caption**: string, Panel title
- **Columns**: ICollection<Field>, define grid columns.
- **SQLExecute**: string, define sql to execute action on row double-click event (read more in IAction section).
- **FormToOpen**: string, form to open action define row double-click (read more in IAction section).
- **Args**: string, semicolon separated arguments to pass to the form.
- **MultiSelect**: string, identifier column for multiselection, when defined activates multi-selection on the grid and uses the identifier as selected values, %GRID_SELECTION% as args allow you to pass the selected identifier.
- **SummaryItems**: ICollection<SummaryItem>, define summaryItems for the grid [read more](#5-SummaryItem)

## 5. SummaryItems

- **Type**: SummaryItemType enum, possible values
  Sum = 0,
  Min = 1,
  Max = 2,
  Count = 3,
  Average = 4,
  Custom = 5,
  None = 6
- **FieldName**: string, based field for calculation
- **Format**: string, display format
- **AddToColumn**: string, specify the columns where to add the summary item
