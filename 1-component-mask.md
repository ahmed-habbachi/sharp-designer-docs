# 1. Component Mask

It is a json representation configuration of a window/form.

## 1.1 ComponentMask

### 1.1.1 Properties

- **Buttons**: ICollection<ActionControl>
  add dynamic buttons to the top bar [read more](#actioncontrol)

- **ButtonsGroups**: ICollection<ActionControlGroup>
  add dynamic buttons group as dropdown menu [read more](#actioncontrolgroup)

- **TilesGroups**: ICollection<ActionControlTileGroup>
  add dynamic tile buttons group, usualy used in main form [read more](#actioncontroltilegroup)

- **DataSources**: IList<DataSource>
  read only, get the list of all the datasources, from main query and from controls that have datasource field seted

- **layoutType**: string, possible values: [EditForm, EditListForm, ListForm, AffectationForm, MainBoard]
  used to define the form template to be used.
  example:

```json
"layoutType": "EditListForm",
```

- **layoutFilling**: string, possible values: [Horizontal, Vertical]
  used to define the from controls filling vertically or horizontally.
  example:

```json
"layoutFilling": "Horizontal",
```

- **TabPanelHeight**: int
  define the height of the tab panel

- **TabPanels**: ICollection<TabPanel>
  collection of panels that holds controls. [read more](#tabpanels)

- **TilesGroups**: ICollection<ActionControlTileGroup>
  allow the user to add dynamic tile group, we cannot add tiles directly to the form

- **GridOptions**: GridOptions
  defines grid options [read more](#gridoptions)

- **ControlFields**: ICollection<ControlField>
  read only, get collection of ControlField\*, collected from all the panels

- **SaveBehaviour**: SaveBehaviourEnum [Save, SaveAndClose, SaveAndNew]
  allow the user to tweek the behaviour of the save button.

  - Save: default only save the current values
  - SaveAndClose: save the current values and close the current window
  - SaveAndNew: save the current values and create a new record

- **ShowType**: ShowTypeEnum [ModalAndRefresh, Modal, NotModal]
  allow the user to open a form with a specific behaviour:

  - ModalAndRefresh: default open a modal form and refresh the current page as soon and the modal form closes.
  - Modal: open a modal form and do not refresh the current form after the modal closes.
  - NotModal: open a form without Modal behaviour.
  - CloseOnFormToOpen: open a form without modal behaviour and close the current one.

- **sqlSelect**: string
  defines the main select statement of the form, "mainQuery" this is the only query that will be executed on each refresh.
  example:

```json
"dataSources": [
  {
    "name": "mainQuery", //main query is the only query that will be reloaded when refresh
    "sqlQuery": "SELECT ID, Genre, Price, ReleaseDate, Title, Email, Phone, Watched FROM Movie"
  },
  {
    "name": "combobox",
    "sqlQuery": "SELECT Movie.ID, Movie.Genre FROM Movie"
  }
],
```

- **sqlInsert**: string
  the insertion query that the form will use. use %value-placeholder% as values place holders to be parsed before executing the query.
  example:

```json
"sqlInsert": "INSERT INTO Movie (Title, Price, ReleaseDate, Watched, Genre) VALUES ('%Title%',  %Price%, CONVERT(datetime,'%ReleaseDate%', 103), '%Watched%', '%Genre%')",
```

- **sqlUpdate**: string
  the update query that the form will use. use %value-placeholder% as values place holders to be parsed before executing the query.
  example:

```json
"sqlUpdate": "UPDATE Movie SET Genre = '%Genre%', Price = %Price%, ReleaseDate = '%ReleaseDate%', Title = '%Title%', Watched = '%Watched%' WHERE ID = %ID%",
```

- **sqlDelete**: string
  the delete query that the form will use. use %value-placeholder% as values place holders to be parsed before executing the query.
  example:

```json
"sqlDelete": "Delete From Movie WHERE ID = %ID%",
```

- **sqlDefault**: string
  the default query represent the datarow that will be used as default values for the corresponding controls. use %value-placeholder% as values place holders to be parsed before executing the query.

- instead of writing the queries in the respective propertie, we can use a parameter reference to get query from database (from the PARAM table) `ParamRef%paramname%` this is usefull to keep all the queries in one place for ease of edition and also it is usefull when the query is too large it is recomanded to relocate it to the param table so it can free some space for the mask layout in the mask column.

- We can use passed argument to the form in the queries using (`%Arg1%`, `%Arg2%` ...), these argument should be indicated from the parent form in the **_Args_** of an IAction base entity ([ActionControl](#actioncontrol) and [GridOptions](#gridoptions)).

### 1.1.2 Exemple

```json
{
  "layoutType": "EditListForm", // EditListForm, DetailEditForm, ListForm
  "caption": "Parametrage : Utilisateur",
  "imagePath": "Resources/golobal-flat-24x24.png",
  "sqlSelect": "ParamRef%FrmUtilisateur.sqlSelect%",
  "sqlInsert": "ParamRef%FrmUtilisateur.sqlInsert%",
  "sqlUpdate": "ParamRef%FrmUtilisateur.sqlUpdate%",
  "sqlDelete": "ParamRef%FrmUtilisateur.sqlDelete%",
  "layoutFilling": "Horizontal", // Horizontal, Vertical
  "buttons": [
    {
      "caption": "User report",
      "imagePath": "navigation/home_32x32.png",
      "reportToOpen": "User",
      "args": "%MAIN_GRID.SELECTION%",
      "AfterMessage": "Test"
    },
  ],
  "tabPanels" : [
    {
      "caption": "Details",
      "layoutColumnCount": 2,
      "ControlFields": [
        {
          "type": "Text",
          "bindingMember": "LOGIN",
          "caption": "Login",
          "columnIndex": 0,
          "mandatory": true,
          "readOnly": false,
          "defaultValue": ""
        },
        {
          "type": "Combo",
          "bindingMember": "ID_PROFIL",
          "dataSource": "ParamRef%cmbProfil.ds%",
          "displayMember": "DESIGNATION",
          "caption": "Profil",
          "DataRowColumns": "DATE_USER_CREAT"
        },
        {
          "type": "Text",
          "bindingMember": "PRENOM",
          "caption": "Prenom",
        },
        {
          "type": "Text",
          "bindingMember": "NOM",
          "caption": "Nom",
        },
        {
          "type": "Text",
          "bindingMember": "PASSWORD",
          "caption": "Mot de passe",
        },
        {
          "type": "Text",
          "bindingMember": "ABREVIATION",
          "caption": "Abreviation",
          "columnIndex": 3
        },
        {
          "type": "Check",
          "bindingMember": "STATUT",
          "caption": "Actif",
        },
        {
          "type": "Check",
          "bindingMember": "B_MODIFIER_PTF",
          "caption": "Modifier les portefeuilles",
        }
      ]
    }
  ],
  "gridoptions": {
    "MultiSelect": "ID_USER"
  }
}
```

## 1.2. TabPanels

### 1.2.3 Properties

- **Caption**: string
  define the panel caption

- **ControlFields**: ICollection<ControlField>
  define the list of controls to add to the panel [ControlField](#controlfield).

- **layoutColumnCount**: integer
  the number of columns that the form will be using to place the controls, possible values number > 0.
  example:

```json
"layoutColumnCount": 3,
```

- **layoutRowCount**: integer
  the number of rows that the form will be using the place the controls, possible values number > 0.
  example:

```json
"layoutRowCount": 3,
```

- **DockStyle**: DockingStyle [Float = 0, Top = 1, Bottom = 2, Left = 3, Right = 4, Fill = 5]

### 1.2.3 Exemple

```json
tabPanels : [
  {
    "caption": "First Tab",
    "layoutColumnCount": 2,
    "controlFields": [
      {
        "type": "Numeric",
        "bindingMember": "ORDRE",
        "caption": "Ordre",
        "mandatory": true,
      },
      {
        // "type": "Memo",
        "name": "txtDesignation",
        "bindingMember": "DESIGNATION",
        "caption": "Designation",
        "mandatory": true,
      },
      {
        "type": "Check",
        "name": "chkInform",
        "bindingMember": "B_INFORME",
        "caption": "Informe",
      },
      {
        "type": "Check",
        "bindingMember": "B_AUTRE_RELANCE",
        "caption": "B_AUTRE_RELANCE",
        "readOnly": true,
      },
    ]
  },
  {
    "caption": "Second Tab",
    "layoutColumnCount": 2,
    "controlFields": [
      {
        "type": "Check",
        "bindingMember": "B_MODIF_DATE_RELANCE",
        "caption": "B_MODIF_DATE_RELANCE",
      },
      {
        "type": "Check",
        "bindingMember": "B_ACTIVER_DATE_RDV",
        "caption": "B_ACTIVER_DATE_RDV",
      },
      {
        "type": "Check",
        "bindingMember": "B_DATE_RDV",
        "caption": "B_DATE_RDV",
      },
      {
        "type": "Text",
        "bindingMember": "NB_JOURS",
        "caption": "NB_JOURS",
      },
      {
        "type": "Check",
        "bindingMember": "B_OBSERVATION",
        "caption": "B_OBSERVATION",
      },
      {
        "type": "Check",
        "bindingMember": "B_LIVRE_BLANC",
        "caption": "B_LIVRE_BLANC",
      },
      {
        "type": "Text",
        "bindingMember": "NB_JOURS_MIN",
        "caption": "NB_JOURS_MIN",
      },
      {
        "type": "Text",
        "bindingMember": "GROUPE",
        "caption": "GROUPE",
      },
      {
        "type": "DateTime",
        "bindingMember": "DATE_USER_MODIF",
        "caption": "GROUPE",
        "format": ""
      }
    ]
  }
],
```

## 1.3. ControlField

define the control to be added to the form.

### 1.3.1 Properties

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

### 1.3.2 Example

```json
controlFields: [
  {
    type: "Text",
    bindingMember: "NOM",
    caption: "NOM",
    columnIndex: 0,
  },
  {
    name: "masterControl",
    type: "Combo",
    bindingMember: "TIERS",
    dataSource: "SELECT TIERS, NOM FROM SREC.TIERS",
    caption: "Client",
  },
  {
    type: "Combo",
    bindingMember: "ID_AFFAIRE",
    dataSource: "SELECT ID_AFFAIRE, TIERS_CLIENT AS TIERS FROM SREC.AFFAIRE WHERE TIERS_CLIENT='%MASTER_VALUE%'",
    caption: "Affaire",
    displayMember: "ID_AFFAIRE",
    valueMember: "ID_AFFAIRE",
    masterControl: "masterControl"
  }
],
```

## 1.4. GridOptions

### 1.4.1 Properties

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
- **SummaryItems**: ICollection<SummaryItem>, define summaryItems for the grid [read more](#summaryitems)

### 1.4.1 Example

```json
"gridoptions": {
  "Heigth": 400,
  "styleEvenRow": "true",
  "styleOddRow": "true",
  "showGroupingPanel": "true",
  "showAutoFilterRow": "true",
  "multiSelect": "id",
  "columns": [
    {
      "bindingMember": "LOGIN",
      "caption": "Modifie par"
    },
    {
      "bindingMember": "DATE_USER_CREAT",
      "Caption": "Date creation"
    },
    {
      "bindingMember": "DATE_USER_MODIF",
      "Caption": "Date modification"
    }
  ],
  "summaryItems":[{
    "type": "Sum",
    "fieldName": "ORDRE",
    "AddToColumn": "ORDRE",
  }
  ]
}
```

## 1.5. SummaryItems

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

## 1.6. ActionControl

Action control are a custom buttons we can add to our form on the very top right next to the edit and save buttons, or tile buttons we can add the the main form, to execute a custom action, these actions are IAction based actions means they need to follow IAction interface or contract.

Properties:

- **Name** string, control name.
- **Caption** string, caption or label.
- **ToolTip** string, tooltip to show on hover over the control
- **ImagePath** string, icon or image path to show inside the button
- **BackColor** Color, tile back color **this propery is reserved to Tiles and not buttons**
- **ItemSize** TileItemSize, tile size enum [Default = 0, Small = 1, Medium = 2, Wide = 3, Large = 4] **this propery is reserved to Tiles and not buttons**

and moste importantly too the IAction interface properties:

### 1.6.1. IAction

Properties:

- **SQLExecute** string, contains a query to execute.
- **FormToOpen** string, form name (identifier) to open.
- **ReportToOpen** string, report name (identifier) to open.
- **Args** string, comma separated args to pass to one of the obove targets.
- **SQLSelect** string, contains a query to override the mask sqlSelect.
- **BeforeMessage** string, message identifier to allow to show a message before action execution from db.
- **AfterMessage** string, message identifier to allow to show a message after action execution from db.

we can also add a drop down menu or a group of buttons to group some buttons that have the same context for example with a "ActionControlGroup":

### 1.6.2. ActionControlGroup

Properties:

- **Caption** string, the grouping caption.
- **ButtonsGroups** ICollection<ActionControlGroup>, a second level group.
- **Buttons** ICollection<ActionControl>, a collection of button to add to the groupping.

### 1.6.3. ActionControlTileGroup

#### 1.6.3.1 Properties

- **Caption** string, the grouping caption.
- **TilesGroups** ICollection<ActionControlTileGroup>, a second level group.
- **Tiles** ICollection<ActionControl>, a collection of tiles to add to the groupping in the main form

#### 1.6.3.2 Example

```json
"tilesGroups": [
  {
    "Caption": "System Settings",
    "tiles": [{
      "Caption": "Table Param",
      "ImagePath": "",
      "ItemSize": "Wide", // Default, Small, Medium, Wide, Large
      "FormToOpen": "FrmSysParam",
      "imagePath": "Resources/Assets/TileImages/Parameter.png",
      "BackColor": "Orange", // System.Drawing.Color
    },
    {
      "Caption": "Table Param List",
      "FormToOpen": "FrmSysParamList",
      "BackColor": "DarkKhaki",
      "imagePath": "Resources/Assets/TileImages/burning-house.png",
    },
    {
      "Caption": "User affectation",
      "FormToOpen": "FrmAffectationUtilisateur",
      "BackColor": "Coral",
      "imagePath": "Resources/Assets/TileImages/user-settings.png",
    },
    {
      "Caption": "Test Form",
      "FormToOpen": "FrmTest",
      "BackColor": "Gray",
      "imagePath": "Resources/Assets/TileImages/user-settings.png",
    }]},
  {"Caption": "Empty Group",},
  {
    "caption": "Others",
    "TilesGroups": [], // Nested groups
    "tiles": [ // Nested Items
      {
        "ItemSize": "Wide", // Default, Small, Medium, Wide, Large
        "Caption": "Parametrage : Profil",
        "FormToOpen": "FrmProfil",
        "BackColor": "Gray",
        "imagePath": "house-fire.png",
      },
      {
        "ItemSize": "Large", // Default, Small, Medium, Wide, Large
        "Caption": "Parametrage : Utilisateur",
        "FormToOpen": "FrmUtilisateur",
        "BackColor": "Cyan",
        "imagePath": "speaker-ptf.png",
      },
      {
        "Caption": "Groupe d'action",
        "FormToOpen": "FrmGroupeAction",
        "BackColor": "LightGreen",
      },
      {
        "Caption": "Action",
        "FormToOpen": "FrmAction",
        "BackColor": "Green",
      },
      {
        "name": "hideFrmSort_show",
        "Caption": "Sort",
        "FormToOpen": "FrmSort",
        "BackColor": "Coral",
      },
      {
        "Caption": "Definition des campagnes",
        "FormToOpen": "FrmCampagne",
        "BackColor": "DarkGreen",
      },
      {
        "Caption": "Definition des portefeuilles",
        "FormToOpen": "FrmPtf",
        "BackColor": "DarkKhaki",
      },
      {
        "Caption": "Definition des scores",
        "FormToOpen": "FrmScore",
        "BackColor": "DarkSalmon",
      },
      {
        "Caption": "Gestion des Rappels",
        "FormToOpen": "FrmRappel",
        "BackColor": "DarkOrchid",
      },
      {
        "Caption": "Gestion des portfeuilles manuels",
        "FormToOpen": "FrmPtfList",
        "BackColor": "DarkSlateGray",
      },
    ]
  }
]
```
