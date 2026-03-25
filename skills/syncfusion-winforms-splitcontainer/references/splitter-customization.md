# Splitter Customization in SplitContainerAdv

## Table of Contents
- [Splitter Distance](#splitter-distance)
- [Fixed Splitter Configuration](#fixed-splitter-configuration)
- [Splitter Dimensions](#splitter-dimensions)
- [Thumbnail Arrow and Grip Appearance](#thumbnail-arrow-and-grip-appearance)
- [Hover State Customization](#hover-state-customization)

## Splitter Distance

The `SplitterDistance` property controls the position of the splitter, determining the initial size of Panel1.

### Setting Initial Splitter Position

```csharp
// Position splitter 200 pixels from the edge
this.splitContainerAdv1.SplitterDistance = 200;
```

```vb
' Position splitter 200 pixels from the edge
Me.splitContainerAdv1.SplitterDistance = 200
```

The measurement is:
- For horizontal orientation: pixels from the left edge
- For vertical orientation: pixels from the top edge

### Getting Current Splitter Position

```csharp
// Get the current splitter position
int currentDistance = this.splitContainerAdv1.SplitterDistance;
```

```vb
' Get the current splitter position
Dim currentDistance As Integer = Me.splitContainerAdv1.SplitterDistance
```

### Dynamic Splitter Positioning

```csharp
// Set splitter to the middle of the container
int middlePosition = this.splitContainerAdv1.Width / 2;
this.splitContainerAdv1.SplitterDistance = middlePosition;
```

```vb
' Set splitter to the middle of the container
Dim middlePosition As Integer = Me.splitContainerAdv1.Width / 2
Me.splitContainerAdv1.SplitterDistance = middlePosition
```

## Fixed Splitter Configuration

Prevent the splitter from being moved by the user using the `IsSplitterFixed` property.

### Making Splitter Fixed

```csharp
// Prevent user from moving the splitter
this.splitContainerAdv1.IsSplitterFixed = true;
```

```vb
' Prevent user from moving the splitter
Me.splitContainerAdv1.IsSplitterFixed = True
```

**Use Case:** When panel sizes should not be adjustable by the user

### Allowing Splitter Movement

```csharp
// Allow user to move the splitter (default behavior)
this.splitContainerAdv1.IsSplitterFixed = false;
```

```vb
' Allow user to move the splitter (default behavior)
Me.splitContainerAdv1.IsSplitterFixed = False
```

## Splitter Dimensions

### Splitter Width

Control the visual width of the splitter bar:

```csharp
// Set splitter width to 20 pixels
this.splitContainerAdv1.SplitterWidth = 20;
```

```vb
' Set splitter width to 20 pixels
Me.splitContainerAdv1.SplitterWidth = 20
```

Default value is typically 4-6 pixels. Larger values make the splitter easier to grab.

### Splitter Increment

Control how many pixels the splitter moves with each keyboard press:

```csharp
// Splitter moves 10 pixels per arrow key press
this.splitContainerAdv1.SplitterIncrement = 10;
```

```vb
' Splitter moves 10 pixels per arrow key press
Me.splitContainerAdv1.SplitterIncrement = 10
```

Default value is 5 pixels.

## Thumbnail Arrow and Grip Appearance

Customize the visual appearance of the splitter's interactive elements.

### Arrow and Grip Colors

Configure the colors of the collapse/expand arrow and grip handle:

```csharp
// Set arrow fill color
this.splitContainerAdv1.ExpandFill = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.AliceBlue);

// Set arrow outline color
this.splitContainerAdv1.ExpandLine = System.Drawing.Color.Red;

// Set grip dark color
this.splitContainerAdv1.GripDark = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Wheat);

// Set grip light color
this.splitContainerAdv1.GripLight = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Crimson);
```

```vb
' Set arrow fill color
Me.splitContainerAdv1.ExpandFill = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.AliceBlue)

' Set arrow outline color
Me.splitContainerAdv1.ExpandLine = System.Drawing.Color.Red

' Set grip dark color
Me.splitContainerAdv1.GripDark = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Wheat)

' Set grip light color
Me.splitContainerAdv1.GripLight = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Crimson)
```

### Background Color

Set the overall background color of the splitter area:

```csharp
this.splitContainerAdv1.BackgroundColor = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.LightGray);
```

```vb
Me.splitContainerAdv1.BackgroundColor = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.LightGray)
```

## Hover State Customization

Define how the splitter appears when the mouse hovers over it.

### Hover Arrow Appearance

```csharp
// Set hover state arrow colors
this.splitContainerAdv1.HotExpandFill = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Red);
this.splitContainerAdv1.HotExpandLine = System.Drawing.Color.DeepPink;
```

```vb
' Set hover state arrow colors
Me.splitContainerAdv1.HotExpandFill = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Red)
Me.splitContainerAdv1.HotExpandLine = System.Drawing.Color.DeepPink
```

### Hover Grip Appearance

```csharp
// Set hover state grip colors
this.splitContainerAdv1.HotGripDark = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.MistyRose);
this.splitContainerAdv1.HotGripLight = new Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Purple);
```

```vb
' Set hover state grip colors
Me.splitContainerAdv1.HotGripDark = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.MistyRose)
Me.splitContainerAdv1.HotGripLight = New Syncfusion.Drawing.BrushInfo(System.Drawing.Color.Purple)
```

### Hover Background

```csharp
// Set hover background with gradient
this.splitContainerAdv1.HotBackgroundColor = new Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Horizontal, 
    System.Drawing.Color.SandyBrown, 
    System.Drawing.Color.AntiqueWhite);
```

```vb
' Set hover background with gradient
Me.splitContainerAdv1.HotBackgroundColor = New Syncfusion.Drawing.BrushInfo(
    Syncfusion.Drawing.GradientStyle.Horizontal,
    System.Drawing.Color.SandyBrown,
    System.Drawing.Color.AntiqueWhite)
```

## Complete Customization Example

```csharp
// Complete splitter customization
private void ConfigureSplitter()
{
    SplitContainerAdv splitContainer = new SplitContainerAdv();
    splitContainer.Orientation = Orientation.Horizontal;
    splitContainer.SplitterDistance = 200;
    splitContainer.SplitterWidth = 8;
    splitContainer.SplitterIncrement = 5;
    
    // Normal state colors
    splitContainer.ExpandFill = new Syncfusion.Drawing.BrushInfo(Color.LightBlue);
    splitContainer.ExpandLine = Color.Navy;
    splitContainer.GripDark = new Syncfusion.Drawing.BrushInfo(Color.Gray);
    splitContainer.GripLight = new Syncfusion.Drawing.BrushInfo(Color.White);
    
    // Hover state colors
    splitContainer.HotExpandFill = new Syncfusion.Drawing.BrushInfo(Color.Blue);
    splitContainer.HotExpandLine = Color.DarkBlue;
    splitContainer.HotGripDark = new Syncfusion.Drawing.BrushInfo(Color.DarkGray);
    splitContainer.HotGripLight = new Syncfusion.Drawing.BrushInfo(Color.LightGray);
    
    // Constraints
    splitContainer.Panel1MinSize = 150;
    splitContainer.Panel2MinSize = 100;
    splitContainer.IsSplitterFixed = false;
    
    this.Controls.Add(splitContainer);
}
```

```vb
' Complete splitter customization
Private Sub ConfigureSplitter()
    Dim splitContainer As New SplitContainerAdv()
    splitContainer.Orientation = Orientation.Horizontal
    splitContainer.SplitterDistance = 200
    splitContainer.SplitterWidth = 8
    splitContainer.SplitterIncrement = 5
    
    ' Normal state colors
    splitContainer.ExpandFill = New Syncfusion.Drawing.BrushInfo(Color.LightBlue)
    splitContainer.ExpandLine = Color.Navy
    splitContainer.GripDark = New Syncfusion.Drawing.BrushInfo(Color.Gray)
    splitContainer.GripLight = New Syncfusion.Drawing.BrushInfo(Color.White)
    
    ' Hover state colors
    splitContainer.HotExpandFill = New Syncfusion.Drawing.BrushInfo(Color.Blue)
    splitContainer.HotExpandLine = Color.DarkBlue
    splitContainer.HotGripDark = New Syncfusion.Drawing.BrushInfo(Color.DarkGray)
    splitContainer.HotGripLight = New Syncfusion.Drawing.BrushInfo(Color.LightGray)
    
    ' Constraints
    splitContainer.Panel1MinSize = 150
    splitContainer.Panel2MinSize = 100
    splitContainer.IsSplitterFixed = False
    
    Me.Controls.Add(splitContainer)
End Sub
```
