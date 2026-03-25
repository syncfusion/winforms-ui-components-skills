# Getting Started with TabbedMDIManager

## Table of Contents
- [Installation](#installation)
- [Assembly References](#assembly-references)
- [Adding the Control](#adding-the-control)
- [Basic Setup Code](#basic-setup-code)
- [Creating MDI Children](#creating-mdi-children)
- [Common Issues](#common-issues)

## Installation

### Step 1: Install NuGet Packages

The easiest way to get started is through NuGet Package Manager:

```
Install-Package Syncfusion.Tools.Windows
```

Or via NuGet Package Manager UI in Visual Studio:
1. Right-click on your project → Manage NuGet Packages
2. Search for "Syncfusion.Windows"
3. Click Install

### Step 2: Required Assemblies

When you add the TabbedMDIManager control, these assemblies are automatically referenced:
- `Syncfusion.Grid.Base`
- `Syncfusion.Grid.Windows`
- `Syncfusion.Shared.Base`
- `Syncfusion.Shared.Windows`
- `Syncfusion.Tools.Base`
- `Syncfusion.Tools.Windows`

If adding manually, add all these references to your project.

## Assembly References

**Manual Reference Method:**
1. Right-click project → Add Reference
2. Navigate to Syncfusion installation folder (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\Assemblies`)
3. Select all assemblies listed above
4. Click Add

**Verify Installation:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

If this namespace resolves without errors, your installation is complete.

## Adding the Control

### Method 1: Designer (Recommended for Beginners)

1. Create a new Windows Forms Application in Visual Studio
2. Open the Designer for your main Form
3. Open the Toolbox panel
4. Search for "TabbedMDIManager" in the toolbox
5. Drag and drop onto your form

**What happens automatically:**
- Form's `IsMdiContainer` property is set to `true`
- TabbedMDIManager is added to the form
- AttachedTo property is automatically set to Form1

### Method 2: Code (Manual)

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TabbedMDIExample
{
    public partial class Form1 : Form
    {
        private TabbedMDIManager tabbedMDIManager;

        public Form1()
        {
            InitializeComponent();
            InitializeTabbedMDI();
        }

        private void InitializeTabbedMDI()
        {
            // Set this form as MDI container
            this.IsMdiContainer = true;

            // Create TabbedMDIManager instance
            tabbedMDIManager = new TabbedMDIManager();
            this.Controls.Add(tabbedMDIManager);

            // Attach to this MDI container
            tabbedMDIManager.AttachToMdiContainer(this);
        }
    }
}
```

## Basic Setup Code

### Complete Minimal Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace MDIApp
{
    public partial class MainForm : Form
    {
        private TabbedMDIManager tabbedMDIManager;

        public MainForm()
        {
            InitializeComponent();
            SetupMDI();
        }

        private void SetupMDI()
        {
            // 1. Set as MDI container
            this.IsMdiContainer = true;
            this.Text = "Tabbed MDI Application";

            // 2. Create and attach TabbedMDIManager
            tabbedMDIManager = new TabbedMDIManager();
            this.Controls.Add(tabbedMDIManager);
            tabbedMDIManager.AttachToMdiContainer(this);

            // 3. Enable themes
            tabbedMDIManager.ThemesEnabled = true;

            // 4. Show close and dropdown buttons
            tabbedMDIManager.CloseButtonVisible = true;
            tabbedMDIManager.DropDownButtonVisible = true;

            // 5. Create menu to add documents
            CreateMenu();
        }

        private void CreateMenu()
        {
            MenuStrip menuStrip = new MenuStrip();
            this.Controls.Add(menuStrip);

            ToolStripMenuItem fileMenu = menuStrip.Items.Add("File") as ToolStripMenuItem;
            fileMenu.DropDownItems.Add("New Document", null, (s, e) => CreateNewDocument());
            fileMenu.DropDownItems.Add("Exit", null, (s, e) => this.Close());
        }

        private void CreateNewDocument()
        {
            Form childForm = new Form();
            childForm.Text = "Document " + (this.MdiChildren.Length + 1);
            childForm.MdiParent = this;
            childForm.Show();
        }
    }
}
```

## Creating MDI Children

### Simple Child Form Creation

When you set `MdiParent`, the form automatically becomes a tabbed child:

```csharp
// Create child form
Form childForm = new Form();
childForm.Text = "Document 1";
childForm.MdiParent = this;  // Make it MDI child
childForm.Show();            // Shows as tab in TabbedMDI

// Result: New tab appears automatically
```

### Creating Multiple Children

```csharp
private void AddMultipleDocuments()
{
    // Add 3 new document tabs
    for (int i = 1; i <= 3; i++)
    {
        Form childForm = new Form();
        childForm.Text = "Document " + i;
        childForm.MdiParent = this;
        childForm.Show();
    }

    // Result: 3 tabs appear in the tab strip
}
```

### With Custom Content

```csharp
private void CreateDocumentWithContent()
{
    Form childForm = new Form();
    childForm.Text = "Rich Document";
    childForm.MdiParent = this;

    // Add content
    RichTextBox rtb = new RichTextBox();
    rtb.Dock = DockStyle.Fill;
    rtb.Text = "Enter your content here...";
    childForm.Controls.Add(rtb);

    childForm.Show();
}
```

### Accessing All MDI Children

```csharp
private void ProcessAllDocuments()
{
    // Get all open MDI children
    foreach (Form childForm in this.MdiChildren)
    {
        Console.WriteLine("Open: " + childForm.Text);
    }
}
```

## Icon Settings

Add icons to your tabs to make them more visually appealing:

```csharp
// Enable icons in tabs
tabbedMDIManager.UseIconsInTabs = true;

// Set icon size
tabbedMDIManager.ImageSize = new System.Drawing.Size(16, 16);

// Assign icon to form
Form childForm = new Form();
childForm.Text = "Document";
childForm.Icon = new Icon("path/to/icon.ico");
childForm.MdiParent = this;
childForm.Show();
```

## Common Issues

### Issue 1: TabbedMDIManager Not Visible
**Solution:** Ensure `IsMdiContainer` is set to `true` before adding the control.

```csharp
this.IsMdiContainer = true;  // Must be set first
tabbedMDIManager.AttachToMdiContainer(this);
```

### Issue 2: Child Forms Not Appearing as Tabs
**Solution:** Use `MdiParent` property to assign child to container.

```csharp
childForm.MdiParent = this;  // This is required
childForm.Show();
```

### Issue 3: Assemblies Not Found
**Solution:** Verify all required assemblies are referenced:
- Syncfusion.Tools.Windows
- Syncfusion.Tools.Base
- Syncfusion.Shared.Windows
- Syncfusion.Shared.Base
- Syncfusion.Grid.Windows
- Syncfusion.Grid.Base

### Issue 4: Designer Shows Error
**Solution:** Rebuild solution and close/reopen the designer. If persists, check all assemblies are installed via NuGet.

## Next Steps

Once you have the basic setup working:
1. **Explore Tab Alignment** to position tabs at different edges
2. **Learn about Tab Groups** for multiple document areas
3. **Customize Appearance** with colors and fonts
4. **Add Events** for advanced interactivity
5. **Implement Serialization** to save layouts between sessions
