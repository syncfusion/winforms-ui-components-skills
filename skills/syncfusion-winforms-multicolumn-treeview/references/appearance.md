# Appearance Customization

## Table of Contents
- [Border Customization](#border-customization)
- [Color Customization](#color-customization)
- [Header Customization](#header-customization)
- [Base Styles](#base-styles)
- [Selected Node Appearance](#selected-node-appearance)
- [SubItem Styling](#subitem-styling)
- [Visual Styles](#visual-styles)
- [Image Customization](#image-customization)
- [Lines and Symbols](#lines-and-symbols)

## Border Customization

Customize the control's border with 2D or 3D styles.

### BorderStyle Options

```csharp
// No border
multiColumnTreeView1.BorderStyle = BorderStyle.None;

// 2D border (customizable)
multiColumnTreeView1.BorderStyle = BorderStyle.FixedSingle;

// 3D border (default)
multiColumnTreeView1.BorderStyle = BorderStyle.Fixed3D;
```

### 2D Border Customization

```csharp
// Configure 2D border
multiColumnTreeView1.BorderStyle = BorderStyle.FixedSingle;
multiColumnTreeView1.BorderColor = Color.SteelBlue;
multiColumnTreeView1.BorderSingle = ButtonBorderStyle.Dashed;

// Border styles: Solid, Dashed, Dotted, Inset, Outset, None
```

### 3D Border Customization

```csharp
// Configure 3D border
multiColumnTreeView1.BorderStyle = BorderStyle.Fixed3D;
multiColumnTreeView1.Border3DStyle = Border3DStyle.RaisedOuter;
multiColumnTreeView1.BorderSides = Border3DSide.All; // or Right, Left, Top, Bottom, Middle
multiColumnTreeView1.BorderColor = Color.SteelBlue;

// 3D Styles: Adjust, Bump, Etched, Flat, Raised, RaisedInner, RaisedOuter,
// Sunken, SunkenInner, SunkenOuter
```

## Color Customization

### Background Color

Simple background color:

```csharp
// Basic background color
multiColumnTreeView1.BackColor = Color.LightBlue;
```

### Background with Gradient

Advanced background with gradient support:

```csharp
using Syncfusion.Drawing;

// Solid color
multiColumnTreeView1.BackgroundColor = new BrushInfo(Color.Beige);

// Gradient
multiColumnTreeView1.BackgroundColor = new BrushInfo(
    GradientStyle.Vertical,
    Color.LightBlue,
    Color.White);
```

### Line Color

Customize the color of connecting lines:

```csharp
multiColumnTreeView1.LineColor = Color.Gray;
```

## Header Customization

Customize column header appearance:

```csharp
// Set header background
multiColumnTreeView1.Columns[0].Background = 
    new Syncfusion.Drawing.BrushInfo(ColorTranslator.FromHtml("#007acc"));

multiColumnTreeView1.Columns[0].TextColor = Color.White;

// Apply to all columns
foreach (TreeColumnAdv column in multiColumnTreeView1.Columns)
{
    column.Background = new Syncfusion.Drawing.BrushInfo(Color.DarkBlue);
    column.TextColor = Color.White;
    column.Font = new Font("Segoe UI", 10, FontStyle.Bold);
}
```

## Base Styles

BaseStyles allow you to define reusable styles for nodes, columns, and subitems at different levels.

### Creating Base Styles

Through designer:
1. Find the `BaseStyles` property
2. Open the Base Style Collection Editor
3. Select style type (Node, NodeLevel1, NodeLevel2, Column, SubItem)
4. Click Add and configure properties

Through code:

```csharp
// Create a base style for level 1 nodes
Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvStyleInfo level1Style = 
    new Syncfusion.Windows.Forms.Tools.MultiColumnTreeView.TreeNodeAdvStyleInfo();

level1Style.Font = new Font("Arial", 10, FontStyle.Bold);
level1Style.ForeColor = Color.DarkBlue;

// Apply style
// Note: BaseStyles are typically configured through designer
```

### Style Types

- **Standard** - Default style for all nodes
- **Node** - Style for specific node
- **Node Level 1, 2, 3...** - Styles for nodes at specific depths
- **Column Style** - Style for column headers
- **SubItem Style** - Style for subitems

## Selected Node Appearance

### Selected Node Colors

```csharp
// Background for selected node
multiColumnTreeView1.SelectedNodeBackground = 
    new Syncfusion.Drawing.BrushInfo(Color.LightSeaGreen);

// Foreground for selected node
multiColumnTreeView1.SelectedNodeForeColor = Color.White;
```

### Inactive Selection

When control loses focus:

```csharp
// Background when inactive
multiColumnTreeView1.InactiveSelectedNodeBackground = 
    new BrushInfo(Color.LightGray);

// Foreground when inactive
multiColumnTreeView1.InactiveSelectedNodeForeColor = Color.Black;
```

## SubItem Styling

Customize individual subitems:

### Background and Text

```csharp
// Style a subitem
TreeNodeAdvSubItem subItem = multiColumnTreeView1.Nodes[0].SubItems[0];
subItem.Background = new Syncfusion.Drawing.BrushInfo(Color.LightYellow);
subItem.ForeColor = Color.DarkRed;
subItem.Font = new Font("Arial", 9, FontStyle.Bold);
```

### SubItem Border

```csharp
subItem.BorderColor = Color.Red;
subItem.BorderStyle = BorderStyle.FixedSingle;
```

### SubItem Images

```csharp
// Add images to subitem
subItem.LeftImage = Image.FromFile("left_icon.png");
subItem.RightImage = Image.FromFile("right_icon.png");
```

## Visual Styles

Apply modern Office themes:

### Office2016Colorful

```csharp
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016Colorful;
```

### Office2016White

```csharp
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016White;
```

### Office2016Black

```csharp
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016Black;
```

### Office2016DarkGray

```csharp
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016DarkGray;
```

## Image Customization

### Left Image List

Images displayed on the left side of nodes:

```csharp
// Create and populate image list
ImageList leftImageList = new ImageList();
leftImageList.Images.Add(Image.FromFile("folder.png"));
leftImageList.Images.Add(Image.FromFile("file.png"));

// Assign to control
multiColumnTreeView1.LeftImageList = leftImageList;

// Set image indices for nodes
multiColumnTreeView1.Nodes[0].LeftImageIndices = new int[] { 0 }; // Folder
multiColumnTreeView1.Nodes[0].Nodes[0].LeftImageIndices = new int[] { 1 }; // File

// Add padding
multiColumnTreeView1.Nodes[0].LeftImagePadding = 5;
```

### Right Image List

Images displayed on the right side of nodes:

```csharp
// Create and populate image list
ImageList rightImageList = new ImageList();
rightImageList.Images.Add(Image.FromFile("status_ok.png"));
rightImageList.Images.Add(Image.FromFile("status_error.png"));

// Assign to control
multiColumnTreeView1.RightImageList = rightImageList;

// Set image indices
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];
node.RightImageIndices = new int[] { 0 }; // OK status

// Add padding
node.RightImagePadding = 5;
```

### State Image List

Different images for expand/collapse states:

```csharp
// Create and populate image list
ImageList stateImageList = new ImageList();
stateImageList.Images.Add(Image.FromFile("collapsed.png")); // Index 0
stateImageList.Images.Add(Image.FromFile("expanded.png"));  // Index 1
stateImageList.Images.Add(Image.FromFile("leaf.png"));      // Index 2

// Assign to control
multiColumnTreeView1.StateImageList = stateImageList;

// Set state images for all nodes
multiColumnTreeView1.ClosedImageIndex = 0; // Collapsed state
multiColumnTreeView1.OpenImageIndex = 1;    // Expanded state
multiColumnTreeView1.NoChildrenImageIndex = 2; // Leaf nodes

// Or set for individual nodes
TreeNodeAdv node = new TreeNodeAdv();
node.ClosedImageIndex = 0;
node.OpenImageIndex = 1;
node.NoChildrenImageIndex = 2;
```

## Lines and Symbols

### Plus/Minus Symbols

Control expand/collapse symbols:

```csharp
// Show plus/minus for all nodes
multiColumnTreeView1.ShowPlusMinus = true;

// Hide plus/minus
multiColumnTreeView1.ShowPlusMinus = false;

// Control for individual node
TreeNodeAdv node = multiColumnTreeView1.Nodes[0];
node.ShowPlusMinus = true;
```

### Connecting Lines

```csharp
// Show lines between nodes
multiColumnTreeView1.ShowLines = true;

// Show lines between root nodes
multiColumnTreeView1.ShowRootLines = true;

// Customize line color
multiColumnTreeView1.LineColor = Color.Gray;
```

## Practical Examples

### Example 1: Professional Theme

```csharp
void ApplyProfessionalTheme()
{
    // Modern Office look
    multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016White;
    
    // Custom headers
    foreach (TreeColumnAdv column in multiColumnTreeView1.Columns)
    {
        column.Background = new BrushInfo(ColorTranslator.FromHtml("#0078D7"));
        column.TextColor = Color.White;
        column.Font = new Font("Segoe UI", 9, FontStyle.Bold);
    }
    
    // Selection colors
    multiColumnTreeView1.SelectedNodeBackground = new BrushInfo(ColorTranslator.FromHtml("#0078D7"));
    multiColumnTreeView1.SelectedNodeForeColor = Color.White;
    multiColumnTreeView1.InactiveSelectedNodeBackground = new BrushInfo(Color.LightGray);
    multiColumnTreeView1.InactiveSelectedNodeForeColor = Color.Black;
    
    // Border
    multiColumnTreeView1.BorderStyle = BorderStyle.FixedSingle;
    multiColumnTreeView1.BorderColor = ColorTranslator.FromHtml("#CCCCCC");
}
```

### Example 2: Dark Theme

```csharp
void ApplyDarkTheme()
{
    multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016Black;
    
    // Background
    multiColumnTreeView1.BackgroundColor = new BrushInfo(ColorTranslator.FromHtml("#1E1E1E"));
    
    // Headers
    foreach (TreeColumnAdv column in multiColumnTreeView1.Columns)
    {
        column.Background = new BrushInfo(ColorTranslator.FromHtml("#2D2D30"));
        column.TextColor = Color.White;
    }
    
    // Node colors
    foreach (TreeNodeAdv node in GetAllNodes(multiColumnTreeView1.Nodes))
    {
        node.ForeColor = Color.White;
    }
    
    // Selection
    multiColumnTreeView1.SelectedNodeBackground = new BrushInfo(ColorTranslator.FromHtml("#094771"));
    multiColumnTreeView1.SelectedNodeForeColor = Color.White;
    
    // Lines
    multiColumnTreeView1.LineColor = ColorTranslator.FromHtml("#3F3F46"));
}

IEnumerable<TreeNodeAdv> GetAllNodes(TreeNodeAdvCollection nodes)
{
    foreach (TreeNodeAdv node in nodes)
    {
        yield return node;
        foreach (var child in GetAllNodes(node.Nodes))
        {
            yield return child;
        }
    }
}
```

### Example 3: Status-Based Coloring

```csharp
void ApplyStatusColors()
{
    foreach (TreeNodeAdv node in multiColumnTreeView1.Nodes)
    {
        ApplyStatusColor(node);
    }
}

void ApplyStatusColor(TreeNodeAdv node)
{
    // Color based on status (stored in Tag or SubItem)
    string status = node.SubItems.Count > 0 ? node.SubItems[0].Text : "";
    
    switch (status.ToLower())
    {
        case "completed":
            node.ForeColor = Color.Green;
            node.Font = new Font(node.Font, FontStyle.Bold);
            break;
        case "in progress":
            node.ForeColor = Color.Blue;
            break;
        case "error":
            node.ForeColor = Color.Red;
            node.Font = new Font(node.Font, FontStyle.Bold);
            break;
        case "warning":
            node.ForeColor = Color.Orange;
            break;
    }
    
    // Recursively apply to children
    foreach (TreeNodeAdv child in node.Nodes)
    {
        ApplyStatusColor(child);
    }
}
```

### Example 4: Alternating Row Colors

```csharp
void ApplyAlternatingRows()
{
    int index = 0;
    ApplyAlternatingColor(multiColumnTreeView1.Nodes, ref index);
}

void ApplyAlternatingColor(TreeNodeAdvCollection nodes, ref int index)
{
    foreach (TreeNodeAdv node in nodes)
    {
        if (index % 2 == 0)
        {
            node.BackColor = Color.White;
        }
        else
        {
            node.BackColor = ColorTranslator.FromHtml("#F5F5F5");
        }
        
        index++;
        
        if (node.Expanded && node.Nodes.Count > 0)
        {
            ApplyAlternatingColor(node.Nodes, ref index);
        }
    }
}
```

### Example 5: Priority Icons

```csharp
void SetupPriorityIcons()
{
    // Create image list
    ImageList imageList = new ImageList();
    imageList.Images.Add("high", Image.FromFile("priority_high.png"));
    imageList.Images.Add("medium", Image.FromFile("priority_medium.png"));
    imageList.Images.Add("low", Image.FromFile("priority_low.png"));
    
    multiColumnTreeView1.LeftImageList = imageList;
    
    // Assign icons based on priority
    foreach (TreeNodeAdv node in multiColumnTreeView1.Nodes)
    {
        string priority = node.Tag as string;
        
        switch (priority)
        {
            case "High":
                node.LeftImageIndices = new int[] { 0 };
                break;
            case "Medium":
                node.LeftImageIndices = new int[] { 1 };
                break;
            case "Low":
                node.LeftImageIndices = new int[] { 2 };
                break;
        }
    }
}
```

## Best Practices

1. **Use visual styles** for consistent modern appearance
2. **Apply gradients sparingly** to avoid visual clutter
3. **Ensure sufficient contrast** between text and background
4. **Use BaseStyles** for consistent appearance across node levels
5. **Test themes** in different lighting conditions
6. **Keep image sizes consistent** within image lists
7. **Provide visual feedback** for selection and hover states
8. **Consider accessibility** when choosing colors

## Common Issues

**Colors not applying:**
- Check if visual style overrides custom colors
- BackgroundColor takes precedence over BackColor
- Verify BrushInfo is properly constructed

**Images not showing:**
- Ensure image list is assigned before setting indices
- Check if image indices are within bounds
- Verify images are properly loaded

**Border not visible:**
- Ensure BorderStyle is not None
- Check if border color contrasts with background
- Verify BorderSides includes desired sides for 3D borders
