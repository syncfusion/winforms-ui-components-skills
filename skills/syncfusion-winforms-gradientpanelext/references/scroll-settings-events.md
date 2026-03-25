# Scroll Settings and Events

Guide to configuring scrollable content and handling events in GradientPanelExt for interactive panel implementations.

## Table of Contents
- [Scroll Settings](#scroll-settings)
- [Events Overview](#events-overview)
- [Common Events](#common-events)
- [Primitive Events](#primitive-events)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Scroll Settings

GradientPanelExt inherits scroll functionality from Panel, allowing scrollable content when child controls exceed panel bounds.

### AutoScroll Property

Enables automatic scrollbars when content overflows.

**Property Type:** `bool`  
**Default Value:** `false`

**C# Example:**
```csharp
gradientPanel.AutoScroll = true;   // Enable scrollbars
gradientPanel.AutoScroll = false;  // Disable scrollbars
```

**VB.NET Example:**
```vb
gradientPanel.AutoScroll = True   ' Enable scrollbars
gradientPanel.AutoScroll = False  ' Disable scrollbars
```

---

### Scrollable Panel Example

**C# Example:**
```csharp
// Create panel with scroll capability
GradientPanelExt scrollPanel = new GradientPanelExt
{
    Size = new Size(400, 300),
    Location = new Point(20, 20),
    AutoScroll = true,  // Enable scrolling
    CornerRadius = 10
};

scrollPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.WhiteSmoke,
    Color.White
);

// Add many controls that exceed panel height
for (int i = 0; i < 20; i++)
{
    Label label = new Label
    {
        Text = $"Item {i + 1}",
        Location = new Point(20, 20 + (i * 35)),
        AutoSize = true,
        BackColor = Color.Transparent
    };
    scrollPanel.Controls.Add(label);
}

this.Controls.Add(scrollPanel);
```

**Result:** Vertical scrollbar appears automatically when content exceeds 300px height

**VB.NET Example:**
```vb
' Create scrollable panel
Dim scrollPanel As New GradientPanelExt With {
    .Size = New Size(400, 300),
    .Location = New Point(20, 20),
    .AutoScroll = True,
    .CornerRadius = 10
}

scrollPanel.BackgroundColor = New BrushInfo( _
    GradientStyle.Vertical, _
    Color.WhiteSmoke, _
    Color.White _
)

' Add many controls
For i As Integer = 0 To 19
    Dim label As New Label With {
        .Text = $"Item {i + 1}",
        .Location = New Point(20, 20 + (i * 35)),
        .AutoSize = True,
        .BackColor = Color.Transparent
    }
    scrollPanel.Controls.Add(label)
Next

Me.Controls.Add(scrollPanel)
```

---

### AutoScrollMinSize Property

Sets minimum size for scroll area (forces scrollbars even if content fits).

**Property Type:** `Size`

**C# Example:**
```csharp
// Force scrollable area larger than panel
gradientPanel.AutoScroll = true;
gradientPanel.AutoScrollMinSize = new Size(400, 600);  // 600px tall scroll area
```

---

## Events Overview

GradientPanelExt supports standard Control events plus some panel-specific behaviors.

### Event Categories

1. **Standard Control Events**: Click, MouseEnter, MouseLeave, Paint, Resize, etc.
2. **Primitive-Related Events**: Clicks on primitives
3. **Collapse Events**: Panel collapse/expand state changes

---

## Common Events

### Click Event

Fires when panel area (not primitives) is clicked.

**C# Example:**
```csharp
gradientPanel.Click += GradientPanel_Click;

private void GradientPanel_Click(object sender, EventArgs e)
{
    MessageBox.Show("Panel clicked!");
}
```

**VB.NET Example:**
```vb
AddHandler gradientPanel.Click, AddressOf GradientPanel_Click

Private Sub GradientPanel_Click(sender As Object, e As EventArgs)
    MessageBox.Show("Panel clicked!")
End Sub
```

---

### MouseEnter / MouseLeave Events

Detect when mouse enters or leaves panel area.

**C# Example:**
```csharp
gradientPanel.MouseEnter += GradientPanel_MouseEnter;
gradientPanel.MouseLeave += GradientPanel_MouseLeave;

private void GradientPanel_MouseEnter(object sender, EventArgs e)
{
    // Highlight panel on hover
    gradientPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.LightBlue,
        Color.White
    );
}

private void GradientPanel_MouseLeave(object sender, EventArgs e)
{
    // Restore original gradient
    gradientPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.LightGray,
        Color.White
    );
}
```

**VB.NET Example:**
```vb
AddHandler gradientPanel.MouseEnter, AddressOf GradientPanel_MouseEnter
AddHandler gradientPanel.MouseLeave, AddressOf GradientPanel_MouseLeave

Private Sub GradientPanel_MouseEnter(sender As Object, e As EventArgs)
    gradientPanel.BackgroundColor = New BrushInfo( _
        GradientStyle.Horizontal, _
        Color.LightBlue, _
        Color.White _
    )
End Sub

Private Sub GradientPanel_MouseLeave(sender As Object, e As EventArgs)
    gradientPanel.BackgroundColor = New BrushInfo( _
        GradientStyle.Horizontal, _
        Color.LightGray, _
        Color.White _
    )
End Sub
```

---

### Paint Event

Customize panel rendering with custom drawing.

**C# Example:**
```csharp
gradientPanel.Paint += GradientPanel_Paint;

private void GradientPanel_Paint(object sender, PaintEventArgs e)
{
    // Draw custom border
    using (Pen pen = new Pen(Color.DarkBlue, 2))
    {
        e.Graphics.DrawRectangle(pen, 0, 0, gradientPanel.Width - 1, gradientPanel.Height - 1);
    }
}
```

---

### Resize Event

Respond to panel size changes.

**C# Example:**
```csharp
gradientPanel.Resize += GradientPanel_Resize;

private void GradientPanel_Resize(object sender, EventArgs e)
{
    // Adjust child control sizes
    foreach (Control control in gradientPanel.Controls)
    {
        if (control is TextBox textBox)
        {
            textBox.Width = gradientPanel.Width - 40;
        }
    }
}
```

---

## Primitive Events

### TextPrimitive Click Event

Handle clicks on TextPrimitive elements (button-style primitives).

**C# Example:**
```csharp
// Create clickable text primitive
TextPrimitive okButton = new TextPrimitive
{
    Text = "OK",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 100,
    Size = new Size(80, 30),
    BackColor = Color.LightGreen,
    TextColor = Color.DarkGreen
};

// Handle click using PrimitiveClick event
gradientPanel.PrimitiveClick += GradientPanel_PrimitiveClick;
gradientPanel.Primitives.Add(okButton);

private void GradientPanel_PrimitiveClick(object sender, PrimitiveEventArgs e)
{
    if (e.Primitive is TextPrimitive textPrimitive)
    {
        if (textPrimitive.Text == "OK")
        {
            MessageBox.Show("OK button clicked!");
        }
        else if (textPrimitive.Text == "Cancel")
        {
            MessageBox.Show("Cancel button clicked!");
        }
    }
}
```

**VB.NET Example:**
```vb
' Create clickable text primitive
Dim okButton As New TextPrimitive With {
    .Text = "OK",
    .Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    .Position = 100,
    .Size = New Size(80, 30),
    .BackColor = Color.LightGreen,
    .TextColor = Color.DarkGreen
}

AddHandler gradientPanel.PrimitiveClick, AddressOf GradientPanel_PrimitiveClick
gradientPanel.Primitives.Add(okButton)

Private Sub GradientPanel_PrimitiveClick(sender As Object, e As PrimitiveEventArgs)
    If TypeOf e.Primitive Is TextPrimitive Then
        Dim textPrimitive As TextPrimitive = CType(e.Primitive, TextPrimitive)
        If textPrimitive.Text = "OK" Then
            MessageBox.Show("OK button clicked!")
        ElseIf textPrimitive.Text = "Cancel" Then
            MessageBox.Show("Cancel button clicked!")
        End If
    End If
End Sub
```

---

### CollapsePrimitive Click

CollapsePrimitive automatically handles collapse/expand. You can detect state changes.

**C# Example:**
```csharp
// CollapsePrimitive handles click automatically, but you can track state
gradientPanel.SizeChanged += GradientPanel_SizeChanged;

private void GradientPanel_SizeChanged(object sender, EventArgs e)
{
    // Detect if collapsed (height is small)
    if (gradientPanel.Height < 100)
    {
        Console.WriteLine("Panel is collapsed");
    }
    else
    {
        Console.WriteLine("Panel is expanded");
    }
}
```

---

### HostPrimitive Control Events

Access events of hosted controls directly.

**C# Example:**
```csharp
// Create button to host
Button settingsButton = new Button
{
    Text = "Settings",
    BackColor = Color.White
};

// Handle button's click event directly
settingsButton.Click += SettingsButton_Click;

// Host in primitive
HostPrimitive buttonHost = new HostPrimitive
{
    HostControl = settingsButton,
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 300,
    Size = new Size(80, 25)
};

gradientPanel.Primitives.Add(buttonHost);

private void SettingsButton_Click(object sender, EventArgs e)
{
    MessageBox.Show("Settings button clicked!");
}
```

---

## Complete Examples

### Example 1: Scrollable Content Panel

```csharp
// Create scrollable content panel
GradientPanelExt contentPanel = new GradientPanelExt
{
    Size = new Size(400, 300),
    Location = new Point(20, 20),
    AutoScroll = true,
    CornerRadius = 10
};

contentPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.FromArgb(250, 250, 250),
    Color.White
);

// Add title primitive
TextPrimitive title = new TextPrimitive
{
    Text = "Content List",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 20,
    Size = new Size(150, 30),
    TextFont = new Font("Arial", 12, FontStyle.Bold),
    TextColor = Color.DarkSlateGray,
    BackColor = Color.Transparent
};
contentPanel.Primitives.Add(title);

// Add many items (triggers scrollbar)
for (int i = 0; i < 25; i++)
{
    Panel itemPanel = new Panel
    {
        Size = new Size(360, 40),
        Location = new Point(10, 10 + (i * 45)),
        BackColor = (i % 2 == 0) ? Color.White : Color.FromArgb(245, 245, 245)
    };
    
    Label itemLabel = new Label
    {
        Text = $"Item {i + 1}: Sample Content",
        Location = new Point(10, 10),
        AutoSize = true,
        BackColor = Color.Transparent
    };
    itemPanel.Controls.Add(itemLabel);
    
    contentPanel.Controls.Add(itemPanel);
}

this.Controls.Add(contentPanel);
```

---

### Example 2: Interactive Panel with Primitive Buttons

```csharp
// Create interactive panel
GradientPanelExt interactivePanel = new GradientPanelExt
{
    Size = new Size(450, 250),
    Location = new Point(30, 30),
    CornerRadius = 12
};

interactivePanel.BackgroundColor = new BrushInfo(
    GradientStyle.Horizontal,
    Color.LightSteelBlue,
    Color.White
);

// Add title
TextPrimitive title = new TextPrimitive
{
    Text = "User Actions",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Top,
    Position = 20,
    Size = new Size(150, 35),
    TextFont = new Font("Segoe UI", 14, FontStyle.Bold),
    TextColor = Color.DarkBlue,
    BackColor = Color.Transparent
};

// Add button primitives
TextPrimitive saveBtn = new TextPrimitive
{
    Text = "Save",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 80,
    Size = new Size(80, 28),
    BackColor = Color.LightGreen,
    TextColor = Color.DarkGreen,
    TextFont = new Font("Arial", 10, FontStyle.Bold)
};

TextPrimitive cancelBtn = new TextPrimitive
{
    Text = "Cancel",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 180,
    Size = new Size(80, 28),
    BackColor = Color.LightCoral,
    TextColor = Color.DarkRed,
    TextFont = new Font("Arial", 10, FontStyle.Bold)
};

TextPrimitive helpBtn = new TextPrimitive
{
    Text = "Help",
    Alignment = Syncfusion.Windows.Forms.Tools.Alignment.Bottom,
    Position = 280,
    Size = new Size(80, 28),
    BackColor = Color.LightBlue,
    TextColor = Color.DarkBlue,
    TextFont = new Font("Arial", 10, FontStyle.Bold)
};

interactivePanel.Primitives.AddRange(new Primitive[] { title, saveBtn, cancelBtn, helpBtn });

// Handle primitive clicks
interactivePanel.PrimitiveClick += InteractivePanel_PrimitiveClick;

this.Controls.Add(interactivePanel);

// Event handler
private void InteractivePanel_PrimitiveClick(object sender, PrimitiveEventArgs e)
{
    if (e.Primitive is TextPrimitive textPrimitive)
    {
        switch (textPrimitive.Text)
        {
            case "Save":
                MessageBox.Show("Saving data...", "Save");
                break;
            case "Cancel":
                MessageBox.Show("Operation cancelled", "Cancel");
                break;
            case "Help":
                MessageBox.Show("Help information...", "Help");
                break;
        }
    }
}
```

---

### Example 3: Event-Driven Panel State

```csharp
// Panel that responds to mouse hover
GradientPanelExt hoverPanel = new GradientPanelExt
{
    Size = new Size(350, 180),
    Location = new Point(40, 40),
    CornerRadius = 10
};

// Default gradient
hoverPanel.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightGray,
    Color.White
);

// Hover effects
hoverPanel.MouseEnter += (s, e) =>
{
    hoverPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.LightBlue,
        Color.White
    );
    hoverPanel.Cursor = Cursors.Hand;
};

hoverPanel.MouseLeave += (s, e) =>
{
    hoverPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Vertical,
        Color.LightGray,
        Color.White
    );
    hoverPanel.Cursor = Cursors.Default;
};

// Click handling
hoverPanel.Click += (s, e) =>
{
    MessageBox.Show("Panel clicked! Performing action...");
};

this.Controls.Add(hoverPanel);
```

---

## Best Practices

### 1. Enable AutoScroll for Dynamic Content

```csharp
// If content may exceed panel size
gradientPanel.AutoScroll = true;

// Add controls dynamically
foreach (var item in dataList)
{
    Control control = CreateControlForItem(item);
    gradientPanel.Controls.Add(control);
}
```

### 2. Unsubscribe from Events

Prevent memory leaks by unsubscribing when disposing.

```csharp
// In Form_FormClosing or Dispose method
gradientPanel.Click -= GradientPanel_Click;
gradientPanel.MouseEnter -= GradientPanel_MouseEnter;
gradientPanel.PrimitiveClick -= GradientPanel_PrimitiveClick;
```

### 3. Use Event Args to Identify Primitives

```csharp
private void Panel_PrimitiveClick(object sender, PrimitiveEventArgs e)
{
    // Type checking
    if (e.Primitive is TextPrimitive textPrim)
    {
        Console.WriteLine($"Text primitive clicked: {textPrim.Text}");
    }
    else if (e.Primitive is ImagePrimitive imgPrim)
    {
        Console.WriteLine("Image primitive clicked");
    }
}
```

### 4. Handle Exceptions in Event Handlers

```csharp
private void GradientPanel_Click(object sender, EventArgs e)
{
    try
    {
        // Your logic
        PerformAction();
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error: {ex.Message}", "Error", 
            MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}
```

---

## Troubleshooting

### Scrollbars Not Appearing

**Check:**
1. AutoScroll = true
2. Child controls actually exceed panel bounds
3. Child control locations are set correctly

```csharp
// Verify AutoScroll
Debug.WriteLine($"AutoScroll: {panel.AutoScroll}");

// Check if content exceeds bounds
int maxY = 0;
foreach (Control ctrl in panel.Controls)
{
    maxY = Math.Max(maxY, ctrl.Bottom);
}
Debug.WriteLine($"Content height: {maxY}, Panel height: {panel.Height}");
```

### Events Not Firing

**Check:**
- Event is subscribed (use += operator)
- Control is enabled
- No other control is blocking clicks

```csharp
// Verify subscription
Debug.WriteLine($"Click event count: {panel.Click?.GetInvocationList().Length ?? 0}");
```

### Primitive Click Not Working

**Check:**
- PrimitiveClick event is subscribed
- Primitive is added to Primitives collection
- Primitive size and position are within panel bounds

```csharp
// Verify primitives
Debug.WriteLine($"Primitive count: {panel.Primitives.Count}");
```

### Memory Leaks from Events

**Solution:** Always unsubscribe in Dispose

```csharp
protected override void Dispose(bool disposing)
{
    if (disposing)
    {
        // Unsubscribe all events
        gradientPanel.Click -= GradientPanel_Click;
        gradientPanel.PrimitiveClick -= GradientPanel_PrimitiveClick;
    }
    base.Dispose(disposing);
}
```

---

## Related Topics

- **Primitives**: Primitive types and usage → [primitives.md](primitives.md)
- **Collapse Animation**: Animation events → [collapse-expand-animation.md](collapse-expand-animation.md)
- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
