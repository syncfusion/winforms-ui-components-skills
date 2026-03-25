# TreeMenuItem Management

This guide covers creating, managing, and manipulating TreeMenuItem objects to build hierarchical navigation structures.

## TreeMenuItem Overview

The **TreeMenuItem** class represents individual navigation items in the TreeNavigator control. Each TreeMenuItem can contain child items, creating hierarchical navigation structures.

**Key Characteristics:**
- Represents a single navigation item
- Can contain child items (Items collection)
- Supports custom appearance properties
- Displays text and optional icons
- Responds to user clicks for navigation

---

## TreeMenuItem Properties

| Property | Type | Description |
|----------|------|-------------|
| **Text** | string | Display text for the item |
| **Items** | Collection | Child TreeMenuItem items |
| **ItemBackColor** | Color | Background color in normal state |
| **ItemHoverColor** | Color | Background color when mouse hovers |
| **SelectedColor** | Color | Background color when selected |
| **SelectedItemForeColor** | Color | Text color when selected |

---

## Creating TreeMenuItem Items

### Basic TreeMenuItem Creation

**C# Example:**

```csharp
// Create simple TreeMenuItem
TreeMenuItem item = new TreeMenuItem();
item.Text = "Documents";
```

**Inline Initialization (C#):**
```csharp
TreeMenuItem item = new TreeMenuItem { Text = "Documents" };
```

**VB.NET Example:**

```vbnet
' Create simple TreeMenuItem
Dim item As New TreeMenuItem()
item.Text = "Documents"
```

**Inline Initialization (VB.NET):**
```vbnet
Dim item As New TreeMenuItem With {.Text = "Documents"}
```

### Creating Multiple Items

**C# Example:**

```csharp
// Create multiple items
TreeMenuItem desktop = new TreeMenuItem { Text = "Desktop" };
TreeMenuItem documents = new TreeMenuItem { Text = "Documents" };
TreeMenuItem downloads = new TreeMenuItem { Text = "Downloads" };
TreeMenuItem pictures = new TreeMenuItem { Text = "Pictures" };
TreeMenuItem music = new TreeMenuItem { Text = "Music" };
```

**VB.NET Example:**

```vbnet
' Create multiple items
Dim desktop As New TreeMenuItem With {.Text = "Desktop"}
Dim documents As New TreeMenuItem With {.Text = "Documents"}
Dim downloads As New TreeMenuItem With {.Text = "Downloads"}
Dim pictures As New TreeMenuItem With {.Text = "Pictures"}
Dim music As New TreeMenuItem With {.Text = "Music"}
```

---

## Adding Items to TreeNavigator

### Adding to Items Collection

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();

// Add items one by one
treeNavigator.Items.Add(new TreeMenuItem { Text = "Home" });
treeNavigator.Items.Add(new TreeMenuItem { Text = "Documents" });
treeNavigator.Items.Add(new TreeMenuItem { Text = "Settings" });
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()

' Add items one by one
treeNavigator.Items.Add(New TreeMenuItem With {.Text = "Home"})
treeNavigator.Items.Add(New TreeMenuItem With {.Text = "Documents"})
treeNavigator.Items.Add(New TreeMenuItem With {.Text = "Settings"})
```

### Adding Multiple Items in a Loop

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();

string[] categories = { "Home", "Products", "Services", "About", "Contact" };

foreach (string category in categories)
{
    TreeMenuItem item = new TreeMenuItem { Text = category };
    treeNavigator.Items.Add(item);
}
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()

Dim categories As String() = {"Home", "Products", "Services", "About", "Contact"}

For Each category As String In categories
    Dim item As New TreeMenuItem With {.Text = category}
    treeNavigator.Items.Add(item)
Next
```

---

## Building Hierarchies with Child Items

### Adding Child Items

TreeMenuItem objects can contain their own Items collection, creating parent-child relationships.

**C# Example:**

```csharp
// Create parent item
TreeMenuItem documents = new TreeMenuItem { Text = "Documents" };

// Create and add child items
TreeMenuItem workFiles = new TreeMenuItem { Text = "Work Files" };
TreeMenuItem personalFiles = new TreeMenuItem { Text = "Personal Files" };
TreeMenuItem projects = new TreeMenuItem { Text = "Projects" };

// Add parent to TreeNavigator
treeNavigator.Items.Add(documents);

documents.Items.Add(workFiles);
documents.Items.Add(personalFiles);
documents.Items.Add(projects);
```

**VB.NET Example:**

```vbnet
' Create parent item
Dim documents As New TreeMenuItem With {.Text = "Documents"}

' Create and add child items
Dim workFiles As New TreeMenuItem With {.Text = "Work Files"}
Dim personalFiles As New TreeMenuItem With {.Text = "Personal Files"}
Dim projects As New TreeMenuItem With {.Text = "Projects"}

' Add parent to TreeNavigator
treeNavigator.Items.Add(documents)

documents.Items.Add(workFiles)
documents.Items.Add(personalFiles)
documents.Items.Add(projects)
```

### Multi-Level Hierarchies

**C# Example:**

```csharp
// Build 3-level hierarchy
TreeMenuItem root = new TreeMenuItem { Text = "Computer" };

// Level 1
TreeMenuItem cDrive = new TreeMenuItem { Text = "C: Drive" };
TreeMenuItem dDrive = new TreeMenuItem { Text = "D: Drive" };

// Level 2
TreeMenuItem programFiles = new TreeMenuItem { Text = "Program Files" };
TreeMenuItem users = new TreeMenuItem { Text = "Users" };

// Level 3
TreeMenuItem currentUser = new TreeMenuItem { Text = "CurrentUser" };
TreeMenuItem publicUser = new TreeMenuItem { Text = "Public" };

// Add to TreeNavigator
treeNavigator.Items.Add(root);

// Build hierarchy
root.Items.Add(cDrive);
root.Items.Add(dDrive);

cDrive.Items.Add(programFiles);
cDrive.Items.Add(users);

users.Items.Add(currentUser);
users.Items.Add(publicUser);
```

**VB.NET Example:**

```vbnet
' Build 3-level hierarchy
Dim root As New TreeMenuItem With {.Text = "Computer"}

' Level 1
Dim cDrive As New TreeMenuItem With {.Text = "C: Drive"}
Dim dDrive As New TreeMenuItem With {.Text = "D: Drive"}

' Level 2
Dim programFiles As New TreeMenuItem With {.Text = "Program Files"}
Dim users As New TreeMenuItem With {.Text = "Users"}

' Level 3
Dim currentUser As New TreeMenuItem With {.Text = "CurrentUser"}
Dim publicUser As New TreeMenuItem With {.Text = "Public"}

' Add to TreeNavigator
treeNavigator.Items.Add(root)

' Build hierarchy
root.Items.Add(cDrive)
root.Items.Add(dDrive)

cDrive.Items.Add(programFiles)
cDrive.Items.Add(users)

users.Items.Add(currentUser)
users.Items.Add(publicUser)
```

---

## Adding Items Through Designer

You can build TreeMenuItem hierarchies visually in Visual Studio.

### Using Smart Tag Collection Editor

**Steps:**

1. **Select TreeNavigator**: Click the TreeNavigator control on the form

2. **Open Smart Tag**: Click the smart tag arrow (▼) in the upper-right corner

3. **Click Items Property**: In the smart tag menu, select "Items"

4. **Add Root Items**:
   - Click "Add" button in the Collection Editor
   - Set "Text" property in the right panel
   - Repeat for each root item

5. **Add Child Items**:
   - Select a parent TreeMenuItem in the left panel
   - Find "Items" property in the right panel
   - Click the ellipsis (...) button next to Items
   - New Collection Editor opens for child items
   - Add child items using "Add" button

6. **Apply Changes**: Click "OK" to close all editors

### Designer Tips

- **Visual Preview**: Changes appear immediately in the designer
- **Property Grid**: Use Properties window for fine-tuning individual items
- **Copy/Paste**: You can copy items within the Collection Editor
- **Reorder Items**: Use up/down arrows to change item order

---

## Customizing TreeMenuItem Appearance

### Setting Colors for Individual Items

**C# Example:**

```csharp
TreeMenuItem important = new TreeMenuItem 
{ 
    Text = "Critical Files",
    ItemBackColor = Color.FromArgb(255, 244, 204), // Light yellow
    ItemHoverColor = Color.FromArgb(255, 235, 156), // Darker yellow
    SelectedColor = Color.FromArgb(255, 193, 7), // Amber
    SelectedItemForeColor = Color.Black // Black text when selected
};

treeNavigator.Items.Add(important);
```

**VB.NET Example:**

```vbnet
Dim important As New TreeMenuItem With
{
    .Text = "Critical Files",
    .ItemBackColor = Color.FromArgb(255, 244, 204), ' Light yellow
    .ItemHoverColor = Color.FromArgb(255, 235, 156), ' Darker yellow
    .SelectedColor = Color.FromArgb(255, 193, 7), ' Amber
    .SelectedItemForeColor = Color.Black ' Black text when selected
}

treeNavigator.Items.Add(important)
```

### Creating Color-Coded Items

**C# Example:**

```csharp
// Success item (green)
TreeMenuItem success = new TreeMenuItem 
{ 
    Text = "Completed Tasks",
    ItemBackColor = Color.FromArgb(212, 237, 218),
    ItemHoverColor = Color.FromArgb(195, 230, 203),
    SelectedColor = Color.FromArgb(40, 167, 69),
    SelectedItemForeColor = Color.White
};

// Warning item (yellow/orange)
TreeMenuItem warning = new TreeMenuItem 
{ 
    Text = "Pending Review",
    ItemBackColor = Color.FromArgb(255, 243, 205),
    ItemHoverColor = Color.FromArgb(255, 236, 179),
    SelectedColor = Color.FromArgb(255, 193, 7),
    SelectedItemForeColor = Color.Black
};

// Error item (red)
TreeMenuItem error = new TreeMenuItem 
{ 
    Text = "Failed Operations",
    ItemBackColor = Color.FromArgb(248, 215, 218),
    ItemHoverColor = Color.FromArgb(245, 198, 203),
    SelectedColor = Color.FromArgb(220, 53, 69),
    SelectedItemForeColor = Color.White
};

treeNavigator.Items.Add(success);
treeNavigator.Items.Add(warning);
treeNavigator.Items.Add(error);
```

---

## Dynamic Item Manipulation

### Adding Items at Runtime

**C# Example:**

```csharp
private void AddNewCategory(string categoryName)
{
    TreeMenuItem newItem = new TreeMenuItem { Text = categoryName };
    treeNavigator.Items.Add(newItem);
}

// Usage
private void btnAddCategory_Click(object sender, EventArgs e)
{
    string name = txtCategoryName.Text;
    if (!string.IsNullOrEmpty(name))
    {
        AddNewCategory(name);
        txtCategoryName.Clear();
    }
}
```

**VB.NET Example:**

```vbnet
Private Sub AddNewCategory(categoryName As String)
    Dim newItem As New TreeMenuItem With {.Text = categoryName}
    treeNavigator.Items.Add(newItem)
End Sub

' Usage
Private Sub btnAddCategory_Click(sender As Object, e As EventArgs)
    Dim name As String = txtCategoryName.Text
    If Not String.IsNullOrEmpty(name) Then
        AddNewCategory(name)
        txtCategoryName.Clear()
    End If
End Sub
```

### Removing Items

**C# Example:**

```csharp
// Remove specific item
TreeMenuItem itemToRemove = treeNavigator.Items[0];
treeNavigator.Items.Remove(itemToRemove);

// Remove at index
treeNavigator.Items.RemoveAt(2);

// Remove all items
treeNavigator.Items.Clear();
```

**VB.NET Example:**

```vbnet
' Remove specific item
Dim itemToRemove As TreeMenuItem = treeNavigator.Items(0)
treeNavigator.Items.Remove(itemToRemove)

' Remove at index
treeNavigator.Items.RemoveAt(2)

' Remove all items
treeNavigator.Items.Clear()
```

### Finding Items

**C# Example:**

```csharp
// Find item by text
TreeMenuItem FindItemByText(string text)
{
    foreach (TreeMenuItem item in treeNavigator.Items)
    {
        if (item.Text == text)
            return item;
    }
    return null;
}

// Usage
TreeMenuItem docs = FindItemByText("Documents");
if (docs != null)
{
    docs.SelectedColor = Color.Blue;
}
```

**VB.NET Example:**

```vbnet
' Find item by text
Function FindItemByText(text As String) As TreeMenuItem
    For Each item As TreeMenuItem In treeNavigator.Items
        If item.Text = text Then
            Return item
        End If
    Next
    Return Nothing
End Function

' Usage
Dim docs As TreeMenuItem = FindItemByText("Documents")
If docs IsNot Nothing Then
    docs.SelectedColor = Color.Blue
End If
```

### Updating Items

**C# Example:**

```csharp
// Update item text
treeNavigator.Items[0].Text = "Updated Name";

// Update item colors
TreeMenuItem item = treeNavigator.Items[1];
item.ItemBackColor = Color.LightGreen;
item.ItemHoverColor = Color.Green;
```

**VB.NET Example:**

```vbnet
' Update item text
treeNavigator.Items(0).Text = "Updated Name"

' Update item colors
Dim item As TreeMenuItem = treeNavigator.Items(1)
item.ItemBackColor = Color.LightGreen
item.ItemHoverColor = Color.Green
```

---

## Complete Examples

### Example 1: File System Browser

```csharp
private void BuildFileSystemNavigation()
{
    TreeNavigator fileNav = new TreeNavigator();
    fileNav.Header.HeaderText = "File System";
    fileNav.Size = new Size(280, 500);
    
    // C: Drive
    TreeMenuItem cDrive = new TreeMenuItem { Text = "Local Disk (C:)" };
    fileNav.Items.Add(cDrive);
    cDrive.Items.Add(new TreeMenuItem { Text = "Program Files" });
    cDrive.Items.Add(new TreeMenuItem { Text = "Program Files (x86)" });
    cDrive.Items.Add(new TreeMenuItem { Text = "Windows" });
    cDrive.Items.Add(new TreeMenuItem { Text = "Users" });
    
    // D: Drive
    TreeMenuItem dDrive = new TreeMenuItem { Text = "Data (D:)" };
    fileNav.Items.Add(dDrive);
    dDrive.Items.Add(new TreeMenuItem { Text = "Documents" });
    dDrive.Items.Add(new TreeMenuItem { Text = "Projects" });
    dDrive.Items.Add(new TreeMenuItem { Text = "Backups" });
    
    // Network
    TreeMenuItem network = new TreeMenuItem { Text = "Network" };
    fileNav.Items.Add(network);
    network.Items.Add(new TreeMenuItem { Text = "Server01" });
    network.Items.Add(new TreeMenuItem { Text = "NAS Storage" });
    
    this.Controls.Add(fileNav);
}
```

### Example 2: Settings Categories

```csharp
private void BuildSettingsNavigation()
{
    TreeNavigator settingsNav = new TreeNavigator();
    settingsNav.Header.HeaderText = "Settings";
    settingsNav.Style = TreeNavigatorStyle.Office2016White;
    settingsNav.Size = new Size(300, 450);
    
    // System settings
    TreeMenuItem system = new TreeMenuItem { Text = "System" };
    settingsNav.Items.Add(system);
    system.Items.Add(new TreeMenuItem { Text = "Display" });
    system.Items.Add(new TreeMenuItem { Text = "Sound" });
    system.Items.Add(new TreeMenuItem { Text = "Notifications" });
    system.Items.Add(new TreeMenuItem { Text = "Power & Battery" });
    
    // Personalization
    TreeMenuItem personalization = new TreeMenuItem { Text = "Personalization" };
    settingsNav.Items.Add(personalization);
    personalization.Items.Add(new TreeMenuItem { Text = "Background" });
    personalization.Items.Add(new TreeMenuItem { Text = "Colors" });
    personalization.Items.Add(new TreeMenuItem { Text = "Themes" });
    personalization.Items.Add(new TreeMenuItem { Text = "Lock Screen" });
    
    // Apps
    TreeMenuItem apps = new TreeMenuItem { Text = "Apps" };
    settingsNav.Items.Add(apps);
    apps.Items.Add(new TreeMenuItem { Text = "Installed Apps" });
    apps.Items.Add(new TreeMenuItem { Text = "Default Apps" });
    apps.Items.Add(new TreeMenuItem { Text = "Startup" });
    
    // Accounts
    TreeMenuItem accounts = new TreeMenuItem { Text = "Accounts" };
    settingsNav.Items.Add(accounts);
    accounts.Items.Add(new TreeMenuItem { Text = "Your Info" });
    accounts.Items.Add(new TreeMenuItem { Text = "Email & Accounts" });
    accounts.Items.Add(new TreeMenuItem { Text = "Sign-in Options" });
    
    this.Controls.Add(settingsNav);
}
```

### Example 3: Dynamic Category Builder

```csharp
public class CategoryBuilder
{
    private TreeNavigator navigator;
    
    public CategoryBuilder(TreeNavigator nav)
    {
        navigator = nav;
    }
    
    public void BuildFromDataSource(List<Category> categories)
    {
        navigator.Items.Clear();
        
        foreach (var category in categories)
        {
            TreeMenuItem parent = new TreeMenuItem 
            { 
                Text = category.Name,
                ItemBackColor = category.BackgroundColor
            };
            
            foreach (var subcategory in category.Subcategories)
            {
                TreeMenuItem child = new TreeMenuItem 
                { 
                    Text = subcategory.Name
                };
                
                foreach (var item in subcategory.Items)
                {
                    child.Items.Add(new TreeMenuItem { Text = item });
                }
                
                parent.Items.Add(child);
            }
            
            navigator.Items.Add(parent);
        }
    }
}

// Usage
public class Category
{
    public string Name { get; set; }
    public Color BackgroundColor { get; set; }
    public List<Subcategory> Subcategories { get; set; }
}

public class Subcategory
{
    public string Name { get; set; }
    public List<string> Items { get; set; }
}
```

---

## Best Practices

### Hierarchy Design

1. **Limit Depth**: Keep hierarchies to 3-4 levels maximum for usability
2. **Logical Grouping**: Group related items under common parents
3. **Consistent Naming**: Use clear, descriptive names for all items
4. **Avoid Deep Nesting**: Deep hierarchies confuse users

### Performance

1. **Lazy Loading**: For large hierarchies, add child items only when parent is clicked
2. **Batch Operations**: Add multiple items before refreshing UI
3. **Clear Unused Items**: Remove items no longer needed to reduce memory

### User Experience

1. **Meaningful Labels**: Use descriptive text that clearly indicates content
2. **Visual Cues**: Use colors to indicate item states or types
3. **Feedback**: Provide visual feedback for hover and selection states
4. **Consistency**: Maintain consistent item appearance throughout

---

## Troubleshooting

### Items Not Appearing

**Problem:** Added TreeMenuItem items don't show in TreeNavigator.

**Solution:**
1. Verify items added to correct collection: `treeNavigator.Items.Add(item)`
2. Check if TreeNavigator has adequate size
3. Ensure items have Text property set
4. Verify control is added to form and visible

### Child Items Not Accessible

**Problem:** Clicking parent item doesn't show child items.

**Solution:**
1. Verify child items added to parent's Items collection: `parent.Items.Add(child)`
2. Check that parent item has children: `parent.Items.Count > 0`
3. Ensure NavigationMode is set correctly

### Items Appearing in Wrong Order

**Problem:** Items display in unexpected order.

**Solution:**
1. Items display in the order they're added
2. Use `Items.Insert(index, item)` for specific positioning
3. Clear and re-add items in desired order
4. Use designer Collection Editor for visual reordering

---

## Next Steps

- **Selection Events**: Handle user navigation and item selection
- **Navigation Modes**: Configure Default or Extended navigation
- **Appearance**: Customize visual styles and colors
