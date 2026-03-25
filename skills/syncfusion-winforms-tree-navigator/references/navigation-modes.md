# Navigation Modes

The TreeNavigator provides two distinct navigation modes that control how users navigate through hierarchical items and return to parent levels.

## NavigationMode Property

The `NavigationMode` property determines the navigation behavior and back button style.

**Available Modes:**
- **Default** - Single back button with selected item at top
- **Extended** - Stacked breadcrumb headers showing full navigation path

**Property:**
```csharp
treeNavigator.NavigationMode = NavigationMode.Default;  // or NavigationMode.Extended
```

---

## Default Mode

In Default mode, the TreeNavigator displays a simple back button with the currently selected item at the top.

### Behavior

- **Selected Item Display**: When you click an item with children, it moves to the top of the TreeNavigator
- **Back Button**: A single back button appears next to the selected item
- **Navigation**: Click the back button to return to the root level (parent items)
- **Hierarchy View**: Only shows the current level - either root items or children of selected item
- **Space Efficient**: Minimal header space consumed

### Visual Layout

```
┌─────────────────────────┐
│ ← Selected Item         │  ← Back button + current item
├─────────────────────────┤
│ Child Item 1            │
│ Child Item 2            │
│ Child Item 3            │
└─────────────────────────┘
```

### When to Use Default Mode

✅ **Use Default mode when:**
- Simple parent-child hierarchies (2-3 levels)
- Space is limited (minimal header consumption)
- Users primarily navigate forward, rarely jump back multiple levels
- Clean, minimal interface is preferred
- Mobile or touch interfaces where breadcrumbs might be too small

### Code Example

**C# Example:**

```csharp
using Syncfusion.Windows.Forms.Tools;

TreeNavigator treeNavigator = new TreeNavigator();
treeNavigator.Header.HeaderText = "File Browser";

// Set Default navigation mode
treeNavigator.NavigationMode = NavigationMode.Default;

// Add items
TreeMenuItem documents = new TreeMenuItem { Text = "Documents" };
treeNavigator.Items.Add(documents);
documents.Items.Add(new TreeMenuItem { Text = "Work Files" });
documents.Items.Add(new TreeMenuItem { Text = "Personal Files" });

treeNavigator.Items.Add(new TreeMenuItem { Text = "Downloads" });
treeNavigator.Items.Add(new TreeMenuItem { Text = "Pictures" });
```

**VB.NET Example:**

```vbnet
Imports Syncfusion.Windows.Forms.Tools

Dim treeNavigator As New TreeNavigator()
treeNavigator.Header.HeaderText = "File Browser"

' Set Default navigation mode
treeNavigator.NavigationMode = NavigationMode.Default

' Add items
Dim documents As New TreeMenuItem With {.Text = "Documents"}
treeNavigator.Items.Add(documents)
documents.Items.Add(New TreeMenuItem With {.Text = "Work Files"})
documents.Items.Add(New TreeMenuItem With {.Text = "Personal Files"})

treeNavigator.Items.Add(New TreeMenuItem With {.Text = "Downloads"})
treeNavigator.Items.Add(New TreeMenuItem With {.Text = "Pictures"})
```

---

## Extended Mode

In Extended mode, the TreeNavigator displays a breadcrumb-style navigation with stacked headers representing each level from root to current.

### Behavior

- **Stacked Headers**: Each navigation level is displayed as a header, stacked vertically from root to current
- **Click Any Level**: Users can click any header in the stack to jump directly to that level
- **Full Path Visibility**: Shows complete navigation path from root to current location
- **Multi-Level Navigation**: Easy to return to any ancestor level, not just parent
- **Context Awareness**: Users always know their position in the hierarchy

### Visual Layout

```
┌─────────────────────────┐
│ Root Level              │  ← Click to return to root
├─────────────────────────┤
│ Parent Item             │  ← Click to return to parent
├─────────────────────────┤
│ Current Item            │  ← Current location
├─────────────────────────┤
│ Child Item 1            │
│ Child Item 2            │
└─────────────────────────┘
```

### When to Use Extended Mode

✅ **Use Extended mode when:**
- Deep hierarchies (3+ levels)
- Users need to frequently jump back multiple levels
- Context awareness is important (showing full path)
- Users might get lost in deep navigation
- Desktop applications with adequate screen space
- Complex navigation structures (file systems, settings, product catalogs)

### Code Example

**C# Example:**

```csharp
using Syncfusion.Windows.Forms.Tools;

TreeNavigator treeNavigator = new TreeNavigator();
treeNavigator.Header.HeaderText = "Settings";

// Set Extended navigation mode (breadcrumb style)
treeNavigator.NavigationMode = NavigationMode.Extended;

// Build deep hierarchy
TreeMenuItem general = new TreeMenuItem { Text = "General" };
treeNavigator.Items.Add(general);
TreeMenuItem appearance = new TreeMenuItem { Text = "Appearance" };
TreeMenuItem themes = new TreeMenuItem { Text = "Themes" };
general.Items.Add(appearance);

// Add sub-items
appearance.Items.Add(themes);
themes.Items.Add(new TreeMenuItem { Text = "Light Theme" });
themes.Items.Add(new TreeMenuItem { Text = "Dark Theme" });
themes.Items.Add(new TreeMenuItem { Text = "Custom Theme" });
treeNavigator.Items.Add(new TreeMenuItem { Text = "Advanced" });
```

**VB.NET Example:**

```vbnet
Imports Syncfusion.Windows.Forms.Tools

Dim treeNavigator As New TreeNavigator()
treeNavigator.Header.HeaderText = "Settings"

' Set Extended navigation mode (breadcrumb style)
treeNavigator.NavigationMode = NavigationMode.Extended

' Build deep hierarchy
Dim general As New TreeMenuItem With {.Text = "General"}
treeNavigator.Items.Add(general)
Dim appearance As New TreeMenuItem With {.Text = "Appearance"}
Dim themes As New TreeMenuItem With {.Text = "Themes"}

' Add sub-items
general.Items.Add(appearance)
appearance.Items.Add(themes)
themes.Items.Add(New TreeMenuItem With {.Text = "Light Theme"})
themes.Items.Add(New TreeMenuItem With {.Text = "Dark Theme"})
themes.Items.Add(New TreeMenuItem With {.Text = "Custom Theme"})

treeNavigator.Items.Add(New TreeMenuItem With {.Text = "Advanced"})
```

---

## Comparison: Default vs Extended

| Feature | Default Mode | Extended Mode |
|---------|-------------|---------------|
| **Back Button** | Single back button to root | Click any level in breadcrumb stack |
| **Header Space** | Minimal (1 header) | Variable (stacks with each level) |
| **Path Visibility** | Current item only | Full path from root to current |
| **Jump Navigation** | Back to root only | Jump to any ancestor level |
| **Best For** | Simple hierarchies (2-3 levels) | Deep hierarchies (3+ levels) |
| **Space Usage** | Compact | Requires more vertical space |
| **User Context** | Current level only | Complete navigation context |

---

## Switching Modes at Runtime

You can change navigation modes dynamically based on user preferences or application state.

**C# Example:**

```csharp
// Toggle between modes
private void ToggleNavigationMode(TreeNavigator navigator)
{
    if (navigator.NavigationMode == NavigationMode.Default)
    {
        navigator.NavigationMode = NavigationMode.Extended;
        MessageBox.Show("Switched to Extended mode (breadcrumb navigation)");
    }
    else
    {
        navigator.NavigationMode = NavigationMode.Default;
        MessageBox.Show("Switched to Default mode (simple back button)");
    }
}

// Example: Toolbar button click
private void btnToggleMode_Click(object sender, EventArgs e)
{
    ToggleNavigationMode(treeNavigator);
}
```

**VB.NET Example:**

```vbnet
' Toggle between modes
Private Sub ToggleNavigationMode(navigator As TreeNavigator)
    If navigator.NavigationMode = NavigationMode.Default Then
        navigator.NavigationMode = NavigationMode.Extended
        MessageBox.Show("Switched to Extended mode (breadcrumb navigation)")
    Else
        navigator.NavigationMode = NavigationMode.Default
        MessageBox.Show("Switched to Default mode (simple back button)")
    End If
End Sub

' Example: Toolbar button click
Private Sub btnToggleMode_Click(sender As Object, e As EventArgs)
    ToggleNavigationMode(treeNavigator)
End Sub
```

---

## Practical Examples

### Example 1: File Explorer with Default Mode

```csharp
// Simple file browser with back button
TreeNavigator fileExplorer = new TreeNavigator();
fileExplorer.Header.HeaderText = "This PC";
fileExplorer.NavigationMode = NavigationMode.Default;
fileExplorer.Style = TreeNavigatorStyle.Office2016White;

// Add drives
TreeMenuItem cDrive = new TreeMenuItem { Text = "Local Disk (C:)" };
TreeMenuItem dDrive = new TreeMenuItem { Text = "Data (D:)" };
fileExplorer.Items.Add(cDrive);
// Add folders to C: drive
cDrive.Items.Add(new TreeMenuItem { Text = "Program Files" });
cDrive.Items.Add(new TreeMenuItem { Text = "Windows" });
cDrive.Items.Add(new TreeMenuItem { Text = "Users" });

fileExplorer.Items.Add(dDrive);
fileExplorer.Items.Add(new TreeMenuItem { Text = "Network" });
```

### Example 2: Settings Panel with Extended Mode

```csharp
// Deep settings hierarchy with breadcrumb navigation
TreeNavigator settingsPanel = new TreeNavigator();
settingsPanel.Header.HeaderText = "Application Settings";
settingsPanel.NavigationMode = NavigationMode.Extended;
settingsPanel.Style = TreeNavigatorStyle.Office2016Colorful;

// Build deep hierarchy
TreeMenuItem system = new TreeMenuItem { Text = "System" };
TreeMenuItem display = new TreeMenuItem { Text = "Display" };
TreeMenuItem advanced = new TreeMenuItem { Text = "Advanced Settings" };
display.Items.Add(advanced);
// Add display options
advanced.Items.Add(new TreeMenuItem { Text = "Scaling Options" });
advanced.Items.Add(new TreeMenuItem { Text = "Color Calibration" });
advanced.Items.Add(new TreeMenuItem { Text = "Multiple Displays" });

display.Items.Add(new TreeMenuItem { Text = "Resolution" });
display.Items.Add(new TreeMenuItem { Text = "Orientation" });


system.Items.Add(display);
system.Items.Add(new TreeMenuItem { Text = "Sound" });
system.Items.Add(new TreeMenuItem { Text = "Notifications" });

settingsPanel.Items.Add(system);
settingsPanel.Items.Add(new TreeMenuItem { Text = "Personalization" });
settingsPanel.Items.Add(new TreeMenuItem { Text = "Privacy" });
```

### Example 3: User Preference Toggle

```csharp
// Allow users to choose their preferred navigation mode
public class NavigationSettings
{
    private TreeNavigator navigator;
    
    public NavigationSettings(TreeNavigator nav)
    {
        navigator = nav;
    }
    
    public void SetSimpleNavigation()
    {
        navigator.NavigationMode = NavigationMode.Default;
        // Save preference
        Properties.Settings.Default.NavMode = "Default";
        Properties.Settings.Default.Save();
    }
    
    public void SetBreadcrumbNavigation()
    {
        navigator.NavigationMode = NavigationMode.Extended;
        // Save preference
        Properties.Settings.Default.NavMode = "Extended";
        Properties.Settings.Default.Save();
    }
    
    public void LoadUserPreference()
    {
        string mode = Properties.Settings.Default.NavMode;
        navigator.NavigationMode = mode == "Extended" 
            ? NavigationMode.Extended 
            : NavigationMode.Default;
    }
}
```

---

## Best Practices

### Choose the Right Mode

1. **Analyze Hierarchy Depth**
   - 2-3 levels → Default mode is sufficient
   - 4+ levels → Extended mode provides better UX

2. **Consider Screen Space**
   - Limited vertical space → Default mode
   - Adequate space available → Extended mode shows more context

3. **User Navigation Patterns**
   - Linear navigation (forward mostly) → Default mode
   - Frequent jumping between levels → Extended mode

### User Experience Tips

1. **Provide Mode Toggle**: Let users choose their preferred navigation style
2. **Persist Preference**: Remember user's choice across sessions
3. **Responsive Design**: Switch to Default mode on smaller screens automatically
4. **Visual Feedback**: Ensure breadcrumb items in Extended mode have clear hover states

---

## Troubleshooting

### Back Button Not Appearing in Default Mode

**Problem:** The back button doesn't show when navigating to child items.

**Solution:**
1. Ensure the parent item has child items: `parentItem.Items.Count > 0`
2. Verify user clicked an item with children (not a leaf node)
3. Check that NavigationMode is set to Default

### Extended Mode Headers Overlapping Content

**Problem:** In Extended mode, stacked headers consume too much space, leaving little room for items.

**Solution:**
1. Increase TreeNavigator height: `treeNavigator.Size = new Size(width, largerHeight)`
2. Reduce header height: `treeNavigator.Header.Height = 35` (default is typically 40-45)
3. Consider switching to Default mode for space-constrained layouts

### Cannot Jump to Specific Level in Extended Mode

**Problem:** Clicking a breadcrumb header doesn't navigate to that level.

**Solution:**
1. Verify NavigationMode is set to Extended (not Default)
2. Ensure the header is part of the breadcrumb stack (not the main header)
3. Check that mouse events aren't being blocked by overlays or other controls

---

## Next Steps

- **Appearance and Styling**: Customize header colors and visual themes
- **Selection Events**: Handle navigation events to respond to user actions
- **TreeMenuItem Management**: Build and manipulate complex hierarchies
