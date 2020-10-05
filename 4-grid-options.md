# 4. GridOptions

- **Caption**: string, Panel title
- **SQLExecute**: string, define sql to execute action on row double-click event (read more in IAction section).
- **FormToOpen**: string, form to open action defined when double-click on grid row (read more in IAction section).
- **ReportToOpen**: string, report to open action defined when double-click on grid row
- **Args**: string, semicolon separated arguments to pass to the form.
- **MultiSelect**: string, identifier column for multiselection, when defined activates multi-selection on the grid and uses the identifier as selected values, %GRID_SELECTION% as args allow you to pass the selected identifier.
- **ShowSearchPanel** bool, show hide the search panel of the grid (default false)
- **ShowFilterPanel** bool, show hide the filter panel of the grid (default false)
- **ShowGroupPanel** bool, show hide the group panel of the grid (default false)
- **ShowAutoFilterRow** bool, show hide the auto-filter sub header row (default false)
- **StyleEvenRow** bool, style the even row
- **StyleOddRow** bool, style the odd row
- **Heigth** int, the overall grid heigth
- **Columns**: ICollection<Field>, define grid columns.
- **SummaryItems**: ICollection<SummaryItem>, define summaryItems for the grid [read more](#5-SummaryItem)
