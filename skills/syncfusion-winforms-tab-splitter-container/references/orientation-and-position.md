# TabSplitterContainer Orientation and Position

## Table of Contents
- [Orientation Overview](#orientation-overview)
- [Horizontal Orientation](#horizontal-orientation)
- [Vertical Orientation](#vertical-orientation)
- [Changing Orientation Programmatically](#changing-orientation-programmatically)
- [SplitterPosition Property](#splitterposition-property)
- [Design-Time Splitter Adjustment](#design-time-splitter-adjustment)
- [Runtime Splitter Position Management](#runtime-splitter-position-management)
- [Layout Considerations](#layout-considerations)
- [Best Practices](#best-practices)

## Orientation Overview

The `Orientation` property determines how the TabSplitterContainer arranges its primary and secondary page collections. This fundamental layout decision affects both the visual appearance and the user interaction patterns of your split-view interface.

**Property Type:** `System.Windows.Forms.Orientation`  
**Default Value:** `Orientation.Horizontal`

**Available Options:**
- `Orientation.Horizontal`: Panes arranged side-by-side (left and right)
- `Orientation.Vertical`: Panes arranged top-to-bottom

**When to Choose Each Orientation:**

**Choose Horizontal when:**
- Working with text editors or code editors (maximizes line width)
- Comparing two documents side-by-side
- Showing code and preview together (common IDE pattern)
- Users need to see both panes simultaneously without scrolling
- Wide-screen displays are common

**Choose Vertical when:**
- Working with long documents or logs (maximizes visible lines)
- Showing content with narrow columns
- Implementing a code-above/output-below pattern
- Limited horizontal screen space
- Building tool windows or panels

## Horizontal Orientation

Horizontal orientation is the default and most common layout, arranging panes side-by-side.

### Visual Layout

```
┌──────────────────────┬──────────────────────┐
│                      │                      │
│   Primary Pages      │   Secondary Pages    │
│   (Left Pane)        │   (Right Pane)       │
│                      │                      │
│                      │                      │
└──────────────────────┴──────────────────────┘
```

### Setting Horizontal Orientation

```csharp
// Set horizontal orientation (default)
this.tabSplitterContainer1.Orientation = System.Windows.Forms.Orientation.Horizontal;

// Alternative using namespace
using System.Windows.Forms;

this.tabSplitterContainer1.Orientation = Orientation.Horizontal;
```

### Horizontal Orientation Use Cases

**Code Editor with Preview:**
```csharp
private void SetupCodeAndPreview()
{
    this.tabSplitterContainer1.Orientation = Orientation.Horizontal;
    
    // Primary: Code editor (left)
    TabSplitterPage codePage = new TabSplitterPage();
    codePage.Text = "Code";
    
    TextBox codeEditor = new TextBox();
    codeEditor.Multiline = true;
    codeEditor.Dock = DockStyle.Fill;
    codeEditor.Font = new Font("Consolas", 10F);
    codeEditor.WordWrap = false;
    codePage.Controls.Add(codeEditor);
    
    // Secondary: Browser preview (right)
    TabSplitterPage previewPage = new TabSplitterPage();
    previewPage.Text = "Preview";
    
    WebBrowser browser = new WebBrowser();
    browser.Dock = DockStyle.Fill;
    previewPage.Controls.Add(browser);
    
    this.tabSplitterContainer1.PrimaryPages.Add(codePage);
    this.tabSplitterContainer1.SecondaryPages.Add(previewPage);
}
```

**Document Comparison:**
```csharp
private void SetupDocumentComparison()
{
    this.tabSplitterContainer1.Orientation = Orientation.Horizontal;
    
    // Left: Original document
    TabSplitterPage originalPage = new TabSplitterPage();
    originalPage.Text = "Original";
    RichTextBox originalText = new RichTextBox();
    originalText.Dock = DockStyle.Fill;
    originalText.ReadOnly = true;
    originalPage.Controls.Add(originalText);
    
    // Right: Modified document
    TabSplitterPage modifiedPage = new TabSplitterPage();
    modifiedPage.Text = "Modified";
    RichTextBox modifiedText = new RichTextBox();
    modifiedText.Dock = DockStyle.Fill;
    modifiedPage.Controls.Add(modifiedText);
    
    this.tabSplitterContainer1.PrimaryPages.Add(originalPage);
    this.tabSplitterContainer1.SecondaryPages.Add(modifiedPage);
}
```

## Vertical Orientation

Vertical orientation stacks panes top-to-bottom, useful for specific workflows and narrow layouts.

### Visual Layout

```
┌────────────────────────────────────────┐
│        Primary Pages (Top Pane)        │
│                                        │
├────────────────────────────────────────┤
│      Secondary Pages (Bottom Pane)     │
│                                        │
└────────────────────────────────────────┘
```

### Setting Vertical Orientation

```csharp
// Set vertical orientation
this.tabSplitterContainer1.Orientation = System.Windows.Forms.Orientation.Vertical;

// Alternative using namespace
using System.Windows.Forms;

this.tabSplitterContainer1.Orientation = Orientation.Vertical;
```

### Vertical Orientation Use Cases

**Code and Output Console:**
```csharp
private void SetupCodeAndConsole()
{
    this.tabSplitterContainer1.Orientation = Orientation.Vertical;
    
    // Top: Code editor
    TabSplitterPage codePage = new TabSplitterPage();
    codePage.Text = "Code";
    
    TextBox codeEditor = new TextBox();
    codeEditor.Multiline = true;
    codeEditor.Dock = DockStyle.Fill;
    codeEditor.Font = new Font("Consolas", 10F);
    codePage.Controls.Add(codeEditor);
    
    // Bottom: Console output
    TabSplitterPage consolePage = new TabSplitterPage();
    consolePage.Text = "Output";
    
    TextBox console = new TextBox();
    console.Multiline = true;
    console.Dock = DockStyle.Fill;
    console.ReadOnly = true;
    console.BackColor = Color.Black;
    console.ForeColor = Color.LightGreen;
    console.Font = new Font("Consolas", 9F);
    consolePage.Controls.Add(console);
    
    this.tabSplitterContainer1.PrimaryPages.Add(codePage);
    this.tabSplitterContainer1.SecondaryPages.Add(consolePage);
}
```

**Log Viewer with Details:**
```csharp
private void SetupLogViewer()
{
    this.tabSplitterContainer1.Orientation = Orientation.Vertical;
    
    // Top: Log list
    TabSplitterPage logListPage = new TabSplitterPage();
    logListPage.Text = "Logs";
    
    ListView logList = new ListView();
    logList.Dock = DockStyle.Fill;
    logList.View = View.Details;
    logList.Columns.Add("Time", 100);
    logList.Columns.Add("Level", 80);
    logList.Columns.Add("Message", 400);
    logListPage.Controls.Add(logList);
    
    // Bottom: Log details
    TabSplitterPage detailsPage = new TabSplitterPage();
    detailsPage.Text = "Details";
    
    RichTextBox details = new RichTextBox();
    details.Dock = DockStyle.Fill;
    details.ReadOnly = true;
    detailsPage.Controls.Add(details);
    
    this.tabSplitterContainer1.PrimaryPages.Add(logListPage);
    this.tabSplitterContainer1.SecondaryPages.Add(detailsPage);
}
```

## Changing Orientation Programmatically

You can change orientation at runtime to adapt to user preferences or different workflows.

### Basic Orientation Toggle

```csharp
private void btnToggleOrientation_Click(object sender, EventArgs e)
{
    if (this.tabSplitterContainer1.Orientation == Orientation.Horizontal)
    {
        this.tabSplitterContainer1.Orientation = Orientation.Vertical;
        btnToggleOrientation.Text = "Switch to Horizontal";
    }
    else
    {
        this.tabSplitterContainer1.Orientation = Orientation.Horizontal;
        btnToggleOrientation.Text = "Switch to Vertical";
    }
}
```

### Orientation Based on Form Size

```csharp
private void Form_Resize(object sender, EventArgs e)
{
    // Use vertical orientation for narrow windows
    if (this.Width < 800)
    {
        this.tabSplitterContainer1.Orientation = Orientation.Vertical;
    }
    else
    {
        this.tabSplitterContainer1.Orientation = Orientation.Horizontal;
    }
}

private void Form_Load(object sender, EventArgs e)
{
    // Set initial orientation based on screen aspect ratio
    double aspectRatio = (double)this.Width / this.Height;
    
    if (aspectRatio > 1.5) // Wide screen
    {
        this.tabSplitterContainer1.Orientation = Orientation.Horizontal;
    }
    else // Narrow or square screen
    {
        this.tabSplitterContainer1.Orientation = Orientation.Vertical;
    }
}
```

### Orientation with User Preferences

```csharp
private void LoadUserPreferences()
{
    // Load from settings
    if (Properties.Settings.Default.PreferVerticalLayout)
    {
        this.tabSplitterContainer1.Orientation = Orientation.Vertical;
    }
    else
    {
        this.tabSplitterContainer1.Orientation = Orientation.Horizontal;
    }
}

private void SaveUserPreferences()
{
    // Save to settings
    Properties.Settings.Default.PreferVerticalLayout = 
        (this.tabSplitterContainer1.Orientation == Orientation.Vertical);
    Properties.Settings.Default.Save();
}
```

## SplitterPosition Property

The `SplitterPosition` property controls the position of the splitter bar, determining how much space each pane receives.

**Property Type:** `int` (position in pixels)  
**Default Value:** Approximately 50% of container size

**Important Notes:**
- The value represents the pixel distance from the left edge (horizontal) or top edge (vertical)
- The position is relative to the TabSplitterContainer's size
- Users can drag the splitter bar to adjust position manually
- The property value updates when users drag the splitter

### Setting Splitter Position

```csharp
// Set splitter at 200 pixels from left (horizontal) or top (vertical)
this.tabSplitterContainer1.SplitterPosition = 200;

// Set splitter to approximately 1/3 of width
int oneThird = this.tabSplitterContainer1.Width / 3;
this.tabSplitterContainer1.SplitterPosition = oneThird;

// Set splitter to 60/40 split
int sixtyPercent = (int)(this.tabSplitterContainer1.Width * 0.6);
this.tabSplitterContainer1.SplitterPosition = sixtyPercent;
```

### Getting Current Splitter Position

```csharp
// Read current position
int currentPosition = this.tabSplitterContainer1.SplitterPosition;

// Calculate percentage
double percentage = 0;
if (this.tabSplitterContainer1.Orientation == Orientation.Horizontal)
{
    percentage = (double)currentPosition / this.tabSplitterContainer1.Width * 100;
}
else
{
    percentage = (double)currentPosition / this.tabSplitterContainer1.Height * 100;
}

Console.WriteLine($"Splitter at {percentage:F1}%");
```

## Design-Time Splitter Adjustment

At design time, you can visually adjust the splitter position:

1. **Select the TabSplitterContainer** in the form designer
2. **Locate the splitter bar** between the two panes
3. **Hover over the splitter bar** until the cursor changes to a resize cursor (↔ or ↕)
4. **Click and drag** to adjust the position
5. **Release** to set the new position

The `SplitterPosition` property in the Properties window updates automatically as you drag.

**Design-Time Best Practice:**
Set a reasonable default position that works for typical use cases, knowing users can adjust it at runtime.

## Runtime Splitter Position Management

### Responsive Splitter Positioning

```csharp
private void Form_Resize(object sender, EventArgs e)
{
    // Maintain proportional split
    if (this.tabSplitterContainer1.Orientation == Orientation.Horizontal)
    {
        // Keep primary pane at 60% of width
        this.tabSplitterContainer1.SplitterPosition = 
            (int)(this.tabSplitterContainer1.Width * 0.6);
    }
    else
    {
        // Keep primary pane at 70% of height
        this.tabSplitterContainer1.SplitterPosition = 
            (int)(this.tabSplitterContainer1.Height * 0.7);
    }
}
```

### Context-Specific Positioning

```csharp
private void LoadDocument(DocumentType docType)
{
    switch (docType)
    {
        case DocumentType.Code:
            // Code view gets more space
            this.tabSplitterContainer1.SplitterPosition = 
                (int)(this.tabSplitterContainer1.Width * 0.7);
            break;
            
        case DocumentType.Design:
            // Design view gets more space
            this.tabSplitterContainer1.SplitterPosition = 
                (int)(this.tabSplitterContainer1.Width * 0.3);
            break;
            
        case DocumentType.Comparison:
            // Equal split for comparison
            this.tabSplitterContainer1.SplitterPosition = 
                this.tabSplitterContainer1.Width / 2;
            break;
    }
}
```

### Persisting Splitter Position

```csharp
private void Form_Load(object sender, EventArgs e)
{
    // Restore saved position
    if (Properties.Settings.Default.SplitterPosition > 0)
    {
        this.tabSplitterContainer1.SplitterPosition = 
            Properties.Settings.Default.SplitterPosition;
    }
}

private void Form_FormClosing(object sender, FormClosingEventArgs e)
{
    // Save current position
    Properties.Settings.Default.SplitterPosition = 
        this.tabSplitterContainer1.SplitterPosition;
    Properties.Settings.Default.Save();
}
```

### Animated Splitter Movement

```csharp
private Timer splitterAnimationTimer;
private int targetPosition;
private int animationStep;

private void AnimateSplitterTo(int targetPos)
{
    targetPosition = targetPos;
    int currentPos = this.tabSplitterContainer1.SplitterPosition;
    animationStep = (targetPosition - currentPos) / 10; // 10 steps
    
    if (animationStep == 0)
        animationStep = (targetPosition > currentPos) ? 1 : -1;
    
    splitterAnimationTimer = new Timer();
    splitterAnimationTimer.Interval = 20; // 20ms per step
    splitterAnimationTimer.Tick += SplitterAnimationTimer_Tick;
    splitterAnimationTimer.Start();
}

private void SplitterAnimationTimer_Tick(object sender, EventArgs e)
{
    int currentPos = this.tabSplitterContainer1.SplitterPosition;
    
    if (Math.Abs(currentPos - targetPosition) <= Math.Abs(animationStep))
    {
        this.tabSplitterContainer1.SplitterPosition = targetPosition;
        splitterAnimationTimer.Stop();
        splitterAnimationTimer.Dispose();
    }
    else
    {
        this.tabSplitterContainer1.SplitterPosition = currentPos + animationStep;
    }
}

// Usage
private void btnFocusCode_Click(object sender, EventArgs e)
{
    AnimateSplitterTo((int)(this.tabSplitterContainer1.Width * 0.8));
}
```

## Layout Considerations

### Minimum Pane Sizes

While TabSplitterContainer doesn't enforce minimum pane sizes by default, you should implement constraints:

```csharp
private const int MIN_PANE_SIZE = 150;

private void tabSplitterContainer1_SplitterMoved(object sender, EventArgs e)
{
    int position = this.tabSplitterContainer1.SplitterPosition;
    int containerSize = (this.tabSplitterContainer1.Orientation == Orientation.Horizontal) 
        ? this.tabSplitterContainer1.Width 
        : this.tabSplitterContainer1.Height;
    
    // Ensure primary pane minimum
    if (position < MIN_PANE_SIZE)
    {
        this.tabSplitterContainer1.SplitterPosition = MIN_PANE_SIZE;
    }
    
    // Ensure secondary pane minimum
    if (containerSize - position < MIN_PANE_SIZE)
    {
        this.tabSplitterContainer1.SplitterPosition = containerSize - MIN_PANE_SIZE;
    }
}
```

### Content-Aware Positioning

```csharp
private void PositionSplitterForContent()
{
    // If primary page has narrow content (like a tree view)
    if (HasNarrowContent())
    {
        this.tabSplitterContainer1.SplitterPosition = 250; // Fixed width for tree
    }
    // If content should be equal
    else if (RequiresEqualSplit())
    {
        if (this.tabSplitterContainer1.Orientation == Orientation.Horizontal)
        {
            this.tabSplitterContainer1.SplitterPosition = 
                this.tabSplitterContainer1.Width / 2;
        }
        else
        {
            this.tabSplitterContainer1.SplitterPosition = 
                this.tabSplitterContainer1.Height / 2;
        }
    }
    // If primary content needs priority
    else
    {
        this.tabSplitterContainer1.SplitterPosition = 
            (int)(this.tabSplitterContainer1.Width * 0.65);
    }
}
```

## Best Practices

### Orientation Selection
1. **Default to horizontal** for most scenarios (matches user expectations)
2. **Use vertical for console-style layouts** (code above, output below)
3. **Allow users to toggle orientation** via toolbar button or menu
4. **Consider screen aspect ratio** when choosing default orientation
5. **Persist orientation preference** across application sessions

### Splitter Position Management
1. **Set reasonable defaults** (50-60% for primary pane is common)
2. **Implement minimum pane sizes** to prevent unusable layouts
3. **Save and restore position** for better user experience
4. **Adjust proportionally on resize** to maintain relative sizing
5. **Consider content requirements** when setting initial position

### Responsive Design
1. **Handle form resize events** to adjust layout appropriately
2. **Test with different screen sizes** and resolutions
3. **Provide preset layouts** for common workflows
4. **Consider collapsing secondary pane** on very small screens

### User Experience
1. **Make splitter bar obvious** and easy to grab
2. **Provide visual feedback** during dragging
3. **Animate position changes** for smooth transitions
4. **Offer quick position presets** (50/50, 60/40, 70/30)
5. **Support keyboard shortcuts** for orientation and position changes

### Performance
1. **Avoid frequent position changes** in loops or timers
2. **Suspend layout** when making multiple adjustments
3. **Don't recalculate on every pixel** during resize
4. **Use BeginUpdate/EndUpdate** patterns for batch changes
