# Advanced Usage and Patterns

## Table of Contents
- [Runtime Item Management](#runtime-item-management)
- [Programmatic Navigation](#programmatic-navigation)
- [Advanced Event Handling](#advanced-event-handling)
- [Data Binding Patterns](#data-binding-patterns)
- [Common Use Cases](#common-use-cases)

## Runtime Item Management

### Adding Items on User Input

Allow users to add new items when they press Enter:

```csharp
private void domainUpDownExt1_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Enter)
    {
        string userInput = domainUpDownExt1.Text;
        
        // Validate before adding
        if (!string.IsNullOrWhiteSpace(userInput) && 
            !domainUpDownExt1.Items.Contains(userInput))
        {
            domainUpDownExt1.Items.Add(userInput);
            domainUpDownExt1.SelectedIndex = domainUpDownExt1.Items.Count - 1;
        }
        
        e.Handled = true;
    }
}
```

### Loading Items from External Source

```csharp
public void LoadItemsFromDatabase()
{
    domainUpDownExt1.Items.Clear();
    
    try
    {
        // Example: Load from database
        var items = GetItemsFromDatabase();
        
        foreach (var item in items)
        {
            if (!domainUpDownExt1.Items.Contains(item))
            {
                domainUpDownExt1.Items.Add(item);
            }
        }
        
        if (domainUpDownExt1.Items.Count > 0)
        {
            domainUpDownExt1.SelectedIndex = 0;
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show("Error loading items: " + ex.Message);
    }
}

private List<string> GetItemsFromDatabase()
{
    // Replace with actual database logic
    return new List<string> { "Item 1", "Item 2", "Item 3" };
}
```

### Removing Items Based on Criteria

```csharp
public void RemoveItemsStartingWith(string prefix)
{
    // Collect items to remove
    var itemsToRemove = new List<object>();
    
    foreach (var item in domainUpDownExt1.Items)
    {
        if (item.ToString().StartsWith(prefix))
        {
            itemsToRemove.Add(item);
        }
    }
    
    // Remove collected items
    foreach (var item in itemsToRemove)
    {
        domainUpDownExt1.Items.Remove(item);
    }
}

public void RemoveItemsContaining(string substring)
{
    var itemsToRemove = new List<object>();
    
    foreach (var item in domainUpDownExt1.Items)
    {
        if (item.ToString().Contains(substring))
        {
            itemsToRemove.Add(item);
        }
    }
    
    foreach (var item in itemsToRemove)
    {
        domainUpDownExt1.Items.Remove(item);
    }
}
```

## Programmatic Navigation

### Circular Navigation

Implement wrapping behavior when reaching the end of the list:

```csharp
public void NavigateCircular(bool goForward)
{
    int currentIndex = domainUpDownExt1.SelectedIndex;
    int itemCount = domainUpDownExt1.Items.Count;
    
    if (itemCount == 0) return;
    
    int newIndex;
    if (goForward)
    {
        newIndex = (currentIndex + 1) % itemCount;  // Wrap around
    }
    else
    {
        newIndex = (currentIndex - 1 + itemCount) % itemCount;  // Wrap around
    }
    
    domainUpDownExt1.SelectedIndex = newIndex;
}

// Usage
private void Form_Load(object sender, EventArgs e)
{
    domainUpDownExt1.Items.Add("First");
    domainUpDownExt1.Items.Add("Second");
    domainUpDownExt1.Items.Add("Third");
    
    // Navigate forward (wraps around)
    NavigateCircular(true);   // Goes to Second
    NavigateCircular(true);   // Goes to Third
    NavigateCircular(true);   // Wraps to First
}
```

### Jump to Item

```csharp
public bool JumpToItem(string itemValue)
{
    int index = domainUpDownExt1.Items.IndexOf(itemValue);
    
    if (index >= 0)
    {
        domainUpDownExt1.SelectedIndex = index;
        return true;
    }
    
    return false;
}

// Usage
if (JumpToItem("Specific Item"))
{
    MessageBox.Show("Item found and selected.");
}
else
{
    MessageBox.Show("Item not found.");
}
```

### Jump to Item with Search

```csharp
public bool JumpToItemStartingWith(string prefix)
{
    for (int i = 0; i < domainUpDownExt1.Items.Count; i++)
    {
        if (domainUpDownExt1.Items[i].ToString().StartsWith(prefix, StringComparison.OrdinalIgnoreCase))
        {
            domainUpDownExt1.SelectedIndex = i;
            return true;
        }
    }
    
    return false;
}

// Usage - Jump to item starting with "Op"
JumpToItemStartingWith("Op");  // Jumps to "Option 1" or "Option 2"
```

## Advanced Event Handling

### Validated Selection Changes

```csharp
private void SetupValidatedSelection()
{
    domainUpDownExt1.SelectedIndexChanged += ValidatedSelectionChanged;
}

private void ValidatedSelectionChanged(object sender, EventArgs e)
{
    if (domainUpDownExt1.SelectedIndex < 0)
    {
        // No valid selection
        return;
    }
    
    string selectedValue = domainUpDownExt1.SelectedItem.ToString();
    
    // Validate and process
    if (IsValidSelection(selectedValue))
    {
        ProcessValidSelection(selectedValue);
    }
    else
    {
        MessageBox.Show("Invalid selection detected.");
        domainUpDownExt1.SelectedIndex = -1;
    }
}

private bool IsValidSelection(string value)
{
    // Implement validation logic
    return !string.IsNullOrWhiteSpace(value);
}

private void ProcessValidSelection(string value)
{
    // Process the valid selection
}
```

### Double Navigation

Respond to rapid navigation (double-click on buttons):

```csharp
private DateTime lastNavigationTime = DateTime.MinValue;
private int navigationCount = 0;

private void SetupDoubleNavigationDetection()
{
    domainUpDownExt1.SelectedIndexChanged += DetectDoubleNavigation;
}

private void DetectDoubleNavigationDetection(object sender, EventArgs e)
{
    DateTime now = DateTime.Now;
    TimeSpan timeSinceLastNavigation = now - lastNavigationTime;
    
    if (timeSinceLastNavigation.TotalMilliseconds < 200)
    {
        navigationCount++;
        
        if (navigationCount >= 2)
        {
            HandleFastNavigation();
            navigationCount = 0;
        }
    }
    else
    {
        navigationCount = 1;
    }
    
    lastNavigationTime = now;
}

private void HandleFastNavigation()
{
    // Response to rapid navigation
    MessageBox.Show("Fast navigation detected!");
}
```

## Data Binding Patterns

### Binding to Data Source

```csharp
public void BindToDataSource(List<string> dataSource)
{
    domainUpDownExt1.Items.Clear();
    
    foreach (var item in dataSource)
    {
        domainUpDownExt1.Items.Add(item);
    }
    
    if (domainUpDownExt1.Items.Count > 0)
    {
        domainUpDownExt1.SelectedIndex = 0;
    }
}

// Usage
List<string> priorities = new List<string> { "Low", "Medium", "High", "Critical" };
BindToDataSource(priorities);
```

### Binding with Objects

```csharp
public class Priority
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public override string ToString()
    {
        return Name;
    }
}

public void BindToObjectList(List<Priority> priorities)
{
    domainUpDownExt1.Items.Clear();
    
    foreach (var priority in priorities)
    {
        domainUpDownExt1.Items.Add(priority);
    }
    
    if (domainUpDownExt1.Items.Count > 0)
    {
        domainUpDownExt1.SelectedIndex = 0;
    }
}

// Usage
var priorities = new List<Priority>
{
    new Priority { Id = 1, Name = "Low" },
    new Priority { Id = 2, Name = "Medium" },
    new Priority { Id = 3, Name = "High" }
};

BindToObjectList(priorities);
```

## Common Use Cases

### Use Case 1: Priority Selector

```csharp
public class PrioritySelectorControl
{
    private DomainUpDownExt control;
    
    public void Setup()
    {
        control.Items.Add("Low");
        control.Items.Add("Medium");
        control.Items.Add("High");
        control.Items.Add("Critical");
        
        control.SelectedIndex = 1; // Default: Medium
        control.SpinOrientation = Orientation.Vertical;
        control.BorderStyle = BorderStyle.FixedSingle;
    }
    
    public string GetSelectedPriority()
    {
        return control.SelectedItem?.ToString() ?? string.Empty;
    }
}
```

### Use Case 2: Month/Date Selector

```csharp
public void SetupMonthSelector()
{
    domainUpDownExt1.Items.Clear();
    
    string[] months = 
    { 
        "January", "February", "March", "April", "May", "June",
        "July", "August", "September", "October", "November", "December"
    };
    
    foreach (var month in months)
    {
        domainUpDownExt1.Items.Add(month);
    }
    
    domainUpDownExt1.SelectedIndex = DateTime.Now.Month - 1;
}
```

### Use Case 3: Year Selector

```csharp
public void SetupYearSelector(int startYear = 2000, int endYear = 2030)
{
    domainUpDownExt1.Items.Clear();
    
    for (int year = startYear; year <= endYear; year++)
    {
        domainUpDownExt1.Items.Add(year.ToString());
    }
    
    int currentYearIndex = DateTime.Now.Year - startYear;
    if (currentYearIndex >= 0 && currentYearIndex < domainUpDownExt1.Items.Count)
    {
        domainUpDownExt1.SelectedIndex = currentYearIndex;
    }
}
```

### Use Case 4: Status Workflow

```csharp
public class WorkflowStatusControl
{
    private DomainUpDownExt statusControl;
    private string[] validStatuses = { "Draft", "In Review", "Approved", "Published" };
    
    public void Setup()
    {
        foreach (var status in validStatuses)
        {
            statusControl.Items.Add(status);
        }
        
        statusControl.SelectedIndex = 0;
        statusControl.SelectedIndexChanged += StatusChanged;
    }
    
    private void StatusChanged(object sender, EventArgs e)
    {
        string newStatus = statusControl.SelectedItem.ToString();
        
        // Validate workflow transition
        if (IsValidTransition(newStatus))
        {
            OnStatusChanged(newStatus);
        }
        else
        {
            MessageBox.Show("Invalid status transition.");
            // Revert selection
        }
    }
    
    private bool IsValidTransition(string newStatus)
    {
        // Implement workflow validation
        return true;
    }
    
    private void OnStatusChanged(string newStatus)
    {
        // Handle status change
    }
}
```

### Use Case 5: Difficulty Level Selector

```csharp
public void SetupDifficultySelector()
{
    domainUpDownExt1.Items.Add("Beginner");
    domainUpDownExt1.Items.Add("Intermediate");
    domainUpDownExt1.Items.Add("Advanced");
    domainUpDownExt1.Items.Add("Expert");
    
    domainUpDownExt1.SelectedIndex = 0;
    domainUpDownExt1.TextAlign = HorizontalAlignment.Center;
    domainUpDownExt1.SpinOrientation = Orientation.Horizontal;
    
    domainUpDownExt1.SelectedIndexChanged += (s, e) =>
    {
        int difficulty = domainUpDownExt1.SelectedIndex;
        UpdateUIBasedOnDifficulty(difficulty);
    };
}

private void UpdateUIBasedOnDifficulty(int difficulty)
{
    // Update UI elements based on selected difficulty
}
```
