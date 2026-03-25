# Getting Started with PopupMenu

This guide covers the foundational steps for adding and configuring PopupMenu in Windows Forms applications, including assembly references, designer and code-based implementation, and basic BarItem setup.

## Assembly References and Dependencies

### Required Assemblies

To use PopupMenu control, add these assemblies to your project:

**Core Assemblies:**
- `Syncfusion.Tools.Windows.dll`
- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Licensing.dll`
- `Syncfusion.SpellChecker.Base.dll`

**Assembly Location:**
```
{System Drive}:\Program Files (x86)\Syncfusion\Essential Studio\{Platform}\{Build Version}\precompiledassemblies\{Framework Version}
```

Example: `C:\Program Files (x86)\Syncfusion\Essential Studio\WindowsForms\20.1.0.47\precompiledassemblies\4.6.0`

## Adding PopupMenu Through Designer

The designer approach provides a visual way to configure PopupMenu with drag-and-drop functionality and property editors.

### Step 1: Add PopupMenu Control

1. Open your Windows Form in designer view
2. Locate the Syncfusion toolbox section: **"Syncfusion Windows {Version} Toolbox Essential Studio {Version}"**
3. Drag **PopupMenu** from the toolbox onto the form designer
4. The control appears in the component tray (non-visual component)
5. Required assemblies are added automatically to your project

### Step 2: Add Default ParentBarItem

PopupMenu requires a ParentBarItem container to hold menu items.

**Option 1: Smart Tag**
1. Click the PopupMenu control in the component tray
2. Click the Smart Tag arrow that appears
3. Select **"Add Default ParentBarItem..."**

**Option 2: Context Menu**
1. Right-click PopupMenu in the component tray
2. Select **"Add Default ParentBarItem..."**

A ParentBarItem is created and assigned to the PopupMenu.

### Step 3: Add BarItems to Menu

1. Select PopupMenu in the component tray
2. In the Properties panel, navigate to: **Misc → ParentBarItem → Items**
3. Click the ellipsis (...) button to open **BarItem Collection Editor**

**In BarItem Collection Editor:**
1. Click the dropdown arrow on the **Add** button
2. Select the desired BarItem type:
   - BarItem (standard menu item)
   - ParentBarItem (submenu container)
   - DropDownBarItem (custom dropdown)
   - ComboBoxBarItem (combo box)
   - ListBarItem (list selection)
   - StaticBarItem (label)
   - TextBoxBarItem (text input)
3. With the item selected, configure properties:
   - **Appearance → Text:** Set display text
   - **Appearance → Image:** Set icon (optional)
   - **Data → Shortcut:** Set keyboard shortcut (optional)

Repeat to add multiple menu items.

### Step 4: Add PopupMenusManager

PopupMenusManager associates popup menus with controls (RichTextBox, Button, Panel, etc.).

1. Drag **PopupMenusManager** from toolbox onto the form
2. It appears in the component tray

### Step 5: Associate PopupMenu with a Control

1. Add a control to your form (e.g., RichTextBox, Button, TextBox)
2. Select the control in designer
3. In Properties panel, find: **XP Menus → XPContextMenu on popupMenusManager1**
4. Select your PopupMenu from the dropdown

Now right-clicking the control will display your popup menu.

### Designer Example Output

The designer generates code in the `.Designer.cs` file:

```csharp
private void InitializeComponent()
{
    this.components = new System.ComponentModel.Container();
    this.popupMenu1 = new Syncfusion.Windows.Forms.Tools.XPMenus.PopupMenu(this.components);
    this.parentBarItem1 = new Syncfusion.Windows.Forms.Tools.XPMenus.ParentBarItem();
    this.barItem1 = new Syncfusion.Windows.Forms.Tools.XPMenus.BarItem();
    this.barItem2 = new Syncfusion.Windows.Forms.Tools.XPMenus.BarItem();
    this.popupMenusManager1 = new Syncfusion.Windows.Forms.Tools.XPMenus.PopupMenusManager(this.components);
    this.richTextBox1 = new System.Windows.Forms.RichTextBox();
    
    // popupMenu1
    this.popupMenu1.ParentBarItem = this.parentBarItem1;
    
    // parentBarItem1
    this.parentBarItem1.Items.AddRange(new Syncfusion.Windows.Forms.Tools.XPMenus.BarItem[] {
        this.barItem1,
        this.barItem2
    });
    this.parentBarItem1.SizeToFit = true;
    
    // barItem1
    this.barItem1.Text = "Cut";
    this.barItem1.SizeToFit = true;
    
    // barItem2
    this.barItem2.Text = "Copy";
    this.barItem2.SizeToFit = true;
    
    // richTextBox1
    this.popupMenusManager1.SetXPContextMenu(this.richTextBox1, this.popupMenu1);
}
```

## Adding PopupMenu Through Code

The code approach offers more control and is ideal for dynamic menu creation or programmatic configuration.

### Step 1: Add Assembly References

Manually add the required DLL references to your project:
1. Right-click **References** in Solution Explorer
2. Select **Add Reference**
3. Browse to the assembly location and add all required DLLs

### Step 2: Import Namespaces

```csharp
using Syncfusion.Windows.Forms.Tools.XPMenus;
using Syncfusion.Windows.Forms;
using System.Windows.Forms;
```

### Step 3: Declare Component Fields

```csharp
public partial class Form1 : Form
{
    private PopupMenu popupMenu1;
    private ParentBarItem parentBarItem1;
    private BarItem barItem1;
    private BarItem barItem2;
    private BarItem barItem3;
    private RichTextBox richTextBox1;
    private PopupMenusManager popupMenusManager1;
}
```

### Step 4: Initialize and Configure

```csharp
public Form1()
{
    InitializeComponent();
    
    // Initialize components
    this.popupMenu1 = new PopupMenu(this.components);
    this.parentBarItem1 = new ParentBarItem();
    this.barItem1 = new BarItem();
    this.barItem2 = new BarItem();
    this.barItem3 = new BarItem();
    this.richTextBox1 = new RichTextBox();
    this.popupMenusManager1 = new PopupMenusManager(this.components);
    
    // Configure PopupMenu
    this.popupMenu1.ParentBarItem = this.parentBarItem1;
    
    // Configure BarItem 1
    this.barItem1.Text = "File";
    this.barItem1.SizeToFit = true;
    this.barItem1.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\file.png"));
    
    // Configure BarItem 2
    this.barItem2.Text = "Edit";
    this.barItem2.SizeToFit = true;
    this.barItem2.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\edit.png"));
    
    // Configure BarItem 3
    this.barItem3.Text = "Help";
    this.barItem3.SizeToFit = true;
    this.barItem3.Image = new ImageExt(System.Drawing.Image.FromFile(@"icons\help.png"));
    
    // Add items to ParentBarItem
    this.parentBarItem1.MetroColor = System.Drawing.Color.LightSkyBlue;
    this.parentBarItem1.SizeToFit = true;
    this.parentBarItem1.Items.AddRange(new BarItem[] {
        this.barItem1,
        this.barItem2,
        this.barItem3
    });
    
    // Configure RichTextBox
    this.richTextBox1.Size = new System.Drawing.Size(400, 300);
    this.richTextBox1.Location = new System.Drawing.Point(10, 10);
    
    // Associate PopupMenu with RichTextBox
    this.popupMenusManager1.SetXPContextMenu(this.richTextBox1, this.popupMenu1);
    
    // Add RichTextBox to form
    this.Controls.Add(this.richTextBox1);
    
    // Configure form
    this.ClientSize = new System.Drawing.Size(500, 400);
    this.Text = "PopupMenu Example";
}
```

### VB.NET Code Example

```vb
'Declaration
Private popupMenu1 As Syncfusion.Windows.Forms.Tools.XPMenus.PopupMenu
Private parentBarItem1 As Syncfusion.Windows.Forms.Tools.XPMenus.ParentBarItem
Private barItem1 As Syncfusion.Windows.Forms.Tools.XPMenus.BarItem
Private barItem2 As Syncfusion.Windows.Forms.Tools.XPMenus.BarItem
Private barItem3 As Syncfusion.Windows.Forms.Tools.XPMenus.BarItem
Private richTextBox1 As System.Windows.Forms.RichTextBox
Private popupMenusManager1 As Syncfusion.Windows.Forms.Tools.XPMenus.PopupMenusManager

Public Sub New()
    InitializeComponent()
    
    'Initializing
    Me.popupMenu1 = New Syncfusion.Windows.Forms.Tools.XPMenus.PopupMenu(Me.components)
    Me.parentBarItem1 = New Syncfusion.Windows.Forms.Tools.XPMenus.ParentBarItem()
    Me.barItem1 = New Syncfusion.Windows.Forms.Tools.XPMenus.BarItem()
    Me.barItem2 = New Syncfusion.Windows.Forms.Tools.XPMenus.BarItem()
    Me.barItem3 = New Syncfusion.Windows.Forms.Tools.XPMenus.BarItem()
    Me.richTextBox1 = New System.Windows.Forms.RichTextBox()
    Me.popupMenusManager1 = New Syncfusion.Windows.Forms.Tools.XPMenus.PopupMenusManager(Me.components)
    
    ' popupMenu1
    Me.popupMenu1.ParentBarItem = Me.parentBarItem1
    
    ' barItem1
    Me.barItem1.Image = New ImageExt(System.Drawing.Image.FromFile("..\..\..\.File.png"))
    Me.barItem1.SizeToFit = True
    Me.barItem1.Text = "File"
    
    ' barItem2
    Me.barItem2.Image = New ImageExt(System.Drawing.Image.FromFile("..\..\..\.Edit.png"))
    Me.barItem2.SizeToFit = True
    Me.barItem2.Text = "Edit"
    
    ' barItem3
    Me.barItem3.Image = New ImageExt(System.Drawing.Image.FromFile("..\..\..\.Help.png"))
    Me.barItem3.SizeToFit = True
    Me.barItem3.Text = "Help"
    
    ' parentBarItem1
    Me.parentBarItem1.MetroColor = System.Drawing.Color.LightSkyBlue
    Me.parentBarItem1.SizeToFit = True
    Me.parentBarItem1.Items.AddRange(New Syncfusion.Windows.Forms.Tools.XPMenus.BarItem() { _
        Me.barItem1, Me.barItem2, Me.barItem3})
    
    ' richTextBox1
    Me.richTextBox1.Size = New System.Drawing.Size(100, 96)
    Me.popupMenusManager1.SetXPContextMenu(Me.richTextBox1, Me.popupMenu1)
    
    ' Form1
    Me.ClientSize = New System.Drawing.Size(282, 253)
    Me.Text = "PopupMenu"
    Me.Controls.Add(Me.richTextBox1)
End Sub
```

## Installing via NuGet Package

NuGet provides an alternative to manual assembly references.

### NuGet Package Name
- **Syncfusion.Tools.WinForms** (contains PopupMenu and related controls)

### Installation Steps

**Using Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**Using NuGet Package Manager UI:**
1. Right-click project → **Manage NuGet Packages**
2. Search for **"Syncfusion.Tools.WinForms"**
3. Click **Install**

Refer to [NuGet installation guide](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages) for detailed steps.

## Basic BarItem Configuration

### Setting Text

```csharp
barItem1.Text = "Save";
```

### Adding Images

Use the `ImageExt` class to load images:

```csharp
barItem1.Image = new ImageExt(System.Drawing.Image.FromFile(@"path\to\icon.png"));
```

**From Resources:**
```csharp
barItem1.Image = new ImageExt(Properties.Resources.SaveIcon);
```

### SizeToFit Property

Always set `SizeToFit = true` for BarItems to auto-size based on content:

```csharp
barItem1.SizeToFit = true;
parentBarItem1.SizeToFit = true;
```

### MetroColor for ParentBarItem

```csharp
parentBarItem1.MetroColor = System.Drawing.Color.LightSkyBlue;
```

## Testing Your PopupMenu

1. Run the application (F5)
2. Right-click on the associated control (e.g., RichTextBox)
3. The popup menu should appear with your configured items
4. Click a menu item (you'll need to add Click event handlers for functionality)

## Next Steps

- **Add Multi-Level Menus:** See multilevel-menus.md for creating hierarchical menu structures
- **Configure BarItem Types:** See baritem-types.md for all 7 BarItem types
- **Add Keyboard Shortcuts:** See keyboard-interaction.md for shortcuts and mnemonics
- **Handle Events:** Add Click event handlers to implement menu item functionality

## Common Getting Started Issues

**Issue: PopupMenu doesn't appear**
- Verify PopupMenusManager.SetXPContextMenu() is called
- Check that ParentBarItem is assigned to PopupMenu
- Ensure the target control is visible and enabled

**Issue: Assembly reference errors**
- Verify all required DLLs are referenced
- Check that assembly versions match
- Ensure .NET Framework version compatibility

**Issue: License key error**
- Register license key in application startup (v16.2.0.x+)
- Verify license key is valid and not expired

**Issue: Designer doesn't show PopupMenu**
- PopupMenu is a non-visual component; it appears in component tray, not on form
- If toolbox is empty, reinstall Syncfusion or configure toolbox manually
