# 6. ActionControl

Action control are a custom buttons we can add to our form on the very top right next to the edit and save buttons, or tile buttons we can add the the main form, to execute a custom action, these actions are IAction based actions means they need to follow IAction interface or contract.

Properties:

- **Name** string, control name.
- **Caption** string, caption or label.
- **ToolTip** string, tooltip to show on hover over the control
- **ImagePath** string, icon or image path to show inside the button
- **BackColor** Color, tile back color **this propery is reserved to Tiles and not buttons**
- **ItemSize** TileItemSize, tile size enum [Default = 0, Small = 1, Medium = 2, Wide = 3, Large = 4] **this propery is reserved to Tiles and not buttons**

and moste importantly too the IAction interface properties:

## 6.1. IAction

Properties:

- **SQLExecute** string, contains a query to execute.
- **FormToOpen** string, form name (identifier) to open.
- **ReportToOpen** string, report name (identifier) to open.
- **Args** string, comma separated args to pass to one of the obove targets.

we can also add a drop down menu or a group of buttons to group some buttons that have the same context for example with a "ActionControlGroup":

## 6.2. ActionControlGroup

Properties:

- **Caption** string, the grouping caption.
- **ButtonsGroups** ICollection<ActionControlGroup>, a second level group.
- **Buttons** ICollection<ActionControl>, a collection of button to add to the groupping.

## 6.3. ActionControlTileGroup

Properties:

- **Caption** string, the grouping caption.
- **TilesGroups** ICollection<ActionControlTileGroup>, a second level group.
- **Tiles** ICollection<ActionControl>, a collection of tiles to add to the groupping in the main form
