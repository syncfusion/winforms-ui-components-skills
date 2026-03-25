# Keyboard Navigation and Interaction

## Table of Contents
- [Arrow Key Support](#arrow-key-support)
- [KeyDown Event Handling](#keydown-event-handling)
- [Programmatic Navigation Methods](#programmatic-navigation-methods)
- [Event Handling Patterns](#event-handling-patterns)
- [Practical Examples](#practical-examples)

## Arrow Key Support

The InterceptArrowKeys property enables or disables arrow key navigation through the item list.

### Enabling Arrow Key Navigation

```csharp
// Enable arrow key navigation (recommended)
domainUpDownExt1.InterceptArrowKeys = true;
```

With this setting:
- **Up Arrow**: Navigate to previous item
- **Down Arrow**: Navigate to next item

### Disabling Arrow Key Navigation

```csharp
// Disable arrow key navigation
domainUpDownExt1.InterceptArrowKeys = false;
```

Useful when:
- You want exclusive spin button control
- Arrow keys are needed for other purposes in the form
- You want to validate input before navigation

### Complete Setup Example

```csharp
private void Form_Load(object sender, EventArgs e)
{
    DomainUpDownExt control = new DomainUpDownExt();
    control.Items.Add("Item 1");
    control.Items.Add("Item 2");
    control.Items.Add("Item 3");
    
    // Enable keyboard navigation
    control.InterceptArrowKeys = true;
    
    this.Controls.Add(control);
}
```

## KeyDown Event Handling

Handle keyboard events to add custom behavior or validation.

### Basic KeyDown Event

```csharp
private void Form_Load(object sender, EventArgs e)
{
    domainUpDownExt1.KeyDown += DomainUpDownExt1_KeyDown;
}

private void DomainUpDownExt1_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Enter)
    {
        // Add new item when Enter is pressed
        if (!domainUpDownExt1.Items.Contains(domainUpDownExt1.Text))
        {
            domainUpDownExt1.Items.Add(domainUpDownExt1.Text);
        }
        e.Handled = true;  // Prevent the beep sound
    }
}
```

### Intercepting Specific Keys

```csharp
private void DomainUpDownExt1_KeyDown(object sender, KeyEventArgs e)
{
    // Ctrl+A: Select all text
    if (e.Control && e.KeyCode == Keys.A)
    {
        domainUpDownExt1.SelectAll();
        e.Handled = true;
    }
    
    // Delete: Clear items
    if (e.KeyCode == Keys.Delete)
    {
        if (domainUpDownExt1.Items.Count > 0)
        {
            domainUpDownExt1.Items.RemoveAt(domainUpDownExt1.SelectedIndex);
        }
        e.Handled = true;
    }
    
    // Tab: Move to next control (default behavior)
    if (e.KeyCode == Keys.Tab)
    {
        e.Handled = false; // Allow default tab behavior
    }
}
```

## Programmatic Navigation Methods

Navigate through items using methods instead of user input.

### UpButton Method

Move to the previous item:

```csharp
// Go to previous item
domainUpDownExt1.UpButton();
```

**Example:**
```csharp
private void PreviousButtonClick(object sender, EventArgs e)
{
    domainUpDownExt1.UpButton();
}
```

### DownButton Method

Move to the next item:

```csharp
// Go to next item
domainUpDownExt1.DownButton();
```

**Example:**
```csharp
private void NextButtonClick(object sender, EventArgs e)
{
    domainUpDownExt1.DownButton();
}
```

### Programmatic Navigation Pattern

```csharp
private void NavigateTo(int index)
{
    if (index >= 0 && index < domainUpDownExt1.Items.Count)
    {
        domainUpDownExt1.SelectedIndex = index;
    }
}

// Navigate to specific item
NavigateTo(2);  // Go to third item

// Navigate to first item
NavigateTo(0);

// Navigate to last item
NavigateTo(domainUpDownExt1.Items.Count - 1);
```

## Event Handling Patterns

### Pattern 1: Validation on Navigation

```csharp
private void SetupValidationPattern()
{
    domainUpDownExt1.KeyDown += ValidateBeforeNavigation;
}

private void ValidateBeforeNavigation(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Up || e.KeyCode == Keys.Down)
    {
        // Validate current input before allowing navigation
        if (string.IsNullOrWhiteSpace(domainUpDownExt1.Text))
        {
            MessageBox.Show("Please select a value before navigating.");
            e.Handled = true;
            e.SuppressKeyPress = true;
        }
    }
}
```

### Pattern 2: Auto-Add Items on Enter

```csharp
private void SetupAutoAddPattern()
{
    domainUpDownExt1.KeyDown += AutoAddItemOnEnter;
}

private void AutoAddItemOnEnter(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.Enter)
    {
        string newValue = domainUpDownExt1.Text.Trim();
        
        if (!string.IsNullOrEmpty(newValue) && !domainUpDownExt1.Items.Contains(newValue))
        {
            domainUpDownExt1.Items.Add(newValue);
            domainUpDownExt1.SelectedIndex = domainUpDownExt1.Items.Count - 1;
        }
        
        e.Handled = true;
    }
}
```

### Pattern 3: Selection Changed Feedback

```csharp
private void SetupSelectionFeedback()
{
    domainUpDownExt1.SelectedIndexChanged += OnSelectionChanged;
}

private void OnSelectionChanged(object sender, EventArgs e)
{
    string selectedItem = domainUpDownExt1.SelectedItem?.ToString();
    if (!string.IsNullOrEmpty(selectedItem))
    {
        // Update dependent controls or perform actions
        UpdateDependentFields(selectedItem);
    }
}

private void UpdateDependentFields(string selectedValue)
{
    // Example: Update status label
    labelStatus.Text = "Selected: " + selectedValue;
}
```

## Practical Examples

### Example 1: Enhanced Navigation Control

```csharp
public class EnhancedNavigationForm : Form
{
    private DomainUpDownExt domainUpDownExt1;
    private Button btnPrevious;
    private Button btnNext;
    
    private void SetupEnhancedNavigation()
    {
        // Create buttons for programmatic navigation
        btnPrevious = new Button { Text = "< Previous", Location = new Point(10, 50) };
        btnNext = new Button { Text = "Next >", Location = new Point(100, 50) };
        
        btnPrevious.Click += (s, e) => domainUpDownExt1.UpButton();
        btnNext.Click += (s, e) => domainUpDownExt1.DownButton();
        
        this.Controls.Add(btnPrevious);
        this.Controls.Add(btnNext);
        
        // Enable keyboard navigation
        domainUpDownExt1.InterceptArrowKeys = true;
    }
}
```

### Example 2: Dynamic List Management

```csharp
private void SetupDynamicListManagement()
{
    domainUpDownExt1.Items.Add("Option 1");
    domainUpDownExt1.Items.Add("Option 2");
    domainUpDownExt1.Items.Add("Option 3");
    
    domainUpDownExt1.KeyDown += (s, e) =>
    {
        if (e.Control && e.KeyCode == Keys.Delete)
        {
            // Remove current item
            if (domainUpDownExt1.SelectedIndex >= 0)
            {
                domainUpDownExt1.Items.RemoveAt(domainUpDownExt1.SelectedIndex);
            }
            e.Handled = true;
        }
        else if (e.Control && e.KeyCode == Keys.N)
        {
            // Add new item
            string newItem = PromptForNewItem();
            if (!string.IsNullOrEmpty(newItem))
            {
                domainUpDownExt1.Items.Add(newItem);
            }
            e.Handled = true;
        }
    };
}

private string PromptForNewItem()
{
    // Implementation for prompting user
    return null;
}
```

### Example 3: Keyboard Shortcuts

```csharp
private void SetupKeyboardShortcuts()
{
    domainUpDownExt1.KeyDown += (s, e) =>
    {
        if (e.Alt)
        {
            switch (e.KeyCode)
            {
                case Keys.D1:
                    domainUpDownExt1.SelectedIndex = 0;
                    e.Handled = true;
                    break;
                case Keys.D2:
                    domainUpDownExt1.SelectedIndex = 1;
                    e.Handled = true;
                    break;
                case Keys.D3:
                    domainUpDownExt1.SelectedIndex = 2;
                    e.Handled = true;
                    break;
            }
        }
    };
}
```

### Example 4: Complete Interaction Pattern

```csharp
public class InteractionPatternForm : Form
{
    private DomainUpDownExt domainUpDownExt1;
    private Label labelStatus;
    
    public InteractionPatternForm()
    {
        InitializeComponents();
        SetupInteractionHandlers();
    }
    
    private void InitializeComponents()
    {
        domainUpDownExt1 = new DomainUpDownExt();
        labelStatus = new Label();
        
        domainUpDownExt1.Items.Add("High");
        domainUpDownExt1.Items.Add("Medium");
        domainUpDownExt1.Items.Add("Low");
        domainUpDownExt1.InterceptArrowKeys = true;
    }
    
    private void SetupInteractionHandlers()
    {
        // Selection changed
        domainUpDownExt1.SelectedIndexChanged += (s, e) =>
        {
            labelStatus.Text = "Selected: " + domainUpDownExt1.SelectedItem;
        };
        
        // Keyboard input
        domainUpDownExt1.KeyDown += (s, e) =>
        {
            if (e.KeyCode == Keys.Enter)
            {
                ProcessSelection(domainUpDownExt1.SelectedItem.ToString());
                e.Handled = true;
            }
        };
    }
    
    private void ProcessSelection(string selectedValue)
    {
        // Process the selection
        labelStatus.Text = "Processing: " + selectedValue;
    }
}
```
