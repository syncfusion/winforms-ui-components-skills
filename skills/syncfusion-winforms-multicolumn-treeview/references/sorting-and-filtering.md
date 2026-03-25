# Sorting and Filtering

This guide covers sorting node collections and filtering tree data dynamically based on custom criteria.

## Sorting

Sort tree nodes in ascending or descending order based on different criteria.

### Basic Sorting

Sort nodes using the `Sort()` method:

```csharp
// Sort in ascending order
multiColumnTreeView1.Nodes.Sort(SortOrder.Ascending);

// Sort in descending order
multiColumnTreeView1.Nodes.Sort(SortOrder.Descending);

// No sorting
multiColumnTreeView1.Nodes.Sort(SortOrder.None);
```

**Note:** By default, `Sort()` only sorts level 1 (root) nodes.

### Sorting with Child Nodes

To sort child nodes as well, set `SortWithChildNode` before calling `Sort()`:

```csharp
// Sort all levels recursively
foreach (TreeNodeAdv node in multiColumnTreeView1.Nodes)
{
    node.SortOrder = SortOrder.Ascending;
}

multiColumnTreeView1.Nodes.Sort(SortOrder.Ascending);
```

### Sort Type

Specify what to sort by using the `SortType` property:

```csharp
// Sort by text (default)
multiColumnTreeView1.Nodes[0].SortType = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvSortType.Text;

// Sort by checkbox state
multiColumnTreeView1.Nodes[0].SortType = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvSortType.CheckBox;

// Sort by tag value
multiColumnTreeView1.Nodes[0].SortType = 
    Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvSortType.Tag;
```

### Sort Order per Node

Set sorting independently for each node:

```csharp
// Different sort orders for different branches
multiColumnTreeView1.Nodes[0].SortOrder = SortOrder.Ascending;
multiColumnTreeView1.Nodes[1].SortOrder = SortOrder.Descending;
```

### Compare Options

Customize text comparison behavior:

```csharp
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];

// Ignore case when sorting
node.CompareOptions = System.Globalization.CompareOptions.IgnoreCase;

// Other options:
// IgnoreKanaType, IgnoreNonSpace, IgnoreSymbols, IgnoreWidth
// Ordinal, OrdinalIgnoreCase, StringSort
```

### Custom Comparer

Implement custom sorting logic using `IComparer`:

```csharp
public class CustomNodeComparer : IComparer
{
    public int Compare(object x, object y)
    {
        TreeNodeAdv node1 = x as TreeNodeAdv;
        TreeNodeAdv node2 = y as TreeNodeAdv;
        
        if (node1 == null || node2 == null)
            return 0;
        
        // Custom comparison logic
        // For example, sort by subitem value
        if (node1.SubItems.Count > 0 && node2.SubItems.Count > 0)
        {
            int val1 = int.Parse(node1.SubItems[0].Text);
            int val2 = int.Parse(node2.SubItems[0].Text);
            return val1.CompareTo(val2);
        }
        
        return string.Compare(node1.Text, node2.Text);
    }
}

// Usage
TreeNodeAdv parentNode = multiColumnTreeView1.Nodes[0];
parentNode.Comparer = new CustomNodeComparer();
parentNode.Nodes.Sort(SortOrder.Ascending);
```

## Filtering

Filter nodes dynamically based on custom criteria, showing only nodes that match the filter.

### Filter Levels

Set the scope of filtering:

```csharp
// Filter only root nodes
multiColumnTreeView1.FilterLevel = FilterLevel.Root;

// Filter all nodes (parent must match for children to be checked)
multiColumnTreeView1.FilterLevel = FilterLevel.All;

// Extended filter (show parent if child matches, even if parent doesn't)
multiColumnTreeView1.FilterLevel = FilterLevel.Extended;
```

**FilterLevel Options:**
- **Root** - Only root nodes are filtered, all children visible if parent matches
- **All** - All nodes filtered; if parent doesn't match, children not evaluated
- **Extended** - All nodes filtered; if child matches, parent shown even if it doesn't match

### Creating a Filter

Define a filter delegate that returns `true` for nodes to show:

```csharp
// Define filter method
public bool FilterNodes(object o)
{
    TreeNodeAdv node = o as TreeNodeAdv;
    
    if (node == null)
        return false;
    
    // Example: Filter by text containing search term
    string searchTerm = "John";
    return node.Text.Contains(searchTerm, StringComparison.OrdinalIgnoreCase);
}

// Apply filter
multiColumnTreeView1.Filter = FilterNodes;
multiColumnTreeView1.RefreshFilter();
```

### Applying Filters

```csharp
// Set filter delegate
multiColumnTreeView1.Filter = FilterNodes;

// Apply the filter
multiColumnTreeView1.RefreshFilter();
```

### Clearing Filters

```csharp
// Remove filter
multiColumnTreeView1.Filter = null;
multiColumnTreeView1.RefreshFilter();
```

## Practical Examples

### Example 1: Sort by SubItem Value

```csharp
void SortBySalary()
{
    // Assuming salary is in SubItems[1]
    TreeNodeAdv deptNode = multiColumnTreeView1.Nodes[0];
    
    // Create custom comparer for salary
    deptNode.Comparer = new SalaryComparer();
    deptNode.Nodes.Sort(SortOrder.Descending); // Highest salary first
}

public class SalaryComparer : IComparer
{
    public int Compare(object x, object y)
    {
        TreeNodeAdv node1 = x as TreeNodeAdv;
        TreeNodeAdv node2 = y as TreeNodeAdv;
        
        if (node1?.SubItems.Count > 1 && node2?.SubItems.Count > 1)
        {
            // Parse salary (remove $ and commas)
            string sal1Str = node1.SubItems[1].Text.Replace("$", "").Replace(",", "");
            string sal2Str = node2.SubItems[1].Text.Replace("$", "").Replace(",", "");
            
            if (decimal.TryParse(sal1Str, out decimal sal1) && 
                decimal.TryParse(sal2Str, out decimal sal2))
            {
                return sal1.CompareTo(sal2);
            }
        }
        
        return 0;
    }
}
```

### Example 2: Search Filter with TextBox

```csharp
private TextBox searchTextBox;
private ComboBox filterLevelComboBox;

void SetupSearch()
{
    // Create search textbox
    searchTextBox = new TextBox();
    searchTextBox.TextChanged += SearchTextBox_TextChanged;
    
    // Create filter level selector
    filterLevelComboBox = new ComboBox();
    filterLevelComboBox.Items.AddRange(new object[] { "Root", "All", "Extended" });
    filterLevelComboBox.SelectedIndex = 2; // Extended
    filterLevelComboBox.SelectedIndexChanged += (s, e) =>
    {
        multiColumnTreeView1.FilterLevel = (FilterLevel)filterLevelComboBox.SelectedIndex;
        ApplySearchFilter();
    };
}

void SearchTextBox_TextChanged(object sender, EventArgs e)
{
    ApplySearchFilter();
}

void ApplySearchFilter()
{
    string searchText = searchTextBox.Text.Trim();
    
    if (string.IsNullOrEmpty(searchText))
    {
        // Clear filter
        multiColumnTreeView1.Filter = null;
    }
    else
    {
        // Apply search filter
        multiColumnTreeView1.Filter = node => SearchFilter(node, searchText);
    }
    
    multiColumnTreeView1.RefreshFilter();
}

bool SearchFilter(object o, string searchText)
{
    TreeNodeAdv node = o as TreeNodeAdv;
    
    if (node == null)
        return false;
    
    // Search in node text
    if (node.Text.Contains(searchText, StringComparison.OrdinalIgnoreCase))
        return true;
    
    // Search in subitems
    foreach (TreeNodeAdvSubItem subItem in node.SubItems)
    {
        if (subItem.Text.Contains(searchText, StringComparison.OrdinalIgnoreCase))
            return true;
    }
    
    return false;
}
```

### Example 3: Filter by Criteria

```csharp
void FilterByCriteria()
{
    // Create filter UI
    CheckBox showCompletedCheckBox = new CheckBox { Text = "Show Completed" };
    CheckBox showHighPriorityCheckBox = new CheckBox { Text = "High Priority Only" };
    NumericUpDown minSalaryUpDown = new NumericUpDown { Minimum = 0, Maximum = 200000, Value = 50000 };
    
    EventHandler applyFilter = (s, e) =>
    {
        multiColumnTreeView1.Filter = node => CriteriaFilter(
            node,
            showCompletedCheckBox.Checked,
            showHighPriorityCheckBox.Checked,
            (decimal)minSalaryUpDown.Value);
        
        multiColumnTreeView1.FilterLevel = FilterLevel.All;
        multiColumnTreeView1.RefreshFilter();
    };
    
    showCompletedCheckBox.CheckedChanged += applyFilter;
    showHighPriorityCheckBox.CheckedChanged += applyFilter;
    minSalaryUpDown.ValueChanged += applyFilter;
}

bool CriteriaFilter(object o, bool showCompleted, bool highPriorityOnly, decimal minSalary)
{
    TreeNodeAdv node = o as TreeNodeAdv;
    
    if (node == null || node.SubItems.Count < 2)
        return false;
    
    // Check completed status (assuming checkbox)
    if (!showCompleted && node.Checked)
        return false;
    
    // Check salary threshold (assuming SubItems[0] is salary)
    if (node.SubItems.Count > 0)
    {
        string salaryText = node.SubItems[0].Text.Replace("$", "").Replace(",", "");
        if (decimal.TryParse(salaryText, out decimal salary))
        {
            if (salary < minSalary)
                return false;
        }
    }
    
    // Check priority (assuming SubItems[1] contains priority)
    if (highPriorityOnly && node.SubItems.Count > 1)
    {
        string priority = node.SubItems[1].Text;
        if (!priority.Equals("High", StringComparison.OrdinalIgnoreCase))
            return false;
    }
    
    return true;
}
```

### Example 4: Dynamic Sort UI

```csharp
void SetupSortUI()
{
    ToolStrip toolStrip = new ToolStrip();
    
    ToolStripButton sortAscButton = new ToolStripButton("Sort A-Z");
    sortAscButton.Click += (s, e) =>
    {
        SortAllLevels(SortOrder.Ascending);
    };
    
    ToolStripButton sortDescButton = new ToolStripButton("Sort Z-A");
    sortDescButton.Click += (s, e) =>
    {
        SortAllLevels(SortOrder.Descending);
    };
    
    ToolStripComboBox sortTypeCombo = new ToolStripComboBox();
    sortTypeCombo.Items.AddRange(new object[] { "By Text", "By Checkbox", "By Tag" });
    sortTypeCombo.SelectedIndex = 0;
    sortTypeCombo.SelectedIndexChanged += (s, e) =>
    {
        SetSortType((TreeNodeAdvSortType)sortTypeCombo.SelectedIndex);
    };
    
    toolStrip.Items.AddRange(new ToolStripItem[] 
    { 
        sortAscButton, 
        sortDescButton, 
        new ToolStripSeparator(), 
        new ToolStripLabel("Sort By:"), 
        sortTypeCombo 
    });
}

void SortAllLevels(SortOrder order)
{
    multiColumnTreeView1.BeginUpdate();
    try
    {
        SortNodesRecursive(multiColumnTreeView1.Nodes, order);
    }
    finally
    {
        multiColumnTreeView1.EndUpdate();
    }
}

void SortNodesRecursive(TreeNodeAdvCollection nodes, SortOrder order)
{
    nodes.Sort(order);
    
    foreach (TreeNodeAdv node in nodes)
    {
        if (node.Nodes.Count > 0)
        {
            SortNodesRecursive(node.Nodes, order);
        }
    }
}

void SetSortType(TreeNodeAdvSortType sortType)
{
    SetSortTypeRecursive(multiColumnTreeView1.Nodes, sortType);
}

void SetSortTypeRecursive(TreeNodeAdvCollection nodes, TreeNodeAdvSortType sortType)
{
    foreach (TreeNodeAdv node in nodes)
    {
        node.SortType = sortType;
        
        if (node.Nodes.Count > 0)
        {
            SetSortTypeRecursive(node.Nodes, sortType);
        }
    }
}
```

### Example 5: Filter Result Count

```csharp
void ShowFilterResults()
{
    multiColumnTreeView1.Filter = MyFilterMethod;
    multiColumnTreeView1.RefreshFilter();
    
    int visibleCount = CountVisibleNodes(multiColumnTreeView1.Nodes);
    int totalCount = CountAllNodes(multiColumnTreeView1.Nodes);
    
    statusLabel.Text = $"Showing {visibleCount} of {totalCount} nodes";
}

int CountVisibleNodes(TreeNodeAdvCollection nodes)
{
    int count = 0;
    
    foreach (TreeNodeAdv node in nodes)
    {
        if (node.Visible)
        {
            count++;
            count += CountVisibleNodes(node.Nodes);
        }
    }
    
    return count;
}

int CountAllNodes(TreeNodeAdvCollection nodes)
{
    int count = nodes.Count;
    
    foreach (TreeNodeAdv node in nodes)
    {
        count += CountAllNodes(node.Nodes);
    }
    
    return count;
}
```

## Best Practices

1. **Use BeginUpdate/EndUpdate** when sorting to prevent flickering
2. **Choose appropriate FilterLevel** based on your tree structure
3. **Cache filter criteria** to avoid recreating filters unnecessarily
4. **Provide visual feedback** when filters are active (e.g., status bar message)
5. **Test with large datasets** to ensure sort/filter performance
6. **Clear filters when not needed** to show all data
7. **Use CompareOptions** for culture-sensitive text sorting
8. **Implement custom comparers** for complex sorting logic

## Common Issues

**Sorting not recursive:**
- Ensure `SortOrder` is set on child nodes
- Use recursive sorting method for all levels

**Filter not applying:**
- Verify `RefreshFilter()` is called after setting the filter
- Check if filter delegate is returning correct boolean values

**Filter showing no results:**
- Test filter logic independently to verify it works
- Check FilterLevel setting - try Extended if nodes not appearing

**Performance issues:**
- Use BeginUpdate/EndUpdate
- Avoid complex logic in filter delegates
- Consider caching filter results for repeated evaluations
