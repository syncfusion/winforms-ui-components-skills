# Tab Styles & Themes

## Table of Contents
- [Understanding Tab Styles](#understanding-tab-styles)
- [All Available Styles](#all-available-styles)
- [Office 2007/2016 Color Schemes](#office-200720016-color-schemes)
- [Setting Themes](#setting-themes)
- [Style Comparison](#style-comparison)

## Understanding Tab Styles

Tab styles control the visual appearance of the entire tab strip. Syncfusion provides 15+ built-in styles covering everything from classic 2D to modern Metro design.

### How to Set Tab Style

```csharp
tabbedMDIManager.TabStyle = typeof(TabRendererClassName);
```

### Enabling Built-in Themes

For built-in themes to work, enable themes:

```csharp
tabbedMDIManager.ThemesEnabled = true;
```

## All Available Styles

### Basic Styles

#### 2D Style (Simple, Flat)
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRenderer2D);
// Flat, minimal appearance - good for lightweight applications
```

#### 3D Style (Beveled, Raised)
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRenderer3D);
// Classic beveled appearance with depth - traditional look
```

#### Workbook Style (Excel-like)
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererWorkbookMode);
// Similar to Excel sheets - spreadsheet-like appearance
```

### Microsoft Office Styles

#### WhidbeyStyle
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererWhidbey);
// Visual Studio 2003 IDE style - professional developer look
```

#### DockingWhidbeyStyle
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererDockingWhidbey);
// VS 2003 docking style - tool window appearance
```

#### DockingWhidbeyBetaStyle
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererDockingWhidbeyBeta);
// VS 2005 Beta appearance - refined docking style
```

#### Office2003 Style
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2003);
// Office 2003 application style - classic professional look
```

#### Office 2007 Style (with Color Schemes)
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2007);
tabbedMDIManager.Office2007ColorScheme = Office2007Theme.Blue;
```

#### Office 2016 Styles
```csharp
// Office 2016 Colorful (Vibrant)
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016Colorful);

// Office 2016 White (Light)
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016White);

// Office 2016 Dark Gray (Dark)
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016DarkGray);

// Office 2016 Black (Very Dark)
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016Black);
```

### Modern Styles

#### OneNote Style
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.OneNoteStyleRenderer);
// OneNote application style - modern, colorful tabs
```

#### OneNote Flat Tabs Style
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.OneNoteStyleFlatTabsRenderer);
// OneNote with flat, modern design - minimalist appearance
```

#### Internet Explorer 7 Style
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererIE7);
// IE7 browser tab style - web-like appearance
```

#### Metro Style
```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererMetro);
// Modern Metro design - Windows 8/10 style appearance
```

## Office 2007/2016 Color Schemes

When using Office 2007 or 2016 styles, you can choose specific color schemes:

### Office 2007 Color Schemes

```csharp
tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2007);

// Available color schemes:
tabbedMDIManager.Office2007ColorScheme = Office2007Theme.Blue;       // Default blue
tabbedMDIManager.Office2007ColorScheme = Office2007Theme.Black;      // Dark black
tabbedMDIManager.Office2007ColorScheme = Office2007Theme.Silver;     // Light silver
```

### Office 2007 Color Example

```csharp
private void SetupOffice2007Themes()
{
    tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2007);
    tabbedMDIManager.ThemesEnabled = true;

    // Blue (default, professional)
    tabbedMDIManager.Office2007ColorScheme = Office2007Theme.Blue;

    // Silver (lighter, softer)
    // tabbedMDIManager.Office2007ColorScheme = Office2007Theme.Silver;

    // Black (darker, high contrast)
    // tabbedMDIManager.Office2007ColorScheme = Office2007Theme.Black;
}
```

## Setting Themes

### Complete Theme Setup

```csharp
private void ConfigureTheme()
{
    // Step 1: Enable themes
    tabbedMDIManager.ThemesEnabled = true;

    // Step 2: Set tab style
    tabbedMDIManager.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016White);

    // Step 3: (Optional) Customize colors if needed
    tabbedMDIManager.ActiveTabBackColor = Color.Blue;
    tabbedMDIManager.TabBackColor = Color.LightGray;
}
```

### Theme Switcher with Dropdown

```csharp
public partial class ThemeSwitcherForm : Form
{
    private TabbedMDIManager tabbedMDI;

    public ThemeSwitcherForm()
    {
        InitializeComponent();
        SetupUI();
    }

    private void SetupUI()
    {
        this.IsMdiContainer = true;
        this.Text = "Tab Style Switcher";
        this.Size = new Size(900, 600);

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        CreateThemeMenu();
        CreateSampleDocuments();
    }

    private void CreateThemeMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem styleMenu = menu.Items.Add("&Styles") as ToolStripMenuItem;

        // Basic styles
        styleMenu.DropDownItems.Add("&2D", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRenderer2D)));
        styleMenu.DropDownItems.Add("&3D", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRenderer3D)));
        styleMenu.DropDownItems.Add("&Workbook", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererWorkbookMode)));

        styleMenu.DropDownItems.AddSeparator();

        // Visual Studio styles
        styleMenu.DropDownItems.Add("&Whidbey", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererWhidbey)));

        styleMenu.DropDownItems.AddSeparator();

        // Office styles
        styleMenu.DropDownItems.Add("Office &2003", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2003)));
        styleMenu.DropDownItems.Add("Office 2007 &Blue", null, (s, e) =>
            ApplyOffice2007Style(Office2007Theme.Blue));
        styleMenu.DropDownItems.Add("Office 2007 &Silver", null, (s, e) =>
            ApplyOffice2007Style(Office2007Theme.Silver));
        styleMenu.DropDownItems.Add("Office 2007 &Black", null, (s, e) =>
            ApplyOffice2007Style(Office2007Theme.Black));

        styleMenu.DropDownItems.AddSeparator();

        // Office 2016 styles
        styleMenu.DropDownItems.Add("Office 2016 &Colorful", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016Colorful)));
        styleMenu.DropDownItems.Add("Office 2016 &White", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016White)));
        styleMenu.DropDownItems.Add("Office 2016 &Dark Gray", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016DarkGray)));
        styleMenu.DropDownItems.Add("Office 2016 Bl&ack", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016Black)));

        styleMenu.DropDownItems.AddSeparator();

        // Modern styles
        styleMenu.DropDownItems.Add("&OneNote", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.OneNoteStyleRenderer)));
        styleMenu.DropDownItems.Add("OneNote &Flat", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.OneNoteStyleFlatTabsRenderer)));
        styleMenu.DropDownItems.Add("&Metro", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererMetro)));
        styleMenu.DropDownItems.Add("&Internet Explorer 7", null, (s, e) =>
            ApplyStyle(typeof(Syncfusion.Windows.Forms.Tools.TabRendererIE7)));
    }

    private void ApplyStyle(Type styleType)
    {
        tabbedMDI.TabStyle = styleType;
        UpdateStatusBar($"Style changed to: {styleType.Name}");
    }

    private void ApplyOffice2007Style(Office2007Theme colorScheme)
    {
        tabbedMDI.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2007);
        tabbedMDI.Office2007ColorScheme = colorScheme;
        UpdateStatusBar($"Office 2007 {colorScheme} applied");
    }

    private void UpdateStatusBar(string message)
    {
        Console.WriteLine(message);
    }

    private void CreateSampleDocuments()
    {
        for (int i = 1; i <= 4; i++)
        {
            Form doc = new Form();
            doc.Text = $"Document {i}";
            doc.MdiParent = this;
            doc.Show();
        }
    }
}
```

## Style Comparison

| Style | Usage | Best For |
|-------|-------|----------|
| 2D | Simple, flat design | Modern applications, minimal UI |
| 3D | Classic beveled | Traditional enterprise apps |
| Workbook | Excel-like | Spreadsheet applications |
| Whidbey | VS 2003 IDE | Developer tools, coding IDEs |
| Office2003 | Classic Office | Business applications |
| Office2007 | Modern Office | Professional appearance |
| Office2016 | Latest Office | Contemporary business apps |
| OneNote | Modern colorful | Educational, creative apps |
| Metro | Windows 8/10 | Modern, touch-friendly apps |
| IE7 | Browser tabs | Web browser-like interfaces |

## Complete Styled Application

```csharp
public partial class StyledMDIApp : Form
{
    private TabbedMDIManager tabbedMDI;

    public StyledMDIApp()
    {
        InitializeComponent();
        SetupStyledMDI();
    }

    private void SetupStyledMDI()
    {
        this.IsMdiContainer = true;
        this.Text = "Professional Styled MDI Application";
        this.Size = new Size(1000, 700);
        this.StartPosition = FormStartPosition.CenterScreen;

        // Initialize TabbedMDI
        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);

        // Apply professional styling
        ApplyProfessionalStyling();

        // Create UI
        CreateMenu();
        CreateToolbar();
        CreateInitialDocuments();
    }

    private void ApplyProfessionalStyling()
    {
        // Enable themes
        tabbedMDI.ThemesEnabled = true;

        // Use Office 2016 White (professional, clean)
        tabbedMDI.TabStyle = typeof(Syncfusion.Windows.Forms.Tools.TabRendererOffice2016White);

        // Customize colors
        tabbedMDI.ActiveTabBackColor = Color.FromArgb(0, 102, 204);      // Professional blue
        tabbedMDI.ActiveTabForeColor = Color.White;
        tabbedMDI.TabBackColor = Color.FromArgb(240, 240, 240);          // Light gray
        tabbedMDI.TabForeColor = Color.FromArgb(80, 80, 80);             // Dark gray text

        // Fonts
        tabbedMDI.ActiveTabFont = new Font("Segoe UI", 10, FontStyle.Bold);
        tabbedMDI.TabFont = new Font("Segoe UI", 10, FontStyle.Regular);

        // Panel
        tabbedMDI.TabPanelBackColor = Color.White;
        tabbedMDI.TabPanelBorderColor = Color.FromArgb(220, 220, 220);

        // Enable icons
        tabbedMDI.UseIconsInTabs = true;
        tabbedMDI.ImageSize = new Size(16, 16);

        // Show buttons
        tabbedMDI.ShowCloseButton = true;
        tabbedMDI.DropDownButtonVisible = true;
    }

    private void CreateMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem fileMenu = menu.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New Document", null, (s, e) => CreateNewDocument());
        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
    }

    private void CreateToolbar()
    {
        ToolStrip toolbar = new ToolStrip();
        this.Controls.Add(toolbar);

        toolbar.Items.Add("New").Click += (s, e) => CreateNewDocument();
        toolbar.Items.Add("Exit").Click += (s, e) => this.Close();
    }

    private void CreateInitialDocuments()
    {
        for (int i = 1; i <= 3; i++)
        {
            CreateNewDocument();
        }
    }

    private void CreateNewDocument()
    {
        Form doc = new Form();
        doc.Text = $"Document {this.MdiChildren.Length + 1}";
        doc.Icon = SystemIcons.Application;
        doc.MdiParent = this;
        doc.Show();
    }
}
```

## Best Practices

1. **Consistency** - Pick one style and stick with it throughout your app
2. **Modern preference** - Office 2016 or Metro styles for modern applications
3. **User accessibility** - Consider high-contrast options for accessibility
4. **Testing** - Test styles across different Windows versions and themes
5. **Documentation** - Document your chosen style for consistency

## Troubleshooting

### Issue: Style Not Changing
**Solution:** Ensure `ThemesEnabled = true` before setting style

### Issue: Office2007 Color Scheme Not Working
**Solution:** Always set both `TabStyle` AND `Office2007ColorScheme`:
```csharp
tabbedMDI.TabStyle = typeof(TabRendererOffice2007);
tabbedMDI.Office2007ColorScheme = Office2007Theme.Blue;
```

### Issue: Style Changes After Adding Documents
**Solution:** Set style BEFORE adding documents, or refresh:
```csharp
tabbedMDI.TabStyle = newStyle;
// May need to refresh: tabbedMDI.Refresh();
```
