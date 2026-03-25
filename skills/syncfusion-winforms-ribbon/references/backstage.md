# BackStage View

## Overview

BackStage is a full-screen application menu introduced in Office 2016 style, providing a modern interface for file operations, settings, and application-wide commands. It replaces the traditional dropdown application menu with a spacious, organized layout.

**When to use BackStage:**
- Office 2016/2013 style applications
- Need for file management UI (Open, Save, Print, etc.)
- Application settings and options
- Account/user information display
- Modern, spacious layout preference

**When to use ApplicationMenu instead:**
- Office 2007 style applications
- Compact dropdown menu preference
- See application-menu.md for details

## Creating BackStage

### Step 1: Add BackStage Control

**Via Toolbox:**
1. Drag **BackStage** control from toolbox
2. Drop it on the form (can be anywhere, renders as overlay)

**Via Code:**
```csharp
BackStage backStage1 = new BackStage();
this.Controls.Add(backStage1);
```

### Step 2: Access BackStage Designer

1. Select BackStage control in designer (appears in component tray)
2. Click smart tag
3. Select **ShowBackstage** to open backstage designer view

### Step 3: Connect to RibbonControlAdv

**Via Designer:**
1. Select RibbonControlAdv
2. In Properties window, find **BackStage** property
3. Select your BackStage control from dropdown

**Via Code:**
```csharp
// Connect backstage to ribbon
ribbonControlAdv1.BackStage = backStage1;

// Set menu button text
ribbonControlAdv1.MenuButtonText = "File";
```

### Complete Basic Setup

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Form1 : RibbonForm
{
    private RibbonControlAdv ribbonControlAdv1;
    private BackStage backStage1;

    private void InitializeBackStage()
    {
        // Create backstage
        backStage1 = new BackStage();
        backStage1.BeforeBorderColor = Color.Gray;
        
        // Connect to ribbon
        ribbonControlAdv1.BackStage = backStage1;
        ribbonControlAdv1.MenuButtonText = "File";
        
        // Add to form
        this.Controls.Add(backStage1);
    }
}
```

## Adding BackStage Tabs

BackStage tabs display content pages (e.g., Info, Open, Save As).

### Adding Tab via Designer

1. Open BackStage designer (ShowBackstage from smart tag)
2. Click BackStage smart tag
3. Select **Add Tab**
4. Set tab properties in Properties window

### Adding Tab via Code

```csharp
// Create backstage tab
BackStageTab infoTab = new BackStageTab();
infoTab.Text = "Info";
infoTab.BackColor = Color.White;

// Add content to tab
Label infoLabel = new Label();
infoLabel.Text = "Document Information";
infoLabel.Font = new Font("Segoe UI", 14, FontStyle.Bold);
infoLabel.Location = new Point(50, 50);

infoTab.Controls.Add(infoLabel);

// Add tab to backstage
backStage1.Controls.Add(infoTab);
```

### Multiple Tabs Example

```csharp
private void CreateBackStageTabs()
{
    // Info Tab
    BackStageTab infoTab = new BackStageTab();
    infoTab.Text = "Info";
    AddInfoContent(infoTab);
    
    // Open Tab
    BackStageTab openTab = new BackStageTab();
    openTab.Text = "Open";
    AddOpenContent(openTab);
    
    // Save As Tab
    BackStageTab saveAsTab = new BackStageTab();
    saveAsTab.Text = "Save As";
    AddSaveAsContent(saveAsTab);
    
    // Add all tabs
    backStage1.Controls.AddRange(new Control[] {
        infoTab,
        openTab,
        saveAsTab
    });
}

private void AddInfoContent(BackStageTab tab)
{
    // Title
    Label titleLabel = new Label();
    titleLabel.Text = "Document Properties";
    titleLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);
    titleLabel.Location = new Point(50, 30);
    titleLabel.AutoSize = true;
    
    // Properties
    Label propertiesLabel = new Label();
    propertiesLabel.Text = "Size: 245 KB\nPages: 15\nAuthor: John Doe\nCreated: 3/20/2026";
    propertiesLabel.Location = new Point(50, 80);
    propertiesLabel.AutoSize = true;
    
    tab.Controls.AddRange(new Control[] { titleLabel, propertiesLabel });
}
```

## Adding BackStage Buttons

BackStage buttons execute commands directly (e.g., Options, Exit).

### Adding Button via Designer

1. Open BackStage designer
2. Click BackStage smart tag
3. Select **Add Button**
4. Set button properties

### Adding Button via Code

```csharp
// Create backstage button
BackStageButton optionsButton = new BackStageButton();
optionsButton.Text = "Options";
optionsButton.Click += (s, e) => ShowOptionsDialog();

// Add to backstage
backStage1.Controls.Add(optionsButton);
```

### Multiple Buttons Example

```csharp
private void CreateBackStageButtons()
{
    // Options Button
    BackStageButton optionsButton = new BackStageButton();
    optionsButton.Text = "Options";
    optionsButton.Click += OptionsButton_Click;
    
    // Exit Button
    BackStageButton exitButton = new BackStageButton();
    exitButton.Text = "Exit";
    exitButton.Click += ExitButton_Click;
    
    // Add buttons
    backStage1.Controls.AddRange(new Control[] {
        optionsButton,
        exitButton
    });
}

private void OptionsButton_Click(object sender, EventArgs e)
{
    // Close backstage first
    ribbonControlAdv1.HideBackStage();
    
    // Show options dialog
    OptionsDialog dialog = new OptionsDialog();
    dialog.ShowDialog();
}

private void ExitButton_Click(object sender, EventArgs e)
{
    // Confirm and exit
    if (MessageBox.Show("Exit application?", "Confirm", 
        MessageBoxButtons.YesNo) == DialogResult.Yes)
    {
        Application.Exit();
    }
}
```

## Adding BackStageSeparator

Separators provide visual grouping between backstage items.

### Adding Separator via Designer

1. Open BackStage designer
2. Click BackStage smart tag
3. Select **Add Separator**

### Adding Separator via Code

```csharp
// Create separator
BackStageSeparator separator1 = new BackStageSeparator();

// Add between items
backStage1.Controls.Add(infoTab);
backStage1.Controls.Add(openTab);
backStage1.Controls.Add(separator1); // Visual separation
backStage1.Controls.Add(optionsButton);
```

## Complete BackStage Example

```csharp
private void SetupCompleteBackStage()
{
    // Create backstage
    BackStage backStage1 = new BackStage();
    backStage1.BeforeBorderColor = Color.FromArgb(0, 114, 198);
    
    // === TABS ===
    
    // Info Tab
    BackStageTab infoTab = new BackStageTab();
    infoTab.Text = "Info";
    CreateInfoPage(infoTab);
    
    // New Tab
    BackStageTab newTab = new BackStageTab();
    newTab.Text = "New";
    CreateNewPage(newTab);
    
    // Open Tab
    BackStageTab openTab = new BackStageTab();
    openTab.Text = "Open";
    CreateOpenPage(openTab);
    
    // Save As Tab
    BackStageTab saveAsTab = new BackStageTab();
    saveAsTab.Text = "Save As";
    CreateSaveAsPage(saveAsTab);
    
    // Print Tab
    BackStageTab printTab = new BackStageTab();
    printTab.Text = "Print";
    CreatePrintPage(printTab);
    
    // Separator
    BackStageSeparator separator1 = new BackStageSeparator();
    
    // === BUTTONS ===
    
    // Options Button
    BackStageButton optionsButton = new BackStageButton();
    optionsButton.Text = "Options";
    optionsButton.Click += (s, e) =>
    {
        ribbonControlAdv1.HideBackStage();
        ShowOptions();
    };
    
    // Exit Button
    BackStageButton exitButton = new BackStageButton();
    exitButton.Text = "Exit";
    exitButton.Click += (s, e) => Application.Exit();
    
    // Add all items
    backStage1.Controls.AddRange(new Control[] {
        infoTab,
        newTab,
        openTab,
        saveAsTab,
        printTab,
        separator1,
        optionsButton,
        exitButton
    });
    
    // Connect to ribbon
    ribbonControlAdv1.BackStage = backStage1;
    ribbonControlAdv1.MenuButtonText = "File";
    
    this.Controls.Add(backStage1);
}

private void CreateInfoPage(BackStageTab tab)
{
    tab.BackColor = Color.White;
    
    // Header
    Label header = new Label();
    header.Text = "Document Information";
    header.Font = new Font("Segoe UI", 18, FontStyle.Bold);
    header.Location = new Point(50, 40);
    header.AutoSize = true;
    
    // Details panel
    Panel detailsPanel = new Panel();
    detailsPanel.Location = new Point(50, 100);
    detailsPanel.Size = new Size(400, 200);
    detailsPanel.BorderStyle = BorderStyle.FixedSingle;
    
    Label detailsLabel = new Label();
    detailsLabel.Text = "Filename: Document1.docx\n" +
                        "Location: C:\\Documents\\\n" +
                        "Size: 245 KB\n" +
                        "Created: 3/20/2026\n" +
                        "Modified: 3/20/2026\n" +
                        "Author: John Doe";
    detailsLabel.Location = new Point(20, 20);
    detailsLabel.AutoSize = true;
    
    detailsPanel.Controls.Add(detailsLabel);
    tab.Controls.AddRange(new Control[] { header, detailsPanel });
}
```

## Opening and Closing BackStage

### Opening BackStage

**User Action:**
- Click File (menu button) in ribbon

**Programmatically:**
```csharp
// Show backstage
ribbonControlAdv1.ShowBackStage();
```

### Closing BackStage

**User Action:**
- Click back arrow (top-left)
- Press Escape key
- Click outside backstage area

**Programmatically:**
```csharp
// Hide backstage
ribbonControlAdv1.HideBackStage();

// Example: Close after action
optionsButton.Click += (s, e) =>
{
    ribbonControlAdv1.HideBackStage();
    ShowOptionsDialog();
};
```

## BackStage Appearance

### Customizing Colors

```csharp
// Border color
backStage1.BeforeBorderColor = Color.FromArgb(0, 114, 198);

// Background color for tabs
infoTab.BackColor = Color.White;
openTab.BackColor = Color.WhiteSmoke;

// Text color
infoTab.ForeColor = Color.Black;
```

### Customizing Tab Appearance

```csharp
// Tab with icon
BackStageTab printTab = new BackStageTab();
printTab.Text = "Print";
printTab.Image = Image.FromFile("print.png");
printTab.ImageSize = new Size(16, 16);
```

## BackStage Events

### BackStageTab Events

```csharp
// Click event (when tab is selected)
infoTab.Click += (s, e) =>
{
    Console.WriteLine("Info tab selected");
    LoadDocumentInfo();
};

// Enter event (tab becomes active)
openTab.Enter += (s, e) =>
{
    Console.WriteLine("Open tab entered");
    RefreshRecentFiles();
};
```

### BackStageButton Events

```csharp
// Click event
optionsButton.Click += (s, e) =>
{
    ribbonControlAdv1.HideBackStage();
    OptionsDialog dialog = new OptionsDialog();
    if (dialog.ShowDialog() == DialogResult.OK)
    {
        ApplyOptions(dialog.Options);
    }
};
```

## Best Practices

1. **Use for Office 2016/2013 style:** BackStage matches modern Office aesthetics

2. **Organize logically:** Group related tabs together, buttons at bottom

3. **Use separators:** Separate file operations from application settings

4. **Close after actions:** Hide backstage after executing button commands

5. **Provide back navigation:** Always allow users to return to document (automatic)

6. **Use white/light backgrounds:** Match Office BackStage design patterns

7. **Include Info tab:** First tab typically shows document/application information

8. **Add Exit button:** Provide clear application exit option

9. **Handle tab content:** Dynamically load content when tabs are activated

10. **Test keyboard navigation:** Ensure Escape key closes backstage

## Troubleshooting

### Issue: BackStage Doesn't Appear

**Cause:** Not connected to RibbonControlAdv or wrong RibbonStyle.

**Solution:**
```csharp
// Ensure connection
ribbonControlAdv1.BackStage = backStage1;

// BackStage works best with Office 2016 style
ribbonControlAdv1.RibbonStyle = RibbonStyle.Office2016;
```

### Issue: Menu Button Doesn't Show

**Cause:** MenuButtonText not set.

**Solution:**
```csharp
ribbonControlAdv1.MenuButtonText = "File";
```

### Issue: Tab Content Not Showing

**Cause:** Controls not added to tab.

**Solution:**
```csharp
// Add content controls to tab
infoTab.Controls.Add(myLabel);
infoTab.Controls.Add(myPanel);
```

## Related Topics

- **ApplicationMenu** - Office 2007-style alternative to BackStage
- **Quick Access Toolbar** - Adding backstage items to QAT
- **Customization** - Runtime customization of backstage items
