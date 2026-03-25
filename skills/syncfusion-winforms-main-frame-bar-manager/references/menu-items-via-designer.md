# Adding Menu Items via Designer

This guide covers creating menus and toolbars using the Windows Forms designer with drag-and-drop functionality and visual configuration dialogs.

## Adding MainFrameBarManager to Form

### Via Toolbox Drag-and-Drop

1. Open your Windows Forms designer
2. Locate **MainFrameBarManager** in the Toolbox (under Syncfusion Tools)
3. Drag it onto your form - it appears in the component tray (bottom of designer)
4. Automatically adds all required assembly references
5. Rename the component if needed via Properties panel

### Access Designer

Once added, right-click the MainFrameBarManager component in the component tray to access designer options.

## Opening the Customize Dialog

The Customize dialog is the main interface for designer-based menu configuration:

1. Right-click MainFrameBarManager in component tray
2. Select **Customize...** from the context menu
3. Opens the Customize dialog with three tabs: **Toolbars**, **Commands**, and **Options**

## Toolbars Tab - Adding Bars

In the **Toolbars** tab:

1. Click **New** button to create a new bar
2. Enter a name (e.g., "File", "Edit", "View")
3. Select bar category/type
4. Click OK to add bar
5. Bar now appears in the Toolbars list and is ready for menu items

### Available Bar Types

- **Main Menu** - Top-level application menu bar
- **Toolbar** - Standard toolbar
- **Status Bar** - Bottom status display area
- **Floating** - Detachable toolbar

## Commands Tab - Adding Menu Items

In the **Commands** tab:

1. Select category from left list (or create new)
2. Available commands appear in the center list
3. Drag commands to bars shown on the right
4. Commands automatically added to selected bar

### Creating Menu Item Hierarchy

Drag items to create menu structure:
- Drop on a ParentBarItem to create sub-menu
- Drop on a Bar to create top-level item
- Drag existing items to reorder

## Options Tab - Configuration

In the **Options** tab:

- **Show Tooltips:** Enable/disable tooltip display
- **Show Shortcut Keys:** Display keyboard shortcuts in menus
- **Large Icons:** Use larger icon sizes in toolbars
- **Menu Animation:** Enable menu opening animation
- **Allow Customization:** Let end-users modify menu layout

## Adding Parent Menu Items

### Via Designer Steps

1. Open Customize dialog
2. Go to Commands tab
3. Create or select a ParentBarItem command
4. Drag to a Bar to create top-level menu
5. Drag additional items onto the ParentBarItem to nest them

### Example: Creating File Menu

1. In Customize dialog, create new bar "File"
2. In Commands, select "ParentBarItem" category
3. Drag "File" item to the File bar
4. Drag "New", "Open", "Save", "Exit" items onto the File parent
5. Items appear indented under File in the hierarchy view

## Adding Dropdown Items

### Creating DropDown Items

1. Open Customize dialog
2. Select DropDownBarItem from commands
3. Drag to desired location
4. Right-click the added DropDownBarItem
5. Select **Edit** to configure popup control

### Assigning Popup Control

1. In DropDownBarItem properties
2. Set **PopupControlContainer** property
3. Choose from available controls (ColorPickerUIAdv, etc.)
4. Configure control properties as needed

## Adding Combo/Text Items

### ComboBoxBarItem Setup

1. Drag ComboBoxBarItem to toolbar
2. Right-click to access properties
3. Click ellipsis on **ChoiceList** property
4. String Collection Editor opens
5. Add items (one per line)
6. Click OK

### TextBoxBarItem Setup

1. Drag TextBoxBarItem to toolbar
2. Set **TextBoxValue** property to default text
3. Set width and other display properties
4. Handle TextChanged event for user input

## Arranging Controls Through Drag-and-Drop (.NET Core)

In .NET Core, ParentBarItem can be directly dragged into the form through the Customize dialog:

1. Open Customize dialog
2. Select a ParentBarItem from Commands
3. Drag item onto the form surface
4. Item appears as draggable control in designer
5. Reposition and resize as needed
6. Properties remain editable

This simplified UI makes visual menu layout easier for .NET Core projects.

## Configuring Properties

### Property Panel Configuration

Select any bar or menu item in the designer and modify in the Properties panel:

| Property | Usage |
|----------|-------|
| **Text** | Display text for the item |
| **Name** | Identifier for accessing in code |
| **Shortcut** | Keyboard shortcut (if applicable) |
| **Tooltip** | Tooltip text or SuperToolTip instance |
| **ShowTooltip** | Enable/disable tooltip |
| **Checked** | For checkable menu items |
| **Enabled** | Enable/disable the item |
| **Category** | Organizational category |

## Previewing the Menu

To preview your menu at runtime:

1. Build the project (Ctrl+Shift+B)
2. Press F5 to run the application
3. Menu appears as configured
4. Test item clicks and keyboard shortcuts

If preview doesn't show menu, verify:
- Form.IsMdIContainer is false (unless MDI application)
- MainFrameBarManager.Form property is set to current form

## Separators and Spacing

### Adding Visual Separators

1. In Customize dialog, Commands tab
2. Select StaticBarItem category
3. Look for separator option (usually "-" character)
4. Drag to menu to create visual divider

### Grouping Related Items

Arrange visually related items together:
- File operations (New, Open, Save) grouped together
- Separator
- Application operations (Settings, Exit) grouped together

## Advanced: Editing Item Collections

### Property Collection Editor

For complex item hierarchies, use the Collection Editor:

1. Select bar or parent item
2. In Properties, find **Items** property
3. Click ellipsis to open Collection Editor
4. Add/remove/reorder items visually
5. Click OK to apply

## Best Practices for Designer-Based Creation

1. **Start with Bars:** Create all bars first (File, Edit, View, etc.)
2. **Create Parent Items:** Add ParentBarItem for main menus
3. **Organize Commands:** Group related commands by category
4. **Add Children:** Drag specific items into parents
5. **Test Hierarchy:** Verify nesting is correct before proceeding
6. **Save Frequently:** Designer changes auto-save to designer.cs

## Switching Between Designer and Code

The designer creates designer.cs code automatically:
- Modifications in designer reflect in designer.cs
- You can also edit designer.cs directly
- Avoid manual edits to designer.cs to prevent designer conflicts

Both code-based and designer-based approaches are compatible and can be mixed in the same application.
