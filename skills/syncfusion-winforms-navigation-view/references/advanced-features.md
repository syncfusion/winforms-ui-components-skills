# Advanced Features

This guide covers advanced NavigationView capabilities including edit mode for quick navigation and popup customization through the BarPopup event.

## Overview

Advanced features enable power users to navigate more efficiently:

1. **Edit Mode:** Type paths directly instead of clicking through hierarchy
2. **BarPopup Event:** Customize dropdown behavior, item count, and conditional display
3. **Popup Control:** Dynamically control what appears in dropdowns

These features are optional but provide significant usability improvements for complex navigation scenarios.

## Edit Mode

Edit mode allows users to directly type or edit the navigation path, enabling quick jumps to any location without clicking through the hierarchy.

### What is Edit Mode?

**Standard Navigation:**
1. User clicks breadcrumb arrow
2. Selects from dropdown
3. Repeats for each level
4. Takes 3-5 clicks to reach deep location

**Edit Mode Navigation:**
1. User clicks on breadcrumb text area
2. Control becomes editable text field
3. User types complete path
4. Press Enter to navigate
5. Instant navigation to any depth

### Enabling Edit Mode

Users can typically click on the text area of the NavigationView to enter edit mode. The breadcrumb display transforms into an editable text box.

**User Actions:**
1. **Click** on the text portion of NavigationView
2. **Text becomes editable** - cursor appears
3. **Type or edit** the path
4. **Press Enter** to navigate to typed path
5. **Press Escape** to cancel and revert

### Edit Mode Usage Example

```csharp
// NavigationView automatically supports edit mode
NavigationView nav = new NavigationView();
nav.Width = 600;
nav.Height = 28;

// Create hierarchy
Bar computer = new Bar { Text = "Computer" };
Bar cDrive = new Bar { Text = "C:" };
Bar users = new Bar { Text = "Users" };

cDrive.Bars.Add(users);
computer.Bars.Add(cDrive);
nav.Bars.Add(computer);
nav.SelectedBar = computer;

// User can click text, type: "Computer > C: > Users", press Enter
```

### Path Format and Validation

**Standard Path Separator:** Use " > " (space-arrow-space) to separate Bar levels. Each segment must match Bar.Text exactly.

```csharp
// Validate typed paths
navigationView1.BarSelected += (sender, e) =>
{
    Bar selected = navigationView1.SelectedBar;
    if (selected?.Tag is string path && !Directory.Exists(path))
    {
        MessageBox.Show("Invalid path entered.");
    }
};
```

### Edit Mode Best Practices

1. **Clear path format:** Ensure users understand the " > " separator
2. **Provide examples:** Show sample paths in UI or help text
3. **Validate input:** Check that typed paths exist before navigation
4. **Error feedback:** Show helpful messages for invalid paths
5. **Case handling:** Consider case-insensitive matching for user convenience

### When Edit Mode is Useful

**Ideal for:**
- Power users familiar with the hierarchy
- Deep nested structures (4+ levels)
- Frequent navigation to specific locations
- Applications where path is known (file systems, URLs)

**Less useful for:**
- Shallow hierarchies (2-3 levels)
- Novice users unfamiliar with structure
- Dynamic or frequently changing hierarchies

## BarPopup Event Advanced Usage

The BarPopup event provides fine-grained control over dropdown behavior, allowing you to customize what appears and when.

### Event Parameters

```csharp
navigationView1.BarPopup += NavigationView1_BarPopup;

private void NavigationView1_BarPopup(object sender, BarPopupEventArgs e)
{
    // e.CurrentBar: The Bar whose dropdown is opening
    // e.MaximumItemsToDisplay: Control how many items show
    // e.Cancel: Set true to prevent dropdown from showing
}
```

### Controlling Maximum Items

Limit dropdown items displayed (10-20 is typical):

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    e.MaximumItemsToDisplay = 15;  // Remaining items accessible via scrolling
    
    // Conditional limits based on Bar
    if (e.CurrentBar.Text == "Program Files")
        e.MaximumItemsToDisplay = 10;
    
    // Dynamic limits based on count
    int count = e.CurrentBar.Bars.Count;
    if (count > 30)
        e.MaximumItemsToDisplay = 10;
};
```

### Canceling Popup Display

Prevent dropdown from showing in specific situations:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    // Don't show if no children
    if (e.CurrentBar.Bars.Count == 0)
    {
        e.Cancel = true;
        return;
    }
    
    // Check permissions
    if (e.CurrentBar.Tag is string path && !HasReadPermission(path))
    {
        e.Cancel = true;
        MessageBox.Show("Access denied.");
        return;
    }
};

private bool HasReadPermission(string path)
{
    try { Directory.GetDirectories(path); return true; }
    catch { return false; }
}
```

### Lazy Loading in BarPopup

Load child Bars on-demand when popup opens:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    // Load children if not already loaded
    if (e.CurrentBar.Bars.Count == 0)
    {
        if (e.CurrentBar.Tag is string path)
            LoadChildBarsFromFileSystem(e.CurrentBar, path);
        else if (e.CurrentBar.Tag is int categoryId)
            LoadChildBarsFromDatabase(e.CurrentBar, categoryId);
    }
    
    e.MaximumItemsToDisplay = 12;
    if (e.CurrentBar.Bars.Count == 0)
        e.Cancel = true;
};

private void LoadChildBarsFromFileSystem(Bar parentBar, string folderPath)
{
    try
    {
        foreach (string folder in Directory.GetDirectories(folderPath))
        {
            parentBar.Bars.Add(new Bar
            {
                Text = Path.GetFileName(folder),
                Tag = folder,
                ImageIndex = 2
            });
        }
    }
    catch { }
}

// Cache loaded paths to avoid reloading
private Dictionary<string, bool> loadedPaths = new Dictionary<string, bool>();
```

## Complete Example: File Browser with Lazy Loading

```csharp
private void CreateAdvancedFileBrowser()
{
    NavigationView nav = new NavigationView();
    nav.Dock = DockStyle.Top;
    nav.Height = 30;
    nav.VisualStyle = VisualStyles.Office2007;
    
    // Setup images
    ImageList imgList = new ImageList();
    imgList.Images.Add(Properties.Resources.Computer);
    imgList.Images.Add(Properties.Resources.Drive);
    imgList.Images.Add(Properties.Resources.Folder);
    nav.ImageList = imgList;
    
    // Create root with drives
    Bar computer = new Bar { Text = "Computer", ImageIndex = 0 };
    foreach (DriveInfo drive in DriveInfo.GetDrives())
    {
        if (drive.IsReady)
            computer.Bars.Add(new Bar { Text = drive.Name, Tag = drive.RootDirectory.FullName, ImageIndex = 1 });
    }
    
    nav.Bars.Add(computer);
    nav.SelectedBar = computer;
    
    // Lazy load folders on demand
    nav.BarPopup += (s, e) =>
    {
        if (e.CurrentBar.Bars.Count == 0 && e.CurrentBar.Tag is string path)
        {
            try
            {
                foreach (string folder in Directory.GetDirectories(path))
                {
                    e.CurrentBar.Bars.Add(new Bar
                    {
                        Text = Path.GetFileName(folder),
                        Tag = folder,
                        ImageIndex = 2
                    });
                }
                e.MaximumItemsToDisplay = 15;
            }
            catch { e.Cancel = true; }
        }
    };
    
    nav.BarSelected += (s, e) =>
    {
        if (nav.SelectedBar.Tag is string path)
            LoadFilesInLocation(path);
    };
    
    this.Controls.Add(nav);
}
```

## Best Practices

### Edit Mode
1. **Provide help text:** Show users the path format and separator
2. **Validate paths:** Check validity before attempting navigation
3. **Show feedback:** Indicate when path is invalid with clear message
4. **Allow escape:** Let users press Escape to cancel edit
5. **Document feature:** Many users won't discover edit mode without guidance

### BarPopup Event
1. **Set reasonable limits:** 10-20 items is good for most cases
2. **Load lazily:** Don't load all children upfront, use BarPopup
3. **Handle errors:** Catch exceptions when accessing data sources
4. **Cancel appropriately:** Don't show empty dropdowns
5. **Optimize performance:** Cache loaded data when possible
6. **User feedback:** Show messages when canceling popup (access denied, etc.)

### Performance
1. **Lazy loading is crucial:** Don't load entire hierarchy at startup
2. **Use BarPopup for on-demand loading:** Only load when dropdown opens
3. **Cache intelligently:** Remember loaded Bars to avoid reloading
4. **Limit recursion:** Don't auto-load child hierarchies multiple levels deep
5. **Async loading:** Consider async/await for database or network calls

## Summary

Advanced features enable powerful navigation:

- **Edit Mode:** Direct path typing using " > " separator for quick navigation
- **BarPopup Event:** Control dropdown behavior, item limits, and conditional display
- **Lazy Loading:** Load children on-demand for performance
- **Conditional Display:** Cancel popups based on permissions or state

These features are optional but improve usability for complex navigation scenarios.
