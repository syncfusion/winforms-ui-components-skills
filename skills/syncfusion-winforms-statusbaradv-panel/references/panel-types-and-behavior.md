# Panel Types and Behavior

This guide covers the different panel types available in StatusBarAdvPanel and behavior configuration options.

## When to Read This

Read this guide when you need to:
- Display key state indicators (NumLock, CapsLock, ScrollLock, Insert)
- Show date and time information in various formats
- Display culture information
- Create custom text panels
- Configure automatic panel sizing
- Set preferred panel dimensions
- Understand panel type behaviors

## PanelType Property

The `PanelType` property determines what information the panel displays.

### Available Panel Types

**StatusBarAdvPanelType Enumeration:**

| Panel Type | Description | Example Output |
|------------|-------------|----------------|
| **Custom** | Displays custom text set via Text property | "Status: Ready" |
| **NumLockState** | Shows NumLock key state | "NUM" or empty |
| **CapsLockState** | Shows CapsLock key state | "CAPS" or empty |
| **ScrollLockState** | Shows ScrollLock key state | "SCRL" or empty |
| **InsertKeyState** | Shows Insert key state | "INS" or "OVR" |
| **ShortDate** | Short date format | "3/21/2026" |
| **LongDate** | Long date format | "Friday, March 21, 2026" |
| **ShortTime** | Short time format | "2:30 PM" |
| **LongTime** | Long time format | "2:30:45 PM" |
| **CurrentCulture** | Current culture information | "en-US" |

## Key State Indicators

### NumLock State Panel

Displays "NUM" when NumLock is ON, empty when OFF.

**C#:**
```csharp
var numLockPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.NumLockState,
    Size = new Size(60, 24),
    BackgroundColor = new BrushInfo(Color.LightGray),
    Alignment = HorizontalAlignment.Center
};

// Add to StatusBarAdv
statusBarAdv1.Controls.Add(numLockPanel);
```

**VB.NET:**
```vb
Dim numLockPanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.NumLockState,
    .Size = New Size(60, 24),
    .BackgroundColor = New BrushInfo(Color.LightGray),
    .Alignment = HorizontalAlignment.Center
}

' Add to StatusBarAdv
statusBarAdv1.Controls.Add(numLockPanel)
```

### CapsLock State Panel

Displays "CAPS" when CapsLock is ON, empty when OFF.

**C#:**
```csharp
var capsLockPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.CapsLockState,
    Size = new Size(60, 24),
    BackgroundColor = new BrushInfo(Color.LightYellow),
    Alignment = HorizontalAlignment.Center
};
```

### ScrollLock State Panel

Displays "SCRL" when ScrollLock is ON, empty when OFF.

**C#:**
```csharp
var scrollLockPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.ScrollLockState,
    Size = new Size(60, 24),
    BackgroundColor = new BrushInfo(Color.LightBlue),
    Alignment = HorizontalAlignment.Center
};
```

### Insert Key State Panel

Displays "INS" for Insert mode, "OVR" for Overwrite mode.

**C#:**
```csharp
var insertKeyPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.InsertKeyState,
    Size = new Size(60, 24),
    BackgroundColor = new BrushInfo(Color.LightGreen),
    Alignment = HorizontalAlignment.Center
};
```

### Complete Key State Indicator Example

**C#:**
```csharp
private void SetupKeyStateIndicators()
{
    // Create all key state panels
    var numLockPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.NumLockState,
        Size = new Size(50, 22),
        HAlign = HorzFlowAlign.Right,
        BackgroundColor = new BrushInfo(Color.FromArgb(240, 240, 240)),
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.Gray,
        Alignment = HorizontalAlignment.Center
    };
    
    var capsLockPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.CapsLockState,
        Size = new Size(50, 22),
        HAlign = HorzFlowAlign.Right,
        BackgroundColor = new BrushInfo(Color.FromArgb(240, 240, 240)),
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.Gray,
        Alignment = HorizontalAlignment.Center
    };
    
    var scrollLockPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ScrollLockState,
        Size = new Size(50, 22),
        HAlign = HorzFlowAlign.Right,
        BackgroundColor = new BrushInfo(Color.FromArgb(240, 240, 240)),
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.Gray,
        Alignment = HorizontalAlignment.Center
    };
    
    var insertKeyPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.InsertKeyState,
        Size = new Size(50, 22),
        HAlign = HorzFlowAlign.Right,
        BackgroundColor = new BrushInfo(Color.FromArgb(240, 240, 240)),
        BorderStyle = BorderStyle.FixedSingle,
        BorderColor = Color.Gray,
        Alignment = HorizontalAlignment.Center
    };
    
    // Add all panels to StatusBarAdv
    statusBarAdv1.Controls.Add(numLockPanel);
    statusBarAdv1.Controls.Add(capsLockPanel);
    statusBarAdv1.Controls.Add(scrollLockPanel);
    statusBarAdv1.Controls.Add(insertKeyPanel);
}
```

## Date and Time Panels

### Short Date Panel

Displays date in short format (e.g., "3/21/2026").

**C#:**
```csharp
var shortDatePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.ShortDate,
    Size = new Size(90, 24),
    HAlign = HorzFlowAlign.Right,
    BackgroundColor = new BrushInfo(Color.AliceBlue),
    Alignment = HorizontalAlignment.Center
};
```

**VB.NET:**
```vb
Dim shortDatePanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.ShortDate,
    .Size = New Size(90, 24),
    .HAlign = HorzFlowAlign.Right,
    .BackgroundColor = New BrushInfo(Color.AliceBlue),
    .Alignment = HorizontalAlignment.Center
}
```

### Long Date Panel

Displays date in long format (e.g., "Friday, March 21, 2026").

**C#:**
```csharp
var longDatePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.LongDate,
    Size = new Size(200, 24),
    HAlign = HorzFlowAlign.Right,
    BackgroundColor = new BrushInfo(Color.Honeydew),
    Alignment = HorizontalAlignment.Left
};
```

### Short Time Panel

Displays time in short format (e.g., "2:30 PM").

**C#:**
```csharp
var shortTimePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.ShortTime,
    Size = new Size(80, 24),
    HAlign = HorzFlowAlign.Right,
    BackgroundColor = new BrushInfo(Color.Lavender),
    Alignment = HorizontalAlignment.Center
};
```

### Long Time Panel

Displays time in long format (e.g., "2:30:45 PM").

**C#:**
```csharp
var longTimePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.LongTime,
    Size = new Size(100, 24),
    HAlign = HorzFlowAlign.Right,
    BackgroundColor = new BrushInfo(Color.MistyRose),
    Alignment = HorizontalAlignment.Center
};
```

## Culture Information Panel

Displays current culture information.

**C#:**
```csharp
var culturePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.CurrentCulture,
    Size = new Size(80, 24),
    HAlign = HorzFlowAlign.Left,
    BackgroundColor = new BrushInfo(Color.LemonChiffon),
    Alignment = HorizontalAlignment.Center
};
```

**VB.NET:**
```vb
Dim culturePanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.CurrentCulture,
    .Size = New Size(80, 24),
    .HAlign = HorzFlowAlign.Left,
    .BackgroundColor = New BrushInfo(Color.LemonChiffon),
    .Alignment = HorizontalAlignment.Center
}
```

## Custom Text Panels

For custom text display, set PanelType to Custom and use the Text property.

**C#:**
```csharp
var customPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.Custom,
    Text = "Ready",
    Size = new Size(150, 24),
    HAlign = HorzFlowAlign.Left,
    BackgroundColor = new BrushInfo(Color.LightBlue),
    Alignment = HorizontalAlignment.Left
};

// Update text dynamically
customPanel.Text = "Processing...";
customPanel.Text = "Complete!";
```

**VB.NET:**
```vb
Dim customPanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.Custom,
    .Text = "Ready",
    .Size = New Size(150, 24),
    .HAlign = HorzFlowAlign.Left,
    .BackgroundColor = New BrushInfo(Color.LightBlue),
    .Alignment = HorizontalAlignment.Left
}

' Update text dynamically
customPanel.Text = "Processing..."
customPanel.Text = "Complete!"
```

## GetText() Method

The `GetText()` method returns the displayed text according to the key state.

**Method Signature:**
```csharp
public string GetText()
```

**Usage Example:**

**C#:**
```csharp
// Get text from key state panel
string numLockText = numLockPanel.GetText();
Console.WriteLine($"NumLock panel displays: '{numLockText}'");

// Get text from date panel
string dateText = shortDatePanel.GetText();
Console.WriteLine($"Date panel displays: '{dateText}'");

// Monitor key state changes
private void CheckKeyStates()
{
    string capsText = capsLockPanel.GetText();
    if (!string.IsNullOrEmpty(capsText))
    {
        MessageBox.Show("CapsLock is ON!");
    }
}
```

**VB.NET:**
```vb
' Get text from key state panel
Dim numLockText As String = numLockPanel.GetText()
Console.WriteLine($"NumLock panel displays: '{numLockText}'")

' Get text from date panel
Dim dateText As String = shortDatePanel.GetText()
Console.WriteLine($"Date panel displays: '{dateText}'")

' Monitor key state changes
Private Sub CheckKeyStates()
    Dim capsText As String = capsLockPanel.GetText()
    If Not String.IsNullOrEmpty(capsText) Then
        MessageBox.Show("CapsLock is ON!")
    End If
End Sub
```

## Panel Sizing Behavior

### SizeToContent Property

Automatically resizes the panel based on its content.

**Property:**
- **Type:** `bool`
- **Default:** `false`
- **Effect:** When `true`, panel automatically adjusts size to fit content

**C#:**
```csharp
var autoSizePanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.LongDate,
    SizeToContent = true,  // Auto-resize to fit date text
    BackgroundColor = new BrushInfo(Color.LightGreen)
};

// Panel will adjust width to accommodate "Friday, March 21, 2026"
```

**VB.NET:**
```vb
Dim autoSizePanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.LongDate,
    .SizeToContent = True,  ' Auto-resize to fit date text
    .BackgroundColor = New BrushInfo(Color.LightGreen)
}

' Panel will adjust width to accommodate "Friday, March 21, 2026"
```

### PreferredSize Property

Specifies the preferred size of the panel in flow layout.

**Property:**
- **Type:** `Size`
- **Effect:** Sets preferred dimensions in parent layout

**C#:**
```csharp
var sizedPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.Custom,
    Text = "Status: Ready",
    PreferredSize = new Size(150, 25),
    BackgroundColor = new BrushInfo(Color.LightCoral)
};

// Panel will try to maintain 150x25 size in layout
```

**VB.NET:**
```vb
Dim sizedPanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.Custom,
    .Text = "Status: Ready",
    .PreferredSize = New Size(150, 25),
    .BackgroundColor = New BrushInfo(Color.LightCoral)
}

' Panel will try to maintain 150x25 size in layout
```

### Sizing Comparison Example

**C#:**
```csharp
private void CompareSizingOptions()
{
    // Fixed size panel
    var fixedPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortDate,
        Size = new Size(100, 24),  // Fixed dimensions
        BackgroundColor = new BrushInfo(Color.LightBlue),
        HAlign = HorzFlowAlign.Left
    };
    
    // Auto-size panel
    var autoPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.LongDate,
        SizeToContent = true,  // Auto-adjusts to content
        BackgroundColor = new BrushInfo(Color.LightGreen),
        HAlign = HorzFlowAlign.Left
    };
    
    // Preferred size panel
    var preferredPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.Custom,
        Text = "Processing",
        PreferredSize = new Size(120, 24),  // Preferred dimensions
        BackgroundColor = new BrushInfo(Color.LightYellow),
        HAlign = HorzFlowAlign.Left
    };
    
    statusBarAdv1.Controls.Add(fixedPanel);
    statusBarAdv1.Controls.Add(autoPanel);
    statusBarAdv1.Controls.Add(preferredPanel);
}
```

## Complete Multi-Type Panel Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class PanelTypesDemo : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel statusPanel;
    private StatusBarAdvPanel culturePanel;
    private StatusBarAdvPanel datePanel;
    private StatusBarAdvPanel timePanel;
    private StatusBarAdvPanel capsPanel;
    private StatusBarAdvPanel numPanel;
    
    public PanelTypesDemo()
    {
        InitializeComponent();
        SetupStatusBar();
    }
    
    private void SetupStatusBar()
    {
        // Create StatusBarAdv
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 28,
            BackColor = Color.WhiteSmoke
        };
        
        // Custom text panel (status message)
        statusPanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.Custom,
            Text = "Ready",
            Size = new Size(150, 24),
            HAlign = HorzFlowAlign.Left,
            Alignment = HorizontalAlignment.Left,
            BackgroundColor = new BrushInfo(Color.LightBlue)
        };
        
        // Culture panel
        culturePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.CurrentCulture,
            Size = new Size(70, 24),
            HAlign = HorzFlowAlign.Left,
            Alignment = HorizontalAlignment.Center,
            BackgroundColor = new BrushInfo(Color.Lavender)
        };
        
        // Date panel (auto-size)
        datePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortDate,
            SizeToContent = true,
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Center,
            BackgroundColor = new BrushInfo(Color.LightGreen)
        };
        
        // Time panel
        timePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortTime,
            Size = new Size(80, 24),
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Center,
            BackgroundColor = new BrushInfo(Color.LightYellow)
        };
        
        // CapsLock indicator
        capsPanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.CapsLockState,
            Size = new Size(50, 24),
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Center,
            BackgroundColor = new BrushInfo(Color.LightCoral)
        };
        
        // NumLock indicator
        numPanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.NumLockState,
            Size = new Size(50, 24),
            HAlign = HorzFlowAlign.Right,
            Alignment = HorizontalAlignment.Center,
            BackgroundColor = new BrushInfo(Color.LightSalmon)
        };
        
        // Add all panels
        statusBarAdv1.Controls.Add(statusPanel);
        statusBarAdv1.Controls.Add(culturePanel);
        statusBarAdv1.Controls.Add(datePanel);
        statusBarAdv1.Controls.Add(timePanel);
        statusBarAdv1.Controls.Add(capsPanel);
        statusBarAdv1.Controls.Add(numPanel);
        
        // Add StatusBarAdv to form
        this.Controls.Add(statusBarAdv1);
    }
    
    // Update status message
    public void UpdateStatus(string message)
    {
        statusPanel.Text = message;
    }
    
    // Check if CapsLock is on
    public bool IsCapsLockOn()
    {
        string capsText = capsPanel.GetText();
        return !string.IsNullOrEmpty(capsText);
    }
    
    private void InitializeComponent()
    {
        this.Text = "Panel Types Demo";
        this.Size = new Size(700, 400);
    }
}
```

## Next Steps

After configuring panel types, explore:
- **[Appearance and Styling](appearance-styling.md)** - Customize panel appearance with gradients and patterns
- **[Text and Marquee](text-and-marquee.md)** - Implement animated marquee text
- **[Alignment and Borders](alignment-and-borders.md)** - Configure panel alignment and borders
