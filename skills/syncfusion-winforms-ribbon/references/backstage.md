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

```csharp
// Create and connect backstage to ribbon
BackStage backStage1 = new BackStage();
ribbonControlAdv1.BackStage = backStage1;
ribbonControlAdv1.MenuButtonText = "File";
this.Controls.Add(backStage1);
```

**Designer**: Drag BackStage from toolbox → Set RibbonControlAdv.BackStage property → Click ShowBackstage in smart tag to open designer.

## Adding BackStage Tabs

```csharp
// Create backstage tab
BackStageTab infoTab = new BackStageTab {
    Text = "Info",
    BackColor = Color.White
};

infoTab.Controls.Add(new Label {
    Text = "Document Information",
    Font = new Font("Segoe UI", 14, FontStyle.Bold),
    Location = new Point(50, 50)
});

backStage1.Controls.Add(infoTab);
```

**Designer**: Click BackStage smart tag → Add Tab → Set properties in Properties window.

### Multiple Tabs Example

```csharp
// Create multiple tabs
BackStageTab[] tabs = {
    new BackStageTab { Text = "Info" },
    new BackStageTab { Text = "Open" },
    new BackStageTab { Text = "Save As" }
};

tabs[0].Controls.Add(new Label {
    Text = "Size: 245 KB\nPages: 15\nAuthor: John Doe",
    Location = new Point(50, 80)
});

backStage1.Controls.AddRange(tabs);
```

## Adding BackStage Buttons

```csharp
// Create backstage button for direct commands
BackStageButton optionsButton = new BackStageButton {
    Text = "Options"
};
optionsButton.Click += (s, e) => ShowOptionsDialog();
backStage1.Controls.Add(optionsButton);
```

**Designer**: Click BackStage smart tag → Add Button → Set properties.



## Adding BackStageSeparator

```csharp
// Add separator for visual grouping
backStage1.Controls.Add(infoTab);
backStage1.Controls.Add(openTab);
backStage1.Controls.Add(new BackStageSeparator());
backStage1.Controls.Add(optionsButton);
```

## Complete BackStage Example

```csharp
private void SetupCompleteBackStage()
{
    BackStage backStage1 = new BackStage {
        BeforeBorderColor = Color.FromArgb(0, 114, 198)
    };
    
    // Create tabs
    BackStageTab[] tabs = {
        new BackStageTab { Text = "Info", BackColor = Color.White },
        new BackStageTab { Text = "New", BackColor = Color.White },
        new BackStageTab { Text = "Open", BackColor = Color.White },
        new BackStageTab { Text = "Save As", BackColor = Color.White },
        new BackStageTab { Text = "Print", BackColor = Color.White }
    };
    
    // Add content to Info tab
    tabs[0].Controls.Add(new Label {
        Text = "Filename: Document1.docx\nSize: 245 KB\nAuthor: John Doe",
        Location = new Point(50, 50),
        AutoSize = true
    });
    
    // Create buttons
    BackStageButton optionsButton = new BackStageButton { Text = "Options" };
    optionsButton.Click += (s, e) => { ribbonControlAdv1.HideBackStage(); ShowOptions(); };
    
    BackStageButton exitButton = new BackStageButton { Text = "Exit" };
    exitButton.Click += (s, e) => Application.Exit();
    
    // Add all items
    backStage1.Controls.AddRange(tabs);
    backStage1.Controls.Add(new BackStageSeparator());
    backStage1.Controls.AddRange(new Control[] { optionsButton, exitButton });
    
    // Connect to ribbon
    ribbonControlAdv1.BackStage = backStage1;
    ribbonControlAdv1.MenuButtonText = "File";
    this.Controls.Add(backStage1);
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
