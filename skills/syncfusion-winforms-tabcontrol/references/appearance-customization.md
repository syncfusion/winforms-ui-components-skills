# Appearance and Customization

## Table of Contents
- [Background Settings](#background-settings)
- [BackgroundImage Settings](#backgroundimage-settings)
- [Color Customization](#color-customization)
- [Font Settings](#font-settings)
- [Border Settings](#border-settings)
- [Tab Styles](#tab-styles)
- [SizeMode Options](#sizemode-options)

Customize the visual appearance of TabControlAdv including colors, fonts, images, borders, and themes.

## Background Settings

### Control Background Color

```csharp
// Set background color for the entire control
tabControlAdv1.BackColor = Color.LightGray;

// Set background for the tab panel area
tabControlAdv1.TabPanelBackColor = Color.White;
```

### Active and Inactive Tab Colors

```csharp
// Active tab background color
tabControlAdv1.ActiveTabColor = Color.Ivory;

// Inactive tabs background color
tabControlAdv1.InactiveTabColor = Color.Silver;
```

### Individual Tab Colors

Set custom background color for specific tabs:

```csharp
// Set color for individual tab
tabPageAdv1.TabBackColor = Color.Pink;
tabPageAdv2.TabBackColor = Color.LightBlue;
tabPageAdv3.TabBackColor = Color.LightGreen;
```

## BackgroundImage Settings

### Images in TabItems

Add icons or images to tab headers:

```csharp
// Create and populate ImageList
ImageList imageList1 = new ImageList();
imageList1.ImageSize = new Size(16, 16);
imageList1.Images.Add("home", Properties.Resources.HomeIcon);
imageList1.Images.Add("settings", Properties.Resources.SettingsIcon);
imageList1.Images.Add("info", Properties.Resources.InfoIcon);

// Assign ImageList to TabControlAdv
tabControlAdv1.ImageList = imageList1;

// Set image for each tab
tabPageAdv1.ImageIndex = 0; // Home icon
tabPageAdv2.ImageIndex = 1; // Settings icon
tabPageAdv3.ImageIndex = 2; // Info icon
```

### Image Alignment

Control image position relative to text:

```csharp
// Image to the left of text (default)
tabControlAdv1.ImageAlignmentR = RelativeImageAlignment.LeftOfText;

// Image to the right of text
tabControlAdv1.ImageAlignmentR = RelativeImageAlignment.RightOfText;

// Image above text
tabControlAdv1.ImageAlignmentR = RelativeImageAlignment.AboveText;

// Image below text
tabControlAdv1.ImageAlignmentR = RelativeImageAlignment.BelowText;
```

### Images Outside TabBounds

Position images outside the normal tab boundaries:

```csharp
// Adjust image position
tabControlAdv1.ImageOffset = new Point(5, 5);

// Adjust top gap for image spacing
tabControlAdv1.AdjustTopGap = 10;

// Level text and image alignment
tabControlAdv1.LevelTextAndImage = true;

// Set custom item size to accommodate image
tabControlAdv1.ItemSize = new Size(100, 30);
```

### Background Image for TabPages

Set background images for individual tab content areas:

```csharp
// Set background image for a tab page
tabPageAdv1.BackgroundImage = Image.FromFile("background.jpg");

// Or from resources
tabPageAdv1.BackgroundImage = Properties.Resources.BackgroundImage;

// Set image layout
tabPageAdv1.BackgroundImageLayout = ImageLayout.Stretch; // Fill entire area
// Or
tabPageAdv1.BackgroundImageLayout = ImageLayout.Center;  // Center image
// Or
tabPageAdv1.BackgroundImageLayout = ImageLayout.Tile;    // Tile image
```

### DisableInactivePageImage

Control whether images are disabled for inactive tabs:

```csharp
// Disable images for inactive tabs (default: true)
tabControlAdv1.DisableInactivePageImage = true;

// Keep images enabled for all tabs
tabControlAdv1.DisableInactivePageImage = false;
```

## Color Customization

### Complete Color Setup

```csharp
// Tab control background
tabControlAdv1.BackColor = Color.FromArgb(240, 240, 240);

// Active tab
tabControlAdv1.ActiveTabColor = Color.White;

// Inactive tabs
tabControlAdv1.InactiveTabColor = Color.FromArgb(200, 200, 200);

// Tab panel (content area)
tabControlAdv1.TabPanelBackColor = Color.White;

// Individual tab override
tabPageAdv1.TabBackColor = Color.LightSkyBlue;
```

### Color Scheme Example

Create a custom color scheme:

```csharp
public void ApplyBlueScheme(TabControlAdv tabControl)
{
    tabControl.ActiveTabColor = Color.FromArgb(0, 120, 215);
    tabControl.InactiveTabColor = Color.FromArgb(200, 220, 240);
    tabControl.TabPanelBackColor = Color.White;
    tabControl.BackColor = Color.FromArgb(240, 240, 240);
    
    // Set active tab font color to white
    foreach (TabPageAdv page in tabControl.TabPages)
    {
        page.TabForeColor = Color.White;
    }
}
```

## Font Settings

### Control-Level Fonts

```csharp
// Font for inactive tabs
tabControlAdv1.Font = new Font("Segoe UI", 9F, FontStyle.Regular);

// Font for active tab (highlighted)
tabControlAdv1.ActiveTabFont = new Font("Segoe UI", 9F, FontStyle.Bold);
```

### Individual Tab Fonts

```csharp
// Set font for specific tab
tabPageAdv1.TabFont = new Font("Arial", 10F, FontStyle.Italic);
tabPageAdv2.TabFont = new Font("Consolas", 8.5F, FontStyle.Regular);
```

### ForeColor Settings

Set text color for tabs:

```csharp
// Text color for specific tab
tabPageAdv1.TabForeColor = Color.DarkBlue;
tabPageAdv2.TabForeColor = Color.DarkGreen;
tabPageAdv3.TabForeColor = Color.DarkRed;
```

### Complete Font Example

```csharp
// Setup fonts and colors
tabControlAdv1.Font = new Font("Segoe UI", 9F, FontStyle.Regular);
tabControlAdv1.ActiveTabFont = new Font("Segoe UI", 9F, FontStyle.Bold);

// Customize first tab
tabPageAdv1.Text = "Important";
tabPageAdv1.TabFont = new Font("Segoe UI", 9F, FontStyle.Bold);
tabPageAdv1.TabForeColor = Color.Red;

// Standard tabs
tabPageAdv2.Text = "Normal";
tabPageAdv2.TabForeColor = Color.Black;
```

## Border Settings

### BorderVisible Property

Show or hide the control border:

```csharp
// Show border
tabControlAdv1.BorderVisible = true;

// Hide border
tabControlAdv1.BorderVisible = false;
```

### BorderWidth Property

Set the width of the border:

```csharp
// Set border width (default: 5)
tabControlAdv1.BorderWidth = 10;

// Thin border
tabControlAdv1.BorderWidth = 2;
```

### BorderStyle for TabPages

Set border style for tab content areas:

```csharp
// Fixed single line border
tabControlAdv1.BorderStyle = BorderStyle.FixedSingle;

// 3D border
tabControlAdv1.BorderStyle = BorderStyle.Fixed3D;

// No border
tabControlAdv1.BorderStyle = BorderStyle.None;
```

### FixedSingleBorderColor

Set border color when using FixedSingle style:

```csharp
// Set border style
tabControlAdv1.BorderStyle = BorderStyle.FixedSingle;

// Set border color
tabControlAdv1.FixedSingleBorderColor = Color.DarkBlue;
```

**Reset to default:**
```csharp
// Reset border color to default
tabControlAdv1.ResetFixedSingleBorderColor();
```

## Tab Styles

TabControlAdv provides 15+ built-in themes.

### 2D Style

```csharp
tabControlAdv1.TabStyle = typeof(TabRenderer2D);
```

### 3D Style

```csharp
tabControlAdv1.TabStyle = typeof(TabRenderer3D);
```

### Metro Style

```csharp
tabControlAdv1.TabStyle = typeof(TabRendererMetro);
```

### Office Styles

```csharp
// Office 2003
tabControlAdv1.TabStyle = typeof(TabRendererOffice2003);

// Office 2007 (Blue, Black, Silver)
tabControlAdv1.TabStyle = typeof(TabRendererOffice2007);
tabControlAdv1.Office2007ColorScheme = Office2007Theme.Blue;
// Or
tabControlAdv1.Office2007ColorScheme = Office2007Theme.Black;
// Or
tabControlAdv1.Office2007ColorScheme = Office2007Theme.Silver;

// Office 2016 Colorful
tabControlAdv1.TabStyle = typeof(TabRendererOffice2016Colorful);

// Office 2016 White
tabControlAdv1.TabStyle = typeof(TabRendererOffice2016White);

// Office 2016 Dark Gray
tabControlAdv1.TabStyle = typeof(TabRendererOffice2016DarkGray);

// Office 2016 Black
tabControlAdv1.TabStyle = typeof(TabRendererOffice2016Black);
```

### Visual Studio Styles

```csharp
// VS2005
tabControlAdv1.TabStyle = typeof(TabRendererWhidbey);

// VS2005 Docking
tabControlAdv1.TabStyle = typeof(TabRendererDockingWhidbey);

// VS2005 Docking Beta
tabControlAdv1.TabStyle = typeof(TabRendererDockingWhidbeyBeta);

// VS2008
tabControlAdv1.TabStyle = typeof(TabRendererVS2008);

// VS2010
tabControlAdv1.TabStyle = typeof(TabRendererVS2010);
```

### Other Styles

```csharp
// Internet Explorer 7
tabControlAdv1.TabStyle = typeof(TabRendererIE7);

// OneNote
tabControlAdv1.TabStyle = typeof(OneNoteStyleRenderer);

// Workbook (Excel-like)
tabControlAdv1.TabStyle = typeof(TabRendererWorkbookMode);
```

### Custom Color Schemes for Office 2007

```csharp
// Apply managed (custom) colors
tabControlAdv1.TabStyle = typeof(TabRendererOffice2007);
tabControlAdv1.Office2007ColorScheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.Green);
```

## SizeMode Options

Control how tabs are sized.

### Normal Mode

Tab size depends on text and image:

```csharp
tabControlAdv1.SizeMode = TabSizeMode.Normal;
```

### Fixed Mode

All tabs have the same size specified by ItemSize:

```csharp
tabControlAdv1.SizeMode = TabSizeMode.Fixed;
tabControlAdv1.ItemSize = new Size(120, 30); // All tabs 120x30
```

### ShrinkToFit Mode

Tabs shrink to fit all in one row:

```csharp
// Only applicable for single-line mode
tabControlAdv1.SizeMode = TabSizeMode.ShrinkToFit;
tabControlAdv1.Multiline = false;
```

### FillToRight Mode

Tabs expand to fill the entire width:

```csharp
// Only applicable for multi-line mode
tabControlAdv1.SizeMode = TabSizeMode.FillToRight;
tabControlAdv1.Multiline = true;
```

## Complete Examples

### Example 1: Modern Blue Theme

```csharp
TabControlAdv modernTabs = new TabControlAdv();
modernTabs.Dock = DockStyle.Fill;

// Apply modern colors
modernTabs.ActiveTabColor = Color.FromArgb(0, 120, 215);
modernTabs.InactiveTabColor = Color.FromArgb(230, 230, 230);
modernTabs.TabPanelBackColor = Color.White;
modernTabs.BackColor = Color.FromArgb(245, 245, 245);

// Font settings
modernTabs.Font = new Font("Segoe UI", 9F);
modernTabs.ActiveTabFont = new Font("Segoe UI", 9F, FontStyle.Bold);

// Border
modernTabs.BorderVisible = true;
modernTabs.BorderStyle = BorderStyle.FixedSingle;
modernTabs.FixedSingleBorderColor = Color.FromArgb(200, 200, 200);

// Add tabs
for (int i = 1; i <= 4; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Tab {i}";
    tab.TabForeColor = Color.White;
    modernTabs.TabPages.Add(tab);
}

this.Controls.Add(modernTabs);
```

### Example 2: Office 2016 Style with Icons

```csharp
TabControlAdv officeTabs = new TabControlAdv();
officeTabs.Dock = DockStyle.Fill;

// Apply Office 2016 Colorful theme
officeTabs.TabStyle = typeof(TabRendererOffice2016Colorful);

// Setup icons
ImageList icons = new ImageList();
icons.ImageSize = new Size(16, 16);
icons.Images.Add("home", Properties.Resources.HomeIcon);
icons.Images.Add("edit", Properties.Resources.EditIcon);
icons.Images.Add("view", Properties.Resources.ViewIcon);
officeTabs.ImageList = icons;

// Create tabs with icons
string[] tabNames = { "Home", "Edit", "View" };
for (int i = 0; i < tabNames.Length; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = tabNames[i];
    tab.ImageIndex = i;
    officeTabs.TabPages.Add(tab);
}

this.Controls.Add(officeTabs);
```

### Example 3: Custom Colored Tabs

```csharp
TabControlAdv coloredTabs = new TabControlAdv();
coloredTabs.Size = new Size(600, 400);
coloredTabs.TabStyle = typeof(TabRenderer2D);

// Create tabs with individual colors
TabPageAdv redTab = new TabPageAdv();
redTab.Text = "Alerts";
redTab.TabBackColor = Color.IndianRed;
redTab.TabForeColor = Color.White;

TabPageAdv greenTab = new TabPageAdv();
greenTab.Text = "Success";
greenTab.TabBackColor = Color.MediumSeaGreen;
greenTab.TabForeColor = Color.White;

TabPageAdv blueTab = new TabPageAdv();
blueTab.Text = "Info";
blueTab.TabBackColor = Color.SteelBlue;
blueTab.TabForeColor = Color.White;

coloredTabs.TabPages.Add(redTab);
coloredTabs.TabPages.Add(greenTab);
coloredTabs.TabPages.Add(blueTab);

this.Controls.Add(coloredTabs);
```

### Example 4: Fixed Size Tabs

```csharp
TabControlAdv fixedTabs = new TabControlAdv();
fixedTabs.Size = new Size(600, 400);

// Fixed size mode
fixedTabs.SizeMode = TabSizeMode.Fixed;
fixedTabs.ItemSize = new Size(150, 35);

// Add tabs - all will be 150x35
for (int i = 1; i <= 6; i++)
{
    TabPageAdv tab = new TabPageAdv();
    tab.Text = $"Document {i}";
    fixedTabs.TabPages.Add(tab);
}

this.Controls.Add(fixedTabs);
```

## Best Practices

### Theme Selection
- Use **Office 2016** themes for modern applications
- Use **Metro** for Windows 8/10 style apps
- Use **VS** styles for developer tools
- Test themes with your color scheme

### Color Contrast
- Ensure sufficient contrast between text and background
- Test with different system color schemes
- Consider accessibility guidelines (WCAG)

### Image Guidelines
- Use 16x16 or 24x24 icons for consistency
- Use high-quality, clear icons
- Provide fallback for missing images
- Consider image visibility on different backgrounds

### Font Sizing
- Use system fonts (Segoe UI for Windows)
- Keep font sizes readable (9-11pt)
- Bold active tabs for better visibility
- Test with different DPI settings

### Border Usage
- Hide borders for seamless integration
- Use borders to define boundaries clearly
- Match border colors with your theme
