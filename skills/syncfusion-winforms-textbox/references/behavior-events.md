# Behavior and Events

## Table of Contents
- [Behavior Properties](#behavior-properties)
  - [MaxLength](#maxlength)
  - [ReadOnly](#readonly)
- [Layout Settings](#layout-settings)
  - [MaximumSize](#maximumsize)
  - [MinimumSize](#minimumsize)
- [TextBoxExt Events](#textboxext-events)
  - [Border3DStyleChanged](#border3dstylechanged)
  - [BorderColorChanged](#bordercolorchanged)
  - [BorderSidesChanged](#bordersideschanged)
  - [BorderStyleChanged](#borderstylechanged)
  - [CharacterCasingChanged](#charactercasingchanged)
  - [HideSelectionChanged](#hideselectionchanged)
  - [MaximumSizeChanged](#maximumsizechanged)
  - [MinimumSizeChanged](#minimumsizechanged)
  - [MultilineChanged](#multilinechanged)
  - [ReadOnlyChanged](#readonlychanged)
  - [TextAlignChanged](#textalignchanged)
  - [ThemesEnabledChanged](#themesenabledchanged)
- [Practical Examples](#practical-examples)

## Behavior Properties

Behavior properties control how users interact with the TextBoxExt control.

### MaxLength

The `MaxLength` property specifies the maximum number of characters that can be entered.

**Property Details:**
- Type: `int`
- Default Value: `32767`
- Range: 0 to Int32.MaxValue

```csharp
// Limit to 4 characters
textBoxExt1.MaxLength = 4;
```

![Specify the maximum number character entered into WF TextBoxExt](../../../../../docs/Behavior-Settings-images/wf-textboxext-maxlength.png)

**Common Use Cases:**
- Product code: `MaxLength = 10`
- ZIP code: `MaxLength = 5`
- Phone number: `MaxLength = 15` (includes formatting)
- Username: `MaxLength = 20`
- No limit: `MaxLength = 32767` (default)

**Validation Example:**
```csharp
textBoxExt1.MaxLength = 50;

textBoxExt1.TextChanged += (s, e) => {
    int remaining = textBoxExt1.MaxLength - textBoxExt1.Text.Length;
    characterCountLabel.Text = $"{textBoxExt1.Text.Length} / {textBoxExt1.MaxLength} characters";
    
    if (remaining <= 10)
    {
        characterCountLabel.ForeColor = Color.Red;
    }
    else
    {
        characterCountLabel.ForeColor = Color.Black;
    }
};
```

### ReadOnly

The `ReadOnly` property determines whether the text can be edited by the user.

**Property Details:**
- Type: `bool`
- Default Value: `false`

```csharp
// Make textbox readonly
textBoxExt1.ReadOnly = true;
```

![Specify whether the text changed or not in WF TextBoxExt](../../../../../docs/Behavior-Settings-images/wf-textboxext-readonly.png)

**Characteristics:**
- Text can be selected and copied
- User cannot type or paste
- Programmatic changes still allowed
- Focus can still be received
- Background typically shows as disabled color

**Common Use Cases:**

Display calculated values, status information, or toggle based on permissions:
```csharp
private void UpdateEditability()
{
    // Allow editing only if user has permission
    addressBox.ReadOnly = !currentUser.CanEditAddress;
    
    if (addressBox.ReadOnly)
    {
        addressBox.BackColor = Color.LightGray;
    }
    else
    {
        addressBox.BackColor = Color.White;
    }
}
```

## Layout Settings

Layout properties control the size constraints of the TextBoxExt control.

### MaximumSize

The `MaximumSize` property sets the maximum dimensions of the control.

**Property Details:**
- Type: `Size`
- Default Value: `Size(0, 0)` (no limit)

```csharp
using System.Drawing;

// Set maximum size (400x25 pixels)
textBoxExt1.MaximumSize = new Size(400, 25);
```

**Use Cases:** Prevent excessive width, fixed height for single-line textboxes.

### MinimumSize

The `MinimumSize` property sets the minimum dimensions of the control.

**Property Details:**
- Type: `Size`
- Default Value: `Size(0, 0)` (no minimum)

```csharp
using System.Drawing;

// Set minimum size (150x20 pixels)
textBoxExt1.MinimumSize = new Size(150, 20);
```

**Use Cases:** Ensure minimum visibility, create fixed size controls by combining with `MaximumSize`.

**Layout Example:**
```csharp
using System.Drawing;
using System.Windows.Forms;

TextBoxExt responsiveBox = new TextBoxExt();
responsiveBox.Location = new Point(20, 20);
responsiveBox.MinimumSize = new Size(150, 25);
responsiveBox.MaximumSize = new Size(500, 25);
responsiveBox.Size = new Size(300, 25);
responsiveBox.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;

// Control will resize with form between 150px and 500px width
```

![Change the layout of WF TextBoxExt control](../../../../../docs/Layout-Settings_images/Layout-Settings_img1.png)

## TextBoxExt Events

TextBoxExt provides twelve property-changed events for detecting and responding to configuration changes.

### Border3DStyleChanged

Occurs when the `Border3DStyle` property is changed.

**Event Signature:**
```csharp
public event EventHandler Border3DStyleChanged
```

```csharp
textBoxExt1.Border3DStyleChanged += textBoxExt1_Border3DStyleChanged;

private void textBoxExt1_Border3DStyleChanged(object sender, EventArgs e)
{
    Console.WriteLine("Border3DStyle changed to: " + textBoxExt1.Border3DStyle);
}
```

**Use Case - Style synchronization:**
```csharp
// Keep multiple textboxes synchronized
private void primaryBox_Border3DStyleChanged(object sender, EventArgs e)
{
    secondaryBox.Border3DStyle = primaryBox.Border3DStyle;
    tertiaryBox.Border3DStyle = primaryBox.Border3DStyle;
}
```

### BorderColorChanged

Occurs when the `BorderColor` property is changed.

**Event Signature:**
```csharp
public event EventHandler BorderColorChanged
```

```csharp
textBoxExt1.BorderColorChanged += textBoxExt1_BorderColorChanged;

private void textBoxExt1_BorderColorChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderColor changed to: " + textBoxExt1.BorderColor.Name);
}
```

**Use Case - Log color changes:**
```csharp
private void textBoxExt1_BorderColorChanged(object sender, EventArgs e)
{
    LogColorChange("BorderColor", textBoxExt1.BorderColor);
}

private void LogColorChange(string property, Color color)
{
    Debug.WriteLine($"[{DateTime.Now:HH:mm:ss}] {property} changed to: " +
                   $"R={color.R}, G={color.G}, B={color.B}");
}
```

### BorderSidesChanged

Occurs when the `BorderSides` property is changed.

**Event Signature:**
```csharp
public event EventHandler BorderSidesChanged
```

```csharp
textBoxExt1.BorderSidesChanged += textBoxExt1_BorderSidesChanged;

private void textBoxExt1_BorderSidesChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderSides changed to: " + textBoxExt1.BorderSides);
}
```

### BorderStyleChanged

Occurs when the `BorderStyle` property is changed.

**Event Signature:**
```csharp
public event EventHandler BorderStyleChanged
```

```csharp
textBoxExt1.BorderStyleChanged += textBoxExt1_BorderStyleChanged;

private void textBoxExt1_BorderStyleChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderStyle changed to: " + textBoxExt1.BorderStyle);
}
```

### CharacterCasingChanged

Occurs when the `CharacterCasing` property is changed.

**Event Signature:**
```csharp
public event EventHandler CharacterCasingChanged
```

```csharp
textBoxExt1.CharacterCasingChanged += textBoxExt1_CharacterCasingChanged;

private void textBoxExt1_CharacterCasingChanged(object sender, EventArgs e)
{
    Console.WriteLine("CharacterCasing changed to: " + textBoxExt1.CharacterCasing);
}
```

### HideSelectionChanged

Occurs when the `HideSelection` property is changed.

**Event Signature:**
```csharp
public event EventHandler HideSelectionChanged
```

```csharp
textBoxExt1.HideSelectionChanged += textBoxExt1_HideSelectionChanged;

private void textBoxExt1_HideSelectionChanged(object sender, EventArgs e)
{
    Console.WriteLine("HideSelection changed to: " + textBoxExt1.HideSelection);
}
```

### MaximumSizeChanged

Occurs when the `MaximumSize` property is changed.

**Event Signature:**
```csharp
public event EventHandler MaximumSizeChanged
```

```csharp
textBoxExt1.MaximumSizeChanged += textBoxExt1_MaximumSizeChanged;

private void textBoxExt1_MaximumSizeChanged(object sender, EventArgs e)
{
    Console.WriteLine($"MaximumSize changed to: {textBoxExt1.MaximumSize.Width}x{textBoxExt1.MaximumSize.Height}");
}
```

### MinimumSizeChanged

Occurs when the `MinimumSize` property is changed.

**Event Signature:**
```csharp
public event EventHandler MinimumSizeChanged
```

```csharp
textBoxExt1.MinimumSizeChanged += textBoxExt1_MinimumSizeChanged;

private void textBoxExt1_MinimumSizeChanged(object sender, EventArgs e)
{
    Console.WriteLine($"MinimumSize changed to: {textBoxExt1.MinimumSize.Width}x{textBoxExt1.MinimumSize.Height}");
}
```

### MultilineChanged

Occurs when the `Multiline` property is changed.

**Event Signature:**
```csharp
public event EventHandler MultilineChanged
```

```csharp
textBoxExt1.MultilineChanged += textBoxExt1_MultilineChanged;

private void textBoxExt1_MultilineChanged(object sender, EventArgs e)
{
    Console.WriteLine("Multiline changed to: " + textBoxExt1.Multiline);
}
```

### ReadOnlyChanged

Occurs when the `ReadOnly` property is changed.

**Event Signature:**
```csharp
public event EventHandler ReadOnlyChanged
```

```csharp
textBoxExt1.ReadOnlyChanged += textBoxExt1_ReadOnlyChanged;

private void textBoxExt1_ReadOnlyChanged(object sender, EventArgs e)
{
    Console.WriteLine("ReadOnly changed to: " + textBoxExt1.ReadOnly);
}
```

### TextAlignChanged

Occurs when the `TextAlign` property is changed.

**Event Signature:**
```csharp
public event EventHandler TextAlignChanged
```

```csharp
textBoxExt1.TextAlignChanged += textBoxExt1_TextAlignChanged;

private void textBoxExt1_TextAlignChanged(object sender, EventArgs e)
{
    Console.WriteLine("TextAlign changed to: " + textBoxExt1.TextAlign);
}
```

### ThemesEnabledChanged

Occurs when the `ThemesEnabled` property is changed.

**Event Signature:**
```csharp
public event EventHandler ThemesEnabledChanged
```

```csharp
textBoxExt1.ThemesEnabledChanged += textBoxExt1_ThemesEnabledChanged;

private void textBoxExt1_ThemesEnabledChanged(object sender, EventArgs e)
{
    Console.WriteLine("ThemesEnabled changed to: " + textBoxExt1.ThemesEnabled);
}
```

## Practical Examples

### Example: Edit Mode Toggle

```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Drawing;
using System.Windows.Forms;

public class EditModeManager
{
    private TextBoxExt textBox;
    private Button editButton;
    private bool isEditing = false;
    
    public EditModeManager(TextBoxExt textBox, Button editButton)
    {
        this.textBox = textBox;
        this.editButton = editButton;
        
        // Initialize as readonly
        textBox.ReadOnly = true;
        textBox.ReadOnlyChanged += OnReadOnlyChanged;
        
        // Setup button
        editButton.Text = "Edit";
        editButton.Click += ToggleEditMode;
        
        OnReadOnlyChanged(null, null);
    }
    
    private void ToggleEditMode(object sender, EventArgs e)
    {
        isEditing = !isEditing;
        textBox.ReadOnly = !isEditing;
        editButton.Text = isEditing ? "Save" : "Edit";
        
        if (!isEditing)
        {
            // Save changes
            SaveChanges();
        }
    }
    
    private void OnReadOnlyChanged(object sender, EventArgs e)
    {
        if (textBox.ReadOnly)
        {
            textBox.BackColor = Color.FromArgb(245, 245, 245);
            textBox.ForeColor = Color.Black;
            textBox.BorderColor = Color.LightGray;
        }
        else
        {
            textBox.BackColor = Color.White;
            textBox.ForeColor = Color.Black;
            textBox.BorderColor = Color.FromArgb(0, 120, 215);
            textBox.Focus();
        }
    }
    
    private void SaveChanges()
    {
        // Implement save logic
        MessageBox.Show("Changes saved!", "Success", MessageBoxButtons.OK, MessageBoxIcon.Information);
    }
}
```

## Summary

TextBoxExt behavior and events provide:

- **MaxLength** property for input length restrictions (default: 32767)
- **ReadOnly** property for non-editable display fields
- **MaximumSize** and **MinimumSize** for layout constraints
- **Twelve property-changed events** for detecting configuration changes
- Event-driven validation and UI updates
- Dynamic appearance adjustments based on state
- Responsive layout capabilities
- Edit mode toggling patterns

These features enable sophisticated input validation, user feedback, and dynamic UI behavior in your Windows Forms applications.
