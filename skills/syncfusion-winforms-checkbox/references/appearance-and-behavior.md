# Appearance and Behavior Settings

This guide covers visual customization and behavior control options for the CheckBoxAdv control.

## Table of Contents
- [Focus Rectangle](#focus-rectangle)
- [Automatic Height](#automatic-height)
- [Read-Only Mode](#read-only-mode)
- [Gradient Backgrounds](#gradient-backgrounds)
- [Border Styles](#border-styles)
- [Border Colors](#border-colors)
- [Common Patterns](#common-patterns)

## Focus Rectangle

The focus rectangle is a visual indicator that appears when the checkbox receives keyboard focus.

### DrawFocusRectangle Property

```csharp
// Show focus rectangle (default)
checkBoxAdv1.DrawFocusRectangle = true;

// Hide focus rectangle
checkBoxAdv1.DrawFocusRectangle = false;
```

```vb
' Show focus rectangle (default)
checkBoxAdv1.DrawFocusRectangle = True

' Hide focus rectangle
checkBoxAdv1.DrawFocusRectangle = False
```

### When to Use

**Enable focus rectangle (true) when:**
- Form is keyboard-navigable
- Accessibility is important
- Following standard Windows UI conventions

**Disable focus rectangle (false) when:**
- Creating custom visual designs
- Checkbox is display-only
- Focus indication is handled elsewhere

### Example: Clean Visual Design

```csharp
CheckBoxAdv cleanCheckBox = new CheckBoxAdv();
cleanCheckBox.Text = "Clean Design";
cleanCheckBox.DrawFocusRectangle = false;
cleanCheckBox.FlatStyle = FlatStyle.Flat;
```

## Automatic Height

The `AutoHeight` property automatically calculates and sets the control height based on font size and content.

### Enabling AutoHeight

```csharp
// Enable automatic height calculation
checkBoxAdv1.AutoHeight = true;
checkBoxAdv1.Text = "Auto-sized checkbox";
checkBoxAdv1.Font = new Font("Arial", 12);
// Height will be calculated automatically
```

```vb
' Enable automatic height calculation
checkBoxAdv1.AutoHeight = True
checkBoxAdv1.Text = "Auto-sized checkbox"
checkBoxAdv1.Font = New Font("Arial", 12)
```

### When to Use AutoHeight

**Enable AutoHeight (true) when:**
- Font size varies dynamically
- Supporting multiple display DPI settings
- Text size is unknown at design time

**Disable AutoHeight (false) when:**
- Fixed layout is required
- Precise height control is needed
- Using custom-sized checkboxes

### Example: Dynamic Font Sizing

```csharp
CheckBoxAdv dynamicCheckBox = new CheckBoxAdv();
dynamicCheckBox.AutoHeight = true;
dynamicCheckBox.Width = 200;

// Height adjusts automatically when font changes
void SetFontSize(int size)
{
    dynamicCheckBox.Font = new Font("Arial", size);
    // Height recalculates automatically
}
```

## Read-Only Mode

The `ReadOnlyMode` property prevents users from changing the checkbox state while maintaining its visual appearance.

### Enabling Read-Only Mode

```csharp
// Make checkbox read-only
checkBoxAdv1.ReadOnlyMode = true;
checkBoxAdv1.Checked = true;
// User cannot change the state
```

```vb
' Make checkbox read-only
checkBoxAdv1.ReadOnlyMode = True
checkBoxAdv1.Checked = True
```

### ReadOnlyMode vs Enabled

| Property | User Interaction | Visual Appearance | Programmatic Changes |
|----------|------------------|-------------------|---------------------|
| ReadOnlyMode = true | Disabled | Normal colors | Allowed |
| Enabled = false | Disabled | Grayed out | Allowed |

### Use Cases for ReadOnlyMode

```csharp
// Use Case 1: Display status without allowing changes
CheckBoxAdv statusDisplay = new CheckBoxAdv();
statusDisplay.Text = "Task Completed";
statusDisplay.Checked = true;
statusDisplay.ReadOnlyMode = true;

// Use Case 2: Conditional editing
CheckBoxAdv conditionalCheckBox = new CheckBoxAdv();
conditionalCheckBox.Text = "Advanced Option";
conditionalCheckBox.ReadOnlyMode = !userHasPermission;

// Use Case 3: Workflow state display
void DisplayWorkflowState(WorkflowStatus status)
{
    checkBoxAdv1.Checked = (status == WorkflowStatus.Approved);
    checkBoxAdv1.ReadOnlyMode = (status != WorkflowStatus.Pending);
}
```

## Gradient Backgrounds

The CheckBoxAdv supports gradient backgrounds for enhanced visual appearance.

### Background Style Options

```csharp
// No gradient (default)
checkBoxAdv1.BackgroundStyle = CheckBoxAdvBackStyle.Default;

// Horizontal gradient
checkBoxAdv1.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;

// Vertical gradient
checkBoxAdv1.BackgroundStyle = CheckBoxAdvBackStyle.VerticalGradient;
```

### Setting Gradient Colors

```csharp
// Configure horizontal gradient
checkBoxAdv1.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
checkBoxAdv1.GradientStart = Color.LightBlue;
checkBoxAdv1.GradientEnd = Color.DarkBlue;
```

```vb
' Configure horizontal gradient
checkBoxAdv1.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient
checkBoxAdv1.GradientStart = Color.LightBlue
checkBoxAdv1.GradientEnd = Color.DarkBlue
```

### Gradient Direction

**Horizontal Gradient:**
- Start color on the left
- End color on the right
- Good for wide controls

**Vertical Gradient:**
- Start color on top
- End color on bottom
- Good for tall controls

### Complete Gradient Example

```csharp
CheckBoxAdv gradientCheckBox = new CheckBoxAdv();
gradientCheckBox.Text = "Gradient Background";
gradientCheckBox.Size = new Size(200, 40);

// Apply vertical gradient
gradientCheckBox.BackgroundStyle = CheckBoxAdvBackStyle.VerticalGradient;
gradientCheckBox.GradientStart = Color.White;
gradientCheckBox.GradientEnd = Color.LightGray;

this.Controls.Add(gradientCheckBox);
```

### Gradient Color Combinations

```csharp
// Subtle gray gradient
checkBoxAdv1.GradientStart = Color.WhiteSmoke;
checkBoxAdv1.GradientEnd = Color.LightGray;

// Blue theme gradient
checkBoxAdv1.GradientStart = Color.AliceBlue;
checkBoxAdv1.GradientEnd = Color.SteelBlue;

// Green theme gradient
checkBoxAdv1.GradientStart = Color.LightGreen;
checkBoxAdv1.GradientEnd = Color.DarkGreen;

// Warning theme gradient
checkBoxAdv1.GradientStart = Color.LightYellow;
checkBoxAdv1.GradientEnd = Color.Orange;
```

### Important Notes

- Gradient backgrounds are not visible when `BackgroundStyle = Default`
- Background images are not compatible with gradient settings
- BackColor property is ignored when gradients are active

## Border Styles

The CheckBoxAdv supports both 2D and 3D border styles.

### BorderStyle Property

```csharp
// No border
checkBoxAdv1.BorderStyle = BorderStyle.None;

// Single-line border (2D)
checkBoxAdv1.BorderStyle = BorderStyle.FixedSingle;

// 3D border
checkBoxAdv1.BorderStyle = BorderStyle.Fixed3D;
```

### 3D Border Styles

When using `BorderStyle.Fixed3D`, customize the 3D effect with `Border3DStyle`:

```csharp
checkBoxAdv1.BorderStyle = BorderStyle.Fixed3D;
checkBoxAdv1.Border3DStyle = Border3DStyle.Raised;
```

Available 3D styles:
- **Raised**: Button-like raised appearance
- **Sunken**: Depressed appearance (default)
- **Etched**: Carved appearance
- **Bump**: Inverse etched
- **RaisedOuter**: Raised outer edge only
- **SunkenOuter**: Sunken outer edge only
- **RaisedInner**: Raised inner edge only
- **SunkenInner**: Sunken inner edge only
- **Flat**: No 3D effect

### 2D Border Styles

When using `BorderStyle.FixedSingle`, customize with `BorderSingle`:

```csharp
checkBoxAdv1.BorderStyle = BorderStyle.FixedSingle;
checkBoxAdv1.BorderSingle = ButtonBorderStyle.Solid;
```

Available 2D styles:
- **Solid**: Solid line
- **Dashed**: Dashed line
- **Dotted**: Dotted line
- **Inset**: Inset appearance
- **Outset**: Outset appearance
- **None**: No border

### Border Examples

```csharp
// Example 1: Solid 2D border
CheckBoxAdv solidBorder = new CheckBoxAdv();
solidBorder.Text = "Solid Border";
solidBorder.BorderStyle = BorderStyle.FixedSingle;
solidBorder.BorderSingle = ButtonBorderStyle.Solid;
solidBorder.BorderColor = Color.Black;

// Example 2: Dotted border
CheckBoxAdv dottedBorder = new CheckBoxAdv();
dottedBorder.Text = "Dotted Border";
dottedBorder.BorderStyle = BorderStyle.FixedSingle;
dottedBorder.BorderSingle = ButtonBorderStyle.Dotted;
dottedBorder.BorderColor = Color.Blue;

// Example 3: Raised 3D border
CheckBoxAdv raisedBorder = new CheckBoxAdv();
raisedBorder.Text = "Raised 3D";
raisedBorder.BorderStyle = BorderStyle.Fixed3D;
raisedBorder.Border3DStyle = Border3DStyle.Raised;

// Example 4: Etched border
CheckBoxAdv etchedBorder = new CheckBoxAdv();
etchedBorder.Text = "Etched Border";
etchedBorder.BorderStyle = BorderStyle.Fixed3D;
etchedBorder.Border3DStyle = Border3DStyle.Etched;
```

## Border Colors

Customize border colors for both normal and hover states.

### BorderColor Property

For `BorderStyle.FixedSingle`, set the border color:

```csharp
checkBoxAdv1.BorderStyle = BorderStyle.FixedSingle;
checkBoxAdv1.BorderColor = Color.Red;
```

```vb
checkBoxAdv1.BorderStyle = BorderStyle.FixedSingle
checkBoxAdv1.BorderColor = Color.Red
```

### HotBorderColor Property

Change border color when mouse hovers over the control:

```csharp
checkBoxAdv1.BorderStyle = BorderStyle.FixedSingle;
checkBoxAdv1.BorderColor = Color.Gray;
checkBoxAdv1.HotBorderColor = Color.Blue;
// Border changes from Gray to Blue on hover
```

```vb
checkBoxAdv1.BorderStyle = BorderStyle.FixedSingle
checkBoxAdv1.BorderColor = Color.Gray
checkBoxAdv1.HotBorderColor = Color.Blue
```

### Important Note

The `HotBorderColor` property only works when `BorderStyle = FixedSingle`. It has no effect with `Fixed3D` or `None` border styles.

### Hover Effect Example

```csharp
CheckBoxAdv hoverCheckBox = new CheckBoxAdv();
hoverCheckBox.Text = "Hover Effect";
hoverCheckBox.Size = new Size(200, 30);

// Configure hover effect
hoverCheckBox.BorderStyle = BorderStyle.FixedSingle;
hoverCheckBox.BorderSingle = ButtonBorderStyle.Solid;
hoverCheckBox.BorderColor = Color.LightGray;
hoverCheckBox.HotBorderColor = Color.DarkBlue;

this.Controls.Add(hoverCheckBox);
```

## Common Patterns

### Pattern 1: Status Display Checkbox

```csharp
CheckBoxAdv CreateStatusCheckBox(string label, bool isActive)
{
    CheckBoxAdv statusBox = new CheckBoxAdv();
    statusBox.Text = label;
    statusBox.Checked = isActive;
    statusBox.ReadOnlyMode = true;
    statusBox.DrawFocusRectangle = false;
    
    // Color based on status
    if (isActive)
    {
        statusBox.ForeColor = Color.Green;
        statusBox.BackColor = Color.LightGreen;
    }
    else
    {
        statusBox.ForeColor = Color.Red;
        statusBox.BackColor = Color.LightPink;
    }
    
    return statusBox;
}
```

### Pattern 2: Themed Checkbox

```csharp
CheckBoxAdv CreateThemedCheckBox(Color themeColor)
{
    CheckBoxAdv themedBox = new CheckBoxAdv();
    themedBox.Size = new Size(200, 35);
    
    // Gradient background matching theme
    themedBox.BackgroundStyle = CheckBoxAdvBackStyle.VerticalGradient;
    themedBox.GradientStart = Color.White;
    themedBox.GradientEnd = Color.FromArgb(themeColor.R, themeColor.G, themeColor.B);
    
    // Matching border
    themedBox.BorderStyle = BorderStyle.FixedSingle;
    themedBox.BorderColor = themeColor;
    themedBox.HotBorderColor = ControlPaint.Dark(themeColor);
    
    return themedBox;
}
```

### Pattern 3: Interactive Hover Effects

```csharp
CheckBoxAdv interactiveCheckBox = new CheckBoxAdv();
interactiveCheckBox.Text = "Interactive";

// Border styling
interactiveCheckBox.BorderStyle = BorderStyle.FixedSingle;
interactiveCheckBox.BorderColor = Color.Silver;
interactiveCheckBox.HotBorderColor = Color.DodgerBlue;

// Background gradient
interactiveCheckBox.BackgroundStyle = CheckBoxAdvBackStyle.HorizontalGradient;
interactiveCheckBox.GradientStart = Color.White;
interactiveCheckBox.GradientEnd = Color.AliceBlue;

// Events for additional interactivity
interactiveCheckBox.MouseEnter += (s, e) => {
    interactiveCheckBox.Font = new Font(interactiveCheckBox.Font, FontStyle.Bold);
};

interactiveCheckBox.MouseLeave += (s, e) => {
    interactiveCheckBox.Font = new Font(interactiveCheckBox.Font, FontStyle.Regular);
};
```

### Pattern 4: Conditional Read-Only

```csharp
void ConfigureCheckBoxPermissions(CheckBoxAdv checkBox, UserRole role)
{
    if (role == UserRole.Administrator)
    {
        // Full access
        checkBox.ReadOnlyMode = false;
        checkBox.DrawFocusRectangle = true;
    }
    else if (role == UserRole.Viewer)
    {
        // Display only
        checkBox.ReadOnlyMode = true;
        checkBox.DrawFocusRectangle = false;
        checkBox.BorderStyle = BorderStyle.None;
    }
    else
    {
        // Limited access
        checkBox.ReadOnlyMode = true;
        checkBox.BorderStyle = BorderStyle.FixedSingle;
        checkBox.BorderColor = Color.Orange;
    }
}
```

### Pattern 5: Responsive DPI-Aware Checkbox

```csharp
CheckBoxAdv CreateDpiAwareCheckBox()
{
    CheckBoxAdv dpiCheckBox = new CheckBoxAdv();
    dpiCheckBox.AutoHeight = true;
    dpiCheckBox.Width = 200;
    
    // Font size adjusts based on DPI
    float currentDpi = this.DeviceDpi;
    float baseDpi = 96.0f;
    float scaleFactor = currentDpi / baseDpi;
    
    int fontSize = (int)(9 * scaleFactor);
    dpiCheckBox.Font = new Font("Segoe UI", fontSize);
    
    // Height automatically adjusts via AutoHeight
    return dpiCheckBox;
}
```
