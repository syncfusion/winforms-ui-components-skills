# Application Menu

## Overview

ApplicationMenu is the Office 2007-style dropdown menu accessed via the menu button (typically "File") in the top-left corner of the ribbon. It provides a compact, dropdown-based interface for file operations and application commands.

**When to use ApplicationMenu:**
- Office 2007 style applications (RibbonStyle.Office2007)
- Compact dropdown menu preference
- Traditional menu experience

**When to use BackStage instead:**
- Office 2016/2013/2010 style applications
- Full-screen application menu experience
- See backstage.md for details

**Important:** ApplicationMenu is primarily available with **Office2007** ribbon style.

## Accessing ApplicationMenu

### Office Menu Button

The ApplicationMenu is accessed through the menu button in the top-left corner.

```csharp
// Set menu button text
ribbonControlAdv1.MenuButtonText = "File";
```

**User Action:** Click the "File" button to open ApplicationMenu dropdown.

## ApplicationMenu Structure

ApplicationMenu consists of two main panels:

1. **Left Panel** - Primary commands (buttons, menu items)
2. **Right Panel** - Secondary content (recent files, options)

### Adding Controls to Left Panel

The left panel typically contains file operation buttons.

**Via Designer:**
1. Select RibbonControlAdv
2. Expand **ApplicationMenu** property in Properties window
3. Find **MainPanel** → **Controls** collection
4. Add buttons, menu items

**Via Code:**

```csharp
// Create button for left panel
ButtonAdv newButton = new ButtonAdv();
newButton.Text = "New";
newButton.Image = Image.FromFile("new.png");
newButton.Click += (s, e) => CreateNewDocument();

// Add to main panel (left)
ribbonControlAdv1.ApplicationMenu.MainPanel.Controls.Add(newButton);
```

### Adding Controls to Right Panel

The right panel typically shows recent files or additional options.

**Via Code:**

```csharp
// Create list for right panel
ListBox recentFilesListBox = new ListBox();
recentFilesListBox.Items.AddRange(new object[] {
    "Document1.docx",
    "Document2.docx",
    "Document3.docx"
});
recentFilesListBox.Dock = DockStyle.Fill;

// Add header label
Label headerLabel = new Label();
headerLabel.Text = "Recent Documents";
headerLabel.Font = new Font("Segoe UI", 10, FontStyle.Bold);
headerLabel.Dock = DockStyle.Top;

// Add to right panel
ribbonControlAdv1.ApplicationMenu.RightPanel.Controls.AddRange(new Control[] {
    headerLabel,
    recentFilesListBox
});
```

## Complete ApplicationMenu Example

```csharp
using Syncfusion.Windows.Forms.Tools;

private void SetupApplicationMenu()
{
    // Ensure Office 2007 style
    ribbonControlAdv1.RibbonStyle = RibbonStyle.Office2007;
    ribbonControlAdv1.MenuButtonText = "File";
    
    // === LEFT PANEL (Main Commands) ===
    
    // New Button
    ButtonAdv newButton = new ButtonAdv();
    newButton.Text = "New";
    newButton.Image = Image.FromFile("new.png");
    newButton.ImageSize = new Size(32, 32);
    newButton.Click += (s, e) => CreateNew();
    
    // Open Button
    ButtonAdv openButton = new ButtonAdv();
    openButton.Text = "Open";
    openButton.Image = Image.FromFile("open.png");
    openButton.ImageSize = new Size(32, 32);
    openButton.Click += (s, e) => OpenDocument();
    
    // Save Button
    ButtonAdv saveButton = new ButtonAdv();
    saveButton.Text = "Save";
    saveButton.Image = Image.FromFile("save.png");
    saveButton.ImageSize = new Size(32, 32);
    saveButton.Click += (s, e) => SaveDocument();
    
    // Save As Button
    ButtonAdv saveAsButton = new ButtonAdv();
    saveAsButton.Text = "Save As";
    saveAsButton.Image = Image.FromFile("saveas.png");
    saveAsButton.ImageSize = new Size(32, 32);
    saveAsButton.Click += (s, e) => SaveDocumentAs();
    
    // Separator
    Panel separator1 = new Panel();
    separator1.Height = 1;
    separator1.BackColor = Color.Gray;
    separator1.Dock = DockStyle.Top;
    
    // Print Button
    ButtonAdv printButton = new ButtonAdv();
    printButton.Text = "Print";
    printButton.Image = Image.FromFile("print.png");
    printButton.ImageSize = new Size(32, 32);
    printButton.Click += (s, e) => Print();
    
    // Exit Button
    ButtonAdv exitButton = new ButtonAdv();
    exitButton.Text = "Exit";
    exitButton.Click += (s, e) => Application.Exit();
    
    // Add to main panel
    ribbonControlAdv1.ApplicationMenu.MainPanel.Controls.AddRange(new Control[] {
        newButton,
        openButton,
        saveButton,
        saveAsButton,
        separator1,
        printButton,
        exitButton
    });
    
    // === RIGHT PANEL (Recent Files) ===
    
    // Header
    Label recentLabel = new Label();
    recentLabel.Text = "Recent Documents";
    recentLabel.Font = new Font("Segoe UI", 11, FontStyle.Bold);
    recentLabel.Padding = new Padding(10);
    recentLabel.Dock = DockStyle.Top;
    
    // Recent files list
    ListBox recentListBox = new ListBox();
    recentListBox.Dock = DockStyle.Fill;
    recentListBox.BorderStyle = BorderStyle.None;
    recentListBox.DoubleClick += (s, e) =>
    {
        if (recentListBox.SelectedItem != null)
        {
            OpenRecentFile(recentListBox.SelectedItem.ToString());
        }
    };
    
    // Populate recent files
    LoadRecentFiles(recentListBox);
    
    // Add to right panel
    ribbonControlAdv1.ApplicationMenu.RightPanel.Controls.AddRange(new Control[] {
        recentLabel,
        recentListBox
    });
}

private void LoadRecentFiles(ListBox listBox)
{
    // Load from settings or registry
    listBox.Items.AddRange(new object[] {
        "C:\\Documents\\Report.docx",
        "C:\\Documents\\Presentation.pptx",
        "C:\\Documents\\Spreadsheet.xlsx",
        "C:\\Documents\\Notes.txt"
    });
}
```

## Mini-ToolBar

ApplicationMenu can include a Mini-ToolBar for quick formatting options.

### Adding Mini-ToolBar

**Via Designer:**
1. Select RibbonControlAdv
2. Expand **ApplicationMenu** → **MiniToolbar**
3. Add controls to mini-toolbar

**Via Code:**

```csharp
// Create mini-toolbar buttons
ToolStripButton boldButton = new ToolStripButton();
boldButton.Text = "B";
boldButton.Font = new Font("Arial", 10, FontStyle.Bold);
boldButton.Click += (s, e) => ApplyBold();

ToolStripButton italicButton = new ToolStripButton();
italicButton.Text = "I";
italicButton.Font = new Font("Arial", 10, FontStyle.Italic);
italicButton.Click += (s, e) => ApplyItalic();

// Add to mini-toolbar
ribbonControlAdv1.ApplicationMenu.MiniToolbar.Items.AddRange(new ToolStripItem[] {
    boldButton,
    italicButton
});
```

## Differences: ApplicationMenu vs BackStage

| Feature | ApplicationMenu | BackStage |
|---------|----------------|-----------|
| **Style** | Office 2007 | Office 2016/2013/2010 |
| **Display** | Dropdown overlay | Full-screen overlay |
| **Layout** | Two-panel vertical | Left sidebar + content area |
| **Use Case** | Compact menu | Spacious, organized |
| **Navigation** | Click outside to close | Back button + Escape |
| **Content** | Limited space | Full-screen space |

## When to Choose ApplicationMenu

**Choose ApplicationMenu when:**
- Using Office 2007 ribbon style
- Application needs compact file menu
- Limited file operations
- Traditional dropdown menu expected
- Screen space is premium

**Choose BackStage when:**
- Using Office 2016/2013/2010 style
- Need extensive file management UI
- Want to display detailed information
- Modern application aesthetic
- Complex settings/options pages

## Best Practices

1. **Match ribbon style:** Use ApplicationMenu with Office2007 ribbon style only

2. **Left panel for actions:** Put primary file operations in left panel

3. **Right panel for content:** Show recent files, documents, or related content

4. **Use appropriate icons:** 32x32 images for main panel buttons

5. **Provide Exit option:** Always include an Exit/Close button

6. **Recent files:** Display recent documents in right panel for quick access

7. **Keep it simple:** Don't overcrowd ApplicationMenu with too many options

8. **Close after action:** Menu closes automatically after button click

## Code Examples

### Simple ApplicationMenu

```csharp
private void SetupSimpleApplicationMenu()
{
    ribbonControlAdv1.RibbonStyle = RibbonStyle.Office2007;
    ribbonControlAdv1.MenuButtonText = "File";
    
    // New button
    ButtonAdv newBtn = new ButtonAdv();
    newBtn.Text = "New";
    newBtn.Click += (s, e) => MessageBox.Show("New");
    
    // Open button
    ButtonAdv openBtn = new ButtonAdv();
    openBtn.Text = "Open";
    openBtn.Click += (s, e) => MessageBox.Show("Open");
    
    // Add buttons
    ribbonControlAdv1.ApplicationMenu.MainPanel.Controls.Add(newBtn);
    ribbonControlAdv1.ApplicationMenu.MainPanel.Controls.Add(openBtn);
}
```

### ApplicationMenu with Recent Files

```csharp
private void SetupApplicationMenuWithRecents()
{
    ribbonControlAdv1.RibbonStyle = RibbonStyle.Office2007;
    ribbonControlAdv1.MenuButtonText = "File";
    
    // Left panel - commands
    AddFileCommands();
    
    // Right panel - recent files
    AddRecentFilesPanel();
}

private void AddFileCommands()
{
    string[] commands = { "New", "Open", "Save", "Save As", "Print", "Exit" };
    
    foreach (string cmd in commands)
    {
        ButtonAdv btn = new ButtonAdv();
        btn.Text = cmd;
        btn.Click += (s, e) => ExecuteCommand(cmd);
        ribbonControlAdv1.ApplicationMenu.MainPanel.Controls.Add(btn);
    }
}

private void AddRecentFilesPanel()
{
    Label header = new Label();
    header.Text = "Recent Documents";
    header.Dock = DockStyle.Top;
    
    ListBox recent = new ListBox();
    recent.Dock = DockStyle.Fill;
    recent.Items.AddRange(new object[] { "Doc1.docx", "Doc2.docx" });
    
    ribbonControlAdv1.ApplicationMenu.RightPanel.Controls.Add(header);
    ribbonControlAdv1.ApplicationMenu.RightPanel.Controls.Add(recent);
}
```

## Troubleshooting

### Issue: ApplicationMenu Doesn't Show

**Cause:** Not using Office2007 ribbon style.

**Solution:**
```csharp
ribbonControlAdv1.RibbonStyle = RibbonStyle.Office2007;
ribbonControlAdv1.MenuButtonText = "File";
```

### Issue: Buttons Not Appearing

**Cause:** Controls not added to correct panel.

**Solution:**
```csharp
// Add to MainPanel (left panel)
ribbonControlAdv1.ApplicationMenu.MainPanel.Controls.Add(button);
```

### Issue: Right Panel Empty

**Cause:** Content not added to RightPanel.

**Solution:**
```csharp
ribbonControlAdv1.ApplicationMenu.RightPanel.Controls.Add(myContent);
```

## Related Topics

- **BackStage** - Modern Office 2016-style application menu
- **Getting Started** - Basic ribbon setup including menu button
- **Customization** - Customizing menu appearance and behavior
