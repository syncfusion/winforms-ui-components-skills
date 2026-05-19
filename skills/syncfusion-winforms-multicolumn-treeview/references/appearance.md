# Appearance Customization

Customize borders, colors, headers, styles, images, and visual elements.

## Border Customization

```csharp
// Border styles
multiColumnTreeView1.BorderStyle = BorderStyle.None;        // No border
multiColumnTreeView1.BorderStyle = BorderStyle.FixedSingle; // 2D border
multiColumnTreeView1.BorderStyle = BorderStyle.Fixed3D;     // 3D border

// 2D border customization
multiColumnTreeView1.BorderColor = Color.SteelBlue;
multiColumnTreeView1.BorderSingle = ButtonBorderStyle.Dashed; // Solid, Dashed, Dotted, Inset, Outset

// 3D border customization
multiColumnTreeView1.Border3DStyle = Border3DStyle.RaisedOuter;
multiColumnTreeView1.BorderSides = Border3DSide.All;
```

## Color Customization

```csharp
// Basic background
multiColumnTreeView1.BackColor = Color.LightBlue;

// Background with gradient
using Syncfusion.Drawing;
multiColumnTreeView1.BackgroundColor = new BrushInfo(Color.Beige);
multiColumnTreeView1.BackgroundColor = new BrushInfo(GradientStyle.Vertical, Color.LightBlue, Color.White);

// Line color
multiColumnTreeView1.LineColor = Color.Gray;
```

## Header Customization

```csharp
// Single column
multiColumnTreeView1.Columns[0].Background = new BrushInfo(ColorTranslator.FromHtml("#007acc"));
multiColumnTreeView1.Columns[0].TextColor = Color.White;
multiColumnTreeView1.Columns[0].Font = new Font("Segoe UI", 10, FontStyle.Bold);

// All columns
foreach (TreeColumnAdv column in multiColumnTreeView1.Columns)
{
    column.Background = new BrushInfo(Color.DarkBlue);
    column.TextColor = Color.White;
}
```

## Selected Node Appearance

```csharp
// Active selection
multiColumnTreeView1.SelectedNodeBackground = new BrushInfo(Color.LightSeaGreen);
multiColumnTreeView1.SelectedNodeForeColor = Color.White;

// Inactive selection
multiColumnTreeView1.InactiveSelectedNodeBackground = new BrushInfo(Color.LightGray);
multiColumnTreeView1.InactiveSelectedNodeForeColor = Color.Black;
```

## SubItem Styling

```csharp
TreeNodeAdvSubItem subItem = multiColumnTreeView1.Nodes[0].SubItems[0];
subItem.Background = new BrushInfo(Color.LightYellow);
subItem.TextColor = Color.DarkRed;
subItem.Font = new Font("Arial", 9, FontStyle.Bold);
subItem.BorderColor = Color.Red;
subItem.BorderStyle = BorderStyle.FixedSingle;
subItem.LeftImage = Image.FromFile("left_icon.png");
```

## Visual Styles

```csharp
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016Colorful;
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016White;
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016Black;
multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016DarkGray;
```

## Image Customization

```csharp
// Left image list
ImageList leftImageList = new ImageList();
leftImageList.Images.Add(Image.FromFile("folder.png"));
leftImageList.Images.Add(Image.FromFile("file.png"));
multiColumnTreeView1.LeftImageList = leftImageList;
multiColumnTreeView1.Nodes[0].LeftImageIndices = new int[] { 0 };
multiColumnTreeView1.Nodes[0].LeftImagePadding = 5;

// Right image list
ImageList rightImageList = new ImageList();
rightImageList.Images.Add(Image.FromFile("status_ok.png"));
multiColumnTreeView1.RightImageList = rightImageList;
multiColumnTreeView1.Nodes[0].RightImageIndices = new int[] { 0 };
multiColumnTreeView1.Nodes[0].RightImagePadding = 5;

// State image list (expand/collapse)
ImageList stateImageList = new ImageList();
stateImageList.Images.Add(Image.FromFile("collapsed.png"));
stateImageList.Images.Add(Image.FromFile("expanded.png"));
stateImageList.Images.Add(Image.FromFile("leaf.png"));
multiColumnTreeView1.StateImageList = stateImageList;
TreeNodeAdv treeNodeAdv1 = new TreeNodeAdv();
treeNodeAdv1.ClosedImgIndex = 0;
treeNodeAdv1.OpenImgIndex = 1;
treeNodeAdv1.NoChildrenImgIndex = 2;
```

## Professional Theme Example

```csharp
void ApplyProfessionalTheme()
{
    multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016White;
    
    foreach (TreeColumnAdv column in multiColumnTreeView1.Columns)
    {
        column.Background = new BrushInfo(ColorTranslator.FromHtml("#0078D7"));
        column.TextColor = Color.White;
        column.Font = new Font("Segoe UI", 9, FontStyle.Bold);
    }
    
    multiColumnTreeView1.SelectedNodeBackground = new BrushInfo(ColorTranslator.FromHtml("#0078D7"));
    multiColumnTreeView1.SelectedNodeForeColor = Color.White;
    multiColumnTreeView1.BorderStyle = BorderStyle.FixedSingle;
    multiColumnTreeView1.BorderColor = ColorTranslator.FromHtml("#CCCCCC");
}
```

## Dark Theme Example

```csharp
void ApplyDarkTheme()
{
    multiColumnTreeView1.Style = MultiColumnVisualStyle.Office2016Black;
    multiColumnTreeView1.BackgroundColor = new BrushInfo(ColorTranslator.FromHtml("#1E1E1E"));
    
    foreach (TreeColumnAdv column in multiColumnTreeView1.Columns)
    {
        column.Background = new BrushInfo(ColorTranslator.FromHtml("#2D2D30"));
        column.TextColor = Color.White;
    }
    
    foreach (TreeNodeAdv node in GetAllNodes(multiColumnTreeView1.Nodes))
        node.TextColor = Color.White;
    
    multiColumnTreeView1.SelectedNodeBackground = new BrushInfo(ColorTranslator.FromHtml("#094771"));
    multiColumnTreeView1.LineColor = ColorTranslator.FromHtml("#3F3F46");
}

IEnumerable<TreeNodeAdv> GetAllNodes(TreeNodeAdvCollection nodes)
{
    foreach (TreeNodeAdv node in nodes)
    {
        yield return node;
        foreach (var child in GetAllNodes(node.Nodes))
            yield return child;
    }
}
```

## Status-Based Coloring

```csharp
void ApplyStatusColor(TreeNodeAdv node)
{
    string status = node.SubItems.Count > 0 ? node.SubItems[0].Text : "";
    
    switch (status.ToLower())
    {
        case "completed":
            node.TextColor = Color.Green;
            node.Font = new Font(node.Font, FontStyle.Bold);
            break;
        case "in progress":
            node.TextColor = Color.Blue;
            break;
        case "error":
            node.TextColor = Color.Red;
            node.Font = new Font(node.Font, FontStyle.Bold);
            break;
        case "warning":
            node.TextColor = Color.Orange;
            break;
    }
    
    foreach (TreeNodeAdv child in node.Nodes)
        ApplyStatusColor(child);
}
```

## Alternating Row Colors

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
        node.Background = (index % 2 == 0) ? new BrushInfo(Color.White) : new BrushInfo(ColorTranslator.FromHtml("#F5F5F5"));
        index++;
        if (node.Expanded && node.Nodes.Count > 0)
            ApplyAlternatingColor(node.Nodes, ref index);
    }
}
```

## Best Practices

- Use visual styles for consistent modern appearance
- Apply gradients sparingly
- Ensure sufficient contrast between text and background
- Test themes in different lighting conditions
- Keep image sizes consistent within image lists
