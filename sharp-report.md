# 1. Sharp Report

Is the core of the reporting system of SharpDesigner it allows you to create new report or use existing one.

## 1.1. DataBase table

Table name: REPORT
Columns:

- ID_REPORT
- ID_REPORT_CATEGORY
- ORDRE
- DESIGNATION
- FORM_NAME
- SQL_SELECT
- SQL_WHERE
- SQL_ORDERBY
- SP
- FILENAME
- MASK

## 1.2. Object Models

### 1.2.1. SharpReport

Properites:

- **Id** int represent the id of the report
- **Category** int represent the category of the report
- **ReportIndex** int represent the visible index of the report inside the list
- **Name** string represent the name of the report
- **Designation** string represent the description or title of the report
- **FileName** string represent the template or base report file name [read more](#report-template)
- **SqlSelect** string represent the SELECT part in the select statement of the report
- **SqlWhere** string represent the WHERE part in the select statemenet (the WHERE Key word will be added automatically do not include it)
- **SqlOrderby** string represent the ODERBY part in the select statement
- **StoredProcedure** string represent the stored procedure to be called instead of the select statement (not yet implemented)
- **Mask** ReportMask represent the mask or report controls and styling [read more](#reportmask)

### 1.2.2. ReportMask

Properties:

- **IsLandscape** bool indicates if the report should be in landscape mode or not (default is false)
- **ReportBandOptions** ReportBandOptions an object encapsulate group report options [read more](#reportbandoptions)
- **StyleSheets** List<SharpStyleSheet> represent the list of report style sheet [read more](#sharpstylesheet)
- **Controls** List<ReportControl> represent the list of controls to add to the report [read more](#reportcontrol)

Example:

```json
{
  "StyleSheets": [
    {
      "Name": "Title",
      "FontFamily": "Georgia",
      "FontSize": 16,
      "FontStyle": "Bold",
      "Padding": "0,0,0,0,"
    },
    {
      "Name": "Title2",
      "FontFamily": "Verdana",
      "FontSize": 12
    },
    {
      "Name": "Detail",
      "FontFamily": "Impact",
      "FontSize": 10
    }
  ],
  "Controls": [
    {
      "Type": "Text",
      "BandType": "PageHeaderBand",
      "Caption": "Report of",
      "StyleName": "Title"
    },
    {
      "Type": "Text",
      "BandType": "PageHeaderBand",
      "Caption": "USERS List",
      "StyleName": "Title2",
      "InNewLine": false
    },
    {
      "Type": "Text",
      "BandType": "PageHeaderBand",
      "Caption": "Date",
      "StyleName": "Title2",
      "BindingMember": "LOGIN"
    },
    {
      "Type": "Table",
      "TableOptions": {
        "Columns": [
        {
          "BindingMember": "PRENOM",
          "Width": 100
        },{
          "BindingMember": "NOM",
          "Width": 125
        },{
          "BindingMember": "LOGIN"
        }
        ],
        "Borders": "5"
      }
    }
  ]
}
```

### 1.2.3. ReportBandOptions

properties:

- **RepeatOnEveryPage** bool indicate wherther or not to repeat the group band (which contains the table header) on every page.
- **GroupFields** ICollection<string> comma separated strings indicating the grouping fields.

example:

down bellow the report mask example show how to create a grouped report and how to add the grouped member as a lable before each table group;

```json
{
  "IsLandscape": false,
  "Controls": [
  {
    "BandType": "GroupHeaderBand",
    "Caption": "CODE QUALITE: ",
  },
  {
    "BandType": "GroupHeaderBand",
    "BindingMember":'CODE_QUALITE',
    "InNewLine": false,
    "Style": {
        "Name": "Title",
        "FontFamily": "Georgia",
        "FontSize": 14,
        "FontStyle": "Bold"
      }
  },
    {
      "Type": "Table",
      "TableOptions": {
        "Columns": [
        {
          "BindingMember": "NOM",
          "Width": 100
        },{
          "BindingMember": "DATE_NAISSANCE",
          "Caption": "DATE NAISSANCE",
          "TextAlignment": "MiddleRight",
        },{
        "BindingMember": "CODE_QUALITE",
          "Caption": "CODE QUALITE",
        }
        ],
        "HeaderBorders": "8",
        "DetailBorders": "0"
      }
    }],
  "ReportBandOptions": {
    "RepeatOnEveryPage": true,
    "GroupFields": ['CODE_QUALITE']
  }
}
```

### 1.2.4. ReportControl

properties:

- **Type** string represent the type to control to add, possible values ["Table", "Text:Default"]
- **BandType** string represent to which band to add this control, possible values ["ReportHeaderBand", "PageHeaderBand", "DetailBand:default"]
- **Caption** string represent the text to show if the type of control is an unbound Text
- **DataSource** string represent the data source of the bound control, Default is the main select query
- **BindingMember** string represent the Binding member from the datasource to which the control is bound to
- **StyleName** string represent the style name to apply
- **Style** SharpStyleSheet represent the style sheet to apply [read more](#sharpstylesheet)
- **Position** string represent a comma seperated values for the x,y coordinate of the position of the control
- **InNewLine** bool default is true allow to write add the control in a new line or in the same line as the previous one
- **WordWrap** bool default is false allow the text to be wraped in a multiple lines
- **IsUnbound** bool represent a readonly property that determine if this control in bound or unbound control depending on the BindingMember and DataSource
- **TableOptions** ReportTableOptions contains extra table option if the control type is Table [read more](#tableoptions)

#### 1.2.4.1. TableOptions

properties:

- **StyleEvenRow**
- **StyleOddRow**
- **HeaderBorders** string that could contain commar separated int or the sum of ints to represent the following borders:
  (None = 0, Left = 1, Top = 2, Right = 4, Bottom = 8, All = 15)
- **DetailBorders** string that could contain commar separated int or the sum of ints to represent the following borders:
(None = 0, Left = 1, Top = 2, Right = 4, Bottom = 8, All = 15)
- **Columns** ICollection<Field> [read more](#field)
- **SummaryItems** ICollection<SummaryItem>
> not yet implemented!

#### 1.2.4.2. Field

properties:

- **Caption** string represent the column name
- **BindingMember** string represent the field name in the query result set that will be bound
- **Width** float represent the column width in the report
- **TextAlignment** TextAlignment text or int

  TopLeft = 1,
  TopCenter = 2,
  TopRight = 4,
  MiddleLeft = 16,
  MiddleCenter = 32,
  MiddleRight = 64,
  BottomLeft = 256,
  BottomCenter = 512,
  BottomRight = 1024,
  TopJustify = 2048,
  MiddleJustify = 4096,
  BottomJustify = 8096

### 1.2.5. SharpStyleSheet

properties:

- **Name** string the name of the stylesheet (used to identify the stylesheet)
- **FontFamily** string the font family to apply (default is 'Tahoma')
- **FontSize** float the font size to apply (default is 8)
- **Padding** string the padding to apply
- **FontStyle** FontStyle enum (Regular = 0, Bold = 1, Italic = 2, Underline = 4, Strikeout = 8)

### 1.2.6. Report Template

for now there is only one template located in Templates/Reports/TableAndPageHeaderReport.repx to select this template just add TableAndPageHeaderReport in the FILENAME column in the REPORT table
