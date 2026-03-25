# Bar Hierarchy and Structure

This guide covers how to create, manage, and navigate hierarchical Bar structures in NavigationView.

## Understanding Bars

A `Bar` is the fundamental building block of NavigationView. Each Bar represents a single node in the navigation hierarchy, similar to a folder in a file system or a category in a menu.

**Key Bar Characteristics:**
- Has a `Text` property for display text
- Can contain child Bars in its `Bars` collection
- Can have an associated image via `ImageIndex`
- Can store custom data using the `Tag` property
- Part of a parent-child hierarchy

**Hierarchy Levels:**
- **Root Bar:** Top-level Bar added directly to `NavigationView.Bars`
- **Child Bar:** Bar nested within another Bar's `Bars` collection
- **Leaf Bar:** Bar with no children (end of a branch)

## Creating Bar Instances

### Basic Bar Creation

```csharp
using Syncfusion.Windows.Forms.Tools.Navigation;

// Create a Bar
Bar myBar = new Bar();
myBar.Text = "Documents";

// Create with object initializer (recommended)
Bar myBar = new Bar 
{ 
    Text = "Documents",
    ImageIndex = 0,
    Tag = @"C:\Users\Documents"
};
```

### Creating Multiple Bars

```csharp
// Method 1: Individual creation
Bar bar1 = new Bar { Text = "Folder 1" };
Bar bar2 = new Bar { Text = "Folder 2" };
Bar bar3 = new Bar { Text = "Folder 3" };

// Method 2: Array creation
Bar[] bars = new Bar[]
{
    new Bar { Text = "Folder 1" },
    new Bar { Text = "Folder 2" },
    new Bar { Text = "Folder 3" }
};
```

## Building Bar Hierarchies

### Adding Root Bars

Root Bars are added to the NavigationView's `Bars` collection:

```csharp
// Create NavigationView
NavigationView navigationView1 = new NavigationView();

// Create root Bar
Bar rootBar = new Bar { Text = "MyComputer" };

// Add to NavigationView - Method 1: Add single
navigationView1.Bars.Add(rootBar);

// Method 2: Add multiple
Bar root1 = new Bar { Text = "Computer" };
Bar root2 = new Bar { Text = "Network" };
navigationView1.Bars.AddRange(new Bar[] { root1, root2 });
```

**Important:** Most NavigationView implementations use a single root Bar, but multiple roots are supported.

### Adding Child Bars

Child Bars are added to a parent Bar's `Bars` collection:

```csharp
// Create parent Bar
Bar parentBar = new Bar { Text = "MyComputer" };

// Create child Bars
Bar childBar1 = new Bar { Text = "Local Disk (C:)" };
Bar childBar2 = new Bar { Text = "Local Disk (D:)" };

// Add children to parent - Method 1: Add single
parentBar.Bars.Add(childBar1);

// Method 2: Add multiple
parentBar.Bars.AddRange(new Bar[] { childBar1, childBar2 });
```

### Creating Multi-Level Hierarchies

Build deep hierarchies by adding children to children:

```csharp
// Level 1: Root
Bar computer = new Bar { Text = "This PC" };

// Level 2: Drives
Bar cDrive = new Bar { Text = "Local Disk (C:)" };
Bar dDrive = new Bar { Text = "Local Disk (D:)" };

// Level 3: Folders
Bar users = new Bar { Text = "Users" };
Bar programFiles = new Bar { Text = "Program Files" };

// Level 4: User folders
Bar documents = new Bar { Text = "Documents" };
Bar downloads = new Bar { Text = "Downloads" };
Bar pictures = new Bar { Text = "Pictures" };

// Build hierarchy
users.Bars.AddRange(new Bar[] { documents, downloads, pictures });
cDrive.Bars.AddRange(new Bar[] { users, programFiles });
computer.Bars.AddRange(new Bar[] { cDrive, dDrive });

// Add root to NavigationView
navigationView1.Bars.Add(computer);
```

**Resulting Structure:**
```
This PC
├── Local Disk (C:)
│   ├── Users
│   │   ├── Documents
│   │   ├── Downloads
│   │   └── Pictures
│   └── Program Files
└── Local Disk (D:)
```

## Setting the Selected Bar

The `SelectedBar` property determines which Bar is currently displayed in the breadcrumb path.

### Basic Selection

```csharp
// Select root Bar
navigationView1.SelectedBar = rootBar;

// Select a child Bar
navigationView1.SelectedBar = documentsBar;
```

**Display Result:** When you set `SelectedBar = documentsBar`, the NavigationView displays the full path:
```
This PC > Local Disk (C:) > Users > Documents
```

### Programmatic Navigation

Navigate through the hierarchy programmatically:

```csharp
// Navigate to a specific location
private void NavigateToDocuments()
{
    Bar computer = navigationView1.Bars[0]; // Root
    Bar cDrive = computer.Bars[0]; // First child
    Bar users = cDrive.Bars[0]; // Users folder
    Bar documents = users.Bars[0]; // Documents folder
    
    navigationView1.SelectedBar = documents;
}

// Navigate using stored reference
private Bar documentsBar;

private void SetupNavigation()
{
    // Store reference during creation
    documentsBar = new Bar { Text = "Documents" };
    
    // ... add to hierarchy ...
}

private void GoToDocuments()
{
    navigationView1.SelectedBar = documentsBar;
}
```

## Bar Properties

### Essential Properties

```csharp
Bar bar = new Bar();

// Text: Display name
bar.Text = "My Folder";

// ImageIndex: Icon from NavigationView.ImageList
bar.ImageIndex = 2;

// Tag: Store custom data
bar.Tag = @"C:\MyFolder"; // Store file path
bar.Tag = new CustomData { Id = 1, Name = "Folder" }; // Store object

// Bars: Child Bar collection
int childCount = bar.Bars.Count;
Bar firstChild = bar.Bars[0];
```

### Using Tag Property for Custom Data

Store associated data with each Bar:

```csharp
// Store file system path
Bar folderBar = new Bar 
{ 
    Text = "Documents", 
    Tag = @"C:\Users\John\Documents" 
};

// Retrieve and use
private void OnBarSelected(object sender, EventArgs e)
{
    Bar selectedBar = navigationView1.SelectedBar;
    
    if (selectedBar.Tag is string path)
    {
        LoadFilesFromPath(path);
    }
}

// Store complex objects
public class FolderInfo
{
    public string Path { get; set; }
    public int FileCount { get; set; }
    public long Size { get; set; }
}

Bar bar = new Bar 
{ 
    Text = "Projects",
    Tag = new FolderInfo 
    { 
        Path = @"C:\Projects",
        FileCount = 150,
        Size = 52428800
    }
};
```

## Complete Hierarchy Example

Here's a comprehensive example showing a file system-like hierarchy:

```csharp
private void CreateFileSystemHierarchy()
{
    // Root
    Bar computer = new Bar { Text = "This PC" };
    
    // C Drive branch
    Bar cDrive = new Bar { Text = "Local Disk (C:)", Tag = @"C:\" };
    
    Bar windows = new Bar { Text = "Windows", Tag = @"C:\Windows" };
    Bar system32 = new Bar { Text = "System32", Tag = @"C:\Windows\System32" };
    windows.Bars.Add(system32);
    
    Bar users = new Bar { Text = "Users", Tag = @"C:\Users" };
    Bar currentUser = new Bar { Text = Environment.UserName, Tag = Environment.GetFolderPath(Environment.SpecialFolder.UserProfile) };
    
    Bar desktop = new Bar { Text = "Desktop", Tag = Environment.GetFolderPath(Environment.SpecialFolder.Desktop) };
    Bar documents = new Bar { Text = "Documents", Tag = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments) };
    Bar downloads = new Bar { Text = "Downloads", Tag = System.IO.Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.UserProfile), "Downloads") };
    
    currentUser.Bars.AddRange(new Bar[] { desktop, documents, downloads });
    users.Bars.Add(currentUser);
    
    Bar programFiles = new Bar { Text = "Program Files", Tag = @"C:\Program Files" };
    Bar programFilesX86 = new Bar { Text = "Program Files (x86)", Tag = @"C:\Program Files (x86)" };
    
    cDrive.Bars.AddRange(new Bar[] { windows, users, programFiles, programFilesX86 });
    
    // D Drive branch
    Bar dDrive = new Bar { Text = "Local Disk (D:)", Tag = @"D:\" };
    
    Bar projects = new Bar { Text = "Projects", Tag = @"D:\Projects" };
    Bar backup = new Bar { Text = "Backup", Tag = @"D:\Backup" };
    
    dDrive.Bars.AddRange(new Bar[] { projects, backup });
    
    // Add drives to computer
    computer.Bars.AddRange(new Bar[] { cDrive, dDrive });
    
    // Add to NavigationView
    navigationView1.Bars.Add(computer);
    navigationView1.SelectedBar = computer;
}
```

## Dynamic Bar Management

### Adding Bars Dynamically

```csharp
// Add child when parent is selected
private void NavigationView1_BarSelected(object sender, EventArgs e)
{
    Bar selectedBar = navigationView1.SelectedBar;
    
    // Load children on demand
    if (selectedBar.Bars.Count == 0 && selectedBar.Tag is string path)
    {
        LoadChildBars(selectedBar, path);
    }
}

private void LoadChildBars(Bar parentBar, string folderPath)
{
    try
    {
        string[] subfolders = System.IO.Directory.GetDirectories(folderPath);
        
        foreach (string subfolder in subfolders)
        {
            string folderName = System.IO.Path.GetFileName(subfolder);
            
            Bar childBar = new Bar 
            { 
                Text = folderName,
                Tag = subfolder
            };
            
            parentBar.Bars.Add(childBar);
        }
    }
    catch (Exception ex)
    {
        // Handle access denied or other errors
        MessageBox.Show($"Cannot access folder: {ex.Message}");
    }
}
```

### Removing Bars

```csharp
// Remove specific Bar
Bar barToRemove = parentBar.Bars[0];
parentBar.Bars.Remove(barToRemove);

// Remove at index
parentBar.Bars.RemoveAt(0);

// Clear all children
parentBar.Bars.Clear();
```

### Finding Bars

```csharp
// Find by text
private Bar FindBarByText(BarCollection bars, string text)
{
    foreach (Bar bar in bars)
    {
        if (bar.Text == text)
            return bar;
        
        // Search recursively in children
        Bar found = FindBarByText(bar.Bars, text);
        if (found != null)
            return found;
    }
    
    return null;
}

// Usage
Bar foundBar = FindBarByText(navigationView1.Bars, "Documents");
if (foundBar != null)
{
    navigationView1.SelectedBar = foundBar;
}
```

## Navigation Patterns

### Building Full Path from Selected Bar

Get the complete path to the selected Bar:

```csharp
private string GetFullPath(Bar bar)
{
    List<string> pathParts = new List<string>();
    Bar current = bar;
    
    while (current != null)
    {
        pathParts.Insert(0, current.Text);
        current = current.Parent as Bar;
    }
    
    return string.Join(" > ", pathParts);
}

// Usage
string path = GetFullPath(navigationView1.SelectedBar);
// Result: "This PC > Local Disk (C:) > Users > Documents"
```

### Navigating by Path String

Navigate to a location by path:

```csharp
private bool NavigateToPath(string[] pathParts)
{
    if (pathParts.Length == 0)
        return false;
    
    // Start from root
    Bar current = FindBarByText(navigationView1.Bars, pathParts[0]);
    
    if (current == null)
        return false;
    
    // Navigate through path
    for (int i = 1; i < pathParts.Length; i++)
    {
        Bar child = FindBarByText(current.Bars, pathParts[i]);
        
        if (child == null)
            return false;
        
        current = child;
    }
    
    navigationView1.SelectedBar = current;
    return true;
}

// Usage
string[] path = { "This PC", "Local Disk (C:)", "Users", "Documents" };
NavigateToPath(path);
```

## Best Practices

1. **Store references:** Keep references to frequently accessed Bars to avoid searching
2. **Use Tag property:** Store associated data (paths, IDs) in Tag for easy retrieval
3. **Lazy loading:** Load child Bars on demand for large hierarchies to improve performance
4. **Error handling:** Always handle exceptions when working with file system or external data
5. **Consistent naming:** Use clear, descriptive Text values that match the actual content
6. **Hierarchy depth:** Keep depth reasonable (3-6 levels) for good user experience

## Next Steps

- Learn about dropdown navigation in [dropdown-and-history.md](dropdown-and-history.md)
- Add icons to Bars in [images-and-styling.md](images-and-styling.md)
- Implement advanced navigation in [advanced-features.md](advanced-features.md)
