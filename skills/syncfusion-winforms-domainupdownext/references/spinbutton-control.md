# Spin Button Control Configuration

## Table of Contents
- [Spin Orientation](#spin-orientation)
- [UpDown Alignment](#updown-alignment)
- [Orientation and Alignment Combinations](#orientation-and-alignment-combinations)
- [Default Behaviors](#default-behaviors)
- [Interaction Patterns](#interaction-patterns)

## Spin Orientation

The SpinOrientation property controls whether the spin buttons are arranged vertically or horizontally.

### Vertical Orientation (Default)

Spin buttons are stacked vertically (Up button on top, Down button below):

```csharp
domainUpDownExt1.SpinOrientation = Orientation.Vertical;
```

**Use when:**
- Space is limited horizontally
- Following standard Windows control patterns
- Creating a compact vertical interface

### Horizontal Orientation

Spin buttons are arranged side by side:

```csharp
domainUpDownExt1.SpinOrientation = Orientation.Horizontal;
```

**Use when:**
- You have limited vertical space
- Designing a wide interface
- Following specific design requirements

## UpDown Alignment

The UpDownAlign property controls whether the spin buttons appear on the left or right side of the text area.

### Right Alignment (Default)

Spin buttons appear to the right of the text:

```csharp
domainUpDownExt1.UpDownAlign = LeftRightAlignment.Right;
```

This is the standard Windows convention and most common usage.

### Left Alignment

Spin buttons appear to the left of the text:

```csharp
domainUpDownExt1.UpDownAlign = LeftRightAlignment.Left;
```

**Use when:**
- Designing for right-to-left (RTL) languages
- Creating custom UI layouts
- Mirroring a right-aligned design

## Orientation and Alignment Combinations

### Combination 1: Vertical-Right (Standard)

```csharp
domainUpDownExt1.SpinOrientation = Orientation.Vertical;
domainUpDownExt1.UpDownAlign = LeftRightAlignment.Right;
```

- Layout: [Text Area] [Up/Down]
- Most compact vertical arrangement
- Default Windows Forms behavior

### Combination 2: Vertical-Left

```csharp
domainUpDownExt1.SpinOrientation = Orientation.Vertical;
domainUpDownExt1.UpDownAlign = LeftRightAlignment.Left;
```

- Layout: [Up/Down] [Text Area]
- Good for RTL interfaces
- Preserves vertical stacking

### Combination 3: Horizontal-Right

```csharp
domainUpDownExt1.SpinOrientation = Orientation.Horizontal;
domainUpDownExt1.UpDownAlign = LeftRightAlignment.Right;
```

- Layout: [Text Area] [<] [>]
- Wider than vertical
- Good for touch interfaces

### Combination 4: Horizontal-Left

```csharp
domainUpDownExt1.SpinOrientation = Orientation.Horizontal;
domainUpDownExt1.UpDownAlign = LeftRightAlignment.Left;
```

- Layout: [<] [>] [Text Area]
- Useful for specific UI requirements
- Inverted horizontal arrangement

## Default Behaviors

### Default Configuration

When you create a new DomainUpDownExt without explicit configuration:

```csharp
DomainUpDownExt control = new DomainUpDownExt();
// Default: Vertical orientation, Right alignment
// Equivalent to:
// control.SpinOrientation = Orientation.Vertical;
// control.UpDownAlign = LeftRightAlignment.Right;
```

### Default Button Behaviors

**Up Button:**
- Navigates to the previous item in the list
- Wraps to the last item when at the first item (depending on configuration)

**Down Button:**
- Navigates to the next item in the list
- Wraps to the first item when at the last item (depending on configuration)

## Interaction Patterns

### Pattern 1: Compact Vertical Interface

```csharp
public void ConfigureCompactControl()
{
    domainUpDownExt1.SpinOrientation = Orientation.Vertical;
    domainUpDownExt1.UpDownAlign = LeftRightAlignment.Right;
    domainUpDownExt1.Width = 150; // Compact width
    
    // Add items
    domainUpDownExt1.Items.Add("Option 1");
    domainUpDownExt1.Items.Add("Option 2");
    domainUpDownExt1.Items.Add("Option 3");
}
```

### Pattern 2: Wide Form Layout

```csharp
public void ConfigureWideFormLayout()
{
    domainUpDownExt1.SpinOrientation = Orientation.Horizontal;
    domainUpDownExt1.UpDownAlign = LeftRightAlignment.Right;
    domainUpDownExt1.Width = 300; // Wider for horizontal buttons
    
    domainUpDownExt1.Items.Add("January");
    domainUpDownExt1.Items.Add("February");
    domainUpDownExt1.Items.Add("March");
}
```

### Pattern 3: Right-to-Left Interface

```csharp
public void ConfigureRTLInterface()
{
    // Mirror the control for RTL languages
    domainUpDownExt1.SpinOrientation = Orientation.Vertical;
    domainUpDownExt1.UpDownAlign = LeftRightAlignment.Left;
    domainUpDownExt1.TextAlign = HorizontalAlignment.Right;
    
    this.RightToLeft = RightToLeft.Yes; // If needed for the entire form
}
```

### Pattern 4: Touch-Friendly Interface

```csharp
public void ConfigureTouchFriendly()
{
    domainUpDownExt1.SpinOrientation = Orientation.Horizontal;
    domainUpDownExt1.UpDownAlign = LeftRightAlignment.Left;
    domainUpDownExt1.Height = 40; // Larger for touch
    
    // Buttons are easier to tap when separated horizontally
}
```

## Complete Configuration Example

```csharp
public class SpinButtonForm : Form
{
    private DomainUpDownExt domainUpDownExt1;
    private DomainUpDownExt domainUpDownExt2;
    
    public SpinButtonForm()
    {
        InitializeComponent();
    }
    
    private void Form_Load(object sender, EventArgs e)
    {
        // Control 1: Vertical-Right (Standard)
        domainUpDownExt1 = new DomainUpDownExt();
        domainUpDownExt1.SpinOrientation = Orientation.Vertical;
        domainUpDownExt1.UpDownAlign = LeftRightAlignment.Right;
        domainUpDownExt1.Location = new Point(10, 10);
        domainUpDownExt1.Items.Add("Low");
        domainUpDownExt1.Items.Add("Medium");
        domainUpDownExt1.Items.Add("High");
        this.Controls.Add(domainUpDownExt1);
        
        // Control 2: Horizontal-Right (Wide Layout)
        domainUpDownExt2 = new DomainUpDownExt();
        domainUpDownExt2.SpinOrientation = Orientation.Horizontal;
        domainUpDownExt2.UpDownAlign = LeftRightAlignment.Right;
        domainUpDownExt2.Location = new Point(10, 50);
        domainUpDownExt2.Width = 200;
        domainUpDownExt2.Items.Add("2024");
        domainUpDownExt2.Items.Add("2025");
        domainUpDownExt2.Items.Add("2026");
        this.Controls.Add(domainUpDownExt2);
    }
}
```

## Considerations

### Choosing Orientation

- **Vertical**: Default choice, compact, follows Windows conventions
- **Horizontal**: Use only when you have specific layout requirements or space constraints

### Choosing Alignment

- **Right**: Default and recommended for left-to-right (LTR) languages
- **Left**: Use for right-to-left (RTL) languages or custom layouts

### Impact on User Experience

1. **Consistency**: Use the same orientation and alignment throughout your application
2. **Accessibility**: Ensure buttons are clearly visible and accessible
3. **Touch Support**: Make buttons larger if designing for touch interfaces
4. **Space**: Choose horizontal if vertical space is limited
