# Spacing Configuration in BorderLayout

## Table of Contents
- [HGap Property](#hgap-property)
- [VGap Property](#vgap-property)
- [Setting Spacing in Designer](#setting-spacing-in-designer)
- [Setting Spacing in Code](#setting-spacing-in-code)
- [Spacing Examples](#spacing-examples)
- [Common Spacing Patterns](#common-spacing-patterns)

## HGap Property

### Overview
The `HGap` property controls the **horizontal spacing** between controls positioned on the left (West) and right (East) edges, as well as the space from these controls to the center area.

### Property Details
```csharp
public int HGap { get; set; }
```

**Type:** Integer (pixels)  
**Default:** 0  
**Unit:** Pixels  
**Range:** Non-negative integers

### HGap Visual Effect

```
HGap = 0 (No spacing)
┌─────┬───────────────────┬─────┐
│West │     Center        │East │
└─────┴───────────────────┴─────┘

HGap = 10 (10-pixel gaps)
┌─────┐   ┌────────────┐   ┌─────┐
│West │   │  Center    │   │East  │
└─────┘   └────────────┘   └─────┘
```

### Setting HGap in Code

**C#:**
```csharp
borderLayout1.HGap = 10;  // 10 pixels horizontal spacing
```

**VB.NET:**
```vb
borderLayout1.HGap = 10  ' 10 pixels horizontal spacing
```

### Common HGap Values
- `0` - No spacing (controls adjacent)
- `5` - Minimal spacing (compact layout)
- `10` - Standard spacing (balanced)
- `15` - Generous spacing (open layout)
- `20+` - Extra spacing (spacious layout)

## VGap Property

### Overview
The `VGap` property controls the **vertical spacing** between controls positioned on the top (North) and bottom (South) edges, as well as the space from these controls to the center area.

### Property Details
```csharp
public int VGap { get; set; }
```

**Type:** Integer (pixels)  
**Default:** 0  
**Unit:** Pixels  
**Range:** Non-negative integers

### VGap Visual Effect

```
VGap = 0 (No spacing)
┌─────────────────────────┐
│        North            │
├─────────────────────────┤
│                         │
│       Center            │
│                         │
├─────────────────────────┤
│        South            │
└─────────────────────────┘

VGap = 10 (10-pixel gaps)
┌─────────────────────────┐
│        North            │
└─────────────────────────┘
    (10 pixels gap)
┌─────────────────────────┐
│                         │
│       Center            │
│                         │
└─────────────────────────┘
    (10 pixels gap)
┌─────────────────────────┐
│        South            │
└─────────────────────────┘
```

### Setting VGap in Code

**C#:**
```csharp
borderLayout1.VGap = 10;  // 10 pixels vertical spacing
```

**VB.NET:**
```vb
borderLayout1.VGap = 10  ' 10 pixels vertical spacing
```

## Setting Spacing in Designer

### Step 1: Select BorderLayout
1. Open your form in designer
2. Click on the BorderLayout component in the form or component tray

### Step 2: Access Properties
1. Open the Properties window (F4 or View → Properties)
2. Look for the BorderLayout properties

### Step 3: Set HGap
1. Find the **HGap** property
2. Enter the desired pixel value
3. Press Enter to apply

### Step 4: Set VGap
1. Find the **VGap** property
2. Enter the desired pixel value
3. Press Enter to apply

### Designer Example
```
Selected Component: borderLayout1
Properties:
├── ContainerControl: Form1
├── HGap: 10 ← Set here
└── VGap: 10 ← Set here
```

## Setting Spacing in Code

### Simple Spacing Setup

**C#:**
```csharp
// Create BorderLayout
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;

// Set spacing
borderLayout1.HGap = 10;
borderLayout1.VGap = 10;
```

**VB.NET:**
```vb
' Create BorderLayout
Dim borderLayout1 As BorderLayout = New BorderLayout()
borderLayout1.ContainerControl = Me

' Set spacing
borderLayout1.HGap = 10
borderLayout1.VGap = 10
```

### Combined with Positioning

**C#:**
```csharp
// Create and configure BorderLayout
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 15;
borderLayout1.VGap = 15;

// Create controls
Panel headerPanel = new Panel() { Height = 50, BackColor = Color.LightBlue };
Panel sidebarPanel = new Panel() { Width = 200, BackColor = Color.LightGray };
Panel contentPanel = new Panel() { BackColor = Color.White };
Panel footerPanel = new Panel() { Height = 40, BackColor = Color.LightBlue };

// Add to form
this.Controls.Add(headerPanel);
this.Controls.Add(sidebarPanel);
this.Controls.Add(contentPanel);
this.Controls.Add(footerPanel);

// Position
borderLayout1.SetPosition(headerPanel, BorderPosition.North);
borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);
borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
borderLayout1.SetPosition(footerPanel, BorderPosition.South);
```

**VB.NET:**
```vb
' Create and configure BorderLayout
Dim borderLayout1 As BorderLayout = New BorderLayout()
borderLayout1.ContainerControl = Me
borderLayout1.HGap = 15
borderLayout1.VGap = 15

' Create controls
Dim headerPanel As Panel = New Panel() With {.Height = 50, .BackColor = Color.LightBlue}
Dim sidebarPanel As Panel = New Panel() With {.Width = 200, .BackColor = Color.LightGray}
Dim contentPanel As Panel = New Panel() With {.BackColor = Color.White}
Dim footerPanel As Panel = New Panel() With {.Height = 40, .BackColor = Color.LightBlue}

' Add to form
Me.Controls.Add(headerPanel)
Me.Controls.Add(sidebarPanel)
Me.Controls.Add(contentPanel)
Me.Controls.Add(footerPanel)

' Position
borderLayout1.SetPosition(headerPanel, BorderPosition.North)
borderLayout1.SetPosition(sidebarPanel, BorderPosition.West)
borderLayout1.SetPosition(contentPanel, BorderPosition.Center)
borderLayout1.SetPosition(footerPanel, BorderPosition.South)
```

### Dynamic Spacing Adjustment

Change spacing at runtime in response to user actions:

**C#:**
```csharp
// Method to adjust spacing based on user preference
private void SetCompactLayout()
{
    borderLayout1.HGap = 5;
    borderLayout1.VGap = 5;
}

private void SetNormalLayout()
{
    borderLayout1.HGap = 10;
    borderLayout1.VGap = 10;
}

private void SetSpaceLayout()
{
    borderLayout1.HGap = 20;
    borderLayout1.VGap = 20;
}

// Call based on user selection
private void compactToolStripMenuItem_Click(object sender, EventArgs e)
{
    SetCompactLayout();
}
```

## Spacing Examples

### Example 1: Standard Layout (Balanced Spacing)

```csharp
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 10;  // 10 pixels
borderLayout1.VGap = 10;  // 10 pixels
```

**Result:**
```
┌─────────────────────────────────────┐
│          Header (50px height)       │
└─────────────────────────────────────┘
    (10px gap)
┌─────┐   ┌──────────────┐   ┌─────┐
│  S  │   │   Content    │   │  E  │
│  I  │10 │  (fills      │10 │  A  │
│  D  │px │   both       │px │  S  │
│  E  │   │   ways)      │   │  T  │
│  B  │   │              │   │     │
│  A  │   │              │   │     │
│  R  │   │              │   │     │
└─────┘   └──────────────┘   └─────┘
  200px    (fills)              200px
    (10px gap)
┌─────────────────────────────────────┐
│          Footer (40px height)       │
└─────────────────────────────────────┘
```

### Example 2: Compact Layout (Minimal Spacing)

```csharp
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 2;  // 2 pixels
borderLayout1.VGap = 2;  // 2 pixels
```

**Result:** Controls are very close together

### Example 3: Open Layout (Generous Spacing)

```csharp
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 20;  // 20 pixels
borderLayout1.VGap = 20;  // 20 pixels
```

**Result:** Lots of white space between controls

### Example 4: Asymmetric Spacing

```csharp
BorderLayout borderLayout1 = new BorderLayout();
borderLayout1.ContainerControl = this;
borderLayout1.HGap = 5;   // Small horizontal spacing
borderLayout1.VGap = 15;  // Large vertical spacing
```

**Result:** More space vertically, compact horizontally

## Common Spacing Patterns

### Pattern 1: Professional Dashboard
**Characteristics:** Moderate, balanced spacing for a professional appearance

```csharp
borderLayout1.HGap = 10;
borderLayout1.VGap = 10;
```

**Use for:** Business applications, data dashboards, admin panels

### Pattern 2: Compact Tool Interface
**Characteristics:** Minimal spacing for maximum content area

```csharp
borderLayout1.HGap = 3;
borderLayout1.VGap = 3;
```

**Use for:** Dense tool interfaces, information-heavy layouts

### Pattern 3: Desktop Publishing Layout
**Characteristics:** Large spacing for visual breathing room

```csharp
borderLayout1.HGap = 15;
borderLayout1.VGap = 15;
```

**Use for:** Creative tools, document editors, visual applications

### Pattern 4: Mobile-Inspired Layout
**Characteristics:** Different spacing for different axes

```csharp
borderLayout1.HGap = 8;
borderLayout1.VGap = 12;
```

**Use for:** Modern Windows Forms apps simulating mobile layouts

## Complete Spacing Configuration Example

```csharp
public partial class LayoutForm : Form
{
    private BorderLayout borderLayout1;

    public LayoutForm()
    {
        InitializeComponent();
    }

    protected override void OnLoad(EventArgs e)
    {
        base.OnLoad(e);

        // Initialize BorderLayout
        borderLayout1 = new BorderLayout();
        borderLayout1.ContainerControl = this;

        // Create styled panels
        Panel toolbarPanel = new Panel()
        {
            Height = 45,
            BackColor = Color.FromArgb(240, 240, 240)
        };
        
        Panel headerPanel = new Panel()
        {
            Height = 35,
            BackColor = Color.FromArgb(220, 230, 240)
        };
        
        Panel sidebarPanel = new Panel()
        {
            Width = 220,
            BackColor = Color.FromArgb(245, 245, 245)
        };
        
        Panel contentPanel = new Panel()
        {
            BackColor = Color.White
        };
        
        Panel statusPanel = new Panel()
        {
            Height = 25,
            BackColor = Color.FromArgb(240, 240, 240)
        };

        // Add panels to form
        this.Controls.Add(toolbarPanel);
        this.Controls.Add(headerPanel);
        this.Controls.Add(sidebarPanel);
        this.Controls.Add(contentPanel);
        this.Controls.Add(statusPanel);

        // Position panels
        borderLayout1.SetPosition(toolbarPanel, BorderPosition.North);
        borderLayout1.SetPosition(headerPanel, BorderPosition.North);
        borderLayout1.SetPosition(sidebarPanel, BorderPosition.West);
        borderLayout1.SetPosition(contentPanel, BorderPosition.Center);
        borderLayout1.SetPosition(statusPanel, BorderPosition.South);

        // Configure spacing for professional appearance
        borderLayout1.HGap = 10;
        borderLayout1.VGap = 10;
    }
}
```

### Result
Professional-looking application with consistent 10-pixel spacing throughout.
