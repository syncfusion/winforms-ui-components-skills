# Customization and Runtime Configuration

## Overview

RibbonControlAdv provides extensive customization options allowing end-users to personalize their ribbon experience at runtime through dialogs, context menus, and programmatic APIs.

## Context Menu Options

Right-clicking ribbon items or tabs opens a context menu with customization options.

### Default Context Menu Options

1. **Add to Quick Access Toolbar** - Add item to QAT (item-specific)
2. **Customize Quick Access Toolbar** - Open QAT customization dialog
3. **Show Quick Access Toolbar Below/Above Ribbon** - Toggle QAT position
4. **Customize the Ribbon** - Open ribbon customization dialog
5. **Collapse the Ribbon** - Minimize ribbon (ShowTabs state)

### Custom Context Menu Items

Add custom items using `BeforeContextMenuOpen` event:

```csharp
ribbonControlAdv1.BeforeContextMenuOpen += RibbonControlAdv1_BeforeContextMenuOpen;

private void RibbonControlAdv1_BeforeContextMenuOpen(object sender, ContextMenuEventArgs e)
{
    // Add custom menu item
    ToolStripMenuItem aboutItem = new ToolStripMenuItem();
    aboutItem.Text = "About Application";
    aboutItem.Click += (s, args) =>
    {
        MessageBox.Show("Application Version 1.0", "About");
    };
    
    e.ContextMenuItems.Add(aboutItem);
    
    // Add separator
    e.ContextMenuItems.Add(new ToolStripSeparator());
    
    // Add help item
    ToolStripMenuItem helpItem = new ToolStripMenuItem();
    helpItem.Text = "Help";
    helpItem.Click += (s, args) =>
    {
        System.Diagnostics.Process.Start("https://help.syncfusion.com/");
    };
    
    e.ContextMenuItems.Add(helpItem);
}
```

## Customize Quick Access Toolbar Dialog

### Opening the Dialog

**User Access:**
- Right-click ribbon → "Customize Quick Access Toolbar"
- Click QAT dropdown → "More Commands"

**Features:**
- Add/remove items from QAT
- Reorder QAT items
- Choose items from different sources (ribbon tabs, File menu, Quick Items)
- Create new QAT items

### Restricting Items from QAT Customization

```csharp
// Prevent item from appearing in customize dialog
ribbonControlAdv1.SetUseInCustomQuickAccessDialog(restrictedButton, false);

// Allow item (default)
ribbonControlAdv1.SetUseInCustomQuickAccessDialog(allowedButton, true);
```

## Customize Ribbon Dialog

### Opening the Dialog

**User Access:**
- Right-click ribbon → "Customize the Ribbon"

**Features:**
- Add new tabs
- Rename existing tabs
- Reorder tabs
- Add panel items to tabs
- Add items from ribbon to new locations
- Show/hide tabs

### Tab Customization at Runtime

Users can:
1. Add new tabs (visible only in current layout mode)
2. Add groups (ToolStripEx) to tabs
3. Add items to groups
4. Rename tabs and groups
5. Reorder tabs

**Example of User Workflow:**
1. Right-click ribbon → "Customize the Ribbon"
2. Click "New Tab"
3. Rename tab to "My Tools"
4. Add items from left panel
5. Click OK
6. New tab appears in ribbon

## Serialization Support

Save and load ribbon state including QAT items, tab order, and customizations.

### Saving Ribbon State

```csharp
using Syncfusion.Runtime.Serialization;

// Save ribbon state to file
private void SaveRibbonState()
{
    string filePath = "ribbon-state.xml";
    
    AppStateSerializer serializer = new AppStateSerializer(
        SerializeMode.XMLSerialization, 
        this);
    
    serializer.SerializeObject("RibbonState", ribbonControlAdv1);
    serializer.PersistState(filePath);
}
```

### Loading Ribbon State

```csharp
// Load ribbon state from file
private void LoadRibbonState()
{
    string filePath = "ribbon-state.xml";
    
    if (File.Exists(filePath))
    {
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLSerialization, 
            this);
        
        serializer.LoadState(filePath);
        serializer.DeserializeObject("RibbonState", ribbonControlAdv1);
    }
}
```

### Complete Serialization Example

```csharp
public partial class Form1 : RibbonForm
{
    private const string RibbonStateFile = "ribbon-config.xml";
    
    protected override void OnLoad(EventArgs e)
    {
        base.OnLoad(e);
        LoadRibbonConfiguration();
    }
    
    protected override void OnFormClosing(FormClosingEventArgs e)
    {
        SaveRibbonConfiguration();
        base.OnFormClosing(e);
    }
    
    private void SaveRibbonConfiguration()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLSerialization, this);
            serializer.SerializeObject("Ribbon", ribbonControlAdv1);
            serializer.PersistState(RibbonStateFile);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error saving ribbon state: {ex.Message}");
        }
    }
    
    private void LoadRibbonConfiguration()
    {
        try
        {
            if (File.Exists(RibbonStateFile))
            {
                AppStateSerializer serializer = new AppStateSerializer(
                    SerializeMode.XMLSerialization, this);
                serializer.LoadState(RibbonStateFile);
                serializer.DeserializeObject("Ribbon", ribbonControlAdv1);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading ribbon state: {ex.Message}");
        }
    }
}
```

## Designer Support

### Smart Tags

RibbonControlAdv provides designer smart tags for quick configuration:

**RibbonControlAdv Smart Tag:**
- Add Tab
- Edit Tabs
- Choose RibbonStyle
- Edit Quick Items

**ToolStripTabItem Smart Tag:**
- Add ToolStrip (group)
- Edit Items

**ToolStripEx Smart Tag:**
- Add items
- Edit items

### Property Grid Customization

All ribbon properties accessible via Properties window in designer:
- RibbonStyle
- LayoutMode
- DisplayOption
- ShowMinimizeButton
- ShowRibbonDisplayOptionButton
- QuickPanelVisible
- And many more...

## Appearance Customization

### Colors and Themes

```csharp
// Office color schemes
ribbonControlAdv1.OfficeColorScheme = ToolStripEx.ColorScheme.Blue;
// Options: Blue, Silver, Black, Managed

// Custom colors
ribbonControlAdv1.RibbonHeaderColor = Color.FromArgb(0, 114, 198);
ribbonControlAdv1.RibbonPanelBackColor = Color.White;
```

### Fonts

```csharp
// Set ribbon font
ribbonControlAdv1.Font = new Font("Segoe UI", 9);

// Set specific tab font
homeTab.Font = new Font("Segoe UI", 10, FontStyle.Bold);
```

## Best Practices

1. **Enable runtime customization:** Allow users to customize ribbon and QAT

2. **Save user preferences:** Use serialization to persist ribbon state

3. **Provide helpful context menu items:** Add application-specific options to context menu

4. **Don't over-restrict:** Only restrict truly inappropriate items from customization

5. **Test customization scenarios:** Verify customization works correctly

6. **Handle serialization errors:** Gracefully handle missing or corrupt state files

7. **Provide reset option:** Allow users to reset to default configuration

8. **Document customization features:** Tell users about customization capabilities

## Complete Customization Example

```csharp
public partial class Form1 : RibbonForm
{
    private const string ConfigFile = "ribbon-config.xml";
    
    public Form1()
    {
        InitializeComponent();
        SetupRibbonCustomization();
        LoadConfiguration();
    }
    
    private void SetupRibbonCustomization()
    {
        // Enable customization features
        ribbonControlAdv1.ShowRibbonDisplayOptionButton = true;
        ribbonControlAdv1.ShowMinimizeButton = true;
        ribbonControlAdv1.QuickPanelVisible = true;
        
        // Add custom context menu items
        ribbonControlAdv1.BeforeContextMenuOpen += AddCustomContextItems;
        
        // Handle form closing to save state
        this.FormClosing += (s, e) => SaveConfiguration();
    }
    
    private void AddCustomContextItems(object sender, ContextMenuEventArgs e)
    {
        // Separator
        e.ContextMenuItems.Add(new ToolStripSeparator());
        
        // Reset ribbon
        ToolStripMenuItem resetItem = new ToolStripMenuItem("Reset Ribbon");
        resetItem.Click += (s, args) => ResetRibbon();
        e.ContextMenuItems.Add(resetItem);
        
        // About
        ToolStripMenuItem aboutItem = new ToolStripMenuItem("About");
        aboutItem.Click += (s, args) => ShowAbout();
        e.ContextMenuItems.Add(aboutItem);
    }
    
    private void SaveConfiguration()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLSerialization, this);
            serializer.SerializeObject("Ribbon", ribbonControlAdv1);
            serializer.PersistState(ConfigFile);
        }
        catch { /* Handle error */ }
    }
    
    private void LoadConfiguration()
    {
        try
        {
            if (File.Exists(ConfigFile))
            {
                AppStateSerializer serializer = new AppStateSerializer(
                    SerializeMode.XMLSerialization, this);
                serializer.LoadState(ConfigFile);
                serializer.DeserializeObject("Ribbon", ribbonControlAdv1);
            }
        }
        catch { /* Handle error */ }
    }
    
    private void ResetRibbon()
    {
        if (MessageBox.Show("Reset ribbon to default?", "Confirm", 
            MessageBoxButtons.YesNo) == DialogResult.Yes)
        {
            // Delete config file
            if (File.Exists(ConfigFile))
                File.Delete(ConfigFile);
            
            // Restart application or reload defaults
            MessageBox.Show("Restart application to apply changes.");
        }
    }
    
    private void ShowAbout()
    {
        MessageBox.Show("My Application v1.0\n© 2026", "About");
    }
}
```

## Related Topics

- **Quick Access Toolbar** - QAT customization details
- **Ribbon States** - State management and persistence
- **Advanced Features** - Additional customization options
