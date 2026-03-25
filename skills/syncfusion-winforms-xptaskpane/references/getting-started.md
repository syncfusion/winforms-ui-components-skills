# Getting Started with XPTaskPane

## Assembly Dependencies

XPTaskPane requires the following assembly references in your Windows Forms project:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**Via NuGet:**
```
Install-Package Syncfusion.Tools.Windows
```

This package includes all required assemblies for Windows Forms controls.

## Adding XPTaskPane via Form Designer

The easiest way to add XPTaskPane to your application:

1. **Open Toolbox** - Locate the Toolbox in Visual Studio
2. **Find XPTaskPane** - Search in Syncfusion Windows Forms category
3. **Drag to Form** - Drag XPTaskPane onto your form designer
4. **Assemblies Added Automatically** - All required references are added
5. **Add Pages** - Click "Add Page" in Smart Tags to create task pages
6. **Design Pages** - Double-click each page in the designer to add controls

**Automatic WizardContainer Creation:**
When you drop XPTaskPane, a WizardContainer is automatically created and assigned as TaskPanePageContainer.

## Adding XPTaskPane Programmatically

For runtime or code-based initialization:

**Step 1: Include Namespace**

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

**Step 2: Create XPTaskPane Instance**

```csharp
XPTaskPane xpTaskPane1 = new XPTaskPane();
this.Controls.Add(xpTaskPane1);
```

```vb
Dim xpTaskPane1 As XPTaskPane = New XPTaskPane()
Me.Controls.Add(xpTaskPane1)
```

## Creating WizardContainer and Setting as TaskPanePageContainer

XPTaskPane requires a container control to host the task pages. WizardContainer serves this purpose:

```csharp
// Create container
WizardContainer wizardContainer1 = new WizardContainer();

// Add container to XPTaskPane
this.xpTaskPane1.Controls.Add(this.wizardContainer1);

// Assign as TaskPanePageContainer
this.xpTaskPane1.TaskPanePageContainer = this.wizardContainer1;
```

```vb
' Create container
Dim wizardContainer1 As WizardContainer = New WizardContainer()

' Add container to XPTaskPane
Me.xpTaskPane1.Controls.Add(Me.wizardContainer1)

' Assign as TaskPanePageContainer
Me.xpTaskPane1.TaskPanePageContainer = Me.wizardContainer1
```

**Why WizardContainer?** It provides the infrastructure needed to manage multiple XPTaskPage instances with proper layout and visibility control.

## Creating XPTaskPage Instances

Create individual task pages and add them to the container:

```csharp
// Create first page
XPTaskPage xpTaskPage1 = new XPTaskPage();
xpTaskPage1.Title = "Page One";
wizardContainer1.Controls.Add(xpTaskPage1);

// Create second page
XPTaskPage xpTaskPage2 = new XPTaskPage();
xpTaskPage2.Title = "Page Two";
wizardContainer1.Controls.Add(xpTaskPage2);
```

```vb
' Create first page
Dim xpTaskPage1 As XPTaskPage = New XPTaskPage()
xpTaskPage1.Title = "Page One"
wizardContainer1.Controls.Add(xpTaskPage1)

' Create second page
Dim xpTaskPage2 As XPTaskPage = New XPTaskPage()
xpTaskPage2.Title = "Page Two"
wizardContainer1.Controls.Add(xpTaskPage2)
```

## Assigning Pages to TaskPages Collection

After creating pages, assign them to the XPTaskPane.TaskPages collection:

```csharp
this.xpTaskPane1.TaskPages = new XPTaskPage[] {
    xpTaskPage1,
    xpTaskPage2
};
```

```vb
Me.xpTaskPane1.TaskPages = New XPTaskPage() {
    xpTaskPage1,
    xpTaskPage2
}
```

**Important:** Pages must be added to the container AND the TaskPages collection for proper functionality.

## Complete Initialization Example

```csharp
public partial class Form1 : Form
{
    private XPTaskPane xpTaskPane1;
    private WizardContainer wizardContainer1;

    public Form1()
    {
        InitializeComponent();
        InitializeXPTaskPane();
    }

    private void InitializeXPTaskPane()
    {
        // Create and add XPTaskPane
        xpTaskPane1 = new XPTaskPane();
        xpTaskPane1.Dock = DockStyle.Fill;
        this.Controls.Add(xpTaskPane1);

        // Create and assign container
        wizardContainer1 = new WizardContainer();
        xpTaskPane1.Controls.Add(wizardContainer1);
        xpTaskPane1.TaskPanePageContainer = wizardContainer1;

        // Create pages
        XPTaskPage page1 = new XPTaskPage();
        page1.Title = "First Task";
        wizardContainer1.Controls.Add(page1);

        XPTaskPage page2 = new XPTaskPage();
        page2.Title = "Second Task";
        wizardContainer1.Controls.Add(page2);

        // Set TaskPages collection
        xpTaskPane1.TaskPages = new XPTaskPage[] { page1, page2 };
    }
}
```

## Adding Controls to Task Pages

Once pages are created, add controls to them:

```csharp
// Add button to page1
Button btn = new Button();
btn.Text = "Click Me";
btn.Location = new Point(10, 10);
page1.Controls.Add(btn);

// Add label to page2
Label lbl = new Label();
lbl.Text = "Welcome to Page 2";
lbl.Location = new Point(10, 10);
page2.Controls.Add(lbl);
```

**Page Content Tips:**
- Pages have their own Controls collection
- Set Location and Size for child controls
- Pages expand to fill available space in container
- Use Dock and Anchor properties for responsive layouts

## Common Initialization Gotchas

**Issue: Pages not appearing**
- Solution: Ensure WizardContainer is assigned to TaskPanePageContainer
- Solution: Verify pages are added to both Controls and TaskPages collection

**Issue: Toolbar buttons not visible**
- Solution: Check HeaderLeftToolbar and HeaderRightToolbar visibility
- Solution: Set appropriate button Visible properties

**Issue: XPTaskPane not sizing properly**
- Solution: Set Dock property or manually set Width/Height
- Solution: Ensure parent container allows size changes

**Next:** Learn to manage pages and customize their properties in page-management.md
