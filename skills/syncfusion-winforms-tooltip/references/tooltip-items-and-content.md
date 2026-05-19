# Tooltip Items and Content

This guide covers how to structure and populate tooltip content using `ToolTipItem` objects, including text, images, custom controls, and layout options.

## Setting ToolTipItem

A `ToolTipItem` represents a single content section within a tooltip. Add items to the `ToolTipInfo.Items` collection using the `Add` method.

**Basic Example:**
```csharp
using Syncfusion.WinForms.Controls;

ToolTipInfo toolTipInfo1 = new ToolTipInfo();

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "The ToolTip information of the Button control.";

toolTipInfo1.Items.Add(toolTipItem1);
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Result:** Single-item tooltip with text content.

## Adding Multiple Items to a Tooltip

Display multiple sections in a single tooltip by adding multiple `ToolTipItem` objects to the collection.

### Using Add Method

Add items individually:

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "First section of information.";

ToolTipItem toolTipItem2 = new ToolTipItem();
toolTipItem2.Text = "Second section of information.";

toolTipInfo1.Items.Add(toolTipItem1);
toolTipInfo1.Items.Add(toolTipItem2);

sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

### Using AddRange Method

Add multiple items at once:

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "The ToolTip information of the Button control.";

ToolTipItem toolTipItem2 = new ToolTipItem();
toolTipItem2.Text = "Additional details about the button.";

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1, toolTipItem2 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Use Case:** Organize related information into logical sections (e.g., title, description, instructions).

### Practical Example: User Profile Tooltip

```csharp
ToolTipInfo profileInfo = new ToolTipInfo();

// Header item
ToolTipItem header = new ToolTipItem();
header.Text = "User Profile";
header.Style.Font = new Font("Arial", 11f, FontStyle.Bold);
header.Style.BackColor = Color.LightBlue;

// Details item
ToolTipItem details = new ToolTipItem();
details.Text = "Name: John Doe\nEmail: john@company.com\nRole: Administrator";

// Status item
ToolTipItem status = new ToolTipItem();
status.Text = "Status: Active";
status.Style.ForeColor = Color.Green;

profileInfo.Items.AddRange(new ToolTipItem[] { header, details, status });
sfToolTip1.SetToolTipInfo(this.userButton, profileInfo);
```

## Spacing Between Items

Control the spacing or padding between tooltip items using the `Padding` property.

**Syntax:**
```csharp
toolTipItem.Padding = new Padding(left, top, right, bottom);
// or
toolTipItem.Padding = new Padding(allSides);
```

**Example:**
```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "ToolTipItem1 Text";
toolTipItem1.Padding = new Padding(12); // 12 pixels on all sides

ToolTipItem toolTipItem2 = new ToolTipItem();
toolTipItem2.Text = "ToolTipItem2 Text";
toolTipItem2.Padding = new Padding(12);

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1, toolTipItem2 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Result:** Increased whitespace around each item for better readability.

**Custom Padding Example:**
```csharp
// Different padding on each side
toolTipItem1.Padding = new Padding(10, 5, 10, 5); // left, top, right, bottom
```

## Adding Images to Tooltips

Enhance tooltips with images using the `Image` or `ImageList` properties.

### Using Image Property

Set a single image directly on the `ToolTipItem`.

**Example:**
```csharp
using System.Drawing;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "This image is initialized with Image property.";
toolTipItem1.Image = global::GettingStarted.Properties.Resources.Image1;
toolTipItem1.Style.ImageSize = new Size(100, 100);

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.Items.Add(toolTipItem1);
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Use Case:** Display product images, user avatars, or icon representations.

### Using ImageList Property

Use `ImageList` when managing multiple images or switching between images dynamically.

**Example:**
```csharp
// Create and populate ImageList
ImageList imageList = new ImageList();
imageList.Images.Add(global::GettingStarted.Properties.Resources.Image1);
imageList.Images.Add(global::GettingStarted.Properties.Resources.Image2);

// Create tooltip items with different images
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "This image is initialized with Image property.";
toolTipItem1.Image = global::GettingStarted.Properties.Resources.Image1;
toolTipItem1.Style.ImageSize = new Size(100, 100);

ToolTipItem toolTipItem2 = new ToolTipItem();
toolTipItem2.Text = "This image is initialized with ImageList property.";
toolTipItem2.ImageList = imageList;
toolTipItem2.ImageIndex = 1; // Use second image
toolTipItem2.ImageList.ImageSize = new Size(100, 100);

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1, toolTipItem2 });
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Important:** The `Image` property takes precedence over `ImageList` when both are set.

### Choosing Between Image and ImageList

| Use `Image` When: | Use `ImageList` When: |
|-------------------|----------------------|
| Single static image | Multiple images to choose from |
| Image embedded in resources | Dynamically switching images |
| Simple implementation | Centralized image management |

## Changing Image Alignment

Control where the image appears relative to text using the `ImageAlignment` property.

**Available Alignments:**
- `ToolTipImageAlignment.Left` (default)
- `ToolTipImageAlignment.Top`
- `ToolTipImageAlignment.Right`

### Left Alignment (Default)

```csharp
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Image = global::GettingStarted.Properties.Resources.Image1;
toolTipItem1.Style.ImageAlignment = ToolTipImageAlignment.Left;
toolTipItem1.Style.ImageSize = new Size(80, 80);
```

**Result:** Image on left, text on right.


### Right Alignment

```csharp
toolTipItem1.Style.ImageAlignment = ToolTipImageAlignment.Right;
```

**Result:** Text on left, image on right.

**Use Case:** RTL layouts or design preference.

## Setting Image Size

Explicitly set image dimensions using the `ImageSize` property.

**Syntax:**
```csharp
toolTipItem.Style.ImageSize = new Size(width, height);
```

**Example:**
```csharp
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Image = global::GettingStarted.Properties.Resources.Image1;
toolTipItem1.Style.ImageSize = new Size(100, 100);
```

**Important:** When `ImageSize` is not set:
- Default size is (32, 32) pixels
- The original `Image.Size` or `ImageList.ImageSize` is **not** used

**Recommendation:** Always set `ImageSize` explicitly for consistent appearance.

**Aspect Ratio Consideration:**
```csharp
// Maintain aspect ratio manually
Image originalImage = Properties.Resources.UserPhoto;
double aspectRatio = (double)originalImage.Width / originalImage.Height;
toolTipItem1.Style.ImageSize = new Size(100, (int)(100 / aspectRatio));
```

## Setting Spacing Between Image and Text

Control the gap between image and text using the `ImageToTextOffset` property.

**Syntax:**
```csharp
toolTipItem.Style.ImageToTextOffset = offsetInPixels;
```

**Example:**
```csharp
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleLeft;
toolTipItem1.Image = global::GettingStarted.Properties.Resources.MORGK;
toolTipItem1.Style.ImageSize = new Size(100, 100);
toolTipItem1.Style.ImageToTextOffset = 20; // 20 pixels spacing

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Use Case:** Increase spacing for better visual separation in dense layouts.

**Default Spacing:** If not specified, a default spacing is applied (typically 4-6 pixels).

## Adding Custom User Controls to Tooltips

Host any Windows Forms control inside a tooltip using the `Control` property.

**Syntax:**
```csharp
toolTipItem.Control = yourCustomControl;
```

### Example: PictureBox with Animated GIF

```csharp
// Create custom control
PictureBox pictureBox1 = new PictureBox();
pictureBox1.Image = Image.FromFile(@"../../Resources/cube.gif");
pictureBox1.SizeMode = PictureBoxSizeMode.CenterImage;
pictureBox1.Size = new Size(200, 100);
pictureBox1.BorderStyle = BorderStyle.FixedSingle;

// Add to tooltip
ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Control = pictureBox1;

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Result:** Animated GIF displays in tooltip.

### Example: Progress Bar in Tooltip

```csharp
ProgressBar progressBar = new ProgressBar();
progressBar.Size = new Size(200, 20);
progressBar.Value = 65;

ToolTipItem progressItem = new ToolTipItem();
progressItem.Control = progressBar;

ToolTipInfo toolTipInfo = new ToolTipInfo();
toolTipInfo.Items.Add(progressItem);
sfToolTip1.SetToolTipInfo(this.processButton, toolTipInfo);
```

### Example: Custom User Control

```csharp
// Assume you have a custom UserControl called "MiniChartControl"
MiniChartControl chartControl = new MiniChartControl();
chartControl.Size = new Size(300, 150);
chartControl.LoadData(chartData);

ToolTipItem chartItem = new ToolTipItem();
chartItem.Control = chartControl;

ToolTipInfo toolTipInfo = new ToolTipInfo();
toolTipInfo.Items.Add(chartItem);
sfToolTip1.SetToolTipInfo(this.dashboardPanel, toolTipInfo);
```

**Use Cases:**
- Display mini charts or graphs
- Show live data visualizations
- Embed video players
- Custom formatted content
- Interactive controls (use with caution)

**Important Considerations:**
1. **Performance:** Complex controls may cause lag when tooltips appear
2. **Event Handling:** Control events work within tooltips but may feel unintuitive to users
3. **Size:** Ensure control size is appropriate for tooltip display
4. **Lifecycle:** Controls are created when tooltip shows; consider resource usage

### Combining Text and Custom Controls

```csharp
ToolTipInfo toolTipInfo = new ToolTipInfo();

// Header text
ToolTipItem header = new ToolTipItem();
header.Text = "Current Status";
header.Style.Font = new Font("Arial", 10f, FontStyle.Bold);

// Custom control
ProgressBar progressBar = new ProgressBar();
progressBar.Size = new Size(200, 20);
progressBar.Value = 75;
ToolTipItem progressItem = new ToolTipItem();
progressItem.Control = progressBar;

// Footer text
ToolTipItem footer = new ToolTipItem();
footer.Text = "75% Complete";

toolTipInfo.Items.AddRange(new ToolTipItem[] { header, progressItem, footer });
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo);
```

## Summary

This guide covered:
- **ToolTipItem basics:** Adding single and multiple items
- **Layout control:** Padding and spacing between items
- **Images:** Using Image and ImageList properties
- **Image configuration:** Alignment, sizing, and text offset
- **Custom controls:** Hosting any Windows Forms control in tooltips

**Best Practices:**
1. Use multiple items to organize information hierarchically
2. Set explicit `ImageSize` for consistent appearance
3. Adjust `ImageToTextOffset` for comfortable reading
4. Use custom controls sparingly to maintain performance
5. Test tooltip display with actual content to ensure proper sizing

**Next Steps:**
- Learn balloon-style tooltips in [balloon-style-and-beak.md](balloon-style-and-beak.md)
- Explore appearance customization in [appearance-customization.md](appearance-customization.md)
