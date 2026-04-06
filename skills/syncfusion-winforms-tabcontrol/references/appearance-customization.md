# Appearance and Customization

## Table of Contents
- [Color Customization](#color-customization)
- [Font Settings](#font-settings)
- [Image Settings](#image-settings)
- [Border Settings](#border-settings)
- [Tab Styles](#tab-styles)
- [SizeMode Options](#sizemode-options)
- [Complete Examples](#complete-examples)

Customize the visual appearance of TabControlAdv including colors, fonts, images, borders, and themes.

## Color Customization

```csharp
// Control and tab panel backgrounds
tabControlAdv1.BackColor = Color.FromArgb(240, 240, 240);
tabControlAdv1.TabPanelBackColor = Color.White;

// Active/inactive tab colors
tabControlAdv1.ActiveTabColor = Color.White;
tabControlAdv1.InactiveTabColor = Color.FromArgb(200, 200, 200);

// Individual tab color overrides
tabPageAdv1.TabBackColor = Color.LightSkyBlue;
tabPageAdv1.TabForeColor = Color.White;
tabPageAdv2.TabBackColor = Color.LightGreen;
tabPageAdv3.TabBackColor = Color.Pink;
```

## Font Settings

```csharp
// Control-level fonts
tabControlAdv1.Font = new Font("Segoe UI", 9F, FontStyle.Regular); // Inactive tabs
tabControlAdv1.ActiveTabFont = new Font("Segoe UI", 9F, FontStyle.Bold); // Active tab

// Individual tab fonts and colors
tabPageAdv1.TabFont = new Font("Arial", 10F, FontStyle.Italic);
tabPageAdv1.TabForeColor = Color.DarkBlue;
tabPageAdv2.TabForeColor = Color.DarkGreen;
```

## Image Settings

```csharp
// Setup ImageList with icons
var imageList = new ImageList { ImageSize = new Size(16, 16) };
imageList.Images.Add("home", Properties.Resources.HomeIcon);
imageList.Images.Add("settings", Properties.Resources.SettingsIcon);
tabControlAdv1.ImageList = imageList;

// Assign images to tabs
tabPageAdv1.ImageIndex = 0;
tabPageAdv2.ImageIndex = 1;

// Image positioning and alignment
tabControlAdv1.ImageAlignmentR = RelativeImageAlignment.LeftOfText; // LeftOfText, RightOfText, AboveText, BelowText
tabControlAdv1.ImageOffset = new Point(5, 5); // Adjust position
tabControlAdv1.LevelTextAndImage = true; // Align text and image
tabControlAdv1.DisableInactivePageImage = true; // Disable images for inactive tabs

// Background image for tab content
tabPageAdv1.BackgroundImage = Properties.Resources.BackgroundImage;
tabPageAdv1.BackgroundImageLayout = ImageLayout.Stretch; // Stretch, Center, Tile
```



## Border Settings

```csharp
// Border visibility and width
tabControlAdv1.BorderVisible = true;
tabControlAdv1.BorderWidth = 5; // Default: 5

// Border styles
tabControlAdv1.BorderStyle = BorderStyle.FixedSingle; // FixedSingle, Fixed3D, None

// Custom border color (FixedSingle only)
tabControlAdv1.FixedSingleBorderColor = Color.DarkBlue;
tabControlAdv1.ResetFixedSingleBorderColor(); // Reset to default
```

## Tab Styles

TabControlAdv provides 15+ built-in themes. Set via `TabStyle` property.

| Style | Type |
|-------|------|
| 2D | `typeof(TabRenderer2D)` |
| 3D | `typeof(TabRenderer3D)` |
| Metro | `typeof(TabRendererMetro)` |
| Office 2003 | `typeof(TabRendererOffice2003)` |
| Office 2007 | `typeof(TabRendererOffice2007)` |
| Office 2016 Colorful | `typeof(TabRendererOffice2016Colorful)` |
| Office 2016 White | `typeof(TabRendererOffice2016White)` |
| Office 2016 Dark Gray | `typeof(TabRendererOffice2016DarkGray)` |
| Office 2016 Black | `typeof(TabRendererOffice2016Black)` |
| VS2005 | `typeof(TabRendererWhidbey)` |
| VS2005 Docking | `typeof(TabRendererDockingWhidbey)` |
| VS2005 Docking Beta | `typeof(TabRendererDockingWhidbeyBeta)` |
| VS2008 | `typeof(TabRendererVS2008)` |
| VS2010 | `typeof(TabRendererVS2010)` |
| IE7 | `typeof(TabRendererIE7)` |
| OneNote | `typeof(OneNoteStyleRenderer)` |
| Workbook | `typeof(TabRendererWorkbookMode)` |

```csharp
// Apply Office 2016 style
tabControlAdv1.TabStyle = typeof(TabRendererOffice2016Colorful);

// Office 2007 with color schemes
tabControlAdv1.TabStyle = typeof(TabRendererOffice2007);
tabControlAdv1.Office2007ColorScheme = Office2007Theme.Blue; // Blue, Black, Silver, Managed

// Custom colors for Office 2007
tabControlAdv1.Office2007ColorScheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.Green);
```

## SizeMode Options

| SizeMode | Description | Requirements |
|----------|-------------|--------------|
| `Normal` | Tab size depends on text and image | - |
| `Fixed` | All tabs same size via ItemSize | Set `ItemSize` property |
| `ShrinkToFit` | Tabs shrink to fit in one row | `Multiline = false` |
| `FillToRight` | Tabs expand to fill width | `Multiline = true` |

```csharp
// Fixed size for all tabs
tabControlAdv1.SizeMode = TabSizeMode.Fixed;
tabControlAdv1.ItemSize = new Size(120, 30);

// Shrink to fit (single line)
tabControlAdv1.SizeMode = TabSizeMode.ShrinkToFit;
tabControlAdv1.Multiline = false;

// Fill to right (multi-line)
tabControlAdv1.SizeMode = TabSizeMode.FillToRight;
tabControlAdv1.Multiline = true;
```

## Complete Examples

### Example 1: Modern Blue Theme

```csharp
var modernTabs = new TabControlAdv
{
    Dock = DockStyle.Fill,
    ActiveTabColor = Color.FromArgb(0, 120, 215),
    InactiveTabColor = Color.FromArgb(230, 230, 230),
    TabPanelBackColor = Color.White,
    BackColor = Color.FromArgb(245, 245, 245),
    Font = new Font("Segoe UI", 9F),
    ActiveTabFont = new Font("Segoe UI", 9F, FontStyle.Bold),
    BorderVisible = true,
    BorderStyle = BorderStyle.FixedSingle,
    FixedSingleBorderColor = Color.FromArgb(200, 200, 200)
};

for (int i = 1; i <= 4; i++)
    modernTabs.TabPages.Add(new TabPageAdv { Text = $"Tab {i}", TabForeColor = Color.White });

this.Controls.Add(modernTabs);
```

### Example 2: Office 2016 Style with Icons

```csharp
var icons = new ImageList { ImageSize = new Size(16, 16) };
icons.Images.Add("home", Properties.Resources.HomeIcon);
icons.Images.Add("edit", Properties.Resources.EditIcon);
icons.Images.Add("view", Properties.Resources.ViewIcon);

var officeTabs = new TabControlAdv
{
    Dock = DockStyle.Fill,
    TabStyle = typeof(TabRendererOffice2016Colorful),
    ImageList = icons
};

string[] tabNames = { "Home", "Edit", "View" };
for (int i = 0; i < tabNames.Length; i++)
    officeTabs.TabPages.Add(new TabPageAdv { Text = tabNames[i], ImageIndex = i });

this.Controls.Add(officeTabs);
```

### Example 3: Custom Colored Tabs

```csharp
var coloredTabs = new TabControlAdv
{
    Size = new Size(600, 400),
    TabStyle = typeof(TabRenderer2D)
};

coloredTabs.TabPages.Add(new TabPageAdv { Text = "Alerts", TabBackColor = Color.IndianRed, TabForeColor = Color.White });
coloredTabs.TabPages.Add(new TabPageAdv { Text = "Success", TabBackColor = Color.MediumSeaGreen, TabForeColor = Color.White });
coloredTabs.TabPages.Add(new TabPageAdv { Text = "Info", TabBackColor = Color.SteelBlue, TabForeColor = Color.White });

this.Controls.Add(coloredTabs);
```

## Best Practices

| Area | Recommendations |
|------|-----------------|
| **Themes** | Office 2016 for modern apps, Metro for Windows 8/10, VS styles for dev tools |
| **Colors** | Ensure sufficient contrast (WCAG), test with system color schemes |
| **Icons** | Use 16x16 or 24x24, high-quality images, consider visibility on backgrounds |
| **Fonts** | System fonts (Segoe UI), 9-11pt size, bold active tabs, test DPI settings |
| **Borders** | Hide for seamless look, use to define boundaries, match theme colors |
