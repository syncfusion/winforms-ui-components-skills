# Orientation and Layout

This guide covers orientation options and layout considerations for the ProgressBarAdv control.

## Progress Orientation

The `ProgressOrientation` property determines whether the progress bar displays horizontally or vertically.

### Horizontal Orientation

The default orientation displays progress from left to right:

```csharp
this.progressBarAdv1.ProgressOrientation = Orientation.Horizontal;
```

**VB.NET:**
```vb
Me.progressBarAdv1.ProgressOrientation = Orientation.Horizontal
```

**Typical dimensions:**
```csharp
progressBarAdv1.Size = new Size(300, 30); // Width > Height
```

**Best for:**
- Standard progress indicators
- Horizontal layouts
- Most common use case
- File operations, downloads, installations

### Vertical Orientation

Displays progress from bottom to top:

```csharp
this.progressBarAdv1.ProgressOrientation = Orientation.Vertical;
```

**VB.NET:**
```vb
Me.progressBarAdv1.ProgressOrientation = Orientation.Vertical
```

**Typical dimensions:**
```csharp
progressBarAdv1.Size = new Size(50, 200); // Height > Width
```

**Best for:**
- Sidebar indicators
- Volume/level meters
- Space-constrained layouts
- Vertical dashboards

## Layout Considerations

### Horizontal Progress Bar Layout

```csharp
// Full-width progress bar
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Dock = DockStyle.Top;
progressBar.Height = 25;
progressBar.ProgressOrientation = Orientation.Horizontal;
this.Controls.Add(progressBar);
```

### Vertical Progress Bar Layout

```csharp
// Full-height progress bar
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Dock = DockStyle.Left;
progressBar.Width = 40;
progressBar.ProgressOrientation = Orientation.Vertical;
this.Controls.Add(progressBar);
```

### Centered Progress Bar

```csharp
// Center in form
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Size = new Size(300, 30);
progressBar.Location = new Point(
    (this.ClientSize.Width - progressBar.Width) / 2,
    (this.ClientSize.Height - progressBar.Height) / 2
);
this.Controls.Add(progressBar);
```

## Text Orientation Coordination

Match text orientation with progress orientation:

```csharp
// Horizontal progress with horizontal text
progressBarAdv1.ProgressOrientation = Orientation.Horizontal;
progressBarAdv1.TextOrientation = Orientation.Horizontal;

// Vertical progress with vertical text
progressBarAdv1.ProgressOrientation = Orientation.Vertical;
progressBarAdv1.TextOrientation = Orientation.Vertical;
```

## Responsive Design

### Adapt to Form Size

```csharp
private void Form_Resize(object sender, EventArgs e)
{
    // Adjust progress bar width
    progressBarAdv1.Width = this.ClientSize.Width - 40;
}
```

### Auto-sizing

```csharp
// Use TableLayoutPanel for responsive layout
TableLayoutPanel panel = new TableLayoutPanel();
panel.Dock = DockStyle.Fill;
panel.ColumnCount = 1;
panel.RowCount = 2;
panel.RowStyles.Add(new RowStyle(SizeType.AutoSize));
panel.RowStyles.Add(new RowStyle(SizeType.Percent, 100));

ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Dock = DockStyle.Fill;
progressBar.Height = 30;

panel.Controls.Add(progressBar, 0, 0);
this.Controls.Add(panel);
```

## Common Layout Patterns

### Pattern 1: Bottom Status Bar

```csharp
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Dock = DockStyle.Bottom;
progressBar.Height = 25;
progressBar.TextStyle = ProgressBarTextStyles.Percentage;
progressBar.TextAlignment = TextAlignment.Center;
this.Controls.Add(progressBar);
```

### Pattern 2: Multiple Vertical Indicators

```csharp
// Create panel for vertical indicators
FlowLayoutPanel panel = new FlowLayoutPanel();
panel.FlowDirection = FlowDirection.LeftToRight;
panel.Dock = DockStyle.Left;
panel.Width = 150;

// Add multiple vertical progress bars
for (int i = 0; i < 3; i++)
{
    ProgressBarAdv vBar = new ProgressBarAdv();
    vBar.ProgressOrientation = Orientation.Vertical;
    vBar.Size = new Size(40, 200);
    vBar.Margin = new Padding(5);
    vBar.Value = (i + 1) * 25;
    panel.Controls.Add(vBar);
}

this.Controls.Add(panel);
```

### Pattern 3: Stacked Horizontal Progress Bars

```csharp
// Create multiple progress bars stacked vertically
string[] operations = { "Loading", "Processing", "Saving" };
int y = 20;

foreach (string operation in operations)
{
    Label label = new Label();
    label.Text = operation;
    label.Location = new Point(20, y);
    label.AutoSize = true;
    this.Controls.Add(label);
    
    ProgressBarAdv progressBar = new ProgressBarAdv();
    progressBar.Location = new Point(100, y);
    progressBar.Size = new Size(300, 20);
    progressBar.TextStyle = ProgressBarTextStyles.Percentage;
    this.Controls.Add(progressBar);
    
    y += 30;
}
```

## Best Practices

### Size Guidelines

**Horizontal:**
- Minimum width: 100px
- Recommended width: 200-400px
- Height: 20-40px

**Vertical:**
- Width: 30-60px
- Minimum height: 100px
- Recommended height: 150-300px

### Placement Guidelines

```csharp
// Good: Clear spacing
progressBar.Margin = new Padding(10);

// Good: Consistent alignment
progressBar.Anchor = AnchorStyles.Left | AnchorStyles.Right | AnchorStyles.Top;

// Good: Visible and accessible
progressBar.Location = new Point(20, 20);
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class OrientationDemo : Form
{
    private ProgressBarAdv horizontalProgress;
    private ProgressBarAdv verticalProgress;
    private Button toggleButton;
    
    public OrientationDemo()
    {
        this.Text = "Progress Bar Orientation Demo";
        this.Size = new Size(500, 400);
        
        InitializeControls();
    }
    
    private void InitializeControls()
    {
        // Horizontal progress bar
        horizontalProgress = new ProgressBarAdv();
        horizontalProgress.Location = new Point(50, 50);
        horizontalProgress.Size = new Size(300, 30);
        horizontalProgress.ProgressOrientation = Orientation.Horizontal;
        horizontalProgress.TextOrientation = Orientation.Horizontal;
        horizontalProgress.Value = 60;
        horizontalProgress.TextStyle = ProgressBarTextStyles.Percentage;
        this.Controls.Add(horizontalProgress);
        
        // Label for horizontal
        Label hLabel = new Label();
        hLabel.Text = "Horizontal:";
        hLabel.Location = new Point(50, 25);
        hLabel.AutoSize = true;
        this.Controls.Add(hLabel);
        
        // Vertical progress bar
        verticalProgress = new ProgressBarAdv();
        verticalProgress.Location = new Point(400, 50);
        verticalProgress.Size = new Size(40, 200);
        verticalProgress.ProgressOrientation = Orientation.Vertical;
        verticalProgress.TextOrientation = Orientation.Vertical;
        verticalProgress.Value = 75;
        verticalProgress.TextStyle = ProgressBarTextStyles.Percentage;
        this.Controls.Add(verticalProgress);
        
        // Label for vertical
        Label vLabel = new Label();
        vLabel.Text = "Vertical:";
        vLabel.Location = new Point(400, 25);
        vLabel.AutoSize = true;
        this.Controls.Add(vLabel);
        
        // Toggle button
        toggleButton = new Button();
        toggleButton.Text = "Update Progress";
        toggleButton.Location = new Point(50, 300);
        toggleButton.Click += ToggleButton_Click;
        this.Controls.Add(toggleButton);
    }
    
    private void ToggleButton_Click(object sender, EventArgs e)
    {
        Random rand = new Random();
        horizontalProgress.Value = rand.Next(0, 101);
        verticalProgress.Value = rand.Next(0, 101);
    }
}
```

## Next Steps

- **Text display:** See [text-display.md](text-display.md) for text orientation options
- **Appearance:** See [appearance-styling.md](appearance-styling.md) for styling
- **Themes:** See [themes.md](themes.md) for visual styles
