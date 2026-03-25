# Appearance and Styling

## Table of Contents
- [Visual Styles](#visual-styles)
- [Header Customization](#header-customization)
- [Border Customization](#border-customization)
- [Item Spacing](#item-spacing)
- [TreeMenuItem Appearance](#treemenuitem-appearance)
- [Text Alignment](#text-alignment)
- [Complete Styling Examples](#complete-styling-examples)

---

## Visual Styles

The TreeNavigator supports multiple built-in visual styles that provide a consistent, professional appearance matching Office themes.

### Available Styles

The `Style` property applies predefined visual themes to the entire TreeNavigator control.

**Style Options:**
- **Office2016Colorful** - Modern, vibrant Office 2016 theme
- **Office2016White** - Clean, minimal white theme
- **Office2016Black** - Dark theme with high contrast
- **Office2016DarkGray** - Reduced contrast dark theme
- **Office2019Colorful** - Latest Office 2019 design language
- **Default** - Standard Windows Forms appearance

### Applying Styles

**C# Example:**

```csharp
using Syncfusion.Windows.Forms.Tools;

TreeNavigator treeNavigator = new TreeNavigator();

// Apply Office2016Colorful style
treeNavigator.Style = TreeNavigatorStyle.Office2016Colorful;
```

**VB.NET Example:**

```vbnet
Imports Syncfusion.Windows.Forms.Tools

Dim treeNavigator As New TreeNavigator()

' Apply Office2016Colorful style
treeNavigator.Style = TreeNavigatorStyle.Office2016Colorful
```

### Style Comparison

| Style | Best For | Characteristics |
|-------|----------|-----------------|
| **Office2016Colorful** | Modern applications | Blue accent colors, vibrant look |
| **Office2016White** | Minimal interfaces | Clean white background, subtle borders |
| **Office2016Black** | Dark mode apps | High contrast, dark background |
| **Office2016DarkGray** | Low-light environments | Dark with reduced contrast |
| **Office2019Colorful** | Latest Office look | Updated Office 2019 design |
| **Default** | Classic Windows apps | Standard WinForms appearance |

### Style Examples

**Office2016Colorful:**
```csharp
treeNavigator.Style = TreeNavigatorStyle.Office2016Colorful;
// Result: Blue-tinted headers, vibrant hover effects
```

**Office2016White:**
```csharp
treeNavigator.Style = TreeNavigatorStyle.Office2016White;
// Result: Clean white background, minimal visual noise
```

**Office2016Black:**
```csharp
treeNavigator.Style = TreeNavigatorStyle.Office2016Black;
// Result: Dark background, light text, high contrast
```

**Office2016DarkGray:**
```csharp
treeNavigator.Style = TreeNavigatorStyle.Office2016DarkGray;
// Result: Medium-dark background, reduced eye strain
```

---

## Header Customization

The TreeNavigator header displays the title and provides visual context for the navigation.

### Header Properties

| Property | Type | Description |
|----------|------|-------------|
| **HeaderText** | string | Text displayed in the header |
| **Height** | int | Header height in pixels |
| **HeaderBackColor** | Color | Background color of the header |
| **HeaderForeColor** | Color | Text color of the header |
| **TextBounds** | Rectangle | Bounds for header text positioning |

### Setting Header Text

**C# Example:**

```csharp
// Set header text
treeNavigator.Header.HeaderText = "File Explorer";
```

**VB.NET Example:**

```vbnet
' Set header text
treeNavigator.Header.HeaderText = "File Explorer"
```

### Customizing Header Height

**C# Example:**

```csharp
// Set header height to 50 pixels
treeNavigator.Header.Height = 50;
```

**VB.NET Example:**

```vbnet
' Set header height to 50 pixels
treeNavigator.Header.Height = 50
```

### Customizing Header Colors

**C# Example:**

```csharp
// Set custom header colors
treeNavigator.Header.HeaderBackColor = Color.Turquoise;
treeNavigator.Header.HeaderForeColor = Color.Black;
```

**VB.NET Example:**

```vbnet
' Set custom header colors
treeNavigator.Header.HeaderBackColor = Color.Turquoise
treeNavigator.Header.HeaderForeColor = Color.Black
```

### Custom Header Text Positioning

**C# Example:**

```csharp
// Position header text at specific bounds
treeNavigator.Header.TextBounds = new Rectangle(40, 0, 30, 20);
// X=40, Y=0, Width=30, Height=20
```

**VB.NET Example:**

```vbnet
' Position header text at specific bounds
treeNavigator.Header.TextBounds = New Rectangle(40, 0, 30, 20)
' X=40, Y=0, Width=30, Height=20
```

### Complete Header Customization Example

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();

// Comprehensive header customization
treeNavigator.Header.Height = 50;
treeNavigator.Header.HeaderText = "My Application";
treeNavigator.Header.HeaderBackColor = Color.FromArgb(0, 120, 215); // Blue
treeNavigator.Header.HeaderForeColor = Color.White;
treeNavigator.Header.TextBounds = new Rectangle(10, 0, 200, 50);
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()

' Comprehensive header customization
treeNavigator.Header.Height = 50
treeNavigator.Header.HeaderText = "My Application"
treeNavigator.Header.HeaderBackColor = Color.FromArgb(0, 120, 215) ' Blue
treeNavigator.Header.HeaderForeColor = Color.White
treeNavigator.Header.TextBounds = New Rectangle(10, 0, 200, 50)
```

### Hiding the Header

You can hide the header completely for a more compact appearance.

**C# Example:**

```csharp
// Hide the header area
treeNavigator.ShowHeader = false;
```

**VB.NET Example:**

```vbnet
' Hide the header area
treeNavigator.ShowHeader = False
```

**When to hide the header:**
- Embedding TreeNavigator in a panel with its own title
- Creating a compact navigation sidebar
- Header text is redundant with surrounding UI

---

## Border Customization

Customize the TreeNavigator's border appearance to match your application design.

### Border Properties

| Property | Type | Description |
|----------|------|-------------|
| **BorderColor** | Color | Color of the control border |
| **BorderThickness** | int | Thickness of the border in pixels |

### Setting Border Color

**C# Example:**

```csharp
// Set border color to black
treeNavigator.BorderColor = Color.Black;
```

**VB.NET Example:**

```vbnet
' Set border color to black
treeNavigator.BorderColor = Color.Black
```

### Setting Border Thickness

**C# Example:**

```csharp
// Set border thickness to 5 pixels
treeNavigator.BorderThickness = 5;
```

**VB.NET Example:**

```vbnet
' Set border thickness to 5 pixels
treeNavigator.BorderThickness = 5
```

### Complete Border Customization

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();

// Custom border: thick red border
treeNavigator.BorderColor = Color.FromArgb(192, 0, 0); // Dark red
treeNavigator.BorderThickness = 3;
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()

' Custom border: thick red border
treeNavigator.BorderColor = Color.FromArgb(192, 0, 0) ' Dark red
treeNavigator.BorderThickness = 3
```

### Common Border Patterns

**Subtle Border:**
```csharp
treeNavigator.BorderColor = Color.LightGray;
treeNavigator.BorderThickness = 1;
```

**Accent Border:**
```csharp
treeNavigator.BorderColor = Color.FromArgb(0, 120, 215); // Blue accent
treeNavigator.BorderThickness = 2;
```

**No Border:**
```csharp
treeNavigator.BorderThickness = 0;
```

---

## Item Spacing

Control the vertical gap between TreeMenuItem items using the `PadY` property.

### PadY Property

**C# Example:**

```csharp
// Set 10 pixels gap between items
treeNavigator.PadY = 10;
```

**VB.NET Example:**

```vbnet
' Set 10 pixels gap between items
treeNavigator.PadY = 10
```

### Spacing Recommendations

| PadY Value | Use Case |
|------------|----------|
| **0-2** | Compact lists, many items |
| **5-8** | Standard spacing (default) |
| **10-15** | Spacious, touch-friendly |
| **20+** | Extra spacing for emphasis |

**Compact Spacing:**
```csharp
treeNavigator.PadY = 2; // Tight spacing
```

**Touch-Friendly Spacing:**
```csharp
treeNavigator.PadY = 15; // Easier to tap on touch screens
```

---

## TreeMenuItem Appearance

Customize individual TreeMenuItem items or set default appearance for all items.

### TreeMenuItem Color Properties

| Property | Scope | Description |
|----------|-------|-------------|
| **ItemBackColor** | TreeNavigator or Item | Background color for normal state |
| **ItemHoverColor** | Item | Background color when mouse hovers |
| **SelectedColor** | Item | Background color when selected |
| **SelectedItemForeColor** | Item | Text color when selected |

### Setting Default Item Background Color

The TreeNavigator's `ItemBackColor` property sets the default background color for all items.

**C# Example:**

```csharp
// Set default background color for all items
treeNavigator.ItemBackColor = Color.LightYellow;
```

**VB.NET Example:**

```vbnet
' Set default background color for all items
treeNavigator.ItemBackColor = Color.LightYellow
```

**Note:** Individual TreeMenuItem `ItemBackColor` takes priority over the TreeNavigator's default.

### Customizing Individual TreeMenuItem Colors

**C# Example:**

```csharp
TreeMenuItem item1 = new TreeMenuItem { Text = "Important Item" };

// Customize specific item colors
item1.ItemBackColor = Color.Turquoise; // Normal state
item1.ItemHoverColor = Color.LightPink; // Hover state
item1.SelectedColor = Color.Gainsboro; // Selected state
item1.SelectedItemForeColor = Color.Blue; // Text color when selected

treeNavigator.Items.Add(item1);
```

**VB.NET Example:**

```vbnet
Dim item1 As New TreeMenuItem With {.Text = "Important Item"}

' Customize specific item colors
item1.ItemBackColor = Color.Turquoise ' Normal state
item1.ItemHoverColor = Color.LightPink ' Hover state
item1.SelectedColor = Color.Gainsboro ' Selected state
item1.SelectedItemForeColor = Color.Blue ' Text color when selected

treeNavigator.Items.Add(item1)
```

### Color Priority Rules

1. **ItemBackColor Priority**: TreeMenuItem.ItemBackColor > TreeNavigator.ItemBackColor
2. **State Precedence**: Selected > Hover > Normal
3. **Color Inheritance**: Child items don't inherit parent item colors

### Common Color Schemes

**Warning Item:**
```csharp
TreeMenuItem warning = new TreeMenuItem 
{ 
    Text = "Critical Settings",
    ItemBackColor = Color.FromArgb(255, 244, 204), // Light yellow
    ItemHoverColor = Color.FromArgb(255, 235, 156),
    SelectedColor = Color.FromArgb(255, 193, 7),
    SelectedItemForeColor = Color.Black
};
```

**Success Item:**
```csharp
TreeMenuItem success = new TreeMenuItem 
{ 
    Text = "Verified Files",
    ItemBackColor = Color.FromArgb(212, 237, 218), // Light green
    ItemHoverColor = Color.FromArgb(195, 230, 203),
    SelectedColor = Color.FromArgb(40, 167, 69),
    SelectedItemForeColor = Color.White
};
```

**Error Item:**
```csharp
TreeMenuItem error = new TreeMenuItem 
{ 
    Text = "Failed Operations",
    ItemBackColor = Color.FromArgb(248, 215, 218), // Light red
    ItemHoverColor = Color.FromArgb(245, 198, 203),
    SelectedColor = Color.FromArgb(220, 53, 69),
    SelectedItemForeColor = Color.White
};
```

---

## Text Alignment

Control how text is aligned within TreeMenuItem items.

### TextAlign Property

**Available Options:**
- **TextAlignment.Left** - Text aligned to left edge
- **TextAlignment.Center** - Text centered horizontally
- **TextAlignment.Right** - Text aligned to right edge

**C# Example:**

```csharp
// Center-align all item text
treeNavigator.TextAlign = TextAlignment.Center;
```

**VB.NET Example:**

```vbnet
' Center-align all item text
treeNavigator.TextAlign = TextAlignment.Center
```

### Alignment Use Cases

| Alignment | Best For |
|-----------|----------|
| **Left** | Standard navigation (default) |
| **Center** | Menu buttons, symmetric layouts |
| **Right** | RTL languages, special formatting |

**Left-Aligned (Default):**
```csharp
treeNavigator.TextAlign = TextAlignment.Left;
```

**Center-Aligned:**
```csharp
treeNavigator.TextAlign = TextAlignment.Center;
// Use for button-like navigation
```

**Right-Aligned:**
```csharp
treeNavigator.TextAlign = TextAlignment.Right;
// Use for right-to-left languages or special layouts
```

---

## Complete Styling Examples

### Example 1: Modern Blue Theme

```csharp
TreeNavigator modernNav = new TreeNavigator();
modernNav.Size = new Size(280, 450);

// Apply Office2016Colorful style
modernNav.Style = TreeNavigatorStyle.Office2016Colorful;

// Customize header
modernNav.Header.HeaderText = "Dashboard";
modernNav.Header.Height = 50;
modernNav.Header.HeaderBackColor = Color.FromArgb(0, 120, 215);
modernNav.Header.HeaderForeColor = Color.White;

// Subtle border
modernNav.BorderColor = Color.FromArgb(0, 120, 215);
modernNav.BorderThickness = 2;

// Standard spacing
modernNav.PadY = 8;

// Add styled items
TreeMenuItem home = new TreeMenuItem 
{ 
    Text = "Home",
    ItemBackColor = Color.White,
    ItemHoverColor = Color.FromArgb(229, 243, 255),
    SelectedColor = Color.FromArgb(0, 120, 215),
    SelectedItemForeColor = Color.White
};

modernNav.Items.Add(home);
```

### Example 2: Dark Mode Theme

```csharp
TreeNavigator darkNav = new TreeNavigator();
darkNav.Size = new Size(280, 450);

// Apply dark style
darkNav.Style = TreeNavigatorStyle.Office2016Black;

// Dark header
darkNav.Header.HeaderText = "Navigation";
darkNav.Header.Height = 45;
darkNav.Header.HeaderBackColor = Color.FromArgb(30, 30, 30);
darkNav.Header.HeaderForeColor = Color.FromArgb(220, 220, 220);

// Dark border
darkNav.BorderColor = Color.FromArgb(60, 60, 60);
darkNav.BorderThickness = 1;

// Reduced spacing
darkNav.PadY = 5;

// Dark item colors
darkNav.ItemBackColor = Color.FromArgb(45, 45, 48);

// Add items with hover effects
TreeMenuItem item1 = new TreeMenuItem 
{ 
    Text = "Files",
    ItemHoverColor = Color.FromArgb(62, 62, 66),
    SelectedColor = Color.FromArgb(0, 122, 204),
    SelectedItemForeColor = Color.White
};

darkNav.Items.Add(item1);
```

### Example 3: Compact Sidebar Navigation

```csharp
TreeNavigator compactNav = new TreeNavigator();
compactNav.Size = new Size(200, 500);

// Use white theme
compactNav.Style = TreeNavigatorStyle.Office2016White;

// Hide header for compact look
compactNav.ShowHeader = false;

// Minimal border
compactNav.BorderColor = Color.LightGray;
compactNav.BorderThickness = 1;

// Tight spacing
compactNav.PadY = 2;

// Left-aligned text
compactNav.TextAlign = TextAlignment.Left;

// Light background
compactNav.ItemBackColor = Color.FromArgb(250, 250, 250);

// Add menu items
string[] menuItems = { "Dashboard", "Reports", "Analytics", "Settings", "Help" };
foreach (string menuText in menuItems)
{
    TreeMenuItem menuItem = new TreeMenuItem 
    { 
        Text = menuText,
        ItemHoverColor = Color.FromArgb(229, 243, 255),
        SelectedColor = Color.FromArgb(0, 120, 215),
        SelectedItemForeColor = Color.White
    };
    compactNav.Items.Add(menuItem);
}
```

### Example 4: Colorful Category Navigation

```csharp
TreeNavigator colorNav = new TreeNavigator();
colorNav.Size = new Size(280, 450);
colorNav.Style = TreeNavigatorStyle.Office2016Colorful;

// Vibrant header
colorNav.Header.HeaderText = "Categories";
colorNav.Header.Height = 55;
colorNav.Header.HeaderBackColor = Color.FromArgb(106, 90, 205); // Slate blue
colorNav.Header.HeaderForeColor = Color.White;

// Matching border
colorNav.BorderColor = Color.FromArgb(106, 90, 205);
colorNav.BorderThickness = 2;

// Comfortable spacing
colorNav.PadY = 10;

// Create color-coded categories
TreeMenuItem sales = new TreeMenuItem 
{ 
    Text = "Sales",
    ItemBackColor = Color.FromArgb(220, 255, 220),
    ItemHoverColor = Color.FromArgb(195, 245, 195),
    SelectedColor = Color.FromArgb(40, 167, 69),
    SelectedItemForeColor = Color.White
};

TreeMenuItem marketing = new TreeMenuItem 
{ 
    Text = "Marketing",
    ItemBackColor = Color.FromArgb(255, 235, 205),
    ItemHoverColor = Color.FromArgb(255, 222, 173),
    SelectedColor = Color.FromArgb(255, 140, 0),
    SelectedItemForeColor = Color.White
};

TreeMenuItem support = new TreeMenuItem 
{ 
    Text = "Support",
    ItemBackColor = Color.FromArgb(230, 230, 250),
    ItemHoverColor = Color.FromArgb(216, 216, 240),
    SelectedColor = Color.FromArgb(106, 90, 205),
    SelectedItemForeColor = Color.White
};

colorNav.Items.Add(sales);
colorNav.Items.Add(marketing);
colorNav.Items.Add(support);
```

---

## Best Practices

### Styling Consistency

1. **Match Application Theme**: Use styles that complement your overall application design
2. **Consistent Color Palette**: Use colors from your brand or design system
3. **Readable Contrast**: Ensure text is readable against background colors (WCAG guidelines)
4. **Hover Feedback**: Always provide hover color feedback for better UX

### Performance Considerations

1. **Avoid Excessive Customization**: Per-item colors impact performance with many items
2. **Use TreeNavigator Defaults**: Set `ItemBackColor` at TreeNavigator level when possible
3. **Limit PadY**: Excessive spacing increases scroll area and reduces visible items

### Accessibility

1. **Color Contrast**: Maintain 4.5:1 contrast ratio for normal text (WCAG AA)
2. **Don't Rely on Color Alone**: Use icons or text indicators in addition to colors
3. **Test with Screen Readers**: Ensure navigation is accessible with assistive technology

---

## Troubleshooting

### Item Colors Not Applying

**Problem:** Custom colors set on TreeMenuItem don't appear.

**Solution:**
1. Verify properties are set before adding item to TreeNavigator
2. Check if TreeNavigator's Style is overriding colors (try Style = Default)
3. Ensure colors aren't transparent: `Color.FromArgb(255, r, g, b)`

### Header Text Not Visible

**Problem:** Header text is cut off or not displaying.

**Solution:**
1. Increase header height: `treeNavigator.Header.Height = 50`
2. Adjust TextBounds if set: `treeNavigator.Header.TextBounds = new Rectangle(10, 5, 200, 40)`
3. Check HeaderForeColor contrasts with HeaderBackColor

### Border Not Showing

**Problem:** Border doesn't appear after setting BorderColor and BorderThickness.

**Solution:**
1. Ensure BorderThickness > 0: `treeNavigator.BorderThickness = 2`
2. Verify BorderColor is not transparent
3. Check if control is docked/anchored, which might clip borders

---

## Next Steps

- **TreeMenuItem Management**: Build complex hierarchies and manage items dynamically
- **Selection Events**: Respond to user navigation with selection events
- **Navigation Modes**: Choose between Default and Extended navigation styles
