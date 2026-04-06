# Dropdown Selection and History Features

## Table of Contents
- [Overview](#overview)
- [Dropdown Selection Mechanism](#dropdown-selection-mechanism)
- [History Button and Tracking](#history-button-and-tracking)
- [Custom Buttons](#custom-buttons)
- [BarPopup Event](#barpopup-event)
- [Complete Examples](#complete-examples)

## Overview

NavigationView provides two powerful navigation features beyond basic breadcrumb display:

1. **Dropdown Selection:** Forward arrow next to each Bar opens a dropdown showing child Bars
2. **History Tracking:** Dropdown button tracks previously visited locations for quick return

These features enable fast navigation through complex hierarchies without repeated clicking.

## Dropdown Selection Mechanism

### How Dropdown Selection Works

Each Bar displays a **forward arrow** (►). Clicking it shows a dropdown list of all child Bars. No configuration required - dropdowns appear automatically for Bars with children.

```csharp
// Create hierarchy
Bar computer = new Bar { Text = "This PC" };

Bar cDrive = new Bar { Text = "Local Disk (C:)" };
Bar dDrive = new Bar { Text = "Local Disk (D:)" };
Bar eDrive = new Bar { Text = "Local Disk (E:)" };

// Add children - they automatically appear in dropdown
computer.Bars.AddRange(new Bar[] { cDrive, dDrive, eDrive });

navigationView1.Bars.Add(computer);
navigationView1.SelectedBar = computer;

// Arrow appears automatically next to "This PC"
// Clicking arrow shows all three drives
```

### Multi-Level Dropdowns and Selection

```csharp
// Create multi-level hierarchy - dropdowns appear at each level
Bar computer = new Bar { Text = "Computer" };
Bar cDrive = new Bar { Text = "C:" };
Bar users = new Bar { Text = "Users" };

users.Bars.AddRange(new Bar[] { 
    new Bar { Text = "John" }, 
    new Bar { Text = "Jane" } 
});
cDrive.Bars.Add(users);
computer.Bars.Add(cDrive);

navigationView1.Bars.Add(computer);
navigationView1.SelectedBar = users;

// Handle dropdown selections
navigationView1.BarSelected += (sender, e) =>
{
    Console.WriteLine($"Navigated to: {navigationView1.SelectedBar.Text}");
    if (navigationView1.SelectedBar.Tag is string path)
        LoadContent(path);
};
```

## History Button and Tracking

The history feature tracks previously visited Bars, allowing users to quickly return to recent locations.

### Enabling History Button

```csharp
// Enable history dropdown button
navigationView1.ShowHistoryButtons = true;
```

**Visual Result:** A dropdown button appears at the right edge of the NavigationView showing previously visited locations.

### How History Tracking Works

1. User navigates to different Bars (via dropdown or code)
2. Each visited Bar is added to history
3. User clicks history button
4. Dropdown shows list of recently visited locations
5. User selects a location to return to it

**Example Flow:**
```
1. User visits: Computer > C: > Users > Documents
2. User visits: Computer > D: > Projects
3. User visits: Computer > C: > Program Files
4. History dropdown shows:
   - Computer > C: > Program Files (current)
   - Computer > D: > Projects
   - Computer > C: > Users > Documents
5. User clicks "Documents" → instantly navigates there
```

### History Button Example

```csharp
private void SetupNavigationWithHistory()
{
    NavigationView navigationView1 = new NavigationView();
    navigationView1.Width = 500;
    navigationView1.Height = 25;
    navigationView1.Location = new Point(20, 20);
    
    // Enable history tracking
    navigationView1.ShowHistoryButtons = true;
    
    // Create hierarchy
    Bar computer = new Bar { Text = "Computer" };
    Bar cDrive = new Bar { Text = "C:" };
    Bar dDrive = new Bar { Text = "D:" };
    
    Bar users = new Bar { Text = "Users" };
    Bar projects = new Bar { Text = "Projects" };
    
    cDrive.Bars.Add(users);
    dDrive.Bars.Add(projects);
    computer.Bars.AddRange(new Bar[] { cDrive, dDrive });
    
    navigationView1.Bars.Add(computer);
    navigationView1.SelectedBar = computer;
    
    this.Controls.Add(navigationView1);
}
```

## Custom Buttons

Add custom functionality buttons to the NavigationView.

### Adding Custom Buttons

```csharp
// Create and configure custom button
CustomButton searchButton = new CustomButton
{
    Name = "searchButton",
    Image = Properties.Resources.SearchIcon,
    Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007,
    Width = 24,
    Height = 24
};

// Handle click event
searchButton.Click += (s, e) => ShowSearchDialog();

// Add to NavigationView
navigationView1.Controls.Add(searchButton);
```

### Multiple Custom Buttons

```csharp
// Add multiple buttons with different actions
var buttons = new[] {
    new { Name = "refresh", Icon = Properties.Resources.RefreshIcon, Action = (EventHandler)((s,e) => RefreshLocation()) },
    new { Name = "settings", Icon = Properties.Resources.SettingsIcon, Action = (EventHandler)((s,e) => ShowSettings()) }
};

foreach (var btn in buttons)
{
    CustomButton button = new CustomButton {
        Name = btn.Name,
        Image = btn.Icon,
        Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007
    };
    button.Click += btn.Action;
    navigationView1.Controls.Add(button);
}
```

## BarPopup Event

The `BarPopup` event fires just before a dropdown opens, allowing you to customize or cancel the popup.

### BarPopup Usage

Fires before dropdown opens. Control item display or cancel popup:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    // Limit items in dropdown
    e.MaximumItemsToDisplay = 10;
    
    // Cancel for empty or restricted Bars
    if (e.CurrentBar.Bars.Count == 0)
    {
        e.Cancel = true;
        return;
    }
    
    // Conditional limits
    if (e.CurrentBar.Text.Equals("Program Files"))
        e.MaximumItemsToDisplay = 15;
};
```

### Dynamic Item Loading in BarPopup

Load child Bars on-demand when popup opens:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    Bar currentBar = e.CurrentBar;
    
    // Load children if not already loaded
    if (currentBar.Bars.Count == 0 && currentBar.Tag is string path)
    {
        try
        {
            // Load folders from file system
            string[] subfolders = Directory.GetDirectories(path);
            
            foreach (string folder in subfolders)
            {
                Bar childBar = new Bar
                {
                    Text = Path.GetFileName(folder),
                    Tag = folder
                };
                
                currentBar.Bars.Add(childBar);
            }
            
            // Limit display
            e.MaximumItemsToDisplay = 12;
        }
        catch (UnauthorizedAccessException)
        {
            // Cancel popup if access denied
            e.Cancel = true;
            MessageBox.Show("Access denied to this folder.");
        }
    }
};
```

## Complete Examples

### Example: Full Featured Navigation with Lazy Loading

```csharp
private void CreateFileBrowser()
{
    NavigationView nav = new NavigationView();
    nav.Dock = DockStyle.Top;
    nav.Height = 28;
    nav.ShowHistoryButtons = true;
    
    // Create root
    Bar computer = new Bar { Text = "Computer" };
    
    // Add drives
    foreach (DriveInfo drive in DriveInfo.GetDrives())
    {
        if (drive.IsReady)
        {
            Bar driveBar = new Bar
            {
                Text = $"{drive.Name} ({drive.VolumeLabel})",
                Tag = drive.RootDirectory.FullName
            };
            
            computer.Bars.Add(driveBar);
        }
    }
    
    nav.Bars.Add(computer);
    nav.SelectedBar = computer;
    
    // Lazy load folders on popup
    nav.BarPopup += (s, e) =>
    {
        if (e.CurrentBar.Bars.Count == 0 && e.CurrentBar.Tag is string path)
        {
            LoadSubfolders(e.CurrentBar, path);
        }
        
        e.MaximumItemsToDisplay = 20;
    };
    
    // Navigate to folder on selection
    nav.BarSelected += (s, e) =>
    {
        if (nav.SelectedBar.Tag is string path)
        {
            LoadFilesInFolder(path);
        }
    };
    
    this.Controls.Add(nav);
}

private void LoadSubfolders(Bar parentBar, string folderPath)
{
    try
    {
        string[] folders = Directory.GetDirectories(folderPath);
        
        foreach (string folder in folders)
        {
            Bar childBar = new Bar
            {
                Text = Path.GetFileName(folder),
                Tag = folder
            };
            
            parentBar.Bars.Add(childBar);
        }
    }
    catch (Exception ex)
    {
        // Handle access errors silently
        Debug.WriteLine($"Cannot access: {folderPath} - {ex.Message}");
    }
}

private void LoadFilesInFolder(string path)
{
    // Your logic to display files in the selected folder
    Debug.WriteLine($"Loading files from: {path}");
}
```

## Best Practices

1. **Enable history for complex hierarchies:** Users appreciate quick return navigation
2. **Limit popup items:** Set MaximumItemsToDisplay between 10-20 for large collections
3. **Load on demand:** Use BarPopup to load children only when needed (performance)
4. **Cancel empty popups:** Don't show dropdown for Bars with no children
5. **Custom buttons for actions:** Use for operations like search, refresh, filter
6. **Match button style:** Keep custom button Appearance consistent with NavigationView
7. **Handle errors gracefully:** Catch exceptions in BarPopup and cancel popup if needed

## Next Steps

- Learn about adding images to Bars in [images-and-styling.md](images-and-styling.md)
- Explore edit mode in [advanced-features.md](advanced-features.md)
