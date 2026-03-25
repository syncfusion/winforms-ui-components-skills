# Text and Marquee Animation

## Table of Contents
- [Overview](#overview)
- [Text Configuration](#text-configuration)
- [Marquee Animation](#marquee-animation)
- [Animation Settings](#animation-settings)
- [Animation Control Methods](#animation-control-methods)
- [Complete Marquee Examples](#complete-marquee-examples)

This guide covers text configuration and marquee animation features for StatusBarAdvPanel.

## Overview

## When to Read This

Read this guide when you need to:
- Configure panel text display
- Implement scrolling marquee text
- Control animation speed and direction
- Configure animation styles (Scroll, Slide, Alternate)
- Start and stop animations dynamically
- Create announcements or notifications with animated text
- Customize marquee timing and behavior

## Text Configuration

### Text Property

The `Text` property sets the displayed text in the panel (when PanelType is Custom).

**C#:**
```csharp
var textPanel = new StatusBarAdvPanel
{
    PanelType = StatusBarAdvPanelType.Custom,
    Text = "StatusBarAdvPanel",
    Size = new Size(180, 24),
    BackgroundColor = new BrushInfo(Color.LightBlue),
    Alignment = HorizontalAlignment.Left
};

// Update text dynamically
textPanel.Text = "Processing...";
textPanel.Text = "Complete!";
```

**VB.NET:**
```vb
Dim textPanel = New StatusBarAdvPanel With {
    .PanelType = StatusBarAdvPanelType.Custom,
    .Text = "StatusBarAdvPanel",
    .Size = New Size(180, 24),
    .BackgroundColor = New BrushInfo(Color.LightBlue),
    .Alignment = HorizontalAlignment.Left
}

' Update text dynamically
textPanel.Text = "Processing..."
textPanel.Text = "Complete!"
```

## Marquee Animation

Marquee animation creates scrolling text effects for announcements and notifications.

### IsMarquee Property

Enable marquee animation with the `IsMarquee` property.

**Property:**
- **Type:** `bool`
- **Default:** `false`
- **Effect:** When `true`, enables marquee-style text animation

**C#:**
```csharp
var marqueePanel = new StatusBarAdvPanel
{
    Text = "Welcome to our application! Check out the latest features...",
    IsMarquee = true,  // Enable marquee animation
    Size = new Size(300, 28),
    BackgroundColor = new BrushInfo(Color.LightYellow)
};

// Start animation
marqueePanel.StartAnimation();
```

**VB.NET:**
```vb
Dim marqueePanel = New StatusBarAdvPanel With {
    .Text = "Welcome to our application! Check out the latest features...",
    .IsMarquee = True,  ' Enable marquee animation
    .Size = New Size(300, 28),
    .BackgroundColor = New BrushInfo(Color.LightYellow)
}

' Start animation
marqueePanel.StartAnimation()
```

**Important:** IsMarquee must be set to `true` for animation settings to take effect.

## Animation Settings

### AnimationStyle Property

Specifies the animation style for marquee text.

**MarqueeStyle Enumeration:**

| Style | Description | Behavior |
|-------|-------------|----------|
| **Scroll** | Text scrolls continuously | Text moves across panel and wraps around |
| **Slide** | Text slides into view | Text slides in from one side and stops |
| **Alternate** | Text bounces back and forth | Text moves to edge, then reverses direction |

**C#:**
```csharp
// Scroll style (continuous scrolling)
var scrollPanel = new StatusBarAdvPanel
{
    Text = "This text scrolls continuously across the panel...",
    IsMarquee = true,
    AnimationStyle = MarqueeStyle.Scroll,
    Size = new Size(300, 26),
    BackgroundColor = new BrushInfo(Color.AliceBlue)
};

// Slide style (slides in and stops)
var slidePanel = new StatusBarAdvPanel
{
    Text = "This text slides in from the edge...",
    IsMarquee = true,
    AnimationStyle = MarqueeStyle.Slide,
    Size = new Size(300, 26),
    BackgroundColor = new BrushInfo(Color.LightGreen)
};

// Alternate style (bounces back and forth)
var alternatePanel = new StatusBarAdvPanel
{
    Text = "This text bounces back and forth...",
    IsMarquee = true,
    AnimationStyle = MarqueeStyle.Alternate,
    Size = new Size(300, 26),
    BackgroundColor = new BrushInfo(Color.LightCoral)
};
```

**VB.NET:**
```vb
' Scroll style (continuous scrolling)
Dim scrollPanel = New StatusBarAdvPanel With {
    .Text = "This text scrolls continuously across the panel...",
    .IsMarquee = True,
    .AnimationStyle = MarqueeStyle.Scroll,
    .Size = New Size(300, 26),
    .BackgroundColor = New BrushInfo(Color.AliceBlue)
}

' Slide style (slides in and stops)
Dim slidePanel = New StatusBarAdvPanel With {
    .Text = "This text slides in from the edge...",
    .IsMarquee = True,
    .AnimationStyle = MarqueeStyle.Slide,
    .Size = New Size(300, 26),
    .BackgroundColor = New BrushInfo(Color.LightGreen)
}

' Alternate style (bounces back and forth)
Dim alternatePanel = New StatusBarAdvPanel With {
    .Text = "This text bounces back and forth...",
    .IsMarquee = True,
    .AnimationStyle = MarqueeStyle.Alternate,
    .Size = New Size(300, 26),
    .BackgroundColor = New BrushInfo(Color.LightCoral)
}
```

### AnimationDirection Property

Controls the direction of marquee animation.

**MarqueeDirection Enumeration:**

| Direction | Description |
|-----------|-------------|
| **Left** | Text moves from right to left |
| **Right** | Text moves from left to right |

**C#:**
```csharp
// Left direction (right to left)
var leftMarquee = new StatusBarAdvPanel
{
    Text = "Moving from right to left >>>",
    IsMarquee = true,
    AnimationDirection = MarqueeDirection.Left,
    AnimationStyle = MarqueeStyle.Scroll,
    Size = new Size(300, 26)
};

// Right direction (left to right)
var rightMarquee = new StatusBarAdvPanel
{
    Text = "<<< Moving from left to right",
    IsMarquee = true,
    AnimationDirection = MarqueeDirection.Right,
    AnimationStyle = MarqueeStyle.Scroll,
    Size = new Size(300, 26)
};
```

**VB.NET:**
```vb
' Left direction (right to left)
Dim leftMarquee = New StatusBarAdvPanel With {
    .Text = "Moving from right to left >>>",
    .IsMarquee = True,
    .AnimationDirection = MarqueeDirection.Left,
    .AnimationStyle = MarqueeStyle.Scroll,
    .Size = New Size(300, 26)
}

' Right direction (left to right)
Dim rightMarquee = New StatusBarAdvPanel With {
    .Text = "<<< Moving from left to right",
    .IsMarquee = True,
    .AnimationDirection = MarqueeDirection.Right,
    .AnimationStyle = MarqueeStyle.Scroll,
    .Size = New Size(300, 26)
}
```

### AnimationSpeed Property

Controls how fast the text animates (higher value = faster).

**Property:**
- **Type:** `int`
- **Default:** Moderate speed
- **Range:** 1 (slow) to 10+ (very fast)

**C#:**
```csharp
// Slow animation
var slowPanel = new StatusBarAdvPanel
{
    Text = "Slow scrolling text...",
    IsMarquee = true,
    AnimationSpeed = 2,  // Slow
    Size = new Size(300, 26)
};

// Medium animation
var mediumPanel = new StatusBarAdvPanel
{
    Text = "Medium speed text...",
    IsMarquee = true,
    AnimationSpeed = 5,  // Medium
    Size = new Size(300, 26)
};

// Fast animation
var fastPanel = new StatusBarAdvPanel
{
    Text = "Fast scrolling text!",
    IsMarquee = true,
    AnimationSpeed = 9,  // Fast
    Size = new Size(300, 26)
};
```

**VB.NET:**
```vb
' Slow animation
Dim slowPanel = New StatusBarAdvPanel With {
    .Text = "Slow scrolling text...",
    .IsMarquee = True,
    .AnimationSpeed = 2,  ' Slow
    .Size = New Size(300, 26)
}

' Medium animation
Dim mediumPanel = New StatusBarAdvPanel With {
    .Text = "Medium speed text...",
    .IsMarquee = True,
    .AnimationSpeed = 5,  ' Medium
    .Size = New Size(300, 26)
}

' Fast animation
Dim fastPanel = New StatusBarAdvPanel With {
    .Text = "Fast scrolling text!",
    .IsMarquee = True,
    .AnimationSpeed = 9,  ' Fast
    .Size = New Size(300, 26)
}
```

### AnimationDelay Property

Sets the delay (in milliseconds) before animation starts.

**Property:**
- **Type:** `int`
- **Default:** 0 (no delay)
- **Unit:** Milliseconds

**C#:**
```csharp
// Animation with 2-second delay
var delayedPanel = new StatusBarAdvPanel
{
    Text = "This starts after a 2-second delay...",
    IsMarquee = true,
    AnimationDelay = 2000,  // 2000 milliseconds = 2 seconds
    AnimationSpeed = 5,
    Size = new Size(300, 26),
    BackgroundColor = new BrushInfo(Color.LightBlue)
};

// Start animation (will wait 2 seconds before beginning)
delayedPanel.StartAnimation();
```

**VB.NET:**
```vb
' Animation with 2-second delay
Dim delayedPanel = New StatusBarAdvPanel With {
    .Text = "This starts after a 2-second delay...",
    .IsMarquee = True,
    .AnimationDelay = 2000,  ' 2000 milliseconds = 2 seconds
    .AnimationSpeed = 5,
    .Size = New Size(300, 26),
    .BackgroundColor = New BrushInfo(Color.LightBlue)
}

' Start animation (will wait 2 seconds before beginning)
delayedPanel.StartAnimation()
```

## Animation Control Methods

### StartAnimation() Method

Starts the marquee animation.

**Method Signature:**
```csharp
public void StartAnimation()
```

**C#:**
```csharp
// Start animation on form load
private void Form1_Load(object sender, EventArgs e)
{
    marqueePanel.StartAnimation();
}

// Start animation on button click
private void btnStartMarquee_Click(object sender, EventArgs e)
{
    marqueePanel.StartAnimation();
}
```

**VB.NET:**
```vb
' Start animation on form load
Private Sub Form1_Load(sender As Object, e As EventArgs)
    marqueePanel.StartAnimation()
End Sub

' Start animation on button click
Private Sub btnStartMarquee_Click(sender As Object, e As EventArgs)
    marqueePanel.StartAnimation()
End Sub
```

### StopAnimation() Method

Stops the marquee animation and restores text to original position.

**Method Signature:**
```csharp
public void StopAnimation()
```

**C#:**
```csharp
// Stop animation on button click
private void btnStopMarquee_Click(object sender, EventArgs e)
{
    marqueePanel.StopAnimation();
}

// Stop animation when form closes
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    if (marqueePanel != null && marqueePanel.IsMarquee)
    {
        marqueePanel.StopAnimation();
    }
}
```

**VB.NET:**
```vb
' Stop animation on button click
Private Sub btnStopMarquee_Click(sender As Object, e As EventArgs)
    marqueePanel.StopAnimation()
End Sub

' Stop animation when form closes
Private Sub Form1_FormClosing(sender As Object, e As FormClosingEventArgs)
    If marqueePanel IsNot Nothing AndAlso marqueePanel.IsMarquee Then
        marqueePanel.StopAnimation()
    End If
End Sub
```

## Complete Marquee Examples

### Example 1: News Ticker with Scroll Animation

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class NewsTickerForm : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel newsPanel;
    private Button btnStart;
    private Button btnStop;
    
    public NewsTickerForm()
    {
        InitializeComponent();
        SetupNewsTicker();
    }
    
    private void SetupNewsTicker()
    {
        // Create StatusBarAdv
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 32,
            BackgroundColor = new BrushInfo(Color.FromArgb(30, 30, 30))
        };
        
        // Create news ticker panel
        newsPanel = new StatusBarAdvPanel
        {
            Text = "BREAKING NEWS: Latest updates available now! " +
                   "Visit our website for more information. " +
                   "Thank you for using our application!",
            IsMarquee = true,
            AnimationStyle = MarqueeStyle.Scroll,
            AnimationDirection = MarqueeDirection.Left,
            AnimationSpeed = 4,
            AnimationDelay = 500,
            Size = new Size(600, 28),
            BackgroundColor = new BrushInfo(Color.FromArgb(45, 45, 48)),
            ForeColor = Color.White,
            HAlign = HorzFlowAlign.Left
        };
        
        statusBarAdv1.Controls.Add(newsPanel);
        this.Controls.Add(statusBarAdv1);
        
        // Add control buttons
        btnStart = new Button
        {
            Text = "Start News",
            Location = new Point(10, 10),
            Size = new Size(100, 30)
        };
        btnStart.Click += (s, e) => newsPanel.StartAnimation();
        
        btnStop = new Button
        {
            Text = "Stop News",
            Location = new Point(120, 10),
            Size = new Size(100, 30)
        };
        btnStop.Click += (s, e) => newsPanel.StopAnimation();
        
        this.Controls.Add(btnStart);
        this.Controls.Add(btnStop);
    }
    
    // Update news text
    public void UpdateNews(string newsText)
    {
        bool wasAnimating = newsPanel.IsMarquee;
        if (wasAnimating)
        {
            newsPanel.StopAnimation();
        }
        
        newsPanel.Text = newsText;
        
        if (wasAnimating)
        {
            newsPanel.StartAnimation();
        }
    }
    
    private void InitializeComponent()
    {
        this.Text = "News Ticker Demo";
        this.Size = new Size(700, 400);
        this.Load += (s, e) => newsPanel.StartAnimation();
        this.FormClosing += (s, e) => newsPanel.StopAnimation();
    }
}
```

### Example 2: Multi-Style Marquee Comparison

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class MarqueeStylesForm : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel scrollPanel;
    private StatusBarAdvPanel slidePanel;
    private StatusBarAdvPanel alternatePanel;
    
    public MarqueeStylesForm()
    {
        InitializeComponent();
        SetupMarqueeStyles();
    }
    
    private void SetupMarqueeStyles()
    {
        // Create StatusBarAdv
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 90,
            BackgroundColor = new BrushInfo(Color.WhiteSmoke)
        };
        
        // Scroll style panel
        scrollPanel = new StatusBarAdvPanel
        {
            Text = "SCROLL: This text scrolls continuously across the panel...",
            IsMarquee = true,
            AnimationStyle = MarqueeStyle.Scroll,
            AnimationDirection = MarqueeDirection.Left,
            AnimationSpeed = 5,
            Size = new Size(650, 26),
            BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.AliceBlue,
                Color.LightSkyBlue
            ),
            Location = new Point(10, 5)
        };
        
        // Slide style panel
        slidePanel = new StatusBarAdvPanel
        {
            Text = "SLIDE: This text slides in and stops at the end...",
            IsMarquee = true,
            AnimationStyle = MarqueeStyle.Slide,
            AnimationDirection = MarqueeDirection.Left,
            AnimationSpeed = 6,
            Size = new Size(650, 26),
            BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.LightGreen,
                Color.PaleGreen
            ),
            Location = new Point(10, 33)
        };
        
        // Alternate style panel
        alternatePanel = new StatusBarAdvPanel
        {
            Text = "ALTERNATE: This text bounces back and forth...",
            IsMarquee = true,
            AnimationStyle = MarqueeStyle.Alternate,
            AnimationDirection = MarqueeDirection.Right,
            AnimationSpeed = 7,
            Size = new Size(650, 26),
            BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.LightCoral,
                Color.LightPink
            ),
            Location = new Point(10, 61)
        };
        
        statusBarAdv1.Controls.Add(scrollPanel);
        statusBarAdv1.Controls.Add(slidePanel);
        statusBarAdv1.Controls.Add(alternatePanel);
        this.Controls.Add(statusBarAdv1);
    }
    
    private void InitializeComponent()
    {
        this.Text = "Marquee Styles Comparison";
        this.Size = new Size(700, 450);
        
        // Start all animations on load
        this.Load += (s, e) =>
        {
            scrollPanel.StartAnimation();
            slidePanel.StartAnimation();
            alternatePanel.StartAnimation();
        };
        
        // Stop all animations on close
        this.FormClosing += (s, e) =>
        {
            scrollPanel.StopAnimation();
            slidePanel.StopAnimation();
            alternatePanel.StopAnimation();
        };
    }
}
```

### Example 3: Controllable Marquee with Speed Adjustment

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;
using System.Drawing;
using System.Windows.Forms;

public class ControllableMarqueeForm : Form
{
    private StatusBarAdv statusBarAdv1;
    private StatusBarAdvPanel marqueePanel;
    private Button btnStart;
    private Button btnStop;
    private TrackBar trackSpeed;
    private Label lblSpeed;
    private ComboBox cmbStyle;
    private ComboBox cmbDirection;
    
    public ControllableMarqueeForm()
    {
        InitializeComponent();
        SetupControls();
        SetupMarquee();
    }
    
    private void SetupMarquee()
    {
        statusBarAdv1 = new StatusBarAdv
        {
            Dock = DockStyle.Bottom,
            Height = 35,
            BackgroundColor = new BrushInfo(Color.FromArgb(240, 240, 240))
        };
        
        marqueePanel = new StatusBarAdvPanel
        {
            Text = "Customizable marquee text - adjust settings above to see effects!",
            IsMarquee = true,
            AnimationStyle = MarqueeStyle.Scroll,
            AnimationDirection = MarqueeDirection.Left,
            AnimationSpeed = 5,
            Size = new Size(650, 30),
            BackgroundColor = new BrushInfo(
                GradientStyle.Horizontal,
                Color.FromArgb(220, 235, 250),
                Color.FromArgb(180, 215, 245)
            ),
            ForeColor = Color.FromArgb(30, 57, 91)
        };
        
        statusBarAdv1.Controls.Add(marqueePanel);
        this.Controls.Add(statusBarAdv1);
    }
    
    private void SetupControls()
    {
        // Start button
        btnStart = new Button
        {
            Text = "Start",
            Location = new Point(10, 10),
            Size = new Size(80, 30)
        };
        btnStart.Click += (s, e) => marqueePanel.StartAnimation();
        
        // Stop button
        btnStop = new Button
        {
            Text = "Stop",
            Location = new Point(100, 10),
            Size = new Size(80, 30)
        };
        btnStop.Click += (s, e) => marqueePanel.StopAnimation();
        
        // Speed label
        lblSpeed = new Label
        {
            Text = "Speed: 5",
            Location = new Point(10, 50),
            Size = new Size(80, 20)
        };
        
        // Speed trackbar
        trackSpeed = new TrackBar
        {
            Minimum = 1,
            Maximum = 10,
            Value = 5,
            Location = new Point(10, 70),
            Size = new Size(200, 45)
        };
        trackSpeed.ValueChanged += (s, e) =>
        {
            bool wasAnimating = marqueePanel.IsMarquee;
            if (wasAnimating) marqueePanel.StopAnimation();
            
            marqueePanel.AnimationSpeed = trackSpeed.Value;
            lblSpeed.Text = $"Speed: {trackSpeed.Value}";
            
            if (wasAnimating) marqueePanel.StartAnimation();
        };
        
        // Style combo box
        cmbStyle = new ComboBox
        {
            Location = new Point(220, 70),
            Size = new Size(120, 25),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        cmbStyle.Items.AddRange(new object[] { "Scroll", "Slide", "Alternate" });
        cmbStyle.SelectedIndex = 0;
        cmbStyle.SelectedIndexChanged += (s, e) =>
        {
            bool wasAnimating = marqueePanel.IsMarquee;
            if (wasAnimating) marqueePanel.StopAnimation();
            
            marqueePanel.AnimationStyle = (MarqueeStyle)cmbStyle.SelectedIndex;
            
            if (wasAnimating) marqueePanel.StartAnimation();
        };
        
        // Direction combo box
        cmbDirection = new ComboBox
        {
            Location = new Point(350, 70),
            Size = new Size(100, 25),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        cmbDirection.Items.AddRange(new object[] { "Left", "Right" });
        cmbDirection.SelectedIndex = 0;
        cmbDirection.SelectedIndexChanged += (s, e) =>
        {
            bool wasAnimating = marqueePanel.IsMarquee;
            if (wasAnimating) marqueePanel.StopAnimation();
            
            marqueePanel.AnimationDirection = (MarqueeDirection)cmbDirection.SelectedIndex;
            
            if (wasAnimating) marqueePanel.StartAnimation();
        };
        
        this.Controls.Add(btnStart);
        this.Controls.Add(btnStop);
        this.Controls.Add(lblSpeed);
        this.Controls.Add(trackSpeed);
        this.Controls.Add(cmbStyle);
        this.Controls.Add(cmbDirection);
    }
    
    private void InitializeComponent()
    {
        this.Text = "Controllable Marquee Demo";
        this.Size = new Size(700, 450);
        this.Load += (s, e) => marqueePanel.StartAnimation();
        this.FormClosing += (s, e) => marqueePanel.StopAnimation();
    }
}
```

## Animation Control Patterns

### Pattern 1: Toggle Animation

**C#:**
```csharp
private bool isAnimating = false;

private void ToggleAnimation()
{
    if (isAnimating)
    {
        marqueePanel.StopAnimation();
        btnToggle.Text = "Start";
    }
    else
    {
        marqueePanel.StartAnimation();
        btnToggle.Text = "Stop";
    }
    isAnimating = !isAnimating;
}
```

### Pattern 2: Automatic Start/Stop Based on Events

**C#:**
```csharp
// Start marquee when mouse enters
private void marqueePanel_MouseEnter(object sender, EventArgs e)
{
    if (marqueePanel.IsMarquee)
    {
        marqueePanel.StartAnimation();
    }
}

// Stop marquee when mouse leaves
private void marqueePanel_MouseLeave(object sender, EventArgs e)
{
    if (marqueePanel.IsMarquee)
    {
        marqueePanel.StopAnimation();
    }
}
```

### Pattern 3: Update Text While Animating

**C#:**
```csharp
private void UpdateMarqueeText(string newText)
{
    // Stop animation
    marqueePanel.StopAnimation();
    
    // Update text
    marqueePanel.Text = newText;
    
    // Restart animation
    marqueePanel.StartAnimation();
}
```

## Next Steps

After configuring text and marquee animation, explore:
- **[Alignment and Borders](alignment-and-borders.md)** - Configure panel alignment and borders
- **[Themes, ToolTips, and Events](themes-tooltips-events.md)** - Add themes and event handling
