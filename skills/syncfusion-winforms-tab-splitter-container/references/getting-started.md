# Getting Started with TabSplitterContainer

## Overview

The `TabSplitterContainer` control provides a Visual Studio 2008-style tabbed split view that allows users to view and edit different aspects of the same document simultaneously. This reference guides you through the initial setup and basic implementation of the TabSplitterContainer control in WinForms applications.

## Assembly and NuGet Package Requirements

### Required Assemblies

To use the TabSplitterContainer control, you must reference the following assemblies in your WinForms project:

**Primary Assembly:**
- `Syncfusion.Tools.Windows.dll`

**Dependency Assemblies:**
- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`

### NuGet Package Installation

Install the TabSplitterContainer control via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Tools.WinForms
```

This package includes all required dependencies automatically.

### Namespace Import

Add the following namespace directive to your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

## Adding TabSplitterContainer via Designer

### Using the Toolbox

1. **Open the Toolbox** in Visual Studio (View → Toolbox or Ctrl+Alt+X)
2. **Locate TabSplitterContainer** under the "Syncfusion Controls" or "Tools" category
3. **Drag and drop** the TabSplitterContainer onto your form
4. The control will be added with default settings and an empty layout

### Using Smart Tags

The TabSplitterContainer designer provides smart tags for quick page management:

**Smart Tag Actions:**
- **Add Primary Page**: Creates a new TabSplitterPage in the PrimaryPages collection
- **Add Secondary Page**: Creates a new TabSplitterPage in the SecondaryPages collection

**To Access Smart Tags:**
1. Select the TabSplitterContainer control on the form
2. Click the smart tag glyph (small arrow) in the upper-right corner
3. Choose "Add primary page" or "Add secondary page"

### Design-Time Page Configuration

Use the Properties window to configure page collections:

1. Select the TabSplitterContainer control
2. In the Properties window, locate the **PrimaryPages** property
3. Click the ellipsis button (...) to open the TabSplitterPage Collection Editor
4. Add, remove, or configure pages using the collection editor

Repeat the same process for the **SecondaryPages** property.

## Adding TabSplitterContainer via Code

### Basic Code Implementation

When you need to create the TabSplitterContainer programmatically (dynamic UI, code-first approach), use this pattern:

```csharp
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class DocumentEditorForm : Form
{
    private TabSplitterContainer tabSplitterContainer1;
    
    public DocumentEditorForm()
    {
        InitializeComponent();
        InitializeTabSplitter();
    }
    
    private void InitializeTabSplitter()
    {
        // Create the TabSplitterContainer instance
        this.tabSplitterContainer1 = new TabSplitterContainer();
        
        // Configure basic properties
        this.tabSplitterContainer1.Location = new System.Drawing.Point(10, 10);
        this.tabSplitterContainer1.Size = new System.Drawing.Size(800, 600);
        this.tabSplitterContainer1.Name = "tabSplitterContainer1";
        
        // Add to form
        this.Controls.Add(this.tabSplitterContainer1);
    }
}
```

## Creating and Adding TabSplitterPages

### Understanding Page Collections

The TabSplitterContainer uses two separate collections for managing split view pages:

- **PrimaryPages**: The primary collection of tab pages (typically left or top pane)
- **SecondaryPages**: The secondary collection of tab pages (typically right or bottom pane)

### Creating TabSplitterPage Instances

```csharp
private void CreateTabPages()
{
    // Create primary pages
    TabSplitterPage primaryPage1 = new TabSplitterPage();
    primaryPage1.Text = "XAML";
    primaryPage1.Name = "xamlPage";
    
    TabSplitterPage primaryPage2 = new TabSplitterPage();
    primaryPage2.Text = "Code";
    primaryPage2.Name = "codePage";
    
    // Create secondary pages
    TabSplitterPage secondaryPage1 = new TabSplitterPage();
    secondaryPage1.Text = "Design";
    secondaryPage1.Name = "designPage";
    
    TabSplitterPage secondaryPage2 = new TabSplitterPage();
    secondaryPage2.Text = "Properties";
    secondaryPage2.Name = "propertiesPage";
}
```

### Adding Pages with AddRange

Use the `AddRange` method for efficient bulk page addition:

```csharp
private void AddPagesToSplitter()
{
    // Create pages
    TabSplitterPage xamlPage = new TabSplitterPage();
    xamlPage.Text = "XAML";
    
    TabSplitterPage codePage = new TabSplitterPage();
    codePage.Text = "Code";
    
    TabSplitterPage designPage = new TabSplitterPage();
    designPage.Text = "Design";
    
    TabSplitterPage propertiesPage = new TabSplitterPage();
    propertiesPage.Text = "Properties";
    
    // Add to PrimaryPages collection
    this.tabSplitterContainer1.PrimaryPages.AddRange(new TabSplitterPage[] 
    { 
        xamlPage, 
        codePage 
    });
    
    // Add to SecondaryPages collection
    this.tabSplitterContainer1.SecondaryPages.AddRange(new TabSplitterPage[] 
    { 
        designPage, 
        propertiesPage 
    });
}
```

### Setting Tab Labels with Text Property

The `Text` property defines the label displayed on each tab:

```csharp
TabSplitterPage page = new TabSplitterPage();
page.Text = "Document View";  // This text appears on the tab
```

**Best Practices for Tab Labels:**
- Keep labels concise (1-2 words preferred)
- Use descriptive names that indicate content
- Avoid special characters that may cause rendering issues
- Consider localization requirements for multi-language support

## Adding Controls to Tab Pages

Each TabSplitterPage is a container control that can host child controls.

### Simple Control Addition

```csharp
private void AddControlsToPages()
{
    TabSplitterPage codePage = new TabSplitterPage();
    codePage.Text = "Code";
    
    // Create a TextBox for code editing
    TextBox codeTextBox = new TextBox();
    codeTextBox.Multiline = true;
    codeTextBox.Dock = DockStyle.Fill;
    codeTextBox.Font = new System.Drawing.Font("Consolas", 10F);
    codeTextBox.ScrollBars = ScrollBars.Both;
    
    // Add the TextBox to the page
    codePage.Controls.Add(codeTextBox);
    
    // Add page to container
    this.tabSplitterContainer1.PrimaryPages.Add(codePage);
}
```

### Complex Layout with Multiple Controls

```csharp
private void CreateDesignPage()
{
    TabSplitterPage designPage = new TabSplitterPage();
    designPage.Text = "Design";
    
    // Create a Panel for toolbar
    Panel toolbarPanel = new Panel();
    toolbarPanel.Height = 40;
    toolbarPanel.Dock = DockStyle.Top;
    
    // Add toolbar buttons
    Button btnSave = new Button();
    btnSave.Text = "Save";
    btnSave.Location = new System.Drawing.Point(5, 5);
    toolbarPanel.Controls.Add(btnSave);
    
    // Create main design surface
    Panel designSurface = new Panel();
    designSurface.Dock = DockStyle.Fill;
    designSurface.BackColor = System.Drawing.Color.White;
    
    // Add controls to page
    designPage.Controls.Add(designSurface);
    designPage.Controls.Add(toolbarPanel);
    
    // Add page to container
    this.tabSplitterContainer1.SecondaryPages.Add(designPage);
}
```

## Complete Basic Implementation Example

Here's a complete example showing TabSplitterContainer setup with functional pages:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class DocumentEditorForm : Form
{
    private TabSplitterContainer tabSplitterContainer1;
    
    public DocumentEditorForm()
    {
        this.Text = "Document Editor";
        this.Size = new Size(1024, 768);
        
        InitializeTabSplitter();
        CreatePages();
    }
    
    private void InitializeTabSplitter()
    {
        this.tabSplitterContainer1 = new TabSplitterContainer();
        this.tabSplitterContainer1.Dock = DockStyle.Fill;
        this.Controls.Add(this.tabSplitterContainer1);
    }
    
    private void CreatePages()
    {
        // Create XAML editor page
        TabSplitterPage xamlPage = CreateEditorPage("XAML", "Consolas");
        
        // Create code editor page
        TabSplitterPage codePage = CreateEditorPage("Code", "Courier New");
        
        // Create design view page
        TabSplitterPage designPage = CreateViewPage("Design", Color.WhiteSmoke);
        
        // Add pages to collections
        this.tabSplitterContainer1.PrimaryPages.AddRange(new TabSplitterPage[] 
        { 
            xamlPage, 
            codePage 
        });
        
        this.tabSplitterContainer1.SecondaryPages.Add(designPage);
    }
    
    private TabSplitterPage CreateEditorPage(string title, string fontName)
    {
        TabSplitterPage page = new TabSplitterPage();
        page.Text = title;
        
        TextBox editor = new TextBox();
        editor.Multiline = true;
        editor.Dock = DockStyle.Fill;
        editor.Font = new Font(fontName, 10F);
        editor.ScrollBars = ScrollBars.Both;
        
        page.Controls.Add(editor);
        return page;
    }
    
    private TabSplitterPage CreateViewPage(string title, Color backColor)
    {
        TabSplitterPage page = new TabSplitterPage();
        page.Text = title;
        page.BackColor = backColor;
        
        Label label = new Label();
        label.Text = $"{title} View";
        label.Font = new Font("Segoe UI", 16F);
        label.Location = new Point(20, 20);
        label.AutoSize = true;
        
        page.Controls.Add(label);
        return page;
    }
    
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new DocumentEditorForm());
    }
}
```

## Troubleshooting Initial Setup

### Assembly Not Found

If you encounter "The type or namespace name 'TabSplitterContainer' could not be found":
1. Verify all required assemblies are referenced
2. Check that the NuGet package is properly installed
3. Ensure the using directive is present: `using Syncfusion.Windows.Forms.Tools;`
4. Clean and rebuild the solution

### Designer Issues

If the TabSplitterContainer doesn't appear in the toolbox:
1. Right-click the Toolbox and select "Reset Toolbox"
2. Verify the Syncfusion assemblies are in the correct location
3. Check that the project targets a compatible .NET Framework version

### Runtime Exceptions

If you get a "Value cannot be null" exception when adding pages:
- Ensure the TabSplitterContainer is fully initialized before adding pages
- Verify page instances are not null before adding to collections
- Check that the form's InitializeComponent() has been called

## Next Steps

After completing the basic setup:
1. Explore splitter components and page properties (see splitter-components.md)
2. Learn about orientation and positioning options (see orientation-and-position.md)
3. Customize visual appearance with styles (see styling-and-appearance.md)
4. Implement advanced features and event handling (see advanced-features.md)
