# Control Settings

This guide covers the appearance, behavior, spacing, scrolling, and orientation settings for the GroupView control.

## Table of Contents
- [Appearance Settings](#appearance-settings)
- [Behavior Settings](#behavior-settings)
- [Spacing Configuration](#spacing-configuration)
- [Scroll Settings](#scroll-settings)
- [Orientation Settings](#orientation-settings)

## Appearance Settings

Configure the visual appearance of the GroupView control.

### FlatLook Property

The **FlatLook** property removes the 3-dimensional edge from GroupView items, providing a modern, flat appearance.

```csharp
// Enable flat look (modern, flat appearance)
this.groupView1.FlatLook = true;

// Disable flat look (3D appearance with raised edges)
this.groupView1.FlatLook = false; // Default
```

**Visual Difference:**
- **FlatLook = true**: Items display with flat borders, no 3D edge effect
- **FlatLook = false**: Items display with raised 3D borders

**Use Case:**
Enable FlatLook for modern UI designs, Windows 10/11-style interfaces, or when creating VS.NET toolbox clones.

### BorderStyle Property

The **BorderStyle** property specifies the border style for the entire GroupView control.

```csharp
// No border
this.groupView1.BorderStyle = BorderStyle.None;

// Single-line border
this.groupView1.BorderStyle = BorderStyle.FixedSingle;

// 3D border (default)
this.groupView1.BorderStyle = BorderStyle.Fixed3D;
```

**Available Options:**
- **None**: No border around the control
- **FixedSingle**: A single-line border
- **Fixed3D**: A three-dimensional border (default)

**Example - Clean Modern Look:**

```csharp
this.groupView1.FlatLook = true;
this.groupView1.BorderStyle = BorderStyle.FixedSingle;
this.groupView1.BackColor = Color.White;
```

## Behavior Settings

Control the interactive behavior of the GroupView control.

### Drag-and-Drop Configuration

Enable drag-and-drop functionality to allow users to reorder items or drag items to other controls.

#### AllowDragDrop Property

```csharp
// Enable drag-and-drop for GroupView items
this.groupView1.AllowDragDrop = true;
```

When enabled, users can:
- Click and drag GroupView items within the control
- Reorder items by dragging and dropping
- Receive reordering notifications via GroupViewItemsReordered event

#### AllowDragAnyObject Property

```csharp
// Allow dragging any object type (including external objects)
this.groupView1.AllowDragAnyObject = true;
```

**Difference between AllowDragDrop and AllowDragAnyObject:**
- **AllowDragDrop**: Enables drag-and-drop for GroupView items within the control
- **AllowDragAnyObject**: Extends support to external objects being dragged into GroupView

**Complete Drag-and-Drop Example:**

```csharp
public void ConfigureDragDrop()
{
    // Enable drag-and-drop
    this.groupView1.AllowDragDrop = true;
    this.groupView1.AllowDragAnyObject = true;
    
    // Handle reorder event
    this.groupView1.GroupViewItemsReordered += (sender, e) =>
    {
        // Items have been reordered
        LogItemOrder();
    };
}

private void LogItemOrder()
{
    for (int i = 0; i < this.groupView1.GroupViewItems.Count; i++)
    {
        Console.WriteLine($"Position {i}: {this.groupView1.GroupViewItems[i].Text}");
    }
}
```

## Spacing Configuration

Control spacing between items and borders.

### ItemXSpacing Property

Sets the horizontal spacing between GroupView items.

```csharp
// Set horizontal spacing to 5 pixels
this.groupView1.ItemXSpacing = 5;

// Default value
this.groupView1.ItemXSpacing = 0;
```

**Effect:**
- Adds horizontal gaps between items in the same row
- Creates visual separation in horizontal layouts
- Useful for reducing visual clutter

### ItemYSpacing Property

Sets the vertical spacing between GroupView items.

```csharp
// Set vertical spacing to 10 pixels
this.groupView1.ItemYSpacing = 10;

// Default value
this.groupView1.ItemYSpacing = 0;
```

**Effect:**
- Adds vertical gaps between items in different rows
- Creates breathing room in vertical layouts
- Improves readability for dense item lists

### Spacing Example - Comfortable Layout

```csharp
public void ConfigureComfortableSpacing()
{
    // Create comfortable spacing for better readability
    this.groupView1.ItemXSpacing = 8;
    this.groupView1.ItemYSpacing = 12;
    
    // Combined with other settings for optimal appearance
    this.groupView1.FlatLook = true;
    this.groupView1.BorderStyle = BorderStyle.FixedSingle;
    this.groupView1.BackColor = Color.WhiteSmoke;
}
```

### Spacing Example - Compact Layout

```csharp
public void ConfigureCompactSpacing()
{
    // Minimal spacing for compact displays
    this.groupView1.ItemXSpacing = 2;
    this.groupView1.ItemYSpacing = 2;
    
    // Useful for toolbox-style interfaces
    this.groupView1.FlowView = true;
    this.groupView1.SmallImageView = true;
}
```

## Scroll Settings

Configure scrolling behavior for GroupView controls with many items.

### IntegratedScrolling Property

Enables automatic scrolling when items exceed the visible area.

```csharp
// Enable integrated scrolling
this.groupView1.IntegratedScrolling = true;

// Disable scrolling (default)
this.groupView1.IntegratedScrolling = false;
```

**Behavior:**
- **true**: Scrollbars appear automatically when content exceeds visible area
- **false**: No scrollbars, content may be clipped if it exceeds bounds

**When to Enable:**
- Lists with many items (more than fit in visible area)
- Dynamic item lists where count may grow
- Toolbox-style interfaces with numerous options

**Example - Scrollable List:**

```csharp
public void CreateScrollableList()
{
    this.groupView1.Size = new Size(200, 300);
    this.groupView1.IntegratedScrolling = true;
    
    // Add many items
    for (int i = 0; i < 50; i++)
    {
        this.groupView1.GroupViewItems.Add(
            new GroupViewItem($"Item {i + 1}", -1, true, null, $"item{i}")
        );
    }
}
```

## Orientation Settings

Configure the layout direction and flow behavior of GroupView items.

### Orientation Property

Sets the arrangement direction for GroupView items.

```csharp
// Vertical orientation (default)
this.groupView1.Orientation = GroupViewOrientation.Vertical;

// Horizontal orientation
this.groupView1.Orientation = GroupViewOrientation.Horizontal;
```

**Orientation.Vertical:**
- Items stack vertically (top to bottom)
- New items appear below previous items
- Standard list layout

**Orientation.Horizontal:**
- Items stack horizontally (left to right)
- New items appear to the right of previous items
- Toolbar-style layout

### FlowView Property

Enables flow layout mode, displaying items with images but minimal or no text.

```csharp
// Enable flow view (image-focused display)
this.groupView1.FlowView = true;

// Disable flow view (standard display with text and images)
this.groupView1.FlowView = false; // Default
```

**FlowView Characteristics:**
- Items display primarily as images
- Text is hidden or truncated by default
- Items wrap to next row/column when space runs out
- Ideal for icon grids or toolbox interfaces

### ShowFlowViewItemText Property

Controls text visibility when FlowView is enabled.

```csharp
// Show text in FlowView mode
this.groupView1.ShowFlowViewItemText = true;

// Hide text in FlowView mode (default)
this.groupView1.ShowFlowViewItemText = false;
```

**Note:** Only effective when FlowView = true.

### FlowViewItemTextLength Property

Sets the maximum character length for item text in FlowView mode.

```csharp
// Limit text to 45 characters
this.groupView1.FlowViewItemTextLength = 45;

// Default value
this.groupView1.FlowViewItemTextLength = 0; // No limit
```

Text exceeding this length will be truncated with ellipsis (...).

### Example - VS.NET Toolbox Style

Create a Visual Studio toolbox-style interface with images and text:

```csharp
public void CreateToolboxStyle()
{
    // Enable flow view with images and text
    this.groupView1.FlowView = true;
    this.groupView1.ShowFlowViewItemText = true;
    this.groupView1.FlowViewItemTextLength = 20;
    
    // Use small images
    this.groupView1.SmallImageView = true;
    this.groupView1.SmallImageList = this.imageList1;
    
    // Flat appearance
    this.groupView1.FlatLook = true;
    this.groupView1.BorderStyle = BorderStyle.FixedSingle;
    
    // Enable scrolling for many items
    this.groupView1.IntegratedScrolling = true;
    
    // Comfortable spacing
    this.groupView1.ItemXSpacing = 4;
    this.groupView1.ItemYSpacing = 4;
    
    // Vertical orientation (default)
    this.groupView1.Orientation = GroupViewOrientation.Vertical;
}
```

### Example - Horizontal Icon Bar

Create a horizontal toolbar with icons:

```csharp
public void CreateIconBar()
{
    // Horizontal layout
    this.groupView1.Orientation = GroupViewOrientation.Horizontal;
    
    // Flow view with images only
    this.groupView1.FlowView = true;
    this.groupView1.ShowFlowViewItemText = false;
    
    // Large images for toolbar
    this.groupView1.LargeImageList = this.largeImageList;
    this.groupView1.SmallImageView = false;
    
    // Minimal spacing
    this.groupView1.ItemXSpacing = 2;
    this.groupView1.ItemYSpacing = 0;
    
    // Flat appearance
    this.groupView1.FlatLook = true;
    this.groupView1.BorderStyle = BorderStyle.None;
    
    // Fixed height for toolbar
    this.groupView1.Height = 48;
    this.groupView1.Dock = DockStyle.Top;
}
```

### Example - Standard Vertical List

Create a standard vertical list with full text and icons:

```csharp
public void CreateStandardList()
{
    // Standard vertical orientation
    this.groupView1.Orientation = GroupViewOrientation.Vertical;
    this.groupView1.FlowView = false;
    
    // Small images with full text
    this.groupView1.SmallImageView = true;
    this.groupView1.SmallImageList = this.imageList1;
    
    // Modern flat appearance
    this.groupView1.FlatLook = true;
    this.groupView1.BorderStyle = BorderStyle.FixedSingle;
    
    // Scrolling for long lists
    this.groupView1.IntegratedScrolling = true;
    
    // Standard spacing
    this.groupView1.ItemXSpacing = 0;
    this.groupView1.ItemYSpacing = 4;
}
```

## Complete Configuration Example

Combine all settings for a fully configured GroupView:

```csharp
public partial class ConfiguredGroupView : Form
{
    private GroupView groupView1;
    private ImageList imageList1;
    
    public ConfiguredGroupView()
    {
        InitializeComponent();
        ConfigureGroupView();
    }
    
    private void ConfigureGroupView()
    {
        // Create and configure GroupView
        this.groupView1 = new GroupView();
        this.groupView1.Location = new Point(20, 20);
        this.groupView1.Size = new Size(250, 400);
        
        // Appearance
        this.groupView1.FlatLook = true;
        this.groupView1.BorderStyle = BorderStyle.FixedSingle;
        
        // Behavior
        this.groupView1.AllowDragDrop = true;
        this.groupView1.AllowDragAnyObject = false;
        
        // Spacing
        this.groupView1.ItemXSpacing = 5;
        this.groupView1.ItemYSpacing = 8;
        
        // Scrolling
        this.groupView1.IntegratedScrolling = true;
        
        // Orientation (VS.NET Toolbox style)
        this.groupView1.FlowView = true;
        this.groupView1.ShowFlowViewItemText = true;
        this.groupView1.FlowViewItemTextLength = 30;
        this.groupView1.Orientation = GroupViewOrientation.Vertical;
        
        // Images
        this.groupView1.SmallImageView = true;
        this.groupView1.SmallImageList = this.imageList1;
        
        // Add items
        this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
            new GroupViewItem("Pointer", 0, true, "Selection tool", "item1"),
            new GroupViewItem("Button", 1, true, "Add button control", "item2"),
            new GroupViewItem("TextBox", 2, true, "Add textbox control", "item3"),
            new GroupViewItem("Label", 3, true, "Add label control", "item4"),
            new GroupViewItem("Panel", 4, true, "Add panel container", "item5")
        });
        
        // Add to form
        this.Controls.Add(this.groupView1);
    }
}
```

## Best Practices

### Performance Considerations
- Use **IntegratedScrolling** for lists with more than 20-30 items
- Set appropriate **FlowViewItemTextLength** to prevent text overflow
- Choose **SmallImageView** for lists with many items to conserve space

### UI/UX Guidelines
- Use **FlatLook = true** for modern Windows 10/11 applications
- Set **ItemYSpacing** between 4-12 pixels for comfortable readability
- Enable **AllowDragDrop** only when reordering is a desired feature
- Use **FlowView** for toolbox-style interfaces with many small items

### Layout Recommendations
- **Vertical + FlowView = false**: Standard file/folder lists
- **Vertical + FlowView = true**: VS.NET toolbox, control palette
- **Horizontal + FlowView = true**: Icon toolbars, quick access bars
- **Horizontal + FlowView = false**: Breadcrumb navigation, horizontal menus
