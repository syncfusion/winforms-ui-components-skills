# Appearance and Behavior Settings

This guide covers appearance customization, styling options, and behavioral settings for ComboBoxBase.

## Overview

ComboBoxBase provides style, appearance, and customization properties for both the TextBox portion and dropdown window. The control offers flexibility in visual presentation while maintaining separation from the ListControl styling.

**Key Customization Areas:**
- Style and FlatStyle properties
- Border drawing
- TextBox appearance
- Dropdown window styling
- Background and foreground colors

**Important:** ComboBoxBase focuses on the combo box chrome (textbox, button, border). The ListControl (ListBox, CheckedListBox, etc.) handles its own appearance separately.

## Style Property

The `Style` property controls the overall visual style of the ComboBoxBase.

### Available Styles

**C# Enum Values:**
```csharp
public enum ComboBoxStyle
{
    DropDown,      // Editable text area with dropdown
    DropDownList,  // Non-editable, selection only
    Simple         // Always-visible list (not commonly used)
}
```

### DropDown Style (Editable)

Allows user to type or select from dropdown.

**C#:**
```csharp
comboBoxBase1.Style = ComboBoxStyle.DropDown;
```

**VB.NET:**
```vb
comboBoxBase1.Style = ComboBoxStyle.DropDown
```

**Characteristics:**
- Text area is editable
- User can type custom values
- Dropdown button shows list
- Supports AutoComplete (if ListControl provides it)

**Use when:**
- Users need to enter custom values
- AutoComplete is desired
- Free-form input is acceptable

### DropDownList Style (Non-Editable)

User can only select from predefined list.

**C#:**
```csharp
comboBoxBase1.Style = ComboBoxStyle.DropDownList;
```

**VB.NET:**
```vb
comboBoxBase1.Style = ComboBoxStyle.DropDownList
```

**Characteristics:**
- Text area is read-only
- User must select from list
- No custom values allowed
- Keyboard navigation through list

**Use when:**
- Only predefined values are valid
- Data integrity is important
- Selection from fixed set required

### Simple Style

List is always visible (rarely used).

**C#:**
```csharp
comboBoxBase1.Style = ComboBoxStyle.Simple;
```

**Characteristics:**
- List portion always visible
- No dropdown button
- Similar to TextBox + ListBox side-by-side

**Use when:** Very specific UI requirements (uncommon)

## FlatStyle Property

Controls the 3D appearance of the combo box border and button.

### Available FlatStyles

**C#:**
```csharp
public enum FlatStyle
{
    Flat,      // Flat appearance
    Popup,     // Flat until hover, then 3D
    Standard,  // Always 3D
    System     // Use system theme
}
```

### Flat

**C#:**
```csharp
comboBoxBase1.FlatStyle = FlatStyle.Flat;
```

**Appearance:** Completely flat, no 3D borders

**Use for:** Modern, minimal designs

### Popup

**C#:**
```csharp
comboBoxBase1.FlatStyle = FlatStyle.Popup;
```

**Appearance:** Flat until mouse hovers, then shows 3D effect

**Use for:** Modern design with subtle interaction feedback

### Standard

**C#:**
```csharp
comboBoxBase1.FlatStyle = FlatStyle.Standard;
```

**Appearance:** Always shows 3D borders

**Use for:** Traditional Windows Forms appearance

### System

**C#:**
```csharp
comboBoxBase1.FlatStyle = FlatStyle.System;
```

**Appearance:** Follows system theme settings

**Use for:** Consistency with OS theme

## Border Drawing

ComboBoxBase provides advanced border drawing capabilities.

### Border Style

**C# Example:**
```csharp
// Set border style
comboBoxBase1.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken;
comboBoxBase1.BorderSides = System.Windows.Forms.Border3DSide.All;
```

**VB.NET Example:**
```vb
' Set border style
comboBoxBase1.Border3DStyle = System.Windows.Forms.Border3DStyle.Sunken
comboBoxBase1.BorderSides = System.Windows.Forms.Border3DSide.All
```

### Border Color

**C#:**
```csharp
comboBoxBase1.BorderColor = Color.Navy;
```

**VB.NET:**
```vb
comboBoxBase1.BorderColor = Color.Navy
```

## TextBox Appearance

Access and customize the internal TextBox.

### Accessing TextBox

The `TextBox` property provides direct access:

**C#:**
```csharp
TextBox textBox = comboBoxBase1.TextBox;
```

### TextBox Customization

**Font:**
```csharp
comboBoxBase1.TextBox.Font = new Font("Arial", 10F);
```

**Text Color:**
```csharp
comboBoxBase1.TextBox.ForeColor = Color.DarkBlue;
```

**Background Color:**
```csharp
comboBoxBase1.TextBox.BackColor = Color.LightYellow;
```

**Text Alignment:**
```csharp
comboBoxBase1.TextBox.TextAlign = HorizontalAlignment.Center;
```

**Read-Only (Alternative to DropDownList style):**
```csharp
comboBoxBase1.TextBox.ReadOnly = true;
```

### Complete TextBox Styling Example

**C#:**
```csharp
private void StyleTextBox()
{
    comboBoxBase1.TextBox.Font = new Font("Segoe UI", 10F, FontStyle.Regular);
    comboBoxBase1.TextBox.ForeColor = Color.DarkSlateGray;
    comboBoxBase1.TextBox.BackColor = Color.White;
    comboBoxBase1.TextBox.BorderStyle = BorderStyle.None; // If parent handles border
}
```

**VB.NET:**
```vb
Private Sub StyleTextBox()
    comboBoxBase1.TextBox.Font = New Font("Segoe UI", 10.0F, FontStyle.Regular)
    comboBoxBase1.TextBox.ForeColor = Color.DarkSlateGray
    comboBoxBase1.TextBox.BackColor = Color.White
    comboBoxBase1.TextBox.BorderStyle = BorderStyle.None ' If parent handles border
End Sub
```

## Dropdown Window Styling

Customize the dropdown popup container.

### Accessing PopupContainer

**C#:**
```csharp
PopupControlContainer popup = comboBoxBase1.PopupContainer;
```

### Dropdown Size

**C#:**
```csharp
// Set dropdown height
comboBoxBase1.DropDownHeight = 200;

// Set dropdown width (usually auto-sized to ComboBoxBase width)
// Handled by PopupContainer.Width
```

### Dropdown Background

**C#:**
```csharp
comboBoxBase1.PopupContainer.BackColor = Color.WhiteSmoke;
```

### Dropdown Border

**C#:**
```csharp
comboBoxBase1.PopupContainer.BorderStyle = BorderStyle.FixedSingle;
```

## Color Properties

### Background Color

**C#:**
```csharp
comboBoxBase1.BackColor = Color.LightBlue;
```

**VB.NET:**
```vb
comboBoxBase1.BackColor = Color.LightBlue
```

### Foreground Color

**C#:**
```csharp
comboBoxBase1.ForeColor = Color.DarkBlue;
```

**VB.NET:**
```vb
comboBoxBase1.ForeColor = Color.DarkBlue
```

## Properties vs Framework ComboBox

ComboBoxBase has a different object model from the standard Framework ComboBox. Some properties are missing from ComboBoxBase because they belong to the ListControl.

### Missing Properties (Set on ListControl Instead)

| Framework ComboBox Property | ComboBoxBase Equivalent |
|----------------------------|-----------------------|
| `Items` | `listBox1.Items` |
| `DataSource` | `listBox1.DataSource` |
| `DisplayMember` | `listBox1.DisplayMember` |
| `ValueMember` | `listBox1.ValueMember` |
| `SelectedItem` | `listBox1.SelectedItem` |
| `SelectedIndex` | `listBox1.SelectedIndex` |
| `SelectedValue` | `listBox1.SelectedValue` |
| `ItemHeight` | `listBox1.ItemHeight` |
| `IntegralHeight` | `listBox1.IntegralHeight` |
| `MaxDropDownItems` | Not directly available |

### Events: ComboBoxBase vs Framework ComboBox

| Framework ComboBox Event | ComboBoxBase Equivalent |
|-------------------------|------------------------|
| `SelectedIndexChanged` | `listBox1.SelectedIndexChanged` |
| `SelectionChangeCommitted` | `comboBoxBase1.SelectionChangedCommitted` |
| `DropDown` | `comboBoxBase1.DropDown` |
| `DropDownClosed` | Not directly available (use DropDownCloseOnClick) |

### Example: Framework ComboBox vs ComboBoxBase

**Framework ComboBox Code:**
```csharp
ComboBox comboBox1 = new ComboBox();
comboBox1.Items.AddRange(new object[] { "Item 1", "Item 2" });
comboBox1.SelectedIndexChanged += (s, e) => { /* ... */ };
```

**ComboBoxBase Equivalent:**
```csharp
ComboBoxBase comboBoxBase1 = new ComboBoxBase();
ListBox listBox1 = new ListBox();
listBox1.Items.AddRange(new object[] { "Item 1", "Item 2" });
comboBoxBase1.ListControl = listBox1;
listBox1.SelectedIndexChanged += (s, e) => { /* ... */ };
```

## ComboBoxAdv Alternative

If you need ComboBoxBase flexibility with Framework ComboBox object model, use **ComboBoxAdv**.

### ComboBoxAdv Overview

ComboBoxAdv is based on ComboBoxBase but provides an object model identical to Framework ComboBox.

**When to use ComboBoxAdv:**
- Want ComboBoxBase flexibility
- Prefer familiar Framework ComboBox API
- Migrating from standard ComboBox
- Need both power and ease of use

**ComboBoxAdv Example:**
```csharp
using Syncfusion.Windows.Forms.Tools;

ComboBoxAdv comboBoxAdv1 = new ComboBoxAdv();

// Framework ComboBox-like API
comboBoxAdv1.Items.AddRange(new object[] { "Option 1", "Option 2" });
comboBoxAdv1.SelectedIndexChanged += (s, e) => {
    Console.WriteLine($"Selected: {comboBoxAdv1.SelectedItem}");
};

// But with ComboBoxBase power
comboBoxAdv1.Style = ComboBoxStyle.DropDownList;
comboBoxAdv1.FlatStyle = FlatStyle.Flat;
```

## Complete Styling Example

Full example with comprehensive styling:

**C#:**
```csharp
public void CreateStyledComboBox()
{
    // Create controls
    ComboBoxBase comboBoxBase1 = new ComboBoxBase();
    ListBox listBox1 = new ListBox();
    
    // ComboBoxBase appearance
    comboBoxBase1.Size = new Size(250, 25);
    comboBoxBase1.Style = ComboBoxStyle.DropDownList;
    comboBoxBase1.FlatStyle = FlatStyle.Flat;
    comboBoxBase1.BackColor = Color.White;
    comboBoxBase1.BorderColor = Color.FromArgb(0, 120, 215); // Blue
    
    // TextBox styling
    comboBoxBase1.TextBox.Font = new Font("Segoe UI", 10F);
    comboBoxBase1.TextBox.ForeColor = Color.FromArgb(50, 50, 50);
    comboBoxBase1.TextBox.BackColor = Color.White;
    
    // Dropdown styling
    comboBoxBase1.DropDownHeight = 150;
    comboBoxBase1.PopupContainer.BackColor = Color.White;
    
    // ListBox styling
    listBox1.Font = new Font("Segoe UI", 9F);
    listBox1.ForeColor = Color.FromArgb(50, 50, 50);
    listBox1.BackColor = Color.White;
    listBox1.BorderStyle = BorderStyle.None;
    
    // Add data
    listBox1.Items.AddRange(new object[] {
        "Modern Option 1",
        "Modern Option 2",
        "Modern Option 3",
        "Modern Option 4"
    });
    
    // Connect
    comboBoxBase1.ListControl = listBox1;
    
    // Add to form
    this.Controls.Add(listBox1);
    this.Controls.Add(comboBoxBase1);
}
```

## Best Practices

**Style Selection:**
- Use `DropDown` for scenarios where custom input is valid
- Use `DropDownList` for strict selection from predefined values
- Avoid `Simple` style unless specific UI requirements

**Visual Consistency:**
- Match ComboBoxBase styling to ListControl styling
- Use consistent fonts and colors
- Consider application theme

**Performance:**
- Avoid complex custom drawing unless necessary
- Use built-in styling properties first
- Test appearance on different DPI settings

**Accessibility:**
- Ensure sufficient color contrast
- Don't rely solely on color for state
- Test with high contrast themes

## Next Steps

- **Event Handling:** Read [event-handling.md](event-handling.md) for interaction events
- **Advanced Scenarios:** Read [advanced-scenarios.md](advanced-scenarios.md) for complex use cases
- **Getting Started:** Return to [getting-started.md](getting-started.md) for basic setup
