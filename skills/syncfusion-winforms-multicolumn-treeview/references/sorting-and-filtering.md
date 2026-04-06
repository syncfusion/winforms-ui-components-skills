# Sorting and Filtering

Sort node collections and filter tree data dynamically based on custom criteria.

## Basic Sorting

```csharp
// Sort nodes
multiColumnTreeView1.Nodes.Sort(SortOrder.Ascending);  // or Descending, None

// Sort all levels recursively
foreach (TreeNodeAdv node in multiColumnTreeView1.Nodes)
    node.SortOrder = SortOrder.Ascending;
multiColumnTreeView1.Nodes.Sort(SortOrder.Ascending);

// Sort by different criteria
multiColumnTreeView1.Nodes[0].SortType = TreeNodeAdvSortType.Text;      // or CheckBox, Tag
multiColumnTreeView1.Nodes[0].CompareOptions = CompareOptions.IgnoreCase;
```

## Custom Comparer

```csharp
public class CustomNodeComparer : IComparer
{
    public int Compare(object x, object y)
    {
        TreeNodeAdv node1 = x as TreeNodeAdv;
        TreeNodeAdv node2 = y as TreeNodeAdv;
        
        if (node1?.SubItems.Count > 0 && node2?.SubItems.Count > 0)
        {
            int val1 = int.Parse(node1.SubItems[0].Text);
            int val2 = int.Parse(node2.SubItems[0].Text);
            return val1.CompareTo(val2);
        }
        return string.Compare(node1.Text, node2.Text);
    }
}

// Usage
parentNode.Comparer = new CustomNodeComparer();
parentNode.Nodes.Sort(SortOrder.Ascending);
```

## Filtering

```csharp
// Set filter scope
multiColumnTreeView1.FilterLevel = FilterLevel.Root;      // Only root nodes
multiColumnTreeView1.FilterLevel = FilterLevel.All;       // All nodes (parent must match)
multiColumnTreeView1.FilterLevel = FilterLevel.Extended;  // Show parent if child matches

// Apply filter
public bool FilterNodes(object o)
{
    TreeNodeAdv node = o as TreeNodeAdv;
    if (node == null) return false;
    
    string searchTerm = "John";
    return node.Text.Contains(searchTerm, StringComparison.OrdinalIgnoreCase);
}

multiColumnTreeView1.Filter = FilterNodes;
multiColumnTreeView1.RefreshFilter();

// Clear filter
multiColumnTreeView1.Filter = null;
multiColumnTreeView1.RefreshFilter();
```

## Search Filter with TextBox

```csharp
private TextBox searchTextBox;

void SetupSearch()
{
    searchTextBox = new TextBox();
    searchTextBox.TextChanged += (s, e) => ApplySearchFilter();
}

void ApplySearchFilter()
{
    string searchText = searchTextBox.Text.Trim();
    multiColumnTreeView1.Filter = string.IsNullOrEmpty(searchText) ? null : node => SearchFilter(node, searchText);
    multiColumnTreeView1.RefreshFilter();
}

bool SearchFilter(object o, string searchText)
{
    TreeNodeAdv node = o as TreeNodeAdv;
    if (node == null) return false;
    
    if (node.Text.Contains(searchText, StringComparison.OrdinalIgnoreCase))
        return true;
    
    foreach (TreeNodeAdvSubItem subItem in node.SubItems)
        if (subItem.Text.Contains(searchText, StringComparison.OrdinalIgnoreCase))
            return true;
    
    return false;
}
```

## Sort All Levels Recursively

```csharp
void SortAllLevels(SortOrder order)
{
    multiColumnTreeView1.BeginUpdate();
    try
    {
        SortNodesRecursive(multiColumnTreeView1.Nodes, order);
    }
    finally { multiColumnTreeView1.EndUpdate(); }
}

void SortNodesRecursive(TreeNodeAdvCollection nodes, SortOrder order)
{
    nodes.Sort(order);
    foreach (TreeNodeAdv node in nodes)
        if (node.Nodes.Count > 0)
            SortNodesRecursive(node.Nodes, order);
}
```

## Multi-Criteria Filter

```csharp
bool CriteriaFilter(object o, bool showCompleted, decimal minSalary)
{
    TreeNodeAdv node = o as TreeNodeAdv;
    if (node == null || node.SubItems.Count < 1) return false;
    
    // Check completed status
    if (!showCompleted && node.Checked) return false;
    
    // Check salary threshold
    string salaryText = node.SubItems[0].Text.Replace("$", "").Replace(",", "");
    if (decimal.TryParse(salaryText, out decimal salary) && salary < minSalary)
        return false;
    
    return true;
}
```

## Best Practices

- Use `BeginUpdate/EndUpdate` when sorting to prevent flickering
- Choose appropriate `FilterLevel` based on tree structure
- Cache filter criteria to avoid recreating filters unnecessarily
- Provide visual feedback when filters are active
- Test with large datasets for performance
