# Getting Started with SplitButton

This guide covers the basic setup and implementation of the Syncfusion WinForms SplitButton control, including both Designer and Code approaches.

## Assembly References

The SplitButton control requires the following assemblies:

**Required Assemblies:**
- `Syncfusion.Shared.Base`
- `Syncfusion.Shared.Windows`
- `Syncfusion.Tools.Base`
- `Syncfusion.Tools.Windows`

**NuGet Package:**
Install via NuGet Package Manager:
```
Install-Package Syncfusion.Tools.Windows
```

The above NuGet package automatically includes all required dependencies.

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Adding SplitButton Through Designer

### Step 1: Add Control from Toolbox

1. Open your Windows Forms application in Visual Studio
2. Locate **SplitButton** in the toolbox (under Syncfusion Controls section)
3. Drag and drop the SplitButton onto your form design surface
4. Visual Studio automatically adds the required assembly references

The dependent assemblies are added automatically when you drop the control onto the form.

### Step 2: Configure Properties

Use the **Properties** window to configure the SplitButton:

**Basic Properties:**
- `Name`: Set control identifier (e.g., "splitButton1")
- `Text`: Set button caption (e.g., "Actions", "Save", "Options")
- `Size`: Set button dimensions (e.g., 120, 40)
- `Location`: Position on form (e.g., 20, 20)
- `Style`: Choose visual theme (Office2016Colorful, Metro, etc.)

### Step 3: Add Dropdown Items

Add dropdown menu items using the **DropDownItems** property:

1. In Properties window, locate **DropDownItems** property
2. Click the ellipsis (...) button to open the Items Collection Editor
3. Click "Add" to create new items (ToolStripMenuItem)
4. Set properties for each item:
   - `Text`: Menu item label
   - `Name`: Item identifier
   - `Image`: Optional icon

**Example Items:**
- Item 1: Text = "Save"
- Item 2: Text = "Save As..."
- Item 3: Text = "Save All"

### Step 4: Run Application

Press F5 to run your application. The SplitButton will display with:
- Primary button showing the Text property value
- Dropdown arrow that opens the menu when clicked
- Menu items displayed when dropdown is activated

## Adding SplitButton Through Code

### Step 1: Add Assembly References

Manually add the required assembly references to your project if they're not already present:

1. Right-click on References in Solution Explorer
2. Select "Add Reference"
3. Browse to Syncfusion assemblies location or use NuGet Package Manager
4. Add the six required assemblies listed above

Add the namespace to your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

### Step 2: Create SplitButton Instance

In your form's constructor or Load event, create and configure the SplitButton:

**C# Example:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create SplitButton instance
    SplitButton splitButton = new SplitButton();
    
    // Set basic properties
    splitButton.Name = "splitButton1";
    splitButton.Text = "SplitButton";
    splitButton.Location = new System.Drawing.Point(236, 115);
    splitButton.Size = new System.Drawing.Size(154, 61);
    splitButton.ThemeName = "Office2019Colorful";
    
    // Add to form
    this.Controls.Add(splitButton);
}
```

**VB.NET Example:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Create SplitButton instance
    Dim splitButton As New SplitButton()
    
    ' Set basic properties
    splitButton.Name = "splitButton1"
    splitButton.Text = "SplitButton"
    splitButton.Location = New System.Drawing.Point(236, 115)
    splitButton.Size = New System.Drawing.Size(154, 61)
    splitButton.ThemeName = "Office2019Colorful"
    
    ' Add to form
    Me.Controls.Add(splitButton)
End Sub
```

## Adding and Removing Dropdown Items

### Adding Items Dynamically

Use the `DropDownItems.Add()` method to add menu items at runtime:

**C# Example:**
```csharp
public partial class Form1 : Form
{
    SplitButton splitButton;
    private ToolStripMenuItem toolstripitem1;
    private ToolStripMenuItem toolstripitem2;
    private ToolStripMenuItem toolstripitem3;
    private ToolStripMenuItem toolstripitem4;
    private ToolStripMenuItem toolstripitem5;
    
    public Form1()
    {
        InitializeComponent();
        
        // Create SplitButton
        splitButton = new SplitButton();
        splitButton.Location = new System.Drawing.Point(236, 115);
        splitButton.Name = "splitButton1";
        splitButton.Size = new System.Drawing.Size(154, 61);
        splitButton.Text = "Countries";
        splitButton.ThemeName = "Office2019Colorful";
        
        // Create dropdown items
        toolstripitem1 = new ToolStripMenuItem();
        toolstripitem1.Name = "toolstripitem1";
        toolstripitem1.Text = "Australia";
        
        toolstripitem2 = new ToolStripMenuItem();
        toolstripitem2.Name = "toolstripitem2";
        toolstripitem2.Text = "Europe";
        
        toolstripitem3 = new ToolStripMenuItem();
        toolstripitem3.Name = "toolstripitem3";
        toolstripitem3.Text = "India";
        
        toolstripitem4 = new ToolStripMenuItem();
        toolstripitem4.Name = "toolstripitem4";
        toolstripitem4.Text = "USA";
        
        toolstripitem5 = new ToolStripMenuItem();
        toolstripitem5.Name = "toolstripitem5";
        toolstripitem5.Text = "UK";
        
        // Add items to SplitButton dropdown
        splitButton.DropDownItems.Add(toolstripitem1);
        splitButton.DropDownItems.Add(toolstripitem2);
        splitButton.DropDownItems.Add(toolstripitem3);
        splitButton.DropDownItems.Add(toolstripitem4);
        splitButton.DropDownItems.Add(toolstripitem5);
        
        // Add SplitButton to form
        this.Controls.Add(splitButton);
    }
}
```

**VB.NET Example:**
```vb
Public Partial Class Form1
    Inherits Form
    
    Private splitButton As SplitButton
    Private toolstripitem1 As ToolStripMenuItem
    Private toolstripitem2 As ToolStripMenuItem
    Private toolstripitem3 As ToolStripMenuItem
    Private toolstripitem4 As ToolStripMenuItem
    Private toolstripitem5 As ToolStripMenuItem
    
    Public Sub New()
        InitializeComponent()
        
        ' Create SplitButton
        splitButton = New SplitButton()
        splitButton.Location = New System.Drawing.Point(236, 115)
        splitButton.Name = "splitButton1"
        splitButton.Size = New System.Drawing.Size(154, 61)
        splitButton.Text = "Countries"
        splitButton.ThemeName = "Office2019Colorful"
        
        ' Create dropdown items
        toolstripitem1 = New ToolStripMenuItem()
        toolstripitem1.Name = "toolstripitem1"
        toolstripitem1.Text = "Australia"
        
        toolstripitem2 = New ToolStripMenuItem()
        toolstripitem2.Name = "toolstripitem2"
        toolstripitem2.Text = "Europe"
        
        toolstripitem3 = New ToolStripMenuItem()
        toolstripitem3.Name = "toolstripitem3"
        toolstripitem3.Text = "India"
        
        toolstripitem4 = New ToolStripMenuItem()
        toolstripitem4.Name = "toolstripitem4"
        toolstripitem4.Text = "USA"
        
        toolstripitem5 = New ToolStripMenuItem()
        toolstripitem5.Name = "toolstripitem5"
        toolstripitem5.Text = "UK"
        
        ' Add items to SplitButton dropdown
        splitButton.DropDownItems.Add(toolstripitem1)
        splitButton.DropDownItems.Add(toolstripitem2)
        splitButton.DropDownItems.Add(toolstripitem3)
        splitButton.DropDownItems.Add(toolstripitem4)
        splitButton.DropDownItems.Add(toolstripitem5)
        
        ' Add SplitButton to form
        Me.Controls.Add(splitButton)
    End Sub
End Class
```

### Removing Items Dynamically

Remove items using `DropDownItems.Remove()` or `DropDownItems.RemoveAt()` methods:

**C# Example:**
```csharp
private void RemoveButton_Click(object sender, EventArgs e)
{
    // Remove specific item by reference
    splitButton.DropDownItems.Remove(this.toolstripitem2);
    
    // Or remove by index
    // splitButton.DropDownItems.RemoveAt(1);
}
```

**VB.NET Example:**
```vb
Private Sub RemoveButton_Click(ByVal sender As Object, ByVal e As EventArgs)
    ' Remove specific item by reference
    splitButton.DropDownItems.Remove(Me.toolstripitem2)
    
    ' Or remove by index
    ' splitButton.DropDownItems.RemoveAt(1)
End Sub
```

## Common Configuration Options

### Basic Setup Properties

**Set Button Text:**
```csharp
splitButton1.Text = "Save";
```

**Set Button Size:**
```csharp
splitButton1.Size = new Size(120, 40);
```

**Set Location:**
```csharp
splitButton1.Location = new Point(50, 100);
```

**Enable/Disable:**
```csharp
splitButton1.Enabled = true; // or false
```

### Quick Setup Pattern

For rapid implementation, use this pattern:

```csharp
// Create and configure in one block
SplitButton btn = new SplitButton
{
    Name = "mySplitButton",
    Text = "Options",
    Size = new Size(100, 35),
    Location = new Point(20, 20),
    ThemeName = "Office2019Colorful"
};

// Add items
btn.DropDownItems.AddRange(new ToolStripItem[]
{
    new ToolStripMenuItem("Option 1"),
    new ToolStripMenuItem("Option 2"),
    new ToolStripMenuItem("Option 3")
});

// Add to form
this.Controls.Add(btn);
```

## Troubleshooting

**Issue: Control not visible in toolbox**
- Ensure Syncfusion WinForms assemblies are installed
- Rebuild the toolbox (Tools → Choose Toolbox Items)
- Check that project targets .NET Framework 4.5+ or .NET 6.0+

**Issue: Assembly reference errors**
- Verify all six required assemblies are referenced
- Use NuGet Package Manager for easier dependency management
- Ensure Syncfusion version matches across all assemblies

**Issue: Dropdown items not appearing**
- Verify items are added to `DropDownItems` collection (not `Items`)
- Check that Text property of menu items is set
- Ensure dropdown arrow portion of button is being clicked

**Issue: Theme not applying**
- Use `Style` property for built-in themes
- Use `ThemeName` property for Office2019 themes
- Ensure theme name string matches exactly (case-sensitive)

## Next Steps

- **Button Modes:** Read [button-modes.md](button-modes.md) for Normal and Toggle mode configuration
- **Dynamic Captions:** Read [button-caption.md](button-caption.md) to update button text from dropdown selection
- **Visual Styling:** Read [visual-styles.md](visual-styles.md) for theme and appearance options
- **Customization:** Read [advanced-customization.md](advanced-customization.md) for custom rendering
