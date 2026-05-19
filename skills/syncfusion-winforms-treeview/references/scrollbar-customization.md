# ScrollBar Customization

TreeViewAdv provides automatic scrolling support with customizable scrollbar appearance and behavior.

## Overview

TreeViewAdv automatically displays scrollbars when content exceeds visible area. Customize scrollbar appearance, colors, and behavior to match application design.

## Automatic Scrolling

Scrollbars appear automatically:

```csharp
// Automatic horizontal scrollbar
if (content width > control width)
    // Horizontal scrollbar shown

// Automatic vertical scrollbar
if (content height > control height)
    // Vertical scrollbar shown
```

## ScrollBar Properties

### Basic Configuration

```csharp
// Show/hide scrollbars
treeViewAdv1.HScroll = true;  // Horizontal scrollbar
treeViewAdv1.VScroll = true;  // Vertical scrollbar
```

## Scroll Behavior

### EnsureVisible

Scroll to make specific node visible:

```csharp
// Scroll to node
TreeNodeAdv node = FindNode("TargetNode");
if (node != null)
{
    node.EnsureVisible();
}
```

### Programmatic Scrolling

```csharp
// Scroll to top
treeViewAdv1.TopNode = treeViewAdv1.Nodes[0];

// Scroll to specific node
treeViewAdv1.TopNode = specificNode;
```


## ScrollBar Theming

Match scrollbars to control theme:

```csharp
// Apply theme to include scrollbars
treeViewAdv1.ThemeName = "Office2019Colorful";

// Custom Office colors
treeViewAdv1.Office2007ScrollBars = true;
treeViewAdv1.Office2007ColorScheme = Office2007Theme.Blue;
```

## Complete Example

```csharp
public class ScrollBarCustomizationExample : Form
{
    private TreeViewAdv treeViewAdv1;
    
    public ScrollBarCustomizationExample()
    {
        InitializeTree();
        CustomizeScrollBars();
        LoadLargeDataset();
    }
    
    private void InitializeTree()
    {
        treeViewAdv1 = new TreeViewAdv();
        treeViewAdv1.Size = new Size(400, 300);
        treeViewAdv1.Location = new Point(20, 20);
        this.Controls.Add(treeViewAdv1);
    }
    
    private void CustomizeScrollBars()
    {
        // Enable scrollbars
        treeViewAdv1.HScroll = true;
        treeViewAdv1.VScroll = true;
        
        // Office theme scrollbars
        treeViewAdv1.Office2007ScrollBars = true;
        treeViewAdv1.Office2007ColorScheme = Office2007Theme.Blue;
        
    }
    
    private void LoadLargeDataset()
    {
        // Add many nodes to trigger scrollbars
        for (int i = 0; i < 100; i++)
        {
            TreeNodeAdv parent = new TreeNodeAdv($"Parent {i}");
            
            for (int j = 0; j < 50; j++)
            {
                parent.Nodes.Add(new TreeNodeAdv($"Child {i}.{j}"));
            }
            
            treeViewAdv1.Nodes.Add(parent);
        }
    }
}
```

## Troubleshooting

**Issue:** Scrollbars not appearing
- **Solution:** Verify content exceeds control size, check `HScroll` and `VScroll` properties

**Issue:** Cannot scroll to specific node
- **Solution:** Use `node.EnsureVisible()` method, ensure node exists and is not hidden

**Issue:** Custom scrollbar colors not applying
- **Solution:** Verify theme settings, some themes override custom colors
