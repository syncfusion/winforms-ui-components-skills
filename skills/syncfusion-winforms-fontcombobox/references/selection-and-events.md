# Selection and Events

Comprehensive guide to handling font selection, responding to user input, and managing events in FontComboBox control.

## Table of Contents
- [Programmatic Selection](#programmatic-selection)
- [SelectedIndexChanged Event](#selectedindexchanged-event)
- [Custom FontSelected Event](#custom-fontselected-event)
- [Applying Selected Fonts](#applying-selected-fonts)
- [Complete Examples](#complete-examples)

---

## Programmatic Selection

Set the selected font programmatically using properties or retrieve current selection.

### SelectedItem Property

Gets or sets the currently selected font by name.

**Property Type:** `object` (typically `string` for font names)  
**Default Value:** `null`

#### Setting Selection by Font Name

**C# Example:**
```csharp
// Select specific font by name
fontComboBox.Text = "Arial";

// Select another font
fontComboBox.Text = "Segoe UI";

// Common fonts
fontComboBox.Text = "Calibri";
fontComboBox.Text = "Times New Roman";
fontComboBox.Text = "Verdana";
```

**VB.NET Example:**
```vb
' Select specific font by name
fontComboBox.Text = "Arial"

' Select another font
fontComboBox.Text = "Segoe UI"

' Common fonts
fontComboBox.Text = "Calibri"
fontComboBox.Text = "Times New Roman"
fontComboBox.Text = "Verdana"
```

#### Getting Selected Font

**C# Example:**
```csharp
// Get selected font name
if (fontComboBox.Text != null)
{
    string selectedFont = fontComboBox.Text.ToString();
    MessageBox.Show($"Selected font: {selectedFont}");
}
else
{
    MessageBox.Show("No font selected");
}
```

**VB.NET Example:**
```vb
' Get selected font name
If fontComboBox.Text IsNot Nothing Then
    Dim selectedFont As String = fontComboBox.Text.ToString()
    MessageBox.Show($"Selected font: {selectedFont}")
Else
    MessageBox.Show("No font selected")
End If
```

#### Null Check Best Practice

```csharp
// Always check for null before using
if (fontComboBox.Text != null)
{
    string fontName = fontComboBox.Text.ToString();
    // Use fontName safely
}
```

**Why:** SelectedItem can be null if no selection has been made.

---

### SelectedIndex Property

Gets or sets the zero-based index of the selected item.

**Property Type:** `int`  
**Default Value:** `-1` (no selection)

#### Setting Selection by Index

**C# Example:**
```csharp
// Select first font in list
fontComboBox.SelectedIndex = 0;

// Select second font
fontComboBox.SelectedIndex = 1;

// Select fifth font
fontComboBox.SelectedIndex = 4;

// Clear selection
fontComboBox.SelectedIndex = -1;
```

**VB.NET Example:**
```vb
' Select first font in list
fontComboBox.SelectedIndex = 0

' Select second font
fontComboBox.SelectedIndex = 1

' Select fifth font
fontComboBox.SelectedIndex = 4

' Clear selection
fontComboBox.SelectedIndex = -1
```

#### Getting Selected Index

**C# Example:**
```csharp
int currentIndex = fontComboBox.SelectedIndex;

if (currentIndex != -1)
{
    MessageBox.Show($"Selected index: {currentIndex}");
}
else
{
    MessageBox.Show("No selection");
}
```

**VB.NET Example:**
```vb
Dim currentIndex As Integer = fontComboBox.SelectedIndex

If currentIndex <> -1 Then
    MessageBox.Show($"Selected index: {currentIndex}")
Else
    MessageBox.Show("No selection")
End If
```

#### Index Validation

```csharp
// Check if index is valid before setting
if (index >= 0 && index < fontComboBox.Items.Count)
{
    fontComboBox.SelectedIndex = index;
}
```

---

### SelectedItem vs SelectedIndex

**Use SelectedItem when:**
- You know the exact font name
- Restoring user preferences (font name saved)
- Setting default font from configuration

**Use SelectedIndex when:**
- Navigating sequentially (next/previous)
- Working with item positions
- No specific font name available

---

## SelectedIndexChanged Event

Fires whenever the selection changes, either programmatically or by user interaction.

**Event Type:** `EventHandler`  
**Sender:** FontComboBox instance  
**EventArgs:** Empty EventArgs

### Basic Event Handling

**C# Example:**
```csharp
// Subscribe to event in constructor or InitializeComponent
fontComboBox.SelectedIndexChanged += FontComboBox_SelectedIndexChanged;

// Event handler
private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    if (fontComboBox.Text != null)
    {
        string selectedFont = fontComboBox.Text.ToString();
        
        // Handle font change
        MessageBox.Show($"Font changed to: {selectedFont}");
    }
}
```

**VB.NET Example:**
```vb
' Subscribe to event in constructor or InitializeComponent
AddHandler fontComboBox.SelectedIndexChanged, AddressOf FontComboBox_SelectedIndexChanged

' Event handler
Private Sub FontComboBox_SelectedIndexChanged(sender As Object, e As EventArgs)
    If fontComboBox.Text IsNot Nothing Then
        Dim selectedFont As String = fontComboBox.Text.ToString()
        
        ' Handle font change
        MessageBox.Show($"Font changed to: {selectedFont}")
    End If
End Sub
```

### Lambda Expression (C# Only)

```csharp
// Inline event handler
fontComboBox.SelectedIndexChanged += (sender, e) =>
{
    if (fontComboBox.Text != null)
    {
        Console.WriteLine($"Selected: {fontComboBox.Text}");
    }
};
```

---

## Applying Selected Fonts

Common scenarios for using the selected font to update other controls.

### Apply to Label

**C# Example:**
```csharp
private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    if (fontComboBox.Text != null)
    {
        // Create font with selected family, size 11, regular style
        label1.Font = new Font(
            fontComboBox.Text.ToString(), 
            11, 
            FontStyle.Regular
        );
    }
}
```

**VB.NET Example:**
```vb
Private Sub FontComboBox_SelectedIndexChanged(sender As Object, e As EventArgs)
    If fontComboBox.Text IsNot Nothing Then
        ' Create font with selected family, size 11, regular style
        label1.Font = New Font(
            fontComboBox.Text.ToString(), 
            11, 
            FontStyle.Regular
        )
    End If
End Sub
```

---

### Apply to TextBox

**C# Example:**
```csharp
private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    if (fontComboBox.Text != null)
    {
        textBox1.Font = new Font(
            fontComboBox.Text.ToString(),
            textBox1.Font.Size, // Keep current size
            textBox1.Font.Style // Keep current style
        );
    }
}
```

---

### Apply to RichTextBox (Selected Text)

**C# Example:**
```csharp
private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    if (fontComboBox.Text != null && richTextBox1.SelectionLength > 0)
    {
        // Apply to selected text only
        richTextBox1.SelectionFont = new Font(
            fontComboBox.Text.ToString(),
            richTextBox1.SelectionFont.Size,
            richTextBox1.SelectionFont.Style
        );
    }
}
```

---

### Apply to Multiple Controls

**C# Example:**
```csharp
private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    if (fontComboBox.Text == null) return;
    
    string fontName = fontComboBox.Text.ToString();
    
    // Apply to multiple labels
    label1.Font = new Font(fontName, 10, FontStyle.Regular);
    label2.Font = new Font(fontName, 12, FontStyle.Bold);
    label3.Font = new Font(fontName, 14, FontStyle.Italic);
    
    // Apply to textbox
    textBox1.Font = new Font(fontName, 11, FontStyle.Regular);
}
```

---

### With Font Size Selector

**C# Example:**
```csharp
private void ApplyFont()
{
    if (fontComboBox.Text == null) return;
    
    string fontName = fontComboBox.Text.ToString();
    float fontSize = float.Parse(fontSizeComboBox.SelectedItem.ToString());
    
    textBox1.Font = new Font(fontName, fontSize, FontStyle.Regular);
}

private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    ApplyFont();
}

private void FontSizeComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    ApplyFont();
}
```

---

## Custom FontSelected Event

Create a custom event for more specific font selection handling.

### Why Custom Event?

- **Separation of concerns**: Differentiate from general SelectedIndexChanged
- **Type-safe**: Pass font-specific information
- **Cleaner code**: Custom event args with font details
- **Extensibility**: Add custom logic without modifying base behavior

### Implementation

#### Step 1: Create Derived Class

**C# Example:**
```csharp
using System;
using Syncfusion.Windows.Forms.Tools;

public class FontComboBoxExtended : FontComboBox
{
    // Define custom event
    public event EventHandler FontSelected;
    
    // Override SelectedIndexChanged to trigger custom event
    protected override void OnSelectedIndexChanged(EventArgs e)
    {
        // Fire custom FontSelected event
        FontSelected?.Invoke(this, e);
        
        // Call base implementation
        base.OnSelectedIndexChanged(e);
    }
}
```

**VB.NET Example:**
```vb
Imports System
Imports Syncfusion.Windows.Forms.Tools

Public Class FontComboBoxExtended
    Inherits FontComboBox
    
    ' Define custom event
    Public Event FontSelected As EventHandler
    
    ' Override SelectedIndexChanged to trigger custom event
    Protected Overrides Sub OnSelectedIndexChanged(e As EventArgs)
        ' Fire custom FontSelected event
        RaiseEvent FontSelected(Me, e)
        
        ' Call base implementation
        MyBase.OnSelectedIndexChanged(e)
    End Sub
End Class
```

#### Step 2: Use Custom Control

**C# Example:**
```csharp
// In form class
private FontComboBoxExtended fontComboBoxExtended;

private void InitializeControls()
{
    fontComboBoxExtended = new FontComboBoxExtended
    {
        Location = new Point(20, 20),
        Size = new Size(200, 25),
        UseAutoComplete = true
    };
    
    // Subscribe to custom event
    fontComboBoxExtended.FontSelected += FontComboBoxExtended_FontSelected;
    
    this.Controls.Add(fontComboBoxExtended);
}

private void FontComboBoxExtended_FontSelected(object sender, EventArgs e)
{
    var comboBox = sender as FontComboBoxExtended;
    
    if (comboBox?.SelectedItem != null)
    {
        MessageBox.Show($"Font selected: {comboBox.SelectedItem}");
    }
}
```

---

### Advanced: Custom Event Args

Create custom event arguments with font information.

**C# Example:**
```csharp
// Custom event args
public class FontSelectedEventArgs : EventArgs
{
    public string FontName { get; set; }
    public Font SelectedFont { get; set; }
    public int SelectedIndex { get; set; }
}

// Extended control with custom event args
public class FontComboBoxAdvanced : FontComboBox
{
    public event EventHandler<FontSelectedEventArgs> FontSelected;
    
    protected override void OnSelectedIndexChanged(EventArgs e)
    {
        if (SelectedItem != null && FontSelected != null)
        {
            var args = new FontSelectedEventArgs
            {
                FontName = SelectedItem.ToString(),
                SelectedFont = new Font(SelectedItem.ToString(), 10),
                SelectedIndex = SelectedIndex
            };
            
            FontSelected(this, args);
        }
        
        base.OnSelectedIndexChanged(e);
    }
}

// Usage
private void FontComboBoxAdvanced_FontSelected(object sender, FontSelectedEventArgs e)
{
    label1.Font = e.SelectedFont;
    labelInfo.Text = $"Selected: {e.FontName} (Index: {e.SelectedIndex})";
}
```

---

## Complete Examples

### Example 1: Font Formatter with Preview

```csharp
public partial class FontFormatterForm : Form
{
    private FontComboBox fontComboBox;
    private Label previewLabel;
    
    public FontFormatterForm()
    {
        InitializeComponent();
        InitializeControls();
    }
    
    private void InitializeControls()
    {
        // Create FontComboBox
        fontComboBox = new FontComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(250, 25),
            UseAutoComplete = true,
            DropDownStyle = ComboBoxStyle.DropDownList,
            Sorted = true,
            SelectedItem = "Arial"
        };
        
        // Create preview label
        previewLabel = new Label
        {
            Location = new Point(20, 60),
            Size = new Size(400, 50),
            Text = "The quick brown fox jumps over the lazy dog",
            Font = new Font("Arial", 12),
            BorderStyle = BorderStyle.FixedSingle,
            AutoSize = false
        };
        
        // Subscribe to event
        fontComboBox.SelectedIndexChanged += FontComboBox_SelectedIndexChanged;
        
        // Add to form
        this.Controls.Add(fontComboBox);
        this.Controls.Add(previewLabel);
    }
    
    private void FontComboBox_SelectedIndexChanged(object sender, EventArgs e)
    {
        if (fontComboBox.Text != null)
        {
            try
            {
                previewLabel.Font = new Font(
                    fontComboBox.Text.ToString(),
                    12,
                    FontStyle.Regular
                );
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Error applying font: {ex.Message}");
            }
        }
    }
}
```

---

### Example 2: Font Preferences Dialog

```csharp
public partial class FontPreferencesDialog : Form
{
    private FontComboBox fontComboBox;
    private Button btnApply;
    private Button btnCancel;
    
    public string SelectedFont { get; private set; }
    
    public FontPreferencesDialog(string currentFont)
    {
        InitializeComponent();
        InitializeControls();
        
        // Set current font
        fontComboBox.Text = currentFont;
    }
    
    private void InitializeControls()
    {
        // FontComboBox
        fontComboBox = new FontComboBox
        {
            Location = new Point(20, 20),
            Size = new Size(250, 25),
            UseAutoComplete = true,
            DropDownStyle = ComboBoxStyle.DropDownList,
            Sorted = true
        };
        
        // Apply button
        btnApply = new Button
        {
            Text = "Apply",
            Location = new Point(100, 60),
            DialogResult = DialogResult.OK
        };
        btnApply.Click += BtnApply_Click;
        
        // Cancel button
        btnCancel = new Button
        {
            Text = "Cancel",
            Location = new Point(180, 60),
            DialogResult = DialogResult.Cancel
        };
        
        this.Controls.AddRange(new Control[] { fontComboBox, btnApply, btnCancel });
    }
    
    private void BtnApply_Click(object sender, EventArgs e)
    {
        if (fontComboBox.Text != null)
        {
            SelectedFont = fontComboBox.Text.ToString();
        }
    }
}

// Usage
private void ShowFontDialog()
{
    using (var dialog = new FontPreferencesDialog(label1.Font.Name))
    {
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            label1.Font = new Font(dialog.SelectedFont, label1.Font.Size);
        }
    }
}
```

---

## Best Practices

### 1. Always Null-Check SelectedItem

```csharp
if (fontComboBox.Text != null)
{
    // Safe to use SelectedItem
}
```

### 2. Handle Font Creation Exceptions

```csharp
try
{
    label.Font = new Font(fontComboBox.Text.ToString(), 12);
}
catch (ArgumentException ex)
{
    MessageBox.Show($"Invalid font: {ex.Message}");
}
```

### 3. Preserve Existing Font Properties

```csharp
// Keep current size and style when changing family
textBox.Font = new Font(
    fontComboBox.Text.ToString(),
    textBox.Font.Size,      // Preserve size
    textBox.Font.Style      // Preserve style
);
```

### 4. Unsubscribe from Events

```csharp
// In Dispose or cleanup
fontComboBox.SelectedIndexChanged -= FontComboBox_SelectedIndexChanged;
```

---

## Troubleshooting

### Event Fires on Initialization

**Problem:** SelectedIndexChanged fires when setting initial value.

**Solution:** Use flag or set selection before subscribing.

```csharp
fontComboBox.Text = "Arial"; // Set before subscribing
fontComboBox.SelectedIndexChanged += Handler; // Subscribe after
```

### Font Not Applied

**Check:**
1. SelectedItem is not null
2. Font name is valid
3. Font is installed on system
4. No exceptions thrown (use try-catch)

---

## Related Topics

- **AutoComplete Features**: Enable font search → [autocomplete.md](autocomplete.md)
- **DropDown Configuration**: Customize selection UI → [dropdown-configuration.md](dropdown-configuration.md)
- **Visual Styles**: Theme the control → [visual-styles.md](visual-styles.md)
