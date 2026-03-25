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

**C#:**
```csharp
// Limit to 4 characters
textBoxExt1.MaxLength = 4;
```

**VB.NET:**
```vb
' Limit to 4 characters
textBoxExt1.MaxLength = 4
```

![Specify the maximum number character entered into WF TextBoxExt](../../../../../docs/Behavior-Settings-images/wf-textboxext-maxlength.png)

**Common Use Cases:**

**Product code (10 characters):**
```csharp
productCodeBox.MaxLength = 10;
productCodeBox.CharacterCasing = System.Windows.Forms.CharacterCasing.Upper;
```

**ZIP code (5 digits):**
```csharp
zipCodeBox.MaxLength = 5;
```

**Phone number (15 characters with formatting):**
```csharp
phoneBox.MaxLength = 15; // Includes dashes/spaces
```

**Username (20 characters):**
```csharp
usernameBox.MaxLength = 20;
```

**No limit (default):**
```csharp
textBoxExt1.MaxLength = 32767; // Maximum allowed
```

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

**C#:**
```csharp
// Make textbox readonly
textBoxExt1.ReadOnly = true;
```

**VB.NET:**
```vb
' Make textbox readonly
textBoxExt1.ReadOnly = True
```

![Specify whether the text changed or not in WF TextBoxExt](../../../../../docs/Behavior-Settings-images/wf-textboxext-readonly.png)

**Characteristics:**
- Text can be selected and copied
- User cannot type or paste
- Programmatic changes still allowed
- Focus can still be received
- Background typically shows as disabled color

**Common Use Cases:**

**Display calculated value:**
```csharp
totalBox.ReadOnly = true;
totalBox.Text = "$1,234.56";
totalBox.BackColor = Color.LightGray;
```

**Show status information:**
```csharp
statusBox.ReadOnly = true;
statusBox.Text = "Status: Connected";
statusBox.ForeColor = Color.Green;
```

**Readonly with active appearance:**
```csharp
displayBox.ReadOnly = true;
displayBox.DrawActiveWhenDisabled = true;
displayBox.BackColor = Color.White; // Keep white instead of gray
```

**Toggle readonly based on condition:**
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

**C#:**
```csharp
using System.Drawing;

// Set maximum size (400x25 pixels)
textBoxExt1.MaximumSize = new Size(400, 25);
```

**VB.NET:**
```vb
Imports System.Drawing

' Set maximum size (400x25 pixels)
textBoxExt1.MaximumSize = New Size(400, 25)
```

**Use Cases:**

**Prevent excessive width:**
```csharp
searchBox.MaximumSize = new Size(500, 30);
searchBox.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;
// Control will grow with form but stop at 500px width
```

**Fixed height for single-line:**
```csharp
singleLineBox.MaximumSize = new Size(0, 25); // Only height limited
```

### MinimumSize

The `MinimumSize` property sets the minimum dimensions of the control.

**Property Details:**
- Type: `Size`
- Default Value: `Size(0, 0)` (no minimum)

**C#:**
```csharp
using System.Drawing;

// Set minimum size (150x20 pixels)
textBoxExt1.MinimumSize = new Size(150, 20);
```

**VB.NET:**
```vb
Imports System.Drawing

' Set minimum size (150x20 pixels)
textBoxExt1.MinimumSize = New Size(150, 20)
```

**Use Cases:**

**Ensure minimum visibility:**
```csharp
importantField.MinimumSize = new Size(200, 25);
// Control never shrinks below 200px wide
```

**Fixed size control:**
```csharp
// Combine minimum and maximum for fixed size
fixedBox.MinimumSize = new Size(300, 25);
fixedBox.MaximumSize = new Size(300, 25);
// Control always stays 300x25
```

**Complete Layout Example:**
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

**C# Example:**
```csharp
textBoxExt1.Border3DStyleChanged += textBoxExt1_Border3DStyleChanged;

private void textBoxExt1_Border3DStyleChanged(object sender, EventArgs e)
{
    Console.WriteLine("Border3DStyle changed to: " + textBoxExt1.Border3DStyle);
    // Update related UI elements
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_Border3DStyleChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("Border3DStyle changed to: " & textBoxExt1.Border3DStyle.ToString())
End Sub
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

**C# Example:**
```csharp
textBoxExt1.BorderColorChanged += textBoxExt1_BorderColorChanged;

private void textBoxExt1_BorderColorChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderColor changed to: " + textBoxExt1.BorderColor.Name);
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_BorderColorChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("BorderColor changed to: " & textBoxExt1.BorderColor.Name)
End Sub
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

**C# Example:**
```csharp
textBoxExt1.BorderSidesChanged += textBoxExt1_BorderSidesChanged;

private void textBoxExt1_BorderSidesChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderSides changed to: " + textBoxExt1.BorderSides);
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_BorderSidesChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("BorderSides changed to: " & textBoxExt1.BorderSides.ToString())
End Sub
```

### BorderStyleChanged

Occurs when the `BorderStyle` property is changed.

**Event Signature:**
```csharp
public event EventHandler BorderStyleChanged
```

**C# Example:**
```csharp
textBoxExt1.BorderStyleChanged += textBoxExt1_BorderStyleChanged;

private void textBoxExt1_BorderStyleChanged(object sender, EventArgs e)
{
    Console.WriteLine("BorderStyle changed to: " + textBoxExt1.BorderStyle);
    
    // Adjust related properties based on border style
    if (textBoxExt1.BorderStyle == System.Windows.Forms.BorderStyle.None)
    {
        textBoxExt1.BackColor = Color.Transparent;
    }
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_BorderStyleChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("BorderStyle changed to: " & textBoxExt1.BorderStyle.ToString())
End Sub
```

### CharacterCasingChanged

Occurs when the `CharacterCasing` property is changed.

**Event Signature:**
```csharp
public event EventHandler CharacterCasingChanged
```

**C# Example:**
```csharp
textBoxExt1.CharacterCasingChanged += textBoxExt1_CharacterCasingChanged;

private void textBoxExt1_CharacterCasingChanged(object sender, EventArgs e)
{
    Console.WriteLine("CharacterCasing changed to: " + textBoxExt1.CharacterCasing);
    
    // Update instruction label
    switch (textBoxExt1.CharacterCasing)
    {
        case System.Windows.Forms.CharacterCasing.Upper:
            instructionLabel.Text = "Text will be converted to UPPERCASE";
            break;
        case System.Windows.Forms.CharacterCasing.Lower:
            instructionLabel.Text = "Text will be converted to lowercase";
            break;
        case System.Windows.Forms.CharacterCasing.Normal:
            instructionLabel.Text = "Text will be entered as typed";
            break;
    }
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_CharacterCasingChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("CharacterCasing changed to: " & textBoxExt1.CharacterCasing.ToString())
End Sub
```

### HideSelectionChanged

Occurs when the `HideSelection` property is changed.

**Event Signature:**
```csharp
public event EventHandler HideSelectionChanged
```

**C# Example:**
```csharp
textBoxExt1.HideSelectionChanged += textBoxExt1_HideSelectionChanged;

private void textBoxExt1_HideSelectionChanged(object sender, EventArgs e)
{
    Console.WriteLine("HideSelection changed to: " + textBoxExt1.HideSelection);
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_HideSelectionChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("HideSelection changed to: " & textBoxExt1.HideSelection.ToString())
End Sub
```

### MaximumSizeChanged

Occurs when the `MaximumSize` property is changed.

**Event Signature:**
```csharp
public event EventHandler MaximumSizeChanged
```

**C# Example:**
```csharp
textBoxExt1.MaximumSizeChanged += textBoxExt1_MaximumSizeChanged;

private void textBoxExt1_MaximumSizeChanged(object sender, EventArgs e)
{
    Console.WriteLine($"MaximumSize changed to: {textBoxExt1.MaximumSize.Width}x{textBoxExt1.MaximumSize.Height}");
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_MaximumSizeChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine($"MaximumSize changed to: {textBoxExt1.MaximumSize.Width}x{textBoxExt1.MaximumSize.Height}")
End Sub
```

### MinimumSizeChanged

Occurs when the `MinimumSize` property is changed.

**Event Signature:**
```csharp
public event EventHandler MinimumSizeChanged
```

**C# Example:**
```csharp
textBoxExt1.MinimumSizeChanged += textBoxExt1_MinimumSizeChanged;

private void textBoxExt1_MinimumSizeChanged(object sender, EventArgs e)
{
    Console.WriteLine($"MinimumSize changed to: {textBoxExt1.MinimumSize.Width}x{textBoxExt1.MinimumSize.Height}");
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_MinimumSizeChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine($"MinimumSize changed to: {textBoxExt1.MinimumSize.Width}x{textBoxExt1.MinimumSize.Height}")
End Sub
```

### MultilineChanged

Occurs when the `Multiline` property is changed.

**Event Signature:**
```csharp
public event EventHandler MultilineChanged
```

**C# Example:**
```csharp
textBoxExt1.MultilineChanged += textBoxExt1_MultilineChanged;

private void textBoxExt1_MultilineChanged(object sender, EventArgs e)
{
    Console.WriteLine("Multiline changed to: " + textBoxExt1.Multiline);
    
    // Adjust size when switching to/from multiline
    if (textBoxExt1.Multiline)
    {
        textBoxExt1.Size = new System.Drawing.Size(300, 100);
        textBoxExt1.ScrollBars = System.Windows.Forms.ScrollBars.Vertical;
    }
    else
    {
        textBoxExt1.Size = new System.Drawing.Size(300, 25);
        textBoxExt1.ScrollBars = System.Windows.Forms.ScrollBars.None;
    }
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_MultilineChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("Multiline changed to: " & textBoxExt1.Multiline.ToString())
End Sub
```

### ReadOnlyChanged

Occurs when the `ReadOnly` property is changed.

**Event Signature:**
```csharp
public event EventHandler ReadOnlyChanged
```

**C# Example:**
```csharp
textBoxExt1.ReadOnlyChanged += textBoxExt1_ReadOnlyChanged;

private void textBoxExt1_ReadOnlyChanged(object sender, EventArgs e)
{
    Console.WriteLine("ReadOnly changed to: " + textBoxExt1.ReadOnly);
    
    // Update appearance based on readonly state
    if (textBoxExt1.ReadOnly)
    {
        textBoxExt1.BackColor = Color.LightGray;
        textBoxExt1.ForeColor = Color.DarkGray;
        editIcon.Visible = false;
    }
    else
    {
        textBoxExt1.BackColor = Color.White;
        textBoxExt1.ForeColor = Color.Black;
        editIcon.Visible = true;
    }
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_ReadOnlyChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("ReadOnly changed to: " & textBoxExt1.ReadOnly.ToString())
End Sub
```

### TextAlignChanged

Occurs when the `TextAlign` property is changed.

**Event Signature:**
```csharp
public event EventHandler TextAlignChanged
```

**C# Example:**
```csharp
textBoxExt1.TextAlignChanged += textBoxExt1_TextAlignChanged;

private void textBoxExt1_TextAlignChanged(object sender, EventArgs e)
{
    Console.WriteLine("TextAlign changed to: " + textBoxExt1.TextAlign);
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_TextAlignChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("TextAlign changed to: " & textBoxExt1.TextAlign.ToString())
End Sub
```

### ThemesEnabledChanged

Occurs when the `ThemesEnabled` property is changed.

**Event Signature:**
```csharp
public event EventHandler ThemesEnabledChanged
```

**C# Example:**
```csharp
textBoxExt1.ThemesEnabledChanged += textBoxExt1_ThemesEnabledChanged;

private void textBoxExt1_ThemesEnabledChanged(object sender, EventArgs e)
{
    Console.WriteLine("ThemesEnabled changed to: " + textBoxExt1.ThemesEnabled);
}
```

**VB.NET Example:**
```vb
Private Sub textBoxExt1_ThemesEnabledChanged(ByVal sender As Object, ByVal e As EventArgs)
    Console.WriteLine("ThemesEnabled changed to: " & textBoxExt1.ThemesEnabled.ToString())
End Sub
```

## Practical Examples

### Example 1: Dynamic Validation with Events

```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Drawing;
using System.Windows.Forms;

public class ValidatingTextBox
{
    private TextBoxExt textBox;
    private Label errorLabel;
    
    public ValidatingTextBox(TextBoxExt textBox, Label errorLabel)
    {
        this.textBox = textBox;
        this.errorLabel = errorLabel;
        
        // Subscribe to events
        textBox.TextChanged += ValidateInput;
        textBox.ReadOnlyChanged += UpdateAppearance;
        textBox.CharacterCasingChanged += UpdateInstructions;
    }
    
    private void ValidateInput(object sender, EventArgs e)
    {
        if (string.IsNullOrWhiteSpace(textBox.Text))
        {
            textBox.BorderColor = Color.Red;
            errorLabel.Text = "This field is required";
            errorLabel.ForeColor = Color.Red;
            errorLabel.Visible = true;
        }
        else if (textBox.Text.Length < 3)
        {
            textBox.BorderColor = Color.Orange;
            errorLabel.Text = "Minimum 3 characters required";
            errorLabel.ForeColor = Color.Orange;
            errorLabel.Visible = true;
        }
        else
        {
            textBox.BorderColor = Color.Green;
            errorLabel.Visible = false;
        }
    }
    
    private void UpdateAppearance(object sender, EventArgs e)
    {
        if (textBox.ReadOnly)
        {
            textBox.BackColor = Color.FromArgb(240, 240, 240);
            errorLabel.Visible = false;
        }
        else
        {
            textBox.BackColor = Color.White;
        }
    }
    
    private void UpdateInstructions(object sender, EventArgs e)
    {
        string instruction = textBox.CharacterCasing switch
        {
            CharacterCasing.Upper => "(Text will be uppercase)",
            CharacterCasing.Lower => "(Text will be lowercase)",
            _ => ""
        };
        
        errorLabel.Text = instruction;
        errorLabel.ForeColor = Color.Gray;
        errorLabel.Visible = !string.IsNullOrEmpty(instruction);
    }
}
```

### Example 2: Character Counter with MaxLength

```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Drawing;
using System.Windows.Forms;

public partial class CharacterCountForm : Form
{
    private TextBoxExt descriptionBox;
    private Label counterLabel;
    
    public CharacterCountForm()
    {
        InitializeComponent();
        SetupCharacterCounter();
    }
    
    private void SetupCharacterCounter()
    {
        descriptionBox = new TextBoxExt();
        descriptionBox.Location = new Point(20, 20);
        descriptionBox.Size = new Size(400, 100);
        descriptionBox.Multiline = true;
        descriptionBox.WordWrap = true;
        descriptionBox.ScrollBars = ScrollBars.Vertical;
        descriptionBox.MaxLength = 500;
        
        counterLabel = new Label();
        counterLabel.Location = new Point(20, 130);
        counterLabel.Size = new Size(400, 20);
        counterLabel.Text = "0 / 500 characters";
        
        // Subscribe to text changed
        descriptionBox.TextChanged += UpdateCharacterCount;
        
        this.Controls.Add(descriptionBox);
        this.Controls.Add(counterLabel);
    }
    
    private void UpdateCharacterCount(object sender, EventArgs e)
    {
        int current = descriptionBox.Text.Length;
        int max = descriptionBox.MaxLength;
        int remaining = max - current;
        
        counterLabel.Text = $"{current} / {max} characters ({remaining} remaining)";
        
        // Color coding
        if (remaining <= 0)
        {
            counterLabel.ForeColor = Color.Red;
            descriptionBox.BorderColor = Color.Red;
        }
        else if (remaining <= 50)
        {
            counterLabel.ForeColor = Color.Orange;
            descriptionBox.BorderColor = Color.Orange;
        }
        else
        {
            counterLabel.ForeColor = Color.Black;
            descriptionBox.BorderColor = Color.Gray;
        }
    }
}
```

### Example 3: Responsive Layout with Size Events

```csharp
using Syncfusion.Windows.Forms.Tools;
using System;
using System.Drawing;
using System.Windows.Forms;

public partial class ResponsiveForm : Form
{
    private TextBoxExt responsiveBox;
    private Label sizeLabel;
    
    public ResponsiveForm()
    {
        InitializeComponent();
        CreateResponsiveTextBox();
    }
    
    private void CreateResponsiveTextBox()
    {
        responsiveBox = new TextBoxExt();
        responsiveBox.Location = new Point(20, 20);
        responsiveBox.Size = new Size(300, 25);
        responsiveBox.MinimumSize = new Size(150, 25);
        responsiveBox.MaximumSize = new Size(600, 25);
        responsiveBox.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;
        
        sizeLabel = new Label();
        sizeLabel.Location = new Point(20, 55);
        sizeLabel.AutoSize = true;
        
        // Subscribe to size events
        responsiveBox.MinimumSizeChanged += UpdateSizeInfo;
        responsiveBox.MaximumSizeChanged += UpdateSizeInfo;
        responsiveBox.Resize += UpdateSizeInfo;
        
        this.Controls.Add(responsiveBox);
        this.Controls.Add(sizeLabel);
        
        UpdateSizeInfo(null, null);
    }
    
    private void UpdateSizeInfo(object sender, EventArgs e)
    {
        sizeLabel.Text = $"Size: {responsiveBox.Width}x{responsiveBox.Height} | " +
                        $"Min: {responsiveBox.MinimumSize.Width}x{responsiveBox.MinimumSize.Height} | " +
                        $"Max: {responsiveBox.MaximumSize.Width}x{responsiveBox.MaximumSize.Height}";
    }
}
```

### Example 4: Edit Mode Toggle

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
