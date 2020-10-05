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
