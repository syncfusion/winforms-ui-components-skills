# Windows Forms AutoLabel Overview

The **AutoLabel** control is a Label-inspired control that lets you pair a label with any other control. Once paired, the AutoLabel is automatically repositioned as the labeled control's position changes.

## Key Features

### Automatic Repositioning

The primary feature of AutoLabel is its ability to track and follow its paired control. When the labeled control moves, the AutoLabel automatically adjusts its position to maintain the defined spatial relationship.

### FlowLayout Integration

The FlowLayout layout manager always treats the AutoLabel-labeled control pair as a unit. You can use AutoLabels and FlowLayouts together to implement complex and powerful form layouts.

### Flexible Positioning

AutoLabel supports multiple positioning modes:
- Left of the control
- Top of the control
- Side (right) of the control
- Custom positioning with precise control

## Architecture

### Component Structure

```
AutoLabel Control
├── LabeledControl Property (references the paired control)
├── Position System (Left, Top, Side, Custom)
├── Spacing System (Gap, DX, DY)
├── Appearance (Font, Colors, Size)
└── Events (PropertyChanged)
```

### How It Works

1. **Pairing**: You associate an AutoLabel with any control via the `LabeledControl` property
2. **Positioning**: Set the `Position` property to define the spatial relationship
3. **Spacing**: Configure `Gap` or `DX/DY` to control distance
4. **Tracking**: AutoLabel monitors the labeled control's position
5. **Auto-Update**: When the control moves, AutoLabel repositions itself automatically

## When to Use AutoLabel

### ✅ Use AutoLabel When:

- **Dynamic Forms**: Forms where controls move or resize dynamically
- **FlowLayout Forms**: Using FlowLayoutPanel for automatic layout management
- **Complex Layouts**: Building forms with many label-control pairs
- **Resizable Forms**: Forms that need to adapt to different sizes
- **Consistent Spacing**: Ensuring labels maintain proper spacing from controls
- **Professional Forms**: Creating polished, professional-looking forms

### ❌ Use Standard Label When:

- **Static Forms**: Controls never move after initial placement
- **Simple Forms**: Few controls with fixed positions
- **Performance Critical**: Every byte of memory matters (AutoLabel has slight overhead)
- **No Pairing Needed**: Labels that don't correspond to specific controls

## Comparison with Standard Label

| Feature | AutoLabel | Standard Label |
|---------|-----------|----------------|
| **Auto-repositioning** | ✅ Yes | ❌ No |
| **FlowLayout integration** | ✅ Treated as unit with control | ❌ Separate element |
| **Relative positioning** | ✅ Built-in (Left, Top, Side) | ❌ Manual only |
| **Control pairing** | ✅ LabeledControl property | ❌ No pairing concept |
| **Memory footprint** | Slightly larger | Minimal |
| **Setup complexity** | Moderate | Simple |
| **Best for** | Dynamic layouts | Static layouts |

## Use Cases

### 1. Data Entry Forms

Perfect for forms where label-control pairs need to maintain spacing:

```csharp
// Each field keeps its label properly positioned
AutoLabel nameLabel, emailLabel, phoneLabel;
// As controls resize or reposition, labels follow
```

### 2. FlowLayout Forms

Essential for forms that reorganize based on available space:

```csharp
FlowLayoutPanel panel = new FlowLayoutPanel();
// FlowLayout treats label+control as single unit
panel.Controls.Add(textBox);
panel.Controls.Add(autoLabel);  // Moves with textBox
```

### 3. Resizable Forms

Forms that adapt to window size changes:

```csharp
// Controls reposition on form resize
// AutoLabels automatically follow
this.SizeChanged += (s, e) => {
    // Control positions update
    // Labels stay properly positioned
};
```

### 4. Tabbed or Multi-Page Forms

Forms where controls move between different views:

```csharp
// Move control to different tab page
tabPage2.Controls.Add(myControl);
// AutoLabel follows to maintain relationship
```

### 5. Custom Form Designers

Building form designers that allow drag-drop:

```csharp
// User drags control in designer
// Label automatically repositions
controlBeingDragged.Location = newLocation;
```

## Best Practices

### 1. Pair Before Adding

Set `LabeledControl` before adding the AutoLabel to the form:

```csharp
autoLabel.LabeledControl = textBox;  // First
this.Controls.Add(autoLabel);         // Then
```

### 2. Use with FlowLayout

Leverage FlowLayoutPanel for automatic layout:

```csharp
flowPanel.Controls.Add(control);
flowPanel.Controls.Add(autoLabel);  // Added after its control
```

### 3. Consistent Positioning

Use the same `Position` value for all labels in a form for visual consistency:

```csharp
const AutoLabelPosition FORM_LABEL_POSITION = AutoLabelPosition.Left;
label1.Position = FORM_LABEL_POSITION;
label2.Position = FORM_LABEL_POSITION;
```

### 4. Standard Gap Values

Define standard gap values for consistent spacing:

```csharp
const int STANDARD_GAP = 10;
label1.Gap = STANDARD_GAP;
label2.Gap = STANDARD_GAP;
```

### 5. Theme Consistently

Apply the same theme to all AutoLabels:

```csharp
foreach (Control ctrl in form.Controls)
{
    if (ctrl is AutoLabel)
    {
        SkinManager.SetVisualStyle((AutoLabel)ctrl, VisualTheme.Office2016Colorful);
    }
}
```

## Performance Considerations

AutoLabel has minimal performance impact, but consider these points for large forms:

- **Memory**: Slightly more memory per label than standard Label
- **Processing**: Automatic repositioning has negligible CPU cost
- **Rendering**: No difference in rendering performance
- **Recommendation**: Use AutoLabel when you need its features; use standard Label for static forms

## Integration with Other Syncfusion Controls

AutoLabel works seamlessly with all Syncfusion WinForms controls:

- **TextBoxExt**: Enhanced text boxes
- **ComboBoxAdv**: Advanced combo boxes
- **DateTimePickerAdv**: Date/time pickers
- **NumericUpDown**: Numeric input controls
- **Any Control**: Works with any System.Windows.Forms.Control

## Framework Support

- .NET Framework 4.5 and above
- .NET 6.0, .NET 7.0, .NET 8.0 (Windows)
- .NET Core 3.1 (Windows)

## See Also

- [Getting Started](getting-started.md) - Basic setup and configuration
- [Positioning and Spacing](positioning-spacing.md) - Control label placement
- [Appearance and Theming](appearance-theming.md) - Customize visual appearance
- [Events](events.md) - Handle property changes and updates
