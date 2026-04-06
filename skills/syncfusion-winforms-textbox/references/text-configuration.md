# Text Configuration

This guide covers all text-related properties and features of the TextBoxExt control, including character casing, alignment, multiline settings, overflow indicators, and text manipulation methods.

## Text Property

The `Text` property gets or sets the current text content displayed in the TextBoxExt.

**C#:**
```csharp
// Set text
textBoxExt1.Text = "Hello, World!";

// Get text
string content = textBoxExt1.Text;

// Check if empty
if (string.IsNullOrEmpty(textBoxExt1.Text))
{
    MessageBox.Show("Please enter text");
}
```

![Set the text to WF TextBoxExt](../../../../../docs/Text-Settings_images/wf-textboxext-text.png)

## SelectedText Property

The `SelectedText` property gets or sets the currently selected text within the control.

**C#:**
```csharp
// Get selected text
string selected = textBoxExt1.SelectedText;

// Replace selected text
textBoxExt1.SelectedText = "Replacement Text";

// Insert text at cursor position
textBoxExt1.SelectedText = "Inserted Text";
```

**Note:** You can use `SelectedText` to get the user's selection or programmatically replace selected content.

## Character Casing

The `CharacterCasing` property automatically transforms text to uppercase, lowercase, or leaves it normal.

### CharacterCasing Options

| Value | Description |
|-------|-------------|
| `CharacterCasing.Normal` | No transformation (default) |
| `CharacterCasing.Upper` | Convert all characters to uppercase |
| `CharacterCasing.Lower` | Convert all characters to lowercase |

### Uppercase Transformation

**C#:**
```csharp
using System.Windows.Forms;

// Convert to uppercase automatically
textBoxExt1.CharacterCasing = CharacterCasing.Upper;
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Convert to uppercase automatically
textBoxExt1.CharacterCasing = CharacterCasing.Upper
```

**Result:** When user types "hello", it displays as "HELLO".

![Change the character casing of WF TextBoxExt](../../../../../docs/Text-Settings_images/wf-textboxext-charcasing.png)

##VB.NET:**
```vb
' Convert to lowercase automatically
textBoxExt1.CharacterCasing = CharacterCasing.Lower
```

**Result:** When user types "HELLO", it displays as "hello".

## User enters: abc123 → Displays: ABC123
```

**Email addresses (lowercase):**
```csharp
emailBox.CharacterCasing = CharacterCasing.Lower;
// User enters: User@Example.COM → Displays: user@example.com
```

## Text Alignment

The `TextAlign` property controls horizontal text alignment within the textbox.

### Alignment Options

| Value | Description |
|-------|-------------|
| `HorizontalAlignment.Left` | Align text to the left (default) |
| `HorizontalAlignment.Center` | Center text horizontally |
| `HorizontalAlignment.Right` | Align text to the right |

### Center Alignment

**C#:**
```csharp
using System.Windows.Forms;

// Center the text
textBoxExt1.TextAlign = HorizontalAlignment.Center;
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Center the text
textBoxExt1.TextAlign = HorizontalAlignment.Center
```

![Change the text align for WF TextBoxExt](../../../../../docs/Text-Settings_images/wf-textboxext-align.png)

### Right Alignment

**C#:**
```csharp
```

### Practical Examples

**Currency display:**
```csharp
currencyBox.Text = "$1,234.56";
currencyBox.TextAlign = HorizontalAlignment.Right;
currencyBox.ReadOnly = true;
```
```

## Right-to-Left Support

The `RightToLeft` property enables right-to-left layout for languages like Arabic and Hebrew.

**C#:**
```csharp
using System.Windows.Forms;

// Enable RTL layout
textBoxExt1.RightToLeft = RightToLeft.Yes;
```

**VB.NET:**
```vb
Imports System.Windows.Forms

' Enable RTL layout
textBoxExt1.RightToLeft = RightToLeft.Yes
```

**Result:** Text flows from right to left, and text alignment is automatically adjusted.

![Change the WF TextBoxExt control position from right to left](../../../../../docs/Text-Settings_images/wf-textboxext-rtl.png)

**Complete RTL Example:**
```csharp
//e `DrawActiveWhenDisabled` property determines whether text appears active even when the control is disabled.

**C#:**
```csharp
// Disable control but keep text visible
textBoxExt1.Enabled = false;
textBoxExt1.DrawActiveWhenDisabled = true;
```

**VB.NET:**
```vb
' Disable control but keep text visible
textBoxExt1.Enabled = False
textBoxExt1.DrawActiveWhenDisabled = True
```

**Result:** Text displays with normal colors instead of grayed out appearance.

![Decides the text should be drawn when WF TextBoxExt is disabled](../../../../../docs/Text-Settings_images/Text-Settings_img4.png)

**Use Case - Display-only field:**
```csharp
// Show calculated result in disabled textbox
re
Configure TextBoxExt for multiline text display with word wrapping and scrollbars.

### Multiline Property

**C#:**
```csharp
// Enable multiline mode
textBoxExt1.Multiline = true;
```

**VB.NET:**
```vb
' Enable multiline mode
textBoxExt1.Multiline = True
```

### WordWrap Property

**C#:**
```csharp
// Enable automatic word wrapping
textBoxExt1.Multiline = true;
textBoxExt1.WordWrap = true;
```


**Note:** `WordWrap` only works when `Multiline` is `true`.

### ScrollBars Property

**C#:**
```csharp
usVB.NET:**
```vb
Imports System.Windows.Forms

' Add vertical scrollbar
textBoxExt1.Multiline = True
textBoxExt1.ScrollBars = ScrollBars.Vertical
```

![Show the multiline text in WF TextBoxExt](../../../../../docs/Text-Settings_images/wf-textboxext-multiline.png)

### ScrollBar Options

| Value | Description | When to Use |
|-------|-------------|-------------|
| `ScrollBars.None` | No scrollbars | Short text, fits in visible area |
| `ScrollBars.Vertical` | Vertical scrollbar only | Long text with word wrap |
| `ScrollBars.Horizontal` | Horizontal scrollbar only | Long lines without word wrap |
| `ScrollBars.Both` | Both scrollbars | Large content, no word wrap |


### ScrollToCaret Method

The `ScrollToCaret()` method scrolls the content to make the caret (cursor) visible.

**C#:**
```csharp
// Append text and scroll to show it
textBoxExt1.AppendText("\nNew line added");
textBoxExt1.ScrollToCaret();
```

**VB.NET:**
```vb
' Append text and scroll to show it
textBoxExt1.AppendText(vbCrLf & "New line added")
textBoxExt1.ScrollToCaret()
```

**Use Case - Log viewer:**
```csharp
public void AppendLog(string message)
{
    logTextBox.AppendText($"[{DateTime.Now:HH:mm:ss}] {message}\n");
    logTextBox.ScrollToCaret(); // Auto-scroll to latest entry
}
```

## Overflow Indicators and Tooltips

Display indicators when text content exceeds the visible area.

### ShowOverflowIndicator

**`vb
' Enable overflow indicator
textBoxExt1.ShowOverflowIndicator = True
```

### ShowOverflowIndicatorToolTip

**C#:**
```csharp
// Enable tooltip on overflow indicator
textBoxExt1.ShowOverflowIndicator = true;
textBoxExt1.ShowOverflowIndicatorToolTip = true;
```

**VB.NET:**
```vb
' Enable tooltip on overflow indicator
textBoxExt1.ShowOverflowIndicator = True
textBoxExt1.ShowOverflowIndicatorToolTip = True
```

### OverflowIndicatorToolTipText

**C#:**
```csharp
// Custom tooltip text
textBoxExt1.ShowOverflowIndicator = true;
textBoxExt1.ShowOverflowIndicatorToolTip = true;
textBoxExt1.ShowOverflowIndicator = True
textBoxExt1.ShowOverflowIndicatorToolTip = True
textBoxExt1.OverflowIndicatorToolTipText = "Text content is too long. Resize to view more."
```

![Show the overflow indicator text in WF TextBoxExt](../../../../../docs/Text-Settings_images/Text-Settings_img9.png)

**Default Behavior:** If `OverflowIndicatorToolTipText` is not set, the `Text` property value is displayed in the tooltip.

statusBox.ShowOverflowIndicatorToolTip = true;
statusBox.OverflowIndicatorToolTipText = "Resize textbox to view full message";
```

## Text Manipulation Methods

TextBoxExt provides several methods for programmatic text manipulation.

### AppendText Method


**VB.NET:**
```vb
textBoxExt1.Text = "Hello"
textBoxExt1.AppendText(" World!")
' Result: "Hello World!"
```

### Cut Method

Cuts the selected text to the clipboard.

**C#:**
```csharp
// Cut selected text
textBoxExt1.Cut();
```

**VB.NET:**
```vb
' Cut selected text
textBoxExt1.Cut()
```

### Copy Method

Copies the selected text to the clipboard.

**C#:**
' Copy selected text
textBoxExt1.Copy()
```

### Paste Method

Pastes text from the clipboard into the textbox.

**C#:**
```csharp
// Paste clipboard content
textBoxExt1.Paste();
```

**VB.NET:**
```vb
' Paste clipboard content

**C#:**
```csharp
// Delete selected text
if (textBoxExt1.SelectionLength > 0)
{
    // Implementation note: Use SelectedText = ""
    textBoxExt1.SelectedText = "";
}
```

**VB.NET:**
```vb
' Delete selected text
If textBoxExt1.SelectionLength > 0 Then
    textBoxExt1.SelectedText = ""

**C#:**
```csharp
// Select(start, length)
textBoxExt1.Text = "Hello World";
textBoxExt1.Select(0, 5); // Selects "Hello"
```

**VB.NET:**
### SelectAll Method

Selects all text in the textbox.

**C#:**
```csharp
// Select all text
textBoxExt1.SelectAll();
```

**VB.NET:**
```vb
' Select all text
textBoxExt1.SelectAll()
## Practical Examples

### Example 1: Auto-Uppercase Product Code Entry

```csharp
TextBoxExt productCodeBox = new TextBoxExt();
productCodeBox.CharacterCasing = CharacterCasing.Upper;
productCodeBox.MaxLength = 12;
productCodeBox.TextAlign = HorizontalAlignment.Center;
productCodeBox.Text = "Enter Code";

// Clear placeholder on focus

### Example 2: Multiline Notes with Character Counter

```csharp
TextBoxExt notesBox = new TextBoxExt();
Label charCountLabel = new Label();

notesBox.Multiline = true;
notesBox.WordWrap = true;
```

### Example 3: Right-Aligned Currency Display

```csharp
TextBoxExt priceBox = new TextBoxExt();
priceBox.Text = "$1,234.56";
priceBox.TextAlign = HorizontalAlignment.Right;
priceBox.ReadOnly = true;
priceBox.DrawActiveWhenDisabled = true;
priceBox.BackColor = Color.LightGreen;
```

### Example 4: Text Editor with Overflow Warning

```csharp
TextBoxExt editorBox = new TextBoxExt();
editorBox.Multiline = false;
editorBox.Size = new Size(200, 25);
editorBox.ShowOverflowIndicator = true;
editorBox.ShowOverflowIndicatorToolTip = true;
editorBox.OverflowIndicatorToolTipText = "Text is too long. Use multiline mode or reduce content.";
```

### Example 5: Log Viewer with Auto-Scroll

```csharp
TextBoxExt logViewer = new TextBoxExt();
logViewer.Multiline = true;
logViewer.WordWrap = true;
logViewer.ScrollBars = ScrollBars.Vertical;
logViewer.ReadOnly = true;
logViewer.Size = new Size(600, 400);

// Method to add log entries
void AddLogEntry(string message)
{
    logViewer.AppendText($"[{DateTime.Now:yyyy-MM-dd HH:mm:ss}] {message}\r\n");
    logViewer.ScrollToCaret(); // Auto-scroll to latest
}
```

## Summary

The TextBoxExt control provides comprehensive text configuration options:

- **Character casing** for automatic uppercase/lowercase transformation
- **Text alignment** (left, center, right)
- **RTL support** for international languages
- **Multiline mode** with word wrap and scrollbars
- **Overflow indicators** with customizable tooltips
- **Text manipulation methods** for programmatic control
- **DrawActiveWhenDisabled** for readable disabled fields

These features make TextBoxExt suitable for a wide range of input scenarios, from simple text entry to complex multiline editors.
