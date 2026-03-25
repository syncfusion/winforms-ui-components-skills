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

Each Bar in the breadcrumb path displays a **forward arrow** (►) to its right. Clicking this arrow shows a dropdown list containing all child Bars directly below the clicked Bar.

**Visual Example:**
```
MyComputer ► | Local Disk (C:) ► | Users ►
              └─ Dropdown opens here
                 ├── Local Disk (C:)
                 ├── Local Disk (D:)
                 └── Local Disk (E:)
```

**User Flow:**
1. User sees breadcrumb: `MyComputer > Local Disk (C:) > Users`
2. User clicks arrow next to "MyComputer"
3. Dropdown shows: C:, D:, E: drives
4. User selects "Local Disk (D:)"
5. Path updates to: `MyComputer > Local Disk (D:)`

### Automatic Dropdown Display

Dropdowns automatically display all child Bars of the clicked parent. No additional configuration is required.

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

### Dropdown for Each Level

Every Bar with children displays a dropdown arrow:

```csharp
// Multi-level with dropdowns at each level
Bar computer = new Bar { Text = "Computer" };
Bar cDrive = new Bar { Text = "C:" };
Bar users = new Bar { Text = "Users" };

Bar user1 = new Bar { Text = "John" };
Bar user2 = new Bar { Text = "Jane" };
Bar user3 = new Bar { Text = "Admin" };

users.Bars.AddRange(new Bar[] { user1, user2, user3 });
cDrive.Bars.Add(users);
computer.Bars.Add(cDrive);

navigationView1.Bars.Add(computer);
navigationView1.SelectedBar = users;

// Displays: Computer > C: > Users
// Arrow after "Computer" → shows C:, D:, etc.
// Arrow after "C:" → shows Users, Program Files, etc.
// Arrow after "Users" → shows John, Jane, Admin
```

### Handling Dropdown Selection

React to user selections from dropdowns:

```csharp
// BarSelected event fires when user picks from dropdown
navigationView1.BarSelected += (sender, e) =>
{
    Bar selected = navigationView1.SelectedBar;
    
    Console.WriteLine($"User navigated to: {selected.Text}");
    
    // Load content for new location
    if (selected.Tag is string path)
    {
        LoadContent(path);
    }
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

### Programmatic Navigation with History

History automatically tracks programmatic navigation too:

```csharp
// Each SelectedBar change adds to history
navigationView1.SelectedBar = documentsBar;  // Added to history
Thread.Sleep(1000);
navigationView1.SelectedBar = projectsBar;   // Added to history
Thread.Sleep(1000);
navigationView1.SelectedBar = downloadsBar;  // Added to history

// User can now use history button to return to any of these
```

## Custom Buttons

Add custom functionality buttons (search, refresh, settings) to the NavigationView.

### Adding Custom Buttons via Designer

1. **Select NavigationView** in the designer
2. **Locate CustomButton Collection** in Properties window
3. **Click the ellipsis (...)** to open CustomButton Collection Editor
4. **Click Add** to create a new custom button
5. **Set properties:**
   - `Name`: Unique identifier
   - `Image`: Button icon
   - `Appearance`: Visual style
   - `ToolTip`: Hover text
6. **Click OK** to apply

### Adding Custom Buttons via Code

```csharp
using Syncfusion.Windows.Forms.Tools.Navigation;

// Create custom button
CustomButton searchButton = new CustomButton();
searchButton.Name = "searchButton";
searchButton.Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007;

// Load image (from resources or file)
Bitmap searchIcon = new Bitmap(@"..\..\Search.gif");
searchButton.Image = searchIcon;

// Optional: Set size
searchButton.Width = 24;
searchButton.Height = 24;

// Handle click event
searchButton.Click += SearchButton_Click;

// Add to NavigationView
navigationView1.Controls.Add(searchButton);

private void SearchButton_Click(object sender, EventArgs e)
{
    // Show search dialog
    ShowSearchDialog();
}
```

### Multiple Custom Buttons

```csharp
// Create multiple custom buttons
CustomButton refreshButton = new CustomButton
{
    Name = "refreshButton",
    Image = Properties.Resources.RefreshIcon,
    Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007
};

CustomButton settingsButton = new CustomButton
{
    Name = "settingsButton",
    Image = Properties.Resources.SettingsIcon,
    Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007
};

CustomButton helpButton = new CustomButton
{
    Name = "helpButton",
    Image = Properties.Resources.HelpIcon,
    Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007
};

// Wire up events
refreshButton.Click += (s, e) => RefreshCurrentLocation();
settingsButton.Click += (s, e) => ShowSettings();
helpButton.Click += (s, e) => ShowHelp();

// Add all to NavigationView
navigationView1.Controls.Add(refreshButton);
navigationView1.Controls.Add(settingsButton);
navigationView1.Controls.Add(helpButton);
```

### Custom Button Appearance Styles

```csharp
// Match NavigationView visual style
CustomButton button = new CustomButton();

// Office 2007 style
button.Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007;

// Metro flat style
button.Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Metro;

// Office 2016 style
button.Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2016;
```

### Custom Button Best Practices

1. **Use clear icons:** Button images should be recognizable at 16x16 or 24x24 pixels
2. **Add tooltips:** Help users understand button purpose
3. **Match style:** Set Appearance to match NavigationView.VisualStyle
4. **Limit count:** 2-4 custom buttons maximum to avoid clutter
5. **Common uses:** Search, refresh, filter, settings, help

## BarPopup Event

The `BarPopup` event fires just before a dropdown opens, allowing you to customize or cancel the popup.

### Event Overview

**When it fires:**
- User clicks forward arrow next to a Bar
- Just before dropdown list displays

**What you can do:**
- Set maximum items to display
- Cancel the popup entirely
- Modify which items appear

### Basic BarPopup Event

```csharp
// Subscribe to event
navigationView1.BarPopup += NavigationView1_BarPopup;

private void NavigationView1_BarPopup(object sender, BarPopupEventArgs e)
{
    // e.CurrentBar: The Bar whose arrow was clicked
    // e.MaximumItemsToDisplay: Control how many children show
    // e.Cancel: Set to true to prevent popup
    
    Console.WriteLine($"Popup opening for: {e.CurrentBar.Text}");
}
```

### Limiting Items in Dropdown

Control how many items appear in dropdown (useful for Bars with many children):

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    // Show maximum 10 items (rest accessible via scrolling)
    e.MaximumItemsToDisplay = 10;
};
```

### Conditional Item Limits

Different limits for different Bars:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    if (e.CurrentBar.Text.Equals("Program Files"))
    {
        // Lots of folders, limit to 15
        e.MaximumItemsToDisplay = 15;
    }
    else if (e.CurrentBar.Text.Equals("Drives"))
    {
        // Few drives, show all
        e.MaximumItemsToDisplay = 50;
    }
    else
    {
        // Default limit
        e.MaximumItemsToDisplay = 10;
    }
};
```

### Canceling Popup Display

Prevent popup for specific Bars:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    // Don't show popup for empty folders
    if (e.CurrentBar.Bars.Count == 0)
    {
        e.Cancel = true;
        return;
    }
    
    // Don't show popup for specific Bar
    if (e.CurrentBar.Text.Equals("RestrictedFolder"))
    {
        e.Cancel = true;
        MessageBox.Show("Access to this folder is restricted.");
        return;
    }
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

### Example 1: Full Featured Navigation

```csharp
private void CreateFullFeaturedNavigation()
{
    // Create NavigationView
    NavigationView nav = new NavigationView();
    nav.Width = 600;
    nav.Height = 28;
    nav.Location = new Point(10, 10);
    
    // Enable history
    nav.ShowHistoryButtons = true;
    
    // Create hierarchy
    Bar computer = new Bar { Text = "This PC" };
    
    Bar cDrive = new Bar { Text = "C:", Tag = @"C:\" };
    Bar dDrive = new Bar { Text = "D:", Tag = @"D:\" };
    
    Bar users = new Bar { Text = "Users" };
    Bar programFiles = new Bar { Text = "Program Files" };
    
    cDrive.Bars.AddRange(new Bar[] { users, programFiles });
    computer.Bars.AddRange(new Bar[] { cDrive, dDrive });
    
    nav.Bars.Add(computer);
    nav.SelectedBar = computer;
    
    // Add custom search button
    CustomButton search = new CustomButton
    {
        Name = "search",
        Image = Properties.Resources.SearchIcon,
        Appearance = Syncfusion.Windows.Forms.ButtonAppearance.Office2007
    };
    search.Click += (s, e) => MessageBox.Show("Search clicked");
    nav.Controls.Add(search);
    
    // Handle bar selection
    nav.BarSelected += (s, e) =>
    {
        string path = nav.SelectedBar.Tag?.ToString() ?? nav.SelectedBar.Text;
        this.Text = $"Location: {path}";
    };
    
    // Control popup behavior
    nav.BarPopup += (s, e) =>
    {
        e.MaximumItemsToDisplay = 15;
        
        if (e.CurrentBar.Bars.Count == 0)
        {
            e.Cancel = true;
        }
    };
    
    this.Controls.Add(nav);
}
```

### Example 2: File Browser with Lazy Loading

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
