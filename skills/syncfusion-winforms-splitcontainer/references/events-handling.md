# Events and Interactions in SplitContainerAdv

## SplitterMoved Event

Triggered after the splitter has been moved to a new position. Use this event to respond after the splitter movement is complete.

### Basic Event Handler

```csharp
private void splitContainerAdv1_SplitterMoved(object sender, SplitterMoveEventArgs args)
{
    MessageBox.Show("Splitter moved from " + args.OldSplitPosition + " to " + args.NewSplitPosition);
}
```

```vb
Private Sub splitContainerAdv1_SplitterMoved(sender As Object, args As SplitterMoveEventArgs)
    MessageBox.Show("Splitter moved from " + args.OldSplitPosition.ToString() + " to " + args.NewSplitPosition.ToString())
End Sub
```

### Event Args

- **OldSplitPosition**: Previous splitter distance in pixels
- **NewSplitPosition**: New splitter distance in pixels

### Practical Example

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    splitContainerAdv1.SplitterMoved += SplitContainerAdv1_SplitterMoved;
}

private void SplitContainerAdv1_SplitterMoved(object sender, SplitterMoveEventArgs args)
{
    // Update status bar with new panel sizes
    int panel1Size = args.NewSplitPosition;
    int panel2Size = splitContainerAdv1.Width - args.NewSplitPosition - splitContainerAdv1.SplitterWidth;
    
    statusLabel.Text = $"Panel1: {panel1Size}px | Panel2: {panel2Size}px";
    
    // Save splitter position for later
    Properties.Settings.Default.SplitterPosition = args.NewSplitPosition;
    Properties.Settings.Default.Save();
}
```

```vb
Private Sub Form1_Load(sender As Object, e As EventArgs)
    AddHandler splitContainerAdv1.SplitterMoved, AddressOf SplitContainerAdv1_SplitterMoved
End Sub

Private Sub SplitContainerAdv1_SplitterMoved(sender As Object, args As SplitterMoveEventArgs)
    ' Update status bar with new panel sizes
    Dim panel1Size As Integer = args.NewSplitPosition
    Dim panel2Size As Integer = splitContainerAdv1.Width - args.NewSplitPosition - splitContainerAdv1.SplitterWidth
    
    statusLabel.Text = $"Panel1: {panel1Size}px | Panel2: {panel2Size}px"
    
    ' Save splitter position for later
    Properties.Settings.Default.SplitterPosition = args.NewSplitPosition
    Properties.Settings.Default.Save()
End Sub
```

## SplitterMoving Event

Triggered while the splitter is being moved. Use this event to validate or restrict splitter movement in real-time.

### Basic Event Handler

```csharp
private void splitContainerAdv2_SplitterMoving(object sender, SplitterMoveEventArgs args)
{
    MessageBox.Show("Splitter is moving to: " + args.NewSplitPosition);
}
```

```vb
Private Sub splitContainerAdv2_SplitterMoving(sender As Object, args As SplitterMoveEventArgs)
    MessageBox.Show("Splitter is moving to: " + args.NewSplitPosition.ToString())
End Sub
```

### Validation Example

```csharp
private void splitContainerAdv1_SplitterMoving(object sender, SplitterMoveEventArgs args)
{
    // Prevent splitter from moving beyond Panel1 minimum 100 pixels
    if (args.NewSplitPosition < 100)
    {
        args.SplitPosition = 100;
    }
    
    // Prevent splitter from leaving less than 150 pixels for Panel2
    int maxPosition = splitContainerAdv1.Width - 150;
    if (args.NewSplitPosition > maxPosition)
    {
        args.SplitPosition = maxPosition;
    }
}
```

```vb
Private Sub splitContainerAdv1_SplitterMoving(sender As Object, args As SplitterMoveEventArgs)
    ' Prevent splitter from moving beyond Panel1 minimum 100 pixels
    If args.NewSplitPosition < 100 Then
        args.SplitPosition = 100
    End If
    
    ' Prevent splitter from leaving less than 150 pixels for Panel2
    Dim maxPosition As Integer = splitContainerAdv1.Width - 150
    If args.NewSplitPosition > maxPosition Then
        args.SplitPosition = maxPosition
    End If
End Sub
```

## Wiring Event Handlers

### In Designer

1. Select the SplitContainerAdv control
2. Open the Properties panel and click the Events icon
3. Double-click on `SplitterMoved` or `SplitterMoving` event
4. The event handler will be auto-generated

### In Code

```csharp
// Wire events in Form_Load
private void Form1_Load(object sender, EventArgs e)
{
    splitContainerAdv1.SplitterMoved += SplitContainerAdv1_SplitterMoved;
    splitContainerAdv1.SplitterMoving += SplitContainerAdv1_SplitterMoving;
}

// Event handlers
private void SplitContainerAdv1_SplitterMoved(object sender, SplitterMoveEventArgs args)
{
    // Handle after splitter moved
}

private void SplitContainerAdv1_SplitterMoving(object sender, SplitterMoveEventArgs args)
{
    // Handle while splitter moving
}
```

```vb
' Wire events in Form_Load
Private Sub Form1_Load(sender As Object, e As EventArgs)
    AddHandler splitContainerAdv1.SplitterMoved, AddressOf SplitContainerAdv1_SplitterMoved
    AddHandler splitContainerAdv1.SplitterMoving, AddressOf SplitContainerAdv1_SplitterMoving
End Sub

' Event handlers
Private Sub SplitContainerAdv1_SplitterMoved(sender As Object, args As SplitterMoveEventArgs)
    ' Handle after splitter moved
End Sub

Private Sub SplitContainerAdv1_SplitterMoving(sender As Object, args As SplitterMoveEventArgs)
    ' Handle while splitter moving
End Sub
```

## Common Event Patterns

### Pattern 1: Update Layout Based on Splitter Position

```csharp
private void splitContainerAdv1_SplitterMoved(object sender, SplitterMoveEventArgs args)
{
    // Adjust child controls based on new panel size
    foreach (Control control in splitContainerAdv1.Panel1.Controls)
    {
        control.Width = splitContainerAdv1.Panel1.Width;
    }
    
    foreach (Control control in splitContainerAdv1.Panel2.Controls)
    {
        control.Width = splitContainerAdv1.Panel2.Width;
    }
}
```

### Pattern 2: Snap to Predefined Positions

```csharp
private void splitContainerAdv1_SplitterMoving(object sender, SplitterMoveEventArgs args)
{
    // Snap to 25%, 50%, or 75% of container width
    int quarter = splitContainerAdv1.Width / 4;
    int newPos = args.NewSplitPosition;
    
    if (newPos < quarter + 25) args.SplitPosition = 0;
    else if (newPos < quarter * 2 + 25) args.SplitPosition = quarter * 2;
    else if (newPos < quarter * 3 + 25) args.SplitPosition = quarter * 3;
    else args.SplitPosition = splitContainerAdv1.Width - quarter;
}
```

### Pattern 3: Persist Splitter Position

```csharp
private void Form1_Load(object sender, EventArgs e)
{
    // Restore previous splitter position
    if (Properties.Settings.Default.SplitterPos > 0)
    {
        splitContainerAdv1.SplitterDistance = Properties.Settings.Default.SplitterPos;
    }
    
    splitContainerAdv1.SplitterMoved += SplitContainerAdv1_SplitterMoved;
}

private void SplitContainerAdv1_SplitterMoved(object sender, SplitterMoveEventArgs args)
{
    // Save current position
    Properties.Settings.Default.SplitterPos = args.NewSplitPosition;
    Properties.Settings.Default.Save();
}
```

## Event Interaction Flow

```
User starts dragging splitter
         ↓
SplitterMoving event triggered
         ↓
Validate new position (can modify args.SplitPosition)
         ↓
User releases splitter
         ↓
SplitterMoved event triggered
         ↓
Update UI or perform final operations
```
