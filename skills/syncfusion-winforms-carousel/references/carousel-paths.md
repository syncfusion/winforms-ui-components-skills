# Carousel Path Support

The Carousel control supports multiple path arrangements for organizing and displaying items. Choose the path that best suits your layout requirements and visual design.

## CarouselPath Enumeration

The `CarouselPath` property determines how items are arranged in the carousel:

```csharp
public enum CarouselPath
{
    Default,  // Elliptical arrangement
    Orbital,  // Orbital curve arrangement
    Oval,     // Oval structure arrangement
    Linear    // Horizontal linear arrangement
}
```

## Default Path (Elliptical)

Items are arranged in a normal elliptical path - the classic carousel appearance.

### When to Use

- Standard 3D carousel displays
- Balanced visual layouts
- Product showcases
- General-purpose carousels
- When you need a familiar circular navigation

### Configuration

**C# Example:**

```csharp
Carousel carousel1 = new Carousel();
carousel1.CarouselPath = CarouselPath.Default;
carousel1.Size = new Size(600, 400);
carousel1.Perspective = 4.0f;

// Add items
for (int i = 1; i <= 6; i++)
{
    ButtonAdv btn = new ButtonAdv { Text = $"Item {i}", Size = new Size(100, 80) };
    carousel1.Controls.Add(btn);
    carousel1.Items.Add(btn);
}

this.Controls.Add(carousel1);
```

**VB.NET Example:**

```vb
Dim carousel1 As New Carousel()
carousel1.CarouselPath = CarouselPath.Default
carousel1.Size = New Size(600, 400)
carousel1.Perspective = 4.0F

' Add items
For i As Integer = 1 To 6
    Dim btn As New ButtonAdv With {
        .Text = $"Item {i}",
        .Size = New Size(100, 80)
    }
    carousel1.Controls.Add(btn)
    carousel1.Items.Add(btn)
Next

Me.Controls.Add(carousel1)
```

### Visual Characteristics

- **Shape**: Elliptical/Circular
- **Depth**: Strong 3D perspective effect
- **Item Spacing**: Evenly distributed around ellipse
- **Best Perspective**: 3.0 - 5.0
- **Recommended Items**: 6-10 items

### Example: Product Showcase

```csharp
Carousel productCarousel = new Carousel();
productCarousel.CarouselPath = CarouselPath.Default;
productCarousel.Size = new Size(700, 500);
productCarousel.Perspective = 4.5f;
productCarousel.TransitionSpeed = 2.0f;
productCarousel.ImageSlides = true;
productCarousel.ShowImagePreview = true;

// Load product images
foreach (var productImage in productImageList)
{
    CarouselImage img = new CarouselImage();
    img.ItemImage = productImage;
    productCarousel.ImageListCollection.Add(img);
}

this.Controls.Add(productCarousel);
```

## Orbital Path

Items are arranged in an orbital curve - resembling planetary orbits or scientific visualizations.

### When to Use

- Space-themed applications
- Scientific or technical UIs
- Unique visual designs
- Dashboard widgets
- Astronomy or physics applications

### Configuration

**C# Example:**

```csharp
Carousel carousel1 = new Carousel();
carousel1.CarouselPath = CarouselPath.Orbital;
carousel1.Size = new Size(600, 400);
carousel1.Perspective = 3.5f;
carousel1.TransitionSpeed = 1.5f;

// Add orbital items
for (int i = 1; i <= 8; i++)
{
    Panel panel = new Panel();
    panel.Size = new Size(100, 100);
    panel.BackColor = Color.FromArgb(50, 150, 200);
    
    Label label = new Label();
    label.Text = $"Node {i}";
    label.Dock = DockStyle.Fill;
    label.TextAlign = ContentAlignment.MiddleCenter;
    label.ForeColor = Color.White;
    
    panel.Controls.Add(label);
    carousel1.Controls.Add(panel);
    carousel1.Items.Add(panel);
}

this.Controls.Add(carousel1);
```

**VB.NET Example:**

```vb
Dim carousel1 As New Carousel()
carousel1.CarouselPath = CarouselPath.Orbital
carousel1.Size = New Size(600, 400)
carousel1.Perspective = 3.5F
carousel1.TransitionSpeed = 1.5F

' Add orbital items
For i As Integer = 1 To 8
    Dim panel As New Panel()
    panel.Size = New Size(100, 100)
    panel.BackColor = Color.FromArgb(50, 150, 200)
    
    Dim label As New Label()
    label.Text = $"Node {i}"
    label.Dock = DockStyle.Fill
    label.TextAlign = ContentAlignment.MiddleCenter
    label.ForeColor = Color.White
    
    panel.Controls.Add(label)
    carousel1.Controls.Add(panel)
    carousel1.Items.Add(panel)
Next

Me.Controls.Add(carousel1)
```

### Visual Characteristics

- **Shape**: Orbital curve
- **Depth**: Moderate 3D effect with orbital emphasis
- **Item Spacing**: Follows orbital trajectory
- **Best Perspective**: 2.5 - 4.0
- **Recommended Items**: 6-12 items

### Example: Dashboard Monitors

```csharp
Carousel dashboardCarousel = new Carousel();
dashboardCarousel.CarouselPath = CarouselPath.Orbital;
dashboardCarousel.Size = new Size(800, 600);
dashboardCarousel.Perspective = 3.0f;
dashboardCarousel.RotateAlways = true;
dashboardCarousel.TransitionSpeed = 1.0f;
dashboardCarousel.BackColor = Color.FromArgb(20, 20, 20);

// Add monitoring panels
string[] metrics = { "CPU", "Memory", "Network", "Disk", "Processes", "Services" };
Color[] colors = {
    Color.FromArgb(255, 99, 71),   // Tomato
    Color.FromArgb(50, 205, 50),   // LimeGreen
    Color.FromArgb(30, 144, 255),  // DodgerBlue
    Color.FromArgb(255, 215, 0),   // Gold
    Color.FromArgb(147, 112, 219), // MediumPurple
    Color.FromArgb(255, 140, 0)    // DarkOrange
};

for (int i = 0; i < metrics.Length; i++)
{
    Panel metricPanel = new Panel();
    metricPanel.Size = new Size(120, 100);
    metricPanel.BackColor = colors[i];
    
    Label lblTitle = new Label();
    lblTitle.Text = metrics[i];
    lblTitle.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    lblTitle.ForeColor = Color.White;
    lblTitle.Location = new Point(10, 10);
    lblTitle.AutoSize = true;
    
    Label lblValue = new Label();
    lblValue.Text = "0%";
    lblValue.Font = new Font("Segoe UI", 16F, FontStyle.Bold);
    lblValue.ForeColor = Color.White;
    lblValue.Location = new Point(10, 40);
    lblValue.AutoSize = true;
    
    metricPanel.Controls.Add(lblTitle);
    metricPanel.Controls.Add(lblValue);
    
    dashboardCarousel.Controls.Add(metricPanel);
    dashboardCarousel.Items.Add(metricPanel);
}

this.Controls.Add(dashboardCarousel);
```

## Oval Path

Items are arranged in an oval structure - a wider, flattened elliptical arrangement.

### When to Use

- Wide display areas
- Horizontal emphasis layouts
- Banner-style presentations
- When you need more horizontal spread
- Landscape-oriented displays

### Configuration

**C# Example:**

```csharp
Carousel carousel1 = new Carousel();
carousel1.CarouselPath = CarouselPath.Oval;
carousel1.Size = new Size(800, 400);
carousel1.Perspective = 3.0f;
carousel1.TransitionSpeed = 2.5f;

// Add items
for (int i = 1; i <= 7; i++)
{
    ButtonAdv btn = new ButtonAdv();
    btn.Text = $"Section {i}";
    btn.Size = new Size(120, 90);
    btn.BackColor = Color.FromArgb(70, 130, 180);
    btn.ForeColor = Color.White;
    
    carousel1.Controls.Add(btn);
    carousel1.Items.Add(btn);
}

this.Controls.Add(carousel1);
```

**VB.NET Example:**

```vb
Dim carousel1 As New Carousel()
carousel1.CarouselPath = CarouselPath.Oval
carousel1.Size = New Size(800, 400)
carousel1.Perspective = 3.0F
carousel1.TransitionSpeed = 2.5F

' Add items
For i As Integer = 1 To 7
    Dim btn As New ButtonAdv()
    btn.Text = $"Section {i}"
    btn.Size = New Size(120, 90)
    btn.BackColor = Color.FromArgb(70, 130, 180)
    btn.ForeColor = Color.White
    
    carousel1.Controls.Add(btn)
    carousel1.Items.Add(btn)
Next

Me.Controls.Add(carousel1)
```

### Visual Characteristics

- **Shape**: Oval (wide ellipse)
- **Depth**: Moderate 3D effect
- **Item Spacing**: More horizontal spread
- **Best Perspective**: 2.5 - 4.0
- **Recommended Items**: 5-9 items

### Example: Wide Gallery

```csharp
Carousel galleryCarousel = new Carousel();
galleryCarousel.CarouselPath = CarouselPath.Oval;
galleryCarousel.Dock = DockStyle.Fill;
galleryCarousel.ImageSlides = true;
galleryCarousel.Perspective = 3.5f;
galleryCarousel.TransitionSpeed = 2.0f;
galleryCarousel.ShowImageShadow = true;
galleryCarousel.ShowImagePreview = true;

// Load wide-format images
string[] imageFiles = Directory.GetFiles("WideImages", "*.jpg");
foreach (string imageFile in imageFiles)
{
    CarouselImage img = new CarouselImage();
    img.ItemImage = Image.FromFile(imageFile);
    galleryCarousel.ImageListCollection.Add(img);
}

this.Controls.Add(galleryCarousel);
```

## Linear Path

Items are arranged in a horizontal linear structure - like a horizontal scrolling strip.

### When to Use

- Navigation bars
- Horizontal menu systems
- Timeline displays
- Category selectors
- Horizontal galleries
- Tab-like navigation

### Configuration

**C# Example:**

```csharp
Carousel carousel1 = new Carousel();
carousel1.CarouselPath = CarouselPath.Linear;
carousel1.Size = new Size(800, 150);
carousel1.Dock = DockStyle.Top;
carousel1.TransitionSpeed = 3.0f;

// Add navigation items
string[] sections = { "Home", "Products", "Services", "Portfolio", "About", "Contact" };

foreach (string section in sections)
{
    ButtonAdv navButton = new ButtonAdv();
    navButton.Text = section;
    navButton.Size = new Size(120, 100);
    navButton.BackColor = Color.FromArgb(41, 128, 185);
    navButton.ForeColor = Color.White;
    navButton.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    
    carousel1.Controls.Add(navButton);
    carousel1.Items.Add(navButton);
}

this.Controls.Add(carousel1);
```

**VB.NET Example:**

```vb
Dim carousel1 As New Carousel()
carousel1.CarouselPath = CarouselPath.Linear
carousel1.Size = New Size(800, 150)
carousel1.Dock = DockStyle.Top
carousel1.TransitionSpeed = 3.0F

' Add navigation items
Dim sections() As String = {"Home", "Products", "Services", "Portfolio", "About", "Contact"}

For Each section As String In sections
    Dim navButton As New ButtonAdv()
    navButton.Text = section
    navButton.Size = New Size(120, 100)
    navButton.BackColor = Color.FromArgb(41, 128, 185)
    navButton.ForeColor = Color.White
    navButton.Font = New Font("Segoe UI", 10F, FontStyle.Bold)
    
    carousel1.Controls.Add(navButton)
    carousel1.Items.Add(navButton)
Next

Me.Controls.Add(carousel1)
```

### Visual Characteristics

- **Shape**: Horizontal line
- **Depth**: Minimal/no 3D effect
- **Item Spacing**: Horizontal linear arrangement
- **Best Perspective**: Not applicable (or 0)
- **Recommended Items**: 4-8 items

### Example: Category Navigator

```csharp
Carousel categoryCarousel = new Carousel();
categoryCarousel.CarouselPath = CarouselPath.Linear;
categoryCarousel.Size = new Size(1000, 120);
categoryCarousel.Location = new Point(0, 50);
categoryCarousel.TransitionSpeed = 4.0f;
categoryCarousel.BackColor = Color.FromArgb(52, 73, 94);

// Add category buttons with icons
var categories = new[] {
    ("Electronics", Color.FromArgb(52, 152, 219)),
    ("Clothing", Color.FromArgb(155, 89, 182)),
    ("Home & Garden", Color.FromArgb(46, 204, 113)),
    ("Sports", Color.FromArgb(241, 196, 15)),
    ("Books", Color.FromArgb(230, 126, 34)),
    ("Toys", Color.FromArgb(231, 76, 60))
};

foreach (var (name, color) in categories)
{
    Panel categoryPanel = new Panel();
    categoryPanel.Size = new Size(140, 90);
    categoryPanel.BackColor = color;
    
    Label label = new Label();
    label.Text = name;
    label.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
    label.ForeColor = Color.White;
    label.TextAlign = ContentAlignment.MiddleCenter;
    label.Dock = DockStyle.Fill;
    
    categoryPanel.Controls.Add(label);
    categoryCarousel.Controls.Add(categoryPanel);
    categoryCarousel.Items.Add(categoryPanel);
}

this.Controls.Add(categoryCarousel);
```

## Changing Paths Dynamically

You can change the carousel path at runtime based on user selection or layout requirements.

```csharp
private void btnDefault_Click(object sender, EventArgs e)
{
    carousel1.CarouselPath = CarouselPath.Default;
    carousel1.Perspective = 4.0f;
}

private void btnOrbital_Click(object sender, EventArgs e)
{
    carousel1.CarouselPath = CarouselPath.Orbital;
    carousel1.Perspective = 3.5f;
}

private void btnOval_Click(object sender, EventArgs e)
{
    carousel1.CarouselPath = CarouselPath.Oval;
    carousel1.Perspective = 3.0f;
}

private void btnLinear_Click(object sender, EventArgs e)
{
    carousel1.CarouselPath = CarouselPath.Linear;
    carousel1.Perspective = 0f; // No perspective for linear
}
```

## Path Comparison Table

| Path | Shape | 3D Depth | Best Use Case | Perspective | Items |
|------|-------|----------|---------------|-------------|-------|
| **Default** | Ellipse | Strong | General carousels, balanced layouts | 3.0-5.0 | 6-10 |
| **Orbital** | Orbital | Moderate | Space themes, dashboards, technical UIs | 2.5-4.0 | 6-12 |
| **Oval** | Wide Ellipse | Moderate | Wide displays, horizontal emphasis | 2.5-4.0 | 5-9 |
| **Linear** | Horizontal Line | Minimal | Navigation, menus, horizontal galleries | 0 | 4-8 |

## Choosing the Right Path

### Decision Guide

**Use Default when:**
- You need a classic 3D carousel appearance
- Layout is roughly square (width ≈ height)
- You want strong depth perception
- Building general-purpose galleries

**Use Orbital when:**
- You want a unique, distinctive look
- Building dashboards or monitoring tools
- Space/scientific theme fits your design
- You need moderate 3D effect with orbital feel

**Use Oval when:**
- Your layout is wider than tall (width >> height)
- You need horizontal emphasis
- Building banner-style presentations
- You want oval stretching effect

**Use Linear when:**
- You need horizontal navigation
- Building menu systems
- You want minimal or no 3D effect
- Items should appear side-by-side
- Building category selectors or tab-like interfaces

## Best Practices

1. **Match Path to Layout**: Choose path based on available space (Default for square, Oval for wide, Linear for horizontal strips)

2. **Adjust Perspective**: Each path works best with different perspective values (see table above)

3. **Item Count**: Don't overload - keep items reasonable for each path type

4. **Transition Speed**: Linear paths often work well with faster speeds (3.0-4.0)

5. **Test All Paths**: Try different paths during development to see which fits best

6. **Consider Content**: Image galleries work well with Default/Oval, navigation with Linear

7. **Dynamic Switching**: Allow users to switch paths if appropriate for your application

## Next Steps

- **Visual Effects**: See [visual-configuration.md](visual-configuration.md) to customize perspective and speed
- **Images**: See [image-slides.md](image-slides.md) for image-specific paths
- **Rotation**: See [rotation-behavior.md](rotation-behavior.md) for auto-rotation with paths
