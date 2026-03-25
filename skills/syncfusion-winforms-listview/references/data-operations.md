# Data Operations: Grouping, Sorting, and Filtering

## Table of Contents
- [Grouping](#grouping)
  - [Programmatic Grouping](#programmatic-grouping)
  - [Custom Grouping Logic](#custom-grouping-logic)
  - [Grouping by First Character](#grouping-by-first-character)
  - [Multiple Property Grouping](#multiple-property-grouping)
  - [Case-Insensitive Grouping](#case-insensitive-grouping)
  - [Expand and Collapse Groups](#expand-and-collapse-groups)
- [Sorting](#sorting)
  - [Programmatic Sorting](#programmatic-sorting)
  - [Custom Sorting Logic](#custom-sorting-logic)
  - [Sorting with Grouping](#sorting-with-grouping)
- [Filtering](#filtering)
  - [Programmatic Filtering](#programmatic-filtering)
  - [Multiple Criteria Filtering](#multiple-criteria-filtering)
  - [Accessing Filtered Data](#accessing-filtered-data)
  - [Clearing Filters](#clearing-filters)
  - [Sorting Filtered Items](#sorting-filtered-items)

---

## Grouping

Grouping organizes data into categories based on key values. Each group is identified by its `Key`, which you use to access the underlying data.

### Programmatic Grouping

Add a `GroupDescriptor` to the `View.GroupDescriptors` collection to group items by a property:

```csharp
sfListView1.View.GroupDescriptors.Add(new Syncfusion.DataSource.GroupDescriptor()
{
    PropertyName = "Continent",
});
```

**GroupDescriptor Properties:**
- `PropertyName`: Name of the property to group by
- `KeySelector`: Function returning the group key
- `Comparer`: Custom comparer for sorting groups

### Custom Grouping Logic

Implement custom grouping with `IComparer<GroupResult>` and `ISortDirection`:

```csharp
sfListView1.View.GroupDescriptors.Add(new Syncfusion.DataSource.GroupDescriptor()
{
    PropertyName = "Continent",
    Comparer = new CustomGroupComparer()
});

public class CustomGroupComparer : IComparer<GroupResult>, ISortDirection
{
    public CustomGroupComparer()
    {
        this.SortDirection = Syncfusion.DataSource.ListSortDirection.Ascending;
    }
    
    public ListSortDirection SortDirection { get; set; }
    
    public int Compare(GroupResult x, GroupResult y)
    {
        int groupX = x.Count;
        int groupY = y.Count;
       
        if (groupX.CompareTo(groupY) > 0)
            return SortDirection == Syncfusion.DataSource.ListSortDirection.Ascending ? 1 : -1;
        else if (groupX.CompareTo(groupY) == -1)
            return SortDirection == Syncfusion.DataSource.ListSortDirection.Ascending ? -1 : 1;
        else
            return 0;
    }
}
```

This example groups by item count in each group.

### Grouping by First Character

Use `KeySelector` to group by the first character of a property:

```csharp
sfListView1.View.GroupDescriptors.Add(new Syncfusion.DataSource.GroupDescriptor()
{
    PropertyName = "Continent",
    KeySelector = (object obj1) =>
    {
        var item = (obj1 as CountryInfo);
        return item.Continent[0].ToString();
    },
});
```

### Multiple Property Grouping

Combine multiple properties in the `KeySelector`:

```csharp
sfListView1.View.GroupDescriptors.Add(new GroupDescriptor()
{
    PropertyName = "Continent",    
    KeySelector = (object obj1) =>
    {
        var item = (obj1 as CountryInfo);
        return item.CountryName + ": " + item.Continent;
    }           
});
```

This creates group headers with data from multiple properties.

### Case-Insensitive Grouping

Normalize case in the `KeySelector` to ignore case sensitivity:

```csharp
sfListView1.View.GroupDescriptors.Add(new Syncfusion.DataSource.GroupDescriptor()
{
    PropertyName = "Continent",
    KeySelector = (object obj1) =>
    {
        return (obj1 as CountryInfo).Continent.ToUpper();
    },               
});
```

### Expand and Collapse Groups

**Default behavior:** Groups are expanded by default. Users can toggle by clicking the group header.

**Programmatic control:**

```csharp
// Expand/collapse a specific group
var group = sfListView1.View.Groups[0];
sfListView1.ExpandGroup(group);
sfListView1.CollapseGroup(group);

// Expand/collapse all groups
sfListView1.ExpandAllGroups();
sfListView1.CollapseAllGroups();
```

**Control expansion/collapse with events:**

```csharp
// Prevent specific group from expanding
sfListView1.GroupExpanding += (sender, e) =>
{
    if (e.Group == (sender as SfListView).View.Groups[0])
        e.Cancel = true;
};

// Prevent specific group from collapsing
sfListView1.GroupCollapsing += (sender, e) =>
{
    if (e.Group == (sender as SfListView).View.Groups[1])
        e.Cancel = true;
};
```

**GroupExpandCollapseChangingEventArgs Properties:**
- `Group`: The group being expanded or collapsed
- `Cancel`: Set to `true` to prevent the operation

---

## Sorting

Sort data in ascending or descending order using `SortDescriptors` or custom logic.

### Programmatic Sorting

Add a `SortDescriptor` to the `View.SortDescriptors` collection:

```csharp
listView.View.SortDescriptors.Add(new SortDescriptor()
{
    PropertyName = "CountryName",
    Direction = ListSortDirection.Ascending,
});
```

**SortDescriptor Properties:**
- `PropertyName`: Name of the property to sort by
- `Direction`: `ListSortDirection.Ascending` or `Descending`
- `Comparer`: Custom comparer for sorting logic

### Custom Sorting Logic

Implement custom sorting with `IComparer<object>` and `ISortDirection`:

```csharp
sfListView1.View.SortDescriptors.Add(new Syncfusion.DataSource.SortDescriptor()
{
    PropertyName = "CountryName",
    Direction = Syncfusion.DataSource.ListSortDirection.Descending,
    Comparer = new CustomSortComparer()
});

public class CustomSortComparer : IComparer<object>, ISortDirection
{
    public CustomSortComparer()
    {
        this.SortDirection = Syncfusion.DataSource.ListSortDirection.Ascending;
    }

    public Syncfusion.DataSource.ListSortDirection SortDirection { get; set; }

    public int Compare(object x, object y)
    {
        int lengthX = (x as CountryInfo).CountryName.Length;
        int lengthY = (y as CountryInfo).CountryName.Length;
            
        if (lengthX.CompareTo(lengthY) > 0)
            return SortDirection == Syncfusion.DataSource.ListSortDirection.Ascending ? 1 : -1;
        else if (lengthX.CompareTo(lengthY) == -1)
            return SortDirection == Syncfusion.DataSource.ListSortDirection.Ascending ? -1 : 1;
        else
            return 0;
    }
}
```

This example sorts by the length of country names.

### Sorting with Grouping

Combine grouping and sorting by adding both descriptors:

```csharp
listView.View.GroupDescriptors.Add(new GroupDescriptor()
{
    PropertyName = "Continent",                     
});

listView.View.SortDescriptors.Add(new SortDescriptor()
{
    PropertyName = "Continent",
    Direction = ListSortDirection.Descending,
});
```

Items are grouped first, then sorted within each group.

---

## Filtering

Filter data to display only items matching specific criteria.

### Programmatic Filtering

Set the `View.Filter` property and call `RefreshFilter()`:

```csharp
listView.View.Filter = CustomFilter;
listView.View.RefreshFilter();

public bool CustomFilter(object obj)
{
    if ((obj as CountryInfo).Continent == "Asia" || 
        (obj as CountryInfo).Continent == "North America" || 
        (obj as CountryInfo).Continent == "Oceania")
        return true;
    return false;
}
```

**FilterChanged Event:**
Raised after filtering is applied. Use to respond to filter changes.

```csharp
sfListView1.View.FilterChanged += (sender, e) =>
{
    // Handle filter change
};
```

### Multiple Criteria Filtering

Combine multiple conditions in the filter predicate:

```csharp
sfListView1.View.Filter = FilterOnMultipleCriteria;
sfListView1.View.RefreshFilter();

public bool FilterOnMultipleCriteria(object obj)
{
    if ((obj as CountryInfo).Continent == "Asia" || 
        (obj as CountryInfo).CountryName[0].ToString().Equals("U"))
        return true;
    return false;
}
```

This filters for items in Asia OR countries starting with "U".

### Accessing Filtered Data

Get filtered items from `View.DisplayItems` in the `FilterChanged` event:

```csharp
sfListView1.View.FilterChanged += (sender, e) =>
{
    ObservableCollection<object> filteredCountries = new ObservableCollection<object>();
    var items = (sender as DataSource).DisplayItems;
    
    foreach (var item in items)
        filteredCountries.Add(item);
};
```

### Clearing Filters

Remove filters by setting `View.Filter` to `null` and refreshing:

```csharp
sfListView1.View.Filter = null;
sfListView1.View.RefreshFilter();
```

### Sorting Filtered Items

Apply sorting to filtered items in the `FilterChanged` event:

```csharp
sfListView1.View.FilterChanged += (sender, e) =>
{
    sfListView1.View.GroupDescriptors.Add(new GroupDescriptor()
    {
        PropertyName = "Continent",
    });
    
    sfListView1.View.SortDescriptors.Add(new SortDescriptor()
    {
        PropertyName = "Continent",
        Direction = Syncfusion.DataSource.ListSortDirection.Ascending,
    });
};
```

---

## Common Scenarios

### Scenario 1: Grouped List with Sorting
```csharp
// Group by continent, sort alphabetically
sfListView1.View.GroupDescriptors.Add(new GroupDescriptor()
{
    PropertyName = "Continent",               
});
sfListView1.View.SortDescriptors.Add(new SortDescriptor()
{
    PropertyName = "Continent",
    Direction = ListSortDirection.Ascending,
});
```

### Scenario 2: Filtered and Sorted List
```csharp
// Filter for Asia, sort by country name
sfListView1.View.Filter = (obj) => (obj as CountryInfo).Continent == "Asia";
sfListView1.View.RefreshFilter();

sfListView1.View.SortDescriptors.Add(new SortDescriptor()
{
    PropertyName = "CountryName",
    Direction = ListSortDirection.Ascending,
});
```

### Scenario 3: Custom Grouped View with Collapsed Groups
```csharp
// Group by first letter, collapse all groups
sfListView1.View.GroupDescriptors.Add(new GroupDescriptor()
{
    PropertyName = "CountryName",
    KeySelector = (obj) => (obj as CountryInfo).CountryName[0].ToString(),
});
sfListView1.CollapseAllGroups();
```

---

## Troubleshooting

**Issue: Groups not appearing**
- Verify `GroupDescriptor` is added to `View.GroupDescriptors`
- Check that `PropertyName` matches a property in your data model
- Ensure data has multiple distinct values for the group property

**Issue: Sorting not working**
- Confirm `SortDescriptor` is added to `View.SortDescriptors`
- Verify `PropertyName` is correct
- Check that the property implements `IComparable` or use a custom comparer

**Issue: Filter shows no items**
- Debug the filter predicate to verify it returns `true` for some items
- Call `RefreshFilter()` after setting `View.Filter`
- Check for case sensitivity issues in string comparisons

**Issue: Custom comparer not applied**
- Ensure your comparer class implements `IComparer<GroupResult>` (grouping) or `IComparer<object>` (sorting)
- Verify `ISortDirection` is implemented if direction is needed
- Check that `Comparer` property is set on the descriptor
