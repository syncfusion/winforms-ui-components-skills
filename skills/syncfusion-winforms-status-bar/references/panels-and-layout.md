# Panels and Layout Configuration

This guide covers managing StatusBarAdvPanel controls, configuring spacing, custom layout bounds, and panel alignment in StatusBarAdv.

## When to Read This

Read this reference when:
- Adding multiple panels to StatusBarAdv
- Configuring spacing between panels
- Setting custom layout bounds for panel positioning
- Aligning panels (center, left, right, or custom)
- Using SetHAlign method for horizontal alignment
- Adding child controls to StatusBarAdv
- Customizing panel sizing and positioning

## StatusBarAdvPanel Collection

StatusBarAdvPanel controls can be added to StatusBarAdv using the **Panels** property or by adding them directly to the **Controls** collection.

### Using Panels Property

**Designer Approach:**
1. Select StatusBarAdv in designer
2. Find **Panels** property in Property Grid
3. Click ellipsis button (...)
4. StatusBarAdvPanel Collection Editor opens
5. Click **Add** to create panels
6. Configure panel properties
7. Click **OK**

**Programmatic Approach:**

**C#:**
```csharp
// Panels can be accessed through the Panels property
foreach (StatusBarAdvPanel panel in statusBarAdv1.Panels)
{
    Console.WriteLine($"Panel Text: {panel.Text}");
}
```

**Note:** Panels are added to the **Controls** collection, not the Panels collection directly. The Panels property provides access to existing panels.

### Adding Panels to Controls Collection

**C#:**
```csharp
StatusBarAdvPanel panel1 = new StatusBarAdvPanel
{
    Text = "Status: Ready",
    Size = new Size(150, 27)
};

StatusBarAdvPanel panel2 = new StatusBarAdvPanel
{
    Text = "Items: 0",
    Size = new Size(100, 27)
};

// Add panels to StatusBarAdv
statusBarAdv1.Controls.Add(panel1);
statusBarAdv1.Controls.Add(panel2);
```

**VB.NET:**
```vbnet
Dim panel1 As New StatusBarAdvPanel With {
    .Text = "Status: Ready",
    .Size = New Size(150, 27)
}

Dim panel2 As New StatusBarAdvPanel With {
    .Text = "Items: 0",
    .Size = New Size(100, 27)
}

' Add panels to StatusBarAdv
statusBarAdv1.Controls.Add(panel1)
statusBarAdv1.Controls.Add(panel2)
```

## Panel Spacing

The **Spacing** property controls the space between panels.

**C#:**
```csharp
// Set horizontal and vertical spacing
statusBarAdv1.Spacing = new Size(5, 5);

// Larger spacing for clearer separation
statusBarAdv1.Spacing = new Size(10, 5);

// No spacing (panels touch each other)
statusBarAdv1.Spacing = new Size(0, 0);
```

**VB.NET:**
```vbnet
' Set horizontal and vertical spacing
statusBarAdv1.Spacing = New Size(5, 5)

' Larger spacing for clearer separation
statusBarAdv1.Spacing = New Size(10, 5)

' No spacing (panels touch each other)
statusBarAdv1.Spacing = New Size(0, 0)
```

### Spacing Configuration Example

**C#:**
```csharp
public void SetupSpacedPanels()
{
    statusBarAdv1.Spacing = new Size(8, 4);
    
    StatusBarAdvPanel[] panels = new StatusBarAdvPanel[]
    {
        new StatusBarAdvPanel { Text = "Panel 1", Size = new Size(100, 25) },
        new StatusBarAdvPanel { Text = "Panel 2", Size = new Size(100, 25) },
        new StatusBarAdvPanel { Text = "Panel 3", Size = new Size(100, 25) }
    };
    
    foreach (var panel in panels)
    {
        statusBarAdv1.Controls.Add(panel);
    }
}
```

## CustomLayoutBounds

The **CustomLayoutBounds** property defines a custom rectangle for panel layout.

**C#:**
```csharp
// Define custom layout area (X, Y, Width, Height)
statusBarAdv1.CustomLayoutBounds = new Rectangle(5, 2, 100, 20);

// Position panels starting at X=10, Y=3 with 500px width
statusBarAdv1.CustomLayoutBounds = new Rectangle(10, 3, 500, 25);
```

**VB.NET:**
```vbnet
' Define custom layout area (X, Y, Width, Height)
statusBarAdv1.CustomLayoutBounds = New Rectangle(5, 2, 100, 20)

' Position panels starting at X=10, Y=3 with 500px width
statusBarAdv1.CustomLayoutBounds = New Rectangle(10, 3, 500, 25)
```

**Parameters:**
- **X**: Horizontal offset from left edge
- **Y**: Vertical offset from top edge
- **Width**: Maximum width for panel layout area
- **Height**: Maximum height for panel layout area

### CustomLayoutBounds Example

**C#:**
```csharp
public void ConfigureCustomLayout()
{
    // Reserve space on left for an icon or logo
    int leftMargin = 40;
    
    // Set custom layout bounds
    statusBarAdv1.CustomLayoutBounds = new Rectangle(
        leftMargin,  // X offset
        2,          // Y offset (slight padding from top)
        statusBarAdv1.Width - leftMargin - 20,  // Width (minus margins)
        statusBarAdv1.Height - 4  // Height (minus padding)
    );
    
    // Add panels
    statusBarAdv1.Controls.Add(new StatusBarAdvPanel 
    { 
        Text = "Application Ready",
        Size = new Size(150, 22)
    });
}
```

## Panel Alignment

The **Alignment** property controls how panels are positioned within the StatusBarAdv.

### Alignment Options

**FlowAlignment.Center:**
```csharp
statusBarAdv1.Alignment = FlowAlignment.Center;
```
Panels are centered horizontally in the status bar.

**FlowAlignment.Near:**
```csharp
statusBarAdv1.Alignment = FlowAlignment.Near;
```
Panels are aligned to the left (near) side.

**FlowAlignment.Far:**
```csharp
statusBarAdv1.Alignment = FlowAlignment.Far;
```
Panels are aligned to the right (far) side.

**FlowAlignment.ChildConstraints:**
```csharp
statusBarAdv1.Alignment = FlowAlignment.ChildConstraints;
```
Use **SetHAlign** method for custom per-panel alignment.

### Alignment Examples

**C#:**
```csharp
// Center all panels
statusBarAdv1.Alignment = FlowAlignment.Center;

// Align panels to left
statusBarAdv1.Alignment = FlowAlignment.Near;

// Align panels to right
statusBarAdv1.Alignment = FlowAlignment.Far;
```

**VB.NET:**
```vbnet
' Center all panels
statusBarAdv1.Alignment = FlowAlignment.Center

' Align panels to left
statusBarAdv1.Alignment = FlowAlignment.Near

' Align panels to right
statusBarAdv1.Alignment = FlowAlignment.Far
```

## SetHAlign Method

The **SetHAlign** method sets horizontal alignment for individual panels when **Alignment** is set to **ChildConstraints**.

**Method Signature:**
```csharp
void SetHAlign(Control control, HorzFlowAlign align)
```

**Parameters:**
- **control**: The panel to configure
- **align**: Alignment option (Left, Center, Right, Justify)

### HorzFlowAlign Options

- **HorzFlowAlign.Left**: Align panel to left
- **HorzFlowAlign.Center**: Center panel horizontally
- **HorzFlowAlign.Right**: Align panel to right
- **HorzFlowAlign.Justify**: Expand panel to fill available space

### SetHAlign Examples

**C#:**
```csharp
public void ConfigureCustomPanelAlignment()
{
    // Enable custom alignment
    statusBarAdv1.Alignment = FlowAlignment.ChildConstraints;
    
    // Create panels
    StatusBarAdvPanel leftPanel = new StatusBarAdvPanel
    {
        Text = "Status: Ready",
        Size = new Size(120, 25)
    };
    
    StatusBarAdvPanel centerPanel = new StatusBarAdvPanel
    {
        Text = "Processing",
        Size = new Size(100, 25)
    };
    
    StatusBarAdvPanel rightPanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortTime,
        Size = new Size(80, 25)
    };
    
    // Add panels
    statusBarAdv1.Controls.Add(leftPanel);
    statusBarAdv1.Controls.Add(centerPanel);
    statusBarAdv1.Controls.Add(rightPanel);
    
    // Set individual alignments
    statusBarAdv1.SetHAlign(leftPanel, HorzFlowAlign.Left);
    statusBarAdv1.SetHAlign(centerPanel, HorzFlowAlign.Center);
    statusBarAdv1.SetHAlign(rightPanel, HorzFlowAlign.Right);
}
```

**VB.NET:**
```vbnet
Public Sub ConfigureCustomPanelAlignment()
    ' Enable custom alignment
    statusBarAdv1.Alignment = FlowAlignment.ChildConstraints
    
    ' Create panels
    Dim leftPanel As New StatusBarAdvPanel With {
        .Text = "Status: Ready",
        .Size = New Size(120, 25)
    }
    
    Dim centerPanel As New StatusBarAdvPanel With {
        .Text = "Processing",
        .Size = New Size(100, 25)
    }
    
    Dim rightPanel As New StatusBarAdvPanel With {
        .PanelType = StatusBarAdvPanelType.ShortTime,
        .Size = New Size(80, 25)
    }
    
    ' Add panels
    statusBarAdv1.Controls.Add(leftPanel)
    statusBarAdv1.Controls.Add(centerPanel)
    statusBarAdv1.Controls.Add(rightPanel)
    
    ' Set individual alignments
    statusBarAdv1.SetHAlign(leftPanel, HorzFlowAlign.Left)
    statusBarAdv1.SetHAlign(centerPanel, HorzFlowAlign.Center)
    statusBarAdv1.SetHAlign(rightPanel, HorzFlowAlign.Right)
End Sub
```

### Justify Alignment

The **Justify** option expands panels to fill extra space:

**C#:**
```csharp
// Enable custom alignment
statusBarAdv1.Alignment = FlowAlignment.ChildConstraints;

StatusBarAdvPanel expandablePanel = new StatusBarAdvPanel
{
    Text = "This panel will expand to fill space",
    Size = new Size(200, 25)
};

statusBarAdv1.Controls.Add(expandablePanel);

// Make panel expand to fill available space
statusBarAdv1.SetHAlign(expandablePanel, HorzFlowAlign.Justify);
```

## Adding Child Controls

Besides StatusBarAdvPanel, you can add any WinForms control to StatusBarAdv.

### Adding Standard Controls

**C#:**
```csharp
// Add a ProgressBar
ProgressBar progressBar = new ProgressBar
{
    Size = new Size(150, 20),
    Style = ProgressBarStyle.Marquee
};
statusBarAdv1.Controls.Add(progressBar);

// Add a Label
Label statusLabel = new Label
{
    Text = "Processing...",
    AutoSize = true,
    TextAlign = ContentAlignment.MiddleLeft
};
statusBarAdv1.Controls.Add(statusLabel);

// Add a Button
Button actionButton = new Button
{
    Text = "Cancel",
    Size = new Size(60, 22)
};
statusBarAdv1.Controls.Add(actionButton);
```

**VB.NET:**
```vbnet
' Add a ProgressBar
Dim progressBar As New ProgressBar With {
    .Size = New Size(150, 20),
    .Style = ProgressBarStyle.Marquee
}
statusBarAdv1.Controls.Add(progressBar)

' Add a Label
Dim statusLabel As New Label With {
    .Text = "Processing...",
    .AutoSize = True,
    .TextAlign = ContentAlignment.MiddleLeft
}
statusBarAdv1.Controls.Add(statusLabel)

' Add a Button
Dim actionButton As New Button With {
    .Text = "Cancel",
    .Size = New Size(60, 22)
}
statusBarAdv1.Controls.Add(actionButton)
```

### Complete Status Bar with Mixed Controls

**C#:**
```csharp
public partial class DocumentEditorForm : Form
{
    private StatusBarAdv statusBar;
    private Label lineColLabel;
    private ProgressBar saveProgress;
    private StatusBarAdvPanel wordCountPanel;
    private StatusBarAdvPanel datePanel;
    
    public DocumentEditorForm()
    {
        InitializeComponent();
        SetupStatusBar();
    }
    
    private void SetupStatusBar()
    {
        // Create status bar
        statusBar = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 28,
            BackColor = Color.FromArgb(245, 245, 245),
            Spacing = new Size(10, 2)
        };
        
        // Line/column label
        lineColLabel = new Label
        {
            Text = "Ln 1, Col 1",
            Size = new Size(80, 24),
            TextAlign = ContentAlignment.MiddleLeft,
            AutoSize = false
        };
        statusBar.Controls.Add(lineColLabel);
        
        // Word count panel
        wordCountPanel = new StatusBarAdvPanel
        {
            Text = "Words: 0",
            Size = new Size(80, 24)
        };
        statusBar.Controls.Add(wordCountPanel);
        
        // Progress bar (initially hidden)
        saveProgress = new ProgressBar
        {
            Size = new Size(120, 20),
            Visible = false
        };
        statusBar.Controls.Add(saveProgress);
        
        // Date panel
        datePanel = new StatusBarAdvPanel
        {
            PanelType = StatusBarAdvPanelType.ShortDate,
            Size = new Size(90, 24)
        };
        statusBar.Controls.Add(datePanel);
        
        // Add to form
        this.Controls.Add(statusBar);
    }
    
    // Update line/column position
    public void UpdatePosition(int line, int column)
    {
        lineColLabel.Text = $"Ln {line}, Col {column}";
    }
    
    // Update word count
    public void UpdateWordCount(int count)
    {
        wordCountPanel.Text = $"Words: {count}";
    }
    
    // Show save progress
    public void ShowSaveProgress(bool show)
    {
        saveProgress.Visible = show;
        if (show)
        {
            saveProgress.Style = ProgressBarStyle.Marquee;
        }
    }
}
```

## Panel Sizing and Positioning

### Setting Panel Size

**C#:**
```csharp
// Fixed size
panel.Size = new Size(120, 25);

// Auto-size based on content
panel.AutoSize = true;

// Minimum size
panel.MinimumSize = new Size(80, 25);
```

### Dynamic Panel Sizing

**C#:**
```csharp
public void ResizePanelToContent(StatusBarAdvPanel panel, string text)
{
    using (Graphics g = panel.CreateGraphics())
    {
        SizeF textSize = g.MeasureString(text, panel.Font);
        
        // Add padding
        int width = (int)textSize.Width + 20;
        int height = (int)textSize.Height + 6;
        
        panel.Size = new Size(width, height);
        panel.Text = text;
    }
}
```

### Responsive Panel Layout

**C#:**
```csharp
public void ConfigureResponsiveLayout()
{
    statusBarAdv1.Alignment = FlowAlignment.ChildConstraints;
    
    // Left-aligned fixed panels
    StatusBarAdvPanel statusPanel = new StatusBarAdvPanel
    {
        Text = "Ready",
        Size = new Size(100, 25)
    };
    statusBarAdv1.Controls.Add(statusPanel);
    statusBarAdv1.SetHAlign(statusPanel, HorzFlowAlign.Left);
    
    // Center-aligned flexible panel
    StatusBarAdvPanel messagePanel = new StatusBarAdvPanel
    {
        Text = "Message area",
        Size = new Size(200, 25)
    };
    statusBarAdv1.Controls.Add(messagePanel);
    statusBarAdv1.SetHAlign(messagePanel, HorzFlowAlign.Justify);
    
    // Right-aligned fixed panels
    StatusBarAdvPanel timePanel = new StatusBarAdvPanel
    {
        PanelType = StatusBarAdvPanelType.ShortTime,
        Size = new Size(70, 25)
    };
    statusBarAdv1.Controls.Add(timePanel);
    statusBarAdv1.SetHAlign(timePanel, HorzFlowAlign.Right);
}
```

## Next Steps

After configuring panels and layout:

1. **Customize Appearance** → Read: [appearance-styling.md](appearance-styling.md)
   - Apply gradient backgrounds
   - Configure Metro colors
   - Add sizing grip

2. **Apply Borders and Themes** → Read: [borders-and-themes.md](borders-and-themes.md)
   - Set border styles
   - Apply Office2016 themes
   - Enable themed backgrounds
