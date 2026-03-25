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
Bar documents = new Bar { Text = "Documents" };

users.Bars.Add(documents);
cDrive.Bars.Add(users);
computer.Bars.Add(cDrive);

nav.Bars.Add(computer);
nav.SelectedBar = computer;

// User can now:
// 1. Click on "Computer" text
// 2. Type: "Computer > C: > Users > Documents"
// 3. Press Enter
// 4. Instantly navigate to Documents
```

### Path Format in Edit Mode

**Standard Path Separator:** Use " > " (space-arrow-space) to separate Bar levels:

```
Computer > C: > Users > Documents
MyComputer > Local Disk (C:) > Program Files
Root > Category > Subcategory > Item
```

**Important:**
- Separator is " > " with spaces
- Each segment must match Bar.Text exactly
- Case sensitivity depends on implementation

### Validating Typed Paths

Handle invalid paths gracefully:

```csharp
// Assuming you track path changes
navigationView1.BarSelected += (sender, e) =>
{
    // Validate selected Bar exists and is accessible
    Bar selected = navigationView1.SelectedBar;
    
    if (selected != null && selected.Tag is string path)
    {
        if (Directory.Exists(path))
        {
            LoadContent(path);
        }
        else
        {
            MessageBox.Show("Invalid path entered.");
            // Optionally revert to previous valid location
        }
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

Limit the number of items displayed in dropdowns:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    // Show maximum 10 items
    e.MaximumItemsToDisplay = 10;
    
    // Remaining items accessible via scrolling
};
```

**Why limit items?**
- Prevents overwhelming dropdowns with 50+ items
- Improves readability
- Faster rendering for large collections
- Better UX for reasonable item counts

### Conditional Item Limits

Different limits for different Bars:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    string barText = e.CurrentBar.Text;
    
    if (barText == "Program Files")
    {
        // Many folders, limit strictly
        e.MaximumItemsToDisplay = 12;
    }
    else if (barText == "Users")
    {
        // Fewer users, show more
        e.MaximumItemsToDisplay = 20;
    }
    else if (barText == "Computer")
    {
        // Drives, show all
        e.MaximumItemsToDisplay = 50;
    }
    else
    {
        // Default
        e.MaximumItemsToDisplay = 15;
    }
};
```

### Dynamic Item Limits Based on Count

Adjust limit based on actual child count:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    int childCount = e.CurrentBar.Bars.Count;
    
    if (childCount <= 5)
    {
        // Few items, show all
        e.MaximumItemsToDisplay = childCount;
    }
    else if (childCount <= 20)
    {
        // Moderate, show most
        e.MaximumItemsToDisplay = 15;
    }
    else
    {
        // Many items, limit more strictly
        e.MaximumItemsToDisplay = 10;
    }
};
```

### Canceling Popup Display

Prevent dropdown from showing in specific situations:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    // Don't show popup if no children
    if (e.CurrentBar.Bars.Count == 0)
    {
        e.Cancel = true;
        return;
    }
    
    // Don't show popup for restricted areas
    if (e.CurrentBar.Tag is string path && path.Contains("System32"))
    {
        e.Cancel = true;
        MessageBox.Show("Access to this folder is restricted.");
        return;
    }
    
    // Don't show during specific operations
    if (isLoadingData)
    {
        e.Cancel = true;
        return;
    }
};
```

### Conditional Popup Based on User Permissions

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    if (e.CurrentBar.Tag is string path)
    {
        // Check if user has read permission
        if (!HasReadPermission(path))
        {
            e.Cancel = true;
            MessageBox.Show("You don't have permission to view this folder's contents.");
            return;
        }
    }
    
    e.MaximumItemsToDisplay = 15;
};

private bool HasReadPermission(string path)
{
    try
    {
        Directory.GetDirectories(path);
        return true;
    }
    catch (UnauthorizedAccessException)
    {
        return false;
    }
    catch
    {
        return false;
    }
}
```

### Lazy Loading in BarPopup

Load child Bars on-demand when popup is about to show:

```csharp
navigationView1.BarPopup += (sender, e) =>
{
    Bar currentBar = e.CurrentBar;
    
    // Check if children already loaded
    if (currentBar.Bars.Count == 0)
    {
        // Load children from data source
        if (currentBar.Tag is string path)
        {
            LoadChildBarsFromFileSystem(currentBar, path);
        }
        else if (currentBar.Tag is int categoryId)
        {
            LoadChildBarsFromDatabase(currentBar, categoryId);
        }
    }
    
    // Set limit after loading
    e.MaximumItemsToDisplay = 12;
    
    // Cancel if loading failed
    if (currentBar.Bars.Count == 0)
    {
        e.Cancel = true;
    }
};

private void LoadChildBarsFromFileSystem(Bar parentBar, string folderPath)
{
    try
    {
        string[] subfolders = Directory.GetDirectories(folderPath);
        
        foreach (string folder in subfolders)
        {
            Bar childBar = new Bar
            {
                Text = Path.GetFileName(folder),
                Tag = folder,
                ImageIndex = 2 // Folder icon
            };
            
            parentBar.Bars.Add(childBar);
        }
    }
    catch (UnauthorizedAccessException)
    {
        // User doesn't have access
        Debug.WriteLine($"Access denied: {folderPath}");
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"Error loading folder: {ex.Message}");
    }
}

private void LoadChildBarsFromDatabase(Bar parentBar, int categoryId)
{
    // Example: Load from database
    var subcategories = Database.GetSubcategories(categoryId);
    
    foreach (var subcat in subcategories)
    {
        Bar childBar = new Bar
        {
            Text = subcat.Name,
            Tag = subcat.Id
        };
        
        parentBar.Bars.Add(childBar);
    }
}
```

### Performance Optimization with BarPopup

```csharp
// Cache to avoid reloading
private Dictionary<string, bool> loadedPaths = new Dictionary<string, bool>();

navigationView1.BarPopup += (sender, e) =>
{
    if (e.CurrentBar.Tag is string path)
    {
        // Only load if not already loaded
        if (!loadedPaths.ContainsKey(path))
        {
            LoadChildBarsFromFileSystem(e.CurrentBar, path);
            loadedPaths[path] = true;
        }
    }
    
    e.MaximumItemsToDisplay = 15;
};
```

## Complete Advanced Examples

### Example 1: Full-Featured File Browser

```csharp
private void CreateAdvancedFileBrowser()
{
    NavigationView nav = new NavigationView();
    nav.Dock = DockStyle.Top;
    nav.Height = 30;
    nav.VisualStyle = VisualStyles.Office2007;
    nav.ShowHistoryButtons = true;
    
    // Setup images
    ImageList imgList = new ImageList();
    imgList.Images.Add(Properties.Resources.Computer);
    imgList.Images.Add(Properties.Resources.Drive);
    imgList.Images.Add(Properties.Resources.Folder);
    nav.ImageList = imgList;
    
    // Create root
    Bar computer = new Bar { Text = "Computer", ImageIndex = 0 };
    
    // Add drives
    foreach (DriveInfo drive in DriveInfo.GetDrives())
    {
        if (drive.IsReady)
        {
            Bar driveBar = new Bar
            {
                Text = drive.Name,
                Tag = drive.RootDirectory.FullName,
                ImageIndex = 1
            };
            computer.Bars.Add(driveBar);
        }
    }
    
    nav.Bars.Add(computer);
    nav.SelectedBar = computer;
    
    // Lazy load with permission checking
    nav.BarPopup += (s, e) =>
    {
        // Load children on demand
        if (e.CurrentBar.Bars.Count == 0 && e.CurrentBar.Tag is string path)
        {
            try
            {
                string[] folders = Directory.GetDirectories(path);
                
                foreach (string folder in folders)
                {
                    Bar childBar = new Bar
                    {
                        Text = Path.GetFileName(folder),
                        Tag = folder,
                        ImageIndex = 2
                    };
                    
                    e.CurrentBar.Bars.Add(childBar);
                }
                
                // Limit display for large folders
                e.MaximumItemsToDisplay = folders.Length > 20 ? 15 : 25;
            }
            catch (UnauthorizedAccessException)
            {
                e.Cancel = true;
                MessageBox.Show($"Access denied to: {path}");
            }
            catch
            {
                e.Cancel = true;
            }
        }
        else
        {
            // Already loaded, just set limit
            e.MaximumItemsToDisplay = 15;
        }
    };
    
    // Handle navigation
    nav.BarSelected += (s, e) =>
    {
        if (nav.SelectedBar.Tag is string path)
        {
            LoadFilesInLocation(path);
        }
    };
    
    this.Controls.Add(nav);
}

private void LoadFilesInLocation(string path)
{
    // Your logic to display files
    Debug.WriteLine($"Showing files in: {path}");
}
```

### Example 2: Database-Driven Navigation with Edit Mode

```csharp
private void CreateDatabaseNavigation()
{
    NavigationView nav = new NavigationView();
    nav.Width = 600;
    nav.Height = 28;
    nav.Location = new Point(10, 10);
    nav.VisualStyle = VisualStyles.Metro;
    
    // Root category
    Bar rootCat = new Bar { Text = "Products", Tag = 0 };
    nav.Bars.Add(rootCat);
    nav.SelectedBar = rootCat;
    
    // Load categories on demand
    nav.BarPopup += (s, e) =>
    {
        if (e.CurrentBar.Bars.Count == 0 && e.CurrentBar.Tag is int categoryId)
        {
            LoadSubcategories(e.CurrentBar, categoryId);
        }
        
        // Limit based on count
        int count = e.CurrentBar.Bars.Count;
        e.MaximumItemsToDisplay = count > 30 ? 10 : 20;
        
        // Cancel if no children
        if (e.CurrentBar.Bars.Count == 0)
        {
            e.Cancel = true;
        }
    };
    
    // Handle selection
    nav.BarSelected += (s, e) =>
    {
        if (nav.SelectedBar.Tag is int categoryId)
        {
            LoadProductsForCategory(categoryId);
        }
    };
    
    this.Controls.Add(nav);
}

private void LoadSubcategories(Bar parentBar, int parentCategoryId)
{
    // Simulated database call
    var subcategories = GetSubcategoriesFromDatabase(parentCategoryId);
    
    foreach (var subcat in subcategories)
    {
        Bar childBar = new Bar
        {
            Text = subcat.Name,
            Tag = subcat.Id
        };
        
        parentBar.Bars.Add(childBar);
    }
}

private List<Category> GetSubcategoriesFromDatabase(int parentId)
{
    // Your database logic here
    return new List<Category>();
}

private void LoadProductsForCategory(int categoryId)
{
    // Your logic to display products
    Debug.WriteLine($"Loading products for category: {categoryId}");
}

public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
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

## Troubleshooting

**Issue:** Edit mode not working
- **Check:** Users may not know to click text area—provide instructions
- **Check:** Path format must match exactly (including separators)

**Issue:** BarPopup not firing
- **Check:** Event is subscribed after NavigationView creation
- **Check:** Bars have children to display

**Issue:** MaximumItemsToDisplay not working
- **Check:** Property set before BarPopup event handler returns
- **Check:** Value is positive integer

**Issue:** Popup cancelled but still appears
- **Check:** e.Cancel = true is set before event handler returns
- **Check:** No other code overriding the cancellation

**Issue:** Lazy loading causes delay
- **Solution:** Show loading indicator
- **Solution:** Consider background loading
- **Solution:** Reduce number of items loaded per popup

## Summary

Advanced features enable powerful navigation:

- **Edit Mode:** Direct path typing for quick navigation
- **BarPopup Event:** Fine-grained control over dropdown behavior
- **Lazy Loading:** Load children on-demand for performance
- **Conditional Display:** Show/hide popups based on context

These features are optional but significantly improve usability for complex navigation scenarios.

## Next Steps

- Review complete working examples in parent SKILL.md
- Test advanced features in your specific use case
- Consider user experience when enabling features
- Provide documentation/help for power user features like edit mode
