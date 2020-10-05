# 3. ControlField

define the control to be added to the form.

Properties:

- **Caption**: string, text to be shown as caption.
- **Type**: string, control type, possible values: ["TEXT"(default), "LABEL", "DATE", "DATETIME", "NUMERIC", "DECIMAL0", "DECIMAL2", "DECIMAL3", "PERCENT", "COMBO", "SEARCH", "CHECK", "GRID", "MEMO"]

- **Name**: string, name of the control used as reference to this control when needed.
- **BindingMember**: string, the binding member to be used.
- **DataSource**: string, sql query or param ref to sql query. only used whith Combo || Grid controls, as we need to provide data to the control.
- **DisplayMember**: string, used in Combo, allow the combo to show a defined column text as representation of the selected value.
- **ValueMember**: string, used in Combo, allow the combo to use a defined column as value.
public int DropDownRows { get; set; }
- **Format**: string, the desplay format.
- **ReadOnly**: bool, set to true if you want the control to be read only
- **Mandatory**: bool, set to true if you want the value for this control to be mandatory
- **MasterControl**: string, use to define this control as slave and use the set name of the master control, with this you can uset he master control value in the datasource query using %MASTER_VALUE%
- **MasterColumn**: string, used to define the column name in the master field to be used
- **DefaultValue**: string
- **ColumnSpan**: string
- **RowSpan**: string
