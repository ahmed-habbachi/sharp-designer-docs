# 1. **ComponentMask**

It is a json representation configuration of a window/form.

Properties:

- **Buttons**: ICollection<ActionControl>
  add dynamic buttons to the top bar [read more](ActionControl.md)

- **ButtonsGroups**: ICollection<ActionControlGroup>
  add dynamic buttons group as dropdown menu [read more](ActionControl.md)

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
  collection of panels that holds controls. [read more](TabPanel.md)

- **TilesGroups**: ICollection<ActionControlTileGroup>
  allow the user to add dynamic tile group, we cannot add tiles directly to the form

- **GridOptions**: GridOptions
  defines grid options [read more](GridOptions.md)

- **ControlFields**: ICollection<ControlField>
  read only, get collection of ControlField\*, collected from all the panels

- **SaveBehaviour**: SaveBehaviourEnum [Save, SaveAndClose, SaveAndNew]
  allow the user to tweek the behaviour of the save button.

  - Save: default only save the current values
  - SaveAndClose: save the current values and close the current window
  - SaveAndNew: save the current values and create a new record

- **ShowType**: ShowTypeEnum [ModalAndRefresh, Modal, NotModal]
  allow the user to open a form with a specific behaviour:

  - ModalAndRefresh: default open a modal form and refresh the current page as soon and the modal form closes
  - Modal: open a modal form and do not refresh the current form after the modal closes
  - NotModal: open a form without Modal behaviour

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

- We can use passed argument to the form in the queries using (`%Arg1%`, `%Arg2%` ...), these argument should be indicated from the parent form in the **_Args_** of an IAction base entity ([ActionControl](ActionControl.md) and [GridOptions](GridOptions.md)).

# 2. TabPanels

Properties:

- **Caption**: string
  define the panel caption

- **ControlFields**: ICollection<ControlField>
  define the list of controls to add to the panel [ControlField](ControlField.md).

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
