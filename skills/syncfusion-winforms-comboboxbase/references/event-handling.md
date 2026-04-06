# Event Handling

This guide covers event handling for ComboBoxBase, including selection events, dropdown events, and integration scenarios.

## Table of Contents
- [Overview](#overview)
- [Selection Events](#selection-events)
- [Event Order and Scenarios](#event-order-and-scenarios)
- [DropDownCloseOnClick Event](#dropdowncloseonclick-event)
- [DropDown Event](#dropdown-event)
- [Validation Events](#validation-events)
- [Complete Examples](#complete-examples)

## Overview

ComboBoxBase fires different events for different user interaction scenarios. Understanding event order and when each event fires is crucial for proper implementation.

**Key Events:**
- `SelectionChangedCommitted` - Selection is committed by user
- `SelectedValueChanged` - Selected value changes (on ListControl)
- `SelectedIndexChanged` - Selected index changes (on ListControl)
- `DropDownCloseOnClick` - Dropdown about to close on click (cancellable)
- `DropDown` - Dropdown is opening
- `Validating`/`Validated` - Control validation

**Important:** Some events (like `SelectedIndexChanged`) are handled on the ListControl, not ComboBoxBase.

## Selection Events

### SelectionChangedCommitted

Fires when user commits a selection through action (Enter key, mouse click, losing focus in editable mode).

**C# Event Handler:**
```csharp
private void comboBoxBase1_SelectionChangedCommitted(object sender, EventArgs e)
{
    string selectedText = comboBoxBase1.TextBox.Text;
    MessageBox.Show($"Selection committed: {selectedText}");
}

// Subscribe
comboBoxBase1.SelectionChangedCommitted += comboBoxBase1_SelectionChangedCommitted;
```

**VB.NET Event Handler:**
```vb
Private Sub comboBoxBase1_SelectionChangedCommitted(sender As Object, e As EventArgs) Handles comboBoxBase1.SelectionChangedCommitted
    Dim selectedText As String = comboBoxBase1.TextBox.Text
    MessageBox.Show($"Selection committed: {selectedText}")
End Sub
```

**When it fires:**
- User presses Enter in text area
- User clicks dropdown button and clicks item
- User loses focus (in DropDown editable mode only)
- Text property changed programmatically

### SelectedValueChanged

Fires when the selected value changes. **This event is on the ListControl, not ComboBoxBase.**

**C# Example:**
```csharp
// Assuming listBox1 is the ListControl
private void listBox1_SelectedValueChanged(object sender, EventArgs e)
{
    if (listBox1.SelectedItem != null)
    {
        string value = listBox1.SelectedItem.ToString();
        Console.WriteLine($"Selected value changed: {value}");
    }
}

// Subscribe
listBox1.SelectedValueChanged += listBox1_SelectedValueChanged;
```

**VB.NET Example:**
```vb
Private Sub listBox1_SelectedValueChanged(sender As Object, e As EventArgs) Handles listBox1.SelectedValueChanged
    If listBox1.SelectedItem IsNot Nothing Then
        Dim value As String = listBox1.SelectedItem.ToString()
        Console.WriteLine($"Selected value changed: {value}")
    End If
End Sub
```

### SelectedIndexChanged

Fires when the selected index changes. **This event is on the ListControl, not ComboBoxBase.**

**C# Example:**
```csharp
private void listBox1_SelectedIndexChanged(object sender, EventArgs e)
{
    int index = listBox1.SelectedIndex;
    Console.WriteLine($"Selected index: {index}");
    
    if (index >= 0)
    {
        object item = listBox1.Items[index];
        Console.WriteLine($"Item: {item}");
    }
}

// Subscribe
listBox1.SelectedIndexChanged += listBox1_SelectedIndexChanged;
```

**VB.NET Example:**
```vb
Private Sub listBox1_SelectedIndexChanged(sender As Object, e As EventArgs) Handles listBox1.SelectedIndexChanged
    Dim index As Integer = listBox1.SelectedIndex
    Console.WriteLine($"Selected index: {index}")
    
    If index >= 0 Then
        Dim item As Object = listBox1.Items(index)
        Console.WriteLine($"Item: {item}")
    End If
End Sub
```

## Event Order and Scenarios

Different user interactions trigger events in different orders. Here's a comprehensive table:

| Scenario | SelectionChangedCommitted | SelectedValueChanged | SelectedIndexChanged | Validating/Validated |
|----------|---------------------------|----------------------|----------------------|----------------------|
| **TextArea - Change Selection by Keys** | Yes (1st) | Yes (2nd) | Yes (3rd) | No |
| **TextArea - On AutoComplete** | No | No | No | No |
| **Drop-Down List - Change by Keys** | No | Yes (1st) | Yes (2nd) | No |
| **Drop-Down List - Change by Mouse Move** | No | No | No | No |
| **Drop-Down Close by Enter Key** | Yes (1st) | No | No | No |
| **Drop-Down Close by Escape Key** | No | No | No | No |
| **Drop-Down Close by Clicking** | Yes (1st) | Yes (2nd) | Yes (3rd) | No |
| **Losing Focus** | Yes (2nd, editable mode only) | No | No | Yes (1st) |
| **Changing Text Property in Code** | Yes (1st) | No | No | No |

### Understanding Event Order

**Scenario 1: User Selects Item by Clicking**
```
1. User opens dropdown
2. User clicks item
3. SelectionChangedCommitted fires (1st)
4. SelectedValueChanged fires (2nd) - on ListControl
5. SelectedIndexChanged fires (3rd) - on ListControl
6. Dropdown closes
```

**Scenario 2: User Navigates with Keyboard**
```
1. User types in text area
2. User presses Down Arrow key
3. SelectionChangedCommitted fires (1st)
4. SelectedValueChanged fires (2nd) - on ListControl
5. SelectedIndexChanged fires (3rd) - on ListControl
```

**Scenario 3: User Presses Enter**
```
1. User opens dropdown
2. User navigates with arrow keys (SelectedValueChanged fires for each movement)
3. User presses Enter
4. SelectionChangedCommitted fires
5. Dropdown closes
```

### Event Handling Example

**Complete event wiring:**
```csharp
public void SetupEvents()
{
    // ComboBoxBase events
    comboBoxBase1.SelectionChangedCommitted += OnSelectionCommitted;
    comboBoxBase1.DropDown += OnDropDown;
    comboBoxBase1.DropDownCloseOnClick += OnDropDownClosing;
    
    // ListControl events (assuming listBox1)
    listBox1.SelectedIndexChanged += OnIndexChanged;
    listBox1.SelectedValueChanged += OnValueChanged;
}

private void OnSelectionCommitted(object sender, EventArgs e)
{
    Console.WriteLine($"[1] SelectionChangedCommitted: {comboBoxBase1.TextBox.Text}");
}

private void OnValueChanged(object sender, EventArgs e)
{
    Console.WriteLine($"[2] SelectedValueChanged: {listBox1.SelectedItem}");
}

private void OnIndexChanged(object sender, EventArgs e)
{
    Console.WriteLine($"[3] SelectedIndexChanged: {listBox1.SelectedIndex}");
}

private void OnDropDown(object sender, EventArgs e)
{
    Console.WriteLine("DropDown opening");
}

private void OnDropDownClosing(object sender, MouseClickCancelEventArgs e)
{
    Console.WriteLine("DropDown closing");
}
```

## DropDownCloseOnClick Event

The `DropDownCloseOnClick` event fires when the dropdown is about to close after a mouse click. You can cancel this event to keep the dropdown open.

### Use Case: CheckedListBox Integration

When using CheckedListBox, prevent dropdown from closing while user checks items:

**C# Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;

private void SetupCheckedListBox()
{
    // Create CheckedListBox
    CheckedListBox checkedListBox1 = new CheckedListBox();
    checkedListBox1.Items.AddRange(new object[] {
        "Option 1",
        "Option 2",
        "Option 3",
        "Option 4",
        "Option 5"
    });
    
    // Connect to ComboBoxBase
    comboBoxBase1.ListControl = checkedListBox1;
    
    // Prevent dropdown from closing on click
    comboBoxBase1.DropDownCloseOnClick += (sender, args) =>
    {
        args.Cancel = true; // Keep dropdown open
    };
    
    // Update text with checked items
    checkedListBox1.ItemCheck += (sender, args) =>
    {
        // Need to use BeginInvoke because ItemCheck fires before check state changes
        this.BeginInvoke(new Action(() =>
        {
            UpdateCheckedItemsText(checkedListBox1);
        }));
    };
}

private void UpdateCheckedItemsText(CheckedListBox checkedList)
{
    List<string> checkedItems = new List<string>();
    foreach (object item in checkedList.CheckedItems)
    {
        checkedItems.Add(item.ToString());
    }
    
    comboBoxBase1.TextBox.Text = string.Join(", ", checkedItems);
}
```

**VB.NET Example:**
```vb
Private Sub SetupCheckedListBox()
    ' Create CheckedListBox
    Dim checkedListBox1 As New CheckedListBox()
    checkedListBox1.Items.AddRange(New Object() {
        "Option 1",
        "Option 2",
        "Option 3",
        "Option 4",
        "Option 5"
    })
    
    ' Connect to ComboBoxBase
    comboBoxBase1.ListControl = checkedListBox1
    
    ' Prevent dropdown from closing on click
    AddHandler comboBoxBase1.DropDownCloseOnClick, Sub(sender, args)
        args.Cancel = True ' Keep dropdown open
    End Sub
    
    ' Update text with checked items
    AddHandler checkedListBox1.ItemCheck, Sub(sender, args)
        Me.BeginInvoke(New Action(Sub()
            UpdateCheckedItemsText(checkedListBox1)
        End Sub))
    End Sub
End Sub

Private Sub UpdateCheckedItemsText(checkedList As CheckedListBox)
    Dim checkedItems As New List(Of String)()
    For Each item As Object In checkedList.CheckedItems
        checkedItems.Add(item.ToString())
    Next
    
    comboBoxBase1.TextBox.Text = String.Join(", ", checkedItems)
End Sub
```

### Conditional Closing

Only cancel closing under specific conditions:

```csharp
comboBoxBase1.DropDownCloseOnClick += (sender, args) =>
{
    // Only prevent closing if Ctrl key is held
    if (Control.ModifierKeys == Keys.Control)
    {
        args.Cancel = true;
        MessageBox.Show("Hold Ctrl to keep dropdown open");
    }
};
```

## DropDown Event

The `DropDown` event fires when the dropdown is opening. Use this to setup relationships with parent PopupControlContainers or perform initialization.

### Basic Usage

**C# Example:**
```csharp
private void comboBoxBase1_DropDown(object sender, EventArgs e)
{
    Console.WriteLine("Dropdown is opening");
    
    // Perform any initialization
    // Update list items if needed
    // Setup popup relationships
}

// Subscribe
comboBoxBase1.DropDown += comboBoxBase1_DropDown;
```

### PopupControlContainer Integration

When ComboBoxBase is inside a PopupControlContainer, setup parent-child relationships:

**C# Example:**
```csharp
private void comboBoxBase1_DropDown(object sender, EventArgs e)
{
    // Setup relationship between ComboBoxBase's dropdown and parent PopupControlContainer
    // This prevents the PopupControlContainer from closing when ComboBoxBase dropdown opens
    
    comboBoxBase1.PopupContainer.PopupParent = this.popupControlContainer1;
    popupControlContainer1.CurrentPopupChild = comboBoxBase1.PopupContainer;
}
```

**VB.NET Example:**
```vb
Private Sub comboBoxBase1_DropDown(sender As Object, e As EventArgs) Handles comboBoxBase1.DropDown
    ' Setup relationship
    comboBoxBase1.PopupContainer.PopupParent = Me.popupControlContainer1
    popupControlContainer1.CurrentPopupChild = comboBoxBase1.PopupContainer
End Sub
```

**Why needed:** Without this relationship, the parent PopupControlContainer would close when ComboBoxBase's dropdown opens.

## Validation Events

Standard WinForms validation events work with ComboBoxBase.

### Validating Event

Fires when control is losing focus, before `Validated` event.

**C# Example:**
```csharp
private void comboBoxBase1_Validating(object sender, System.ComponentModel.CancelEventArgs e)
{
    string text = comboBoxBase1.TextBox.Text;
    
    if (string.IsNullOrWhiteSpace(text))
    {
        e.Cancel = true; // Prevent focus change
        errorProvider1.SetError(comboBoxBase1, "Selection required");
    }
    else
    {
        errorProvider1.SetError(comboBoxBase1, "");
    }
}

// Subscribe
comboBoxBase1.Validating += comboBoxBase1_Validating;
```

**VB.NET Example:**
```vb
Private Sub comboBoxBase1_Validating(sender As Object, e As System.ComponentModel.CancelEventArgs) Handles comboBoxBase1.Validating
    Dim text As String = comboBoxBase1.TextBox.Text
    
    If String.IsNullOrWhiteSpace(text) Then
        e.Cancel = True ' Prevent focus change
        errorProvider1.SetError(comboBoxBase1, "Selection required")
    Else
        errorProvider1.SetError(comboBoxBase1, "")
    End If
End Sub
```

### Validated Event

Fires after validation succeeds.

**C# Example:**
```csharp
private void comboBoxBase1_Validated(object sender, EventArgs e)
{
    Console.WriteLine("Validation passed");
    SaveSelection();
}

// Subscribe
comboBoxBase1.Validated += comboBoxBase1_Validated;
```

## Complete Examples

### Example 1: Selection Tracking

Track all selection changes with proper event handling:

```csharp
public class SelectionTracker
{
    private ComboBoxBase comboBoxBase1;
    private ListBox listBox1;
    private TextBox logTextBox;
    
    public void Setup()
    {
        comboBoxBase1 = new ComboBoxBase();
        listBox1 = new ListBox();
        logTextBox = new TextBox { Multiline = true, Dock = DockStyle.Bottom, Height = 150 };
        
        // Setup data
        listBox1.Items.AddRange(new object[] { "Item 1", "Item 2", "Item 3" });
        comboBoxBase1.ListControl = listBox1;
        
        // Wire events
        comboBoxBase1.SelectionChangedCommitted += OnSelectionCommitted;
        comboBoxBase1.DropDown += OnDropDown;
        comboBoxBase1.DropDownCloseOnClick += OnDropDownClose;
        listBox1.SelectedIndexChanged += OnIndexChanged;
        listBox1.SelectedValueChanged += OnValueChanged;
    }
    
    private void Log(string message)
    {
        logTextBox.AppendText($"[{DateTime.Now:HH:mm:ss.fff}] {message}\r\n");
    }
    
    private void OnSelectionCommitted(object sender, EventArgs e)
    {
        Log($"SelectionChangedCommitted: {comboBoxBase1.TextBox.Text}");
    }
    
    private void OnDropDown(object sender, EventArgs e)
    {
        Log("DropDown: Opening");
    }
    
    private void OnDropDownClose(object sender, MouseClickCancelEventArgs e)
    {
        Log($"DropDownCloseOnClick: Closing (Cancel={e.Cancel})");
    }
    
    private void OnIndexChanged(object sender, EventArgs e)
    {
        Log($"SelectedIndexChanged: Index={listBox1.SelectedIndex}");
    }
    
    private void OnValueChanged(object sender, EventArgs e)
    {
        Log($"SelectedValueChanged: Value={listBox1.SelectedItem}");
    }
}
```

### Example 2: CheckedListBox with Auto-Close Button

CheckedListBox with a "Done" button to close dropdown:

```csharp
public void CreateCheckedComboBox()
{
    // Create controls
    CheckedListBox checkedList = new CheckedListBox();
    Button btnDone = new Button { Text = "Done", Dock = DockStyle.Bottom };
    Panel container = new Panel();
    
    // Setup CheckedListBox
    checkedList.Items.AddRange(new object[] {
        "Feature A", "Feature B", "Feature C", "Feature D"
    });
    checkedList.Dock = DockStyle.Fill;
    
    // Add to container
    container.Controls.Add(checkedList);
    container.Controls.Add(btnDone);
    
    // Connect container to ComboBoxBase via its PopupContainer
    // Use PopupContainer to host non-ListControl UI (Panel with controls)
    comboBoxBase1.PopupContainer.Size = new System.Drawing.Size(300, 200);
    comboBoxBase1.PopupContainer.Controls.Add(container);

    // Prevent auto-close while interacting with contained controls
    comboBoxBase1.DropDownCloseOnClick += (s, e) => e.Cancel = true;
    
    // Handle Done button
    btnDone.Click += (s, e) =>
    {
        UpdateTextFromCheckedItems(checkedList);
        comboBoxBase1.PopupContainer.Hide(); // Close dropdown
    };
    
    // Update text as items are checked
    checkedList.ItemCheck += (s, e) =>
    {
        this.BeginInvoke(new Action(() => UpdateTextFromCheckedItems(checkedList)));
    };
}

private void UpdateTextFromCheckedItems(CheckedListBox checkedList)
{
    var items = checkedList.CheckedItems.Cast<object>().Select(i => i.ToString());
    comboBoxBase1.TextBox.Text = string.Join(", ", items);
}
```

### Example 3: Validated Input with Error Provider

```csharp
public void SetupValidatedComboBox()
{
    ErrorProvider errorProvider = new ErrorProvider();
    
    // Setup ComboBoxBase
    comboBoxBase1.Validating += (s, e) =>
    {
        if (string.IsNullOrWhiteSpace(comboBoxBase1.TextBox.Text))
        {
            e.Cancel = true;
            errorProvider.SetError(comboBoxBase1, "Please select an item");
        }
        else if (!listBox1.Items.Contains(comboBoxBase1.TextBox.Text))
        {
            e.Cancel = true;
            errorProvider.SetError(comboBoxBase1, "Invalid selection");
        }
        else
        {
            errorProvider.SetError(comboBoxBase1, "");
        }
    };
    
    comboBoxBase1.Validated += (s, e) =>
    {
        MessageBox.Show($"Valid selection: {comboBoxBase1.TextBox.Text}");
    };
}
```

## Best Practices

**Event Subscription:**
- Subscribe to events after control creation and setup
- Unsubscribe when disposing controls to prevent memory leaks
- Use named methods for complex event handlers (easier debugging)

**Event Order:**
- Remember `SelectedIndexChanged` and `SelectedValueChanged` are on ListControl
- Use `SelectionChangedCommitted` for committed user actions
- Don't rely on exact event order for complex scenarios

**CheckedListBox:**
- Always cancel `DropDownCloseOnClick` for multi-select
- Use `BeginInvoke` when updating text in `ItemCheck` handler
- Provide visual feedback for checked items

**Validation:**
- Use `Validating` event to prevent invalid input
- Provide clear error messages via ErrorProvider
- Allow cancellation of focus change if validation fails

## Next Steps

- **Advanced Scenarios:** Read [advanced-scenarios.md](advanced-scenarios.md) for PopupControlContainer integration
- **ListControl Architecture:** Read [listcontrol-architecture.md](listcontrol-architecture.md) for custom controls
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
