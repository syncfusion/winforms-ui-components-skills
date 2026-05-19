# Styling and Appearance for TabSplitterContainer

## Table of Contents
- [Overview](#overview)
- [Visual Styles](#visual-styles)
- [Office2016 Themes](#office2016-themes)
- [Splitter Appearance](#splitter-appearance)
- [Tab Appearance](#tab-appearance)
- [Page Background Customization](#page-background-customization)
- [Border Styles](#border-styles)
- [Custom Rendering](#custom-rendering)
- [Theme-Based Complete Examples](#theme-based-complete-examples)
- [Best Practices](#best-practices)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

The TabSplitterContainer control provides extensive styling capabilities to match your application's visual design. This guide covers the various appearance customization options, including Office2016 themes, colors, borders, and custom rendering.

## Visual Styles

The TabSplitterContainer supports multiple visual styles through the `Style` property.

### Available Styles

```csharp
public enum TabSplitterContainerStyle
{
    Default,
    Office2016Colorful,
    Office2016White,
    Office2016DarkGray,
    Office2016Black
}
```

### Setting Visual Style Programmatically

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TabSplitterStyling
{
    public partial class MainForm : Form
    {
        private TabSplitterContainer tabSplitterContainer1;
        
        public MainForm()
        {
            InitializeComponent();
            SetVisualStyle();
        }
        
        private void SetVisualStyle()
        {
            // Set Office2016 Colorful theme
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Colorful;
        }
    }
}
```

### Changing Style at Runtime

```csharp
private void CreateStyleSelector()
{
    ComboBox styleComboBox = new ComboBox();
    styleComboBox.Items.AddRange(new object[] 
    {
        "Default",
        "Office2016Colorful",
        "Office2016White",
        "Office2016DarkGray",
        "Office2016Black"
    });
    styleComboBox.SelectedIndex = 0;
    styleComboBox.SelectedIndexChanged += StyleComboBox_SelectedIndexChanged;
    
    // Add to form (example: docked at top)
    styleComboBox.Dock = DockStyle.Top;
    this.Controls.Add(styleComboBox);
    styleComboBox.BringToFront();
}

private void StyleComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    ComboBox comboBox = sender as ComboBox;
    
    switch (comboBox.SelectedItem.ToString())
    {
        case "Default":
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Default;
            break;
        case "Office2016Colorful":
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Colorful;
            break;
        case "Office2016White":
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016White;
            break;
        case "Office2016DarkGray":
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016DarkGray;
            break;
        case "Office2016Black":
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Black;
            break;
    }
}
```

## Office2016 Themes

The Office2016 themes provide a modern, professional appearance consistent with Microsoft Office 2016.

### Office2016Colorful Theme

The Office2016Colorful theme features a vibrant color scheme with blue accents:

```csharp
private void ApplyOffice2016ColorfulTheme()
{
    this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Colorful;
    
    // Optional: Customize accent colors
    this.tabSplitterContainer1.SplitterBackColor = System.Drawing.Color.FromArgb(0, 114, 198);
}
```

### Office2016White Theme

The Office2016White theme provides a clean, light appearance:

```csharp
private void ApplyOffice2016WhiteTheme()
{
    this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016White;
    
    // Optional: Customize background
    this.tabSplitterContainer1.BackColor = System.Drawing.Color.White;
}
```

### Office2016DarkGray Theme

The Office2016DarkGray theme offers a professional dark appearance:

```csharp
private void ApplyOffice2016DarkGrayTheme()
{
    this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016DarkGray;
    
    // Optional: Adjust page backgrounds for consistency
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        page.BackColor = System.Drawing.Color.FromArgb(62, 62, 66);
    }
    
    foreach (TabSplitterPage page in this.tabSplitterContainer1.SecondaryPages)
    {
        page.BackColor = System.Drawing.Color.FromArgb(62, 62, 66);
    }
}
```

### Office2016Black Theme

The Office2016Black theme provides the darkest visual experience:

```csharp
private void ApplyOffice2016BlackTheme()
{
    this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Black;
    
    // Optional: Set consistent dark backgrounds
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        page.BackColor = System.Drawing.Color.FromArgb(37, 37, 38);
    }
    
    foreach (TabSplitterPage page in this.tabSplitterContainer1.SecondaryPages)
    {
        page.BackColor = System.Drawing.Color.FromArgb(37, 37, 38);
    }
}
```

### Complete Office2016 Theme Application

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace Office2016ThemeDemo
{
    public partial class ThemedForm : Form
    {
        private TabSplitterContainer tabSplitterContainer1;
        private Panel themePanel;
        
        public ThemedForm()
        {
            InitializeComponent();
            InitializeThemedLayout();
        }
        
        private void InitializeThemedLayout()
        {
            // Create theme selector panel
            this.themePanel = new Panel();
            this.themePanel.Dock = DockStyle.Top;
            this.themePanel.Height = 50;
            this.themePanel.BackColor = Color.FromArgb(43, 87, 154);
            
            // Create theme buttons
            CreateThemeButton("Colorful", TabSplitterContainerStyle.Office2016Colorful, 10);
            CreateThemeButton("White", TabSplitterContainerStyle.Office2016White, 120);
            CreateThemeButton("Dark Gray", TabSplitterContainerStyle.Office2016DarkGray, 230);
            CreateThemeButton("Black", TabSplitterContainerStyle.Office2016Black, 340);
            
            this.Controls.Add(this.themePanel);
            
            // Create and configure TabSplitterContainer
            this.tabSplitterContainer1 = new TabSplitterContainer();
            this.tabSplitterContainer1.Dock = DockStyle.Fill;
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Colorful;
            
            // Add sample pages
            AddSamplePages();
            
            this.Controls.Add(this.tabSplitterContainer1);
            this.tabSplitterContainer1.SendToBack();
        }
        
        private void CreateThemeButton(string text, TabSplitterContainerStyle style, int x)
        {
            Button button = new Button();
            button.Text = text;
            button.Location = new Point(x, 10);
            button.Size = new Size(100, 30);
            button.FlatStyle = FlatStyle.Flat;
            button.ForeColor = Color.White;
            button.FlatAppearance.BorderColor = Color.White;
            button.Tag = style;
            button.Click += ThemeButton_Click;
            
            this.themePanel.Controls.Add(button);
        }
        
        private void ThemeButton_Click(object sender, EventArgs e)
        {
            Button button = sender as Button;
            TabSplitterContainerStyle style = (TabSplitterContainerStyle)button.Tag;
            ApplyTheme(style);
        }
        
        private void ApplyTheme(TabSplitterContainerStyle style)
        {
            this.tabSplitterContainer1.Style = style;
            
            // Apply theme-specific colors
            switch (style)
            {
                case TabSplitterContainerStyle.Office2016Colorful:
                    this.themePanel.BackColor = Color.FromArgb(43, 87, 154);
                    break;
                case TabSplitterContainerStyle.Office2016White:
                    this.themePanel.BackColor = Color.FromArgb(230, 230, 230);
                    foreach (Control control in this.themePanel.Controls)
                    {
                        if (control is Button)
                        {
                            control.ForeColor = Color.Black;
                        }
                    }
                    break;
                case TabSplitterContainerStyle.Office2016DarkGray:
                    this.themePanel.BackColor = Color.FromArgb(62, 62, 66);
                    foreach (Control control in this.themePanel.Controls)
                    {
                        if (control is Button)
                        {
                            control.ForeColor = Color.White;
                        }
                    }
                    break;
                case TabSplitterContainerStyle.Office2016Black:
                    this.themePanel.BackColor = Color.FromArgb(37, 37, 38);
                    foreach (Control control in this.themePanel.Controls)
                    {
                        if (control is Button)
                        {
                            control.ForeColor = Color.White;
                        }
                    }
                    break;
            }
        }
        
        private void AddSamplePages()
        {
            // Add primary pages
            TabSplitterPage page1 = new TabSplitterPage();
            page1.Text = "Explorer";
            this.tabSplitterContainer1.PrimaryPages.Add(page1);
            
            TabSplitterPage page2 = new TabSplitterPage();
            page2.Text = "Properties";
            this.tabSplitterContainer1.PrimaryPages.Add(page2);
            
            // Add secondary pages
            TabSplitterPage page3 = new TabSplitterPage();
            page3.Text = "Editor";
            this.tabSplitterContainer1.SecondaryPages.Add(page3);
            
            TabSplitterPage page4 = new TabSplitterPage();
            page4.Text = "Output";
            this.tabSplitterContainer1.SecondaryPages.Add(page4);
        }
    }
}
```

## Splitter Appearance

Customize the appearance of the splitter bar that divides the two panels.

### SplitterBackColor Property

Set a custom color for the splitter bar:

```csharp
private void CustomizeSplitterColor()
{
    // Set a custom splitter color
    this.tabSplitterContainer1.SplitterBackColor = System.Drawing.Color.FromArgb(0, 120, 215);
}
```

### Themed Splitter Colors

Apply theme-consistent splitter colors:

```csharp
private void ApplyThemedSplitterColors()
{
    switch (this.tabSplitterContainer1.Style)
    {
        case TabSplitterContainerStyle.Office2016Colorful:
            this.tabSplitterContainer1.SplitterBackColor = Color.FromArgb(0, 114, 198);
            break;
            
        case TabSplitterContainerStyle.Office2016White:
            this.tabSplitterContainer1.SplitterBackColor = Color.FromArgb(171, 171, 171);
            break;
            
        case TabSplitterContainerStyle.Office2016DarkGray:
            this.tabSplitterContainer1.SplitterBackColor = Color.FromArgb(54, 54, 57);
            break;
            
        case TabSplitterContainerStyle.Office2016Black:
            this.tabSplitterContainer1.SplitterBackColor = Color.FromArgb(30, 30, 30);
            break;
            
        default:
            this.tabSplitterContainer1.SplitterBackColor = SystemColors.ControlDark;
            break;
    }
}
```

## Tab Appearance

Customize the appearance of individual tabs within the TabSplitterContainer.

### Tab Text Styling

```csharp
private void CustomizeTabText()
{
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        // Tab text is set via the Text property
        page.Text = page.Text.ToUpper(); // Example: uppercase tabs
    }
}
```

### Tab Font Customization

```csharp
private void SetTabFont()
{
    // Note: Font applies to the page content, not tab headers
    // Tab header styling is controlled by the overall Style property
    
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        page.Font = new Font("Segoe UI", 9F, FontStyle.Regular);
    }
    
    foreach (TabSplitterPage page in this.tabSplitterContainer1.SecondaryPages)
    {
        page.Font = new Font("Segoe UI", 9F, FontStyle.Regular);
    }
}
```

## Page Background Customization

Customize the background appearance of individual TabSplitterPages.

### Setting Solid Colors

```csharp
private void SetPageBackgrounds()
{
    // Set solid background colors
    if (this.tabSplitterContainer1.PrimaryPages.Count > 0)
    {
        this.tabSplitterContainer1.PrimaryPages[0].BackColor = Color.WhiteSmoke;
    }
    
    if (this.tabSplitterContainer1.SecondaryPages.Count > 0)
    {
        this.tabSplitterContainer1.SecondaryPages[0].BackColor = Color.AliceBlue;
    }
}
```

### Applying Gradient Backgrounds

```csharp
using System.Drawing.Drawing2D;

private void ApplyGradientBackground(TabSplitterPage page)
{
    // Create a panel with gradient background
    Panel gradientPanel = new Panel();
    gradientPanel.Dock = DockStyle.Fill;
    gradientPanel.Paint += (sender, e) =>
    {
        using (LinearGradientBrush brush = new LinearGradientBrush(
            gradientPanel.ClientRectangle,
            Color.FromArgb(240, 248, 255),
            Color.FromArgb(200, 220, 240),
            LinearGradientMode.Vertical))
        {
            e.Graphics.FillRectangle(brush, gradientPanel.ClientRectangle);
        }
    };
    
    page.Controls.Add(gradientPanel);
    gradientPanel.SendToBack();
}
```

### Theme-Consistent Page Backgrounds

```csharp
private void ApplyThemeConsistentBackgrounds()
{
    Color pageBackColor;
    
    switch (this.tabSplitterContainer1.Style)
    {
        case TabSplitterContainerStyle.Office2016Colorful:
        case TabSplitterContainerStyle.Office2016White:
            pageBackColor = Color.White;
            break;
            
        case TabSplitterContainerStyle.Office2016DarkGray:
            pageBackColor = Color.FromArgb(62, 62, 66);
            break;
            
        case TabSplitterContainerStyle.Office2016Black:
            pageBackColor = Color.FromArgb(37, 37, 38);
            break;
            
        default:
            pageBackColor = SystemColors.Control;
            break;
    }
    
    // Apply to all pages
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        page.BackColor = pageBackColor;
    }
    
    foreach (TabSplitterPage page in this.tabSplitterContainer1.SecondaryPages)
    {
        page.BackColor = pageBackColor;
    }
}
```

## Border Styles

Control the border appearance of the TabSplitterPage.

### Container Border

```csharp
private void SetContainerBorder()
{
    // Set border style for the entire page
    this.tabSpliterPage1.BorderStyle = BorderStyle.FixedSingle;

    // Alternative: Remove border for seamless integration
    //this.tabSpliterPage1.BorderStyle = BorderStyle.None;
}
```

### Page Border Customization

```csharp
private void CustomizePageBorders()
{
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        // Add a border to the page
        page.BorderStyle = BorderStyle.FixedSingle;
        
        // Add padding for spacing
        page.Padding = new Padding(5);
    }
}
```

### Custom Border Rendering

```csharp
private void AddCustomBorderToPage(TabSplitterPage page)
{
    Panel borderPanel = new Panel();
    borderPanel.Dock = DockStyle.Fill;
    borderPanel.Padding = new Padding(2);
    borderPanel.Paint += (sender, e) =>
    {
        // Draw custom border
        using (Pen pen = new Pen(Color.FromArgb(0, 120, 215), 2))
        {
            Rectangle rect = borderPanel.ClientRectangle;
            rect.Width -= 1;
            rect.Height -= 1;
            e.Graphics.DrawRectangle(pen, rect);
        }
    };
    
    page.Controls.Add(borderPanel);
    borderPanel.SendToBack();
}
```

## Custom Rendering

Implement custom painting for advanced visual effects.

### Custom Splitter Rendering

```csharp
private void EnableCustomSplitterRendering()
{
    // The splitter rendering can be customized through the SplitterBackColor
    // For more advanced customization, you can overlay controls
    
    Panel splitterOverlay = new Panel();
    splitterOverlay.Height = this.tabSplitterContainer1.Height;
    splitterOverlay.Left = this.tabSplitterContainer1.SplitterPosition;
    splitterOverlay.BackColor = Color.Transparent;
    
    splitterOverlay.Paint += (sender, e) =>
    {
        // Custom rendering logic
        using (LinearGradientBrush brush = new LinearGradientBrush(
            splitterOverlay.ClientRectangle,
            Color.FromArgb(100, 0, 120, 215),
            Color.FromArgb(200, 0, 120, 215),
            LinearGradientMode.Horizontal))
        {
            e.Graphics.FillRectangle(brush, splitterOverlay.ClientRectangle);
        }
    };
    
    // Note: This is an overlay demonstration
    // Actual implementation may need additional event handling
}
```

### Custom Page Headers

```csharp
private void AddCustomHeaderToPage(TabSplitterPage page, string title)
{
    Panel headerPanel = new Panel();
    headerPanel.Dock = DockStyle.Top;
    headerPanel.Height = 35;
    headerPanel.BackColor = Color.FromArgb(0, 120, 215);
    
    Label headerLabel = new Label();
    headerLabel.Text = title;
    headerLabel.ForeColor = Color.White;
    headerLabel.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
    headerLabel.Dock = DockStyle.Fill;
    headerLabel.TextAlign = ContentAlignment.MiddleLeft;
    headerLabel.Padding = new Padding(10, 0, 0, 0);
    
    headerPanel.Controls.Add(headerLabel);
    page.Controls.Add(headerPanel);
}
```

## Theme-Based Complete Examples

### Complete Dark Theme Implementation

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace DarkThemeExample
{
    public partial class DarkThemedForm : Form
    {
        private TabSplitterContainer tabSplitterContainer1;
        
        public DarkThemedForm()
        {
            InitializeComponent();
            ApplyCompleteDarkTheme();
        }
        
        private void ApplyCompleteDarkTheme()
        {
            // Form styling
            this.BackColor = Color.FromArgb(30, 30, 30);
            this.ForeColor = Color.White;
            
            // Create TabSplitterContainer
            this.tabSplitterContainer1 = new TabSplitterContainer();
            this.tabSplitterContainer1.Dock = DockStyle.Fill;
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Black;
            this.tabSplitterContainer1.SplitterBackColor = Color.FromArgb(45, 45, 48);
            this.tabSplitterContainer1.BackColor = Color.FromArgb(37, 37, 38);
            
            // Add styled pages
            AddDarkThemedPage("Code Editor", true, true);
            AddDarkThemedPage("File Explorer", true, false);
            AddDarkThemedPage("Output Console", false, true);
            AddDarkThemedPage("Error List", false, false);
            
            this.Controls.Add(this.tabSplitterContainer1);
        }
        
        private void AddDarkThemedPage(string title, bool isPrimary, bool addContent)
        {
            TabSplitterPage page = new TabSplitterPage();
            page.Text = title;
            page.BackColor = Color.FromArgb(37, 37, 38);
            page.ForeColor = Color.White;
            
            if (addContent)
            {
                if (title.Contains("Editor"))
                {
                    RichTextBox editor = new RichTextBox();
                    editor.Dock = DockStyle.Fill;
                    editor.BackColor = Color.FromArgb(30, 30, 30);
                    editor.ForeColor = Color.FromArgb(220, 220, 220);
                    editor.Font = new Font("Consolas", 10F);
                    editor.BorderStyle = BorderStyle.None;
                    editor.Text = "// Dark themed code editor\npublic class Example\n{\n    // Your code here\n}";
                    page.Controls.Add(editor);
                }
                else if (title.Contains("Explorer"))
                {
                    TreeView treeView = new TreeView();
                    treeView.Dock = DockStyle.Fill;
                    treeView.BackColor = Color.FromArgb(37, 37, 38);
                    treeView.ForeColor = Color.White;
                    treeView.BorderStyle = BorderStyle.None;
                    treeView.Nodes.Add("Project Files");
                    treeView.Nodes[0].Nodes.Add("src");
                    treeView.Nodes[0].Nodes.Add("bin");
                    page.Controls.Add(treeView);
                }
                else if (title.Contains("Output"))
                {
                    TextBox output = new TextBox();
                    output.Dock = DockStyle.Fill;
                    output.Multiline = true;
                    output.BackColor = Color.FromArgb(30, 30, 30);
                    output.ForeColor = Color.FromArgb(220, 220, 220);
                    output.BorderStyle = BorderStyle.None;
                    output.Font = new Font("Consolas", 9F);
                    output.Text = "Build started...\nBuild succeeded.";
                    page.Controls.Add(output);
                }
            }
            
            if (isPrimary)
            {
                this.tabSplitterContainer1.PrimaryPages.Add(page);
            }
            else
            {
                this.tabSplitterContainer1.SecondaryPages.Add(page);
            }
        }
    }
}
```

### Complete Light Theme Implementation

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace LightThemeExample
{
    public partial class LightThemedForm : Form
    {
        private TabSplitterContainer tabSplitterContainer1;
        
        public LightThemedForm()
        {
            InitializeComponent();
            ApplyCompleteLightTheme();
        }
        
        private void ApplyCompleteLightTheme()
        {
            // Form styling
            this.BackColor = Color.White;
            this.ForeColor = Color.Black;
            
            // Create TabSplitterContainer
            this.tabSplitterContainer1 = new TabSplitterContainer();
            this.tabSplitterContainer1.Dock = DockStyle.Fill;
            this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016White;
            this.tabSplitterContainer1.SplitterBackColor = Color.FromArgb(171, 171, 171);
            this.tabSplitterContainer1.BackColor = Color.White;
            
            // Add styled pages
            AddLightThemedPage("Document", true);
            AddLightThemedPage("Properties", true);
            AddLightThemedPage("Preview", false);
            AddLightThemedPage("Comments", false);
            
            this.Controls.Add(this.tabSplitterContainer1);
        }
        
        private void AddLightThemedPage(string title, bool isPrimary)
        {
            TabSplitterPage page = new TabSplitterPage();
            page.Text = title;
            page.BackColor = Color.White;
            page.ForeColor = Color.Black;
            page.Padding = new Padding(10);
            
            // Add sample content
            Label contentLabel = new Label();
            contentLabel.Text = $"{title} Content Area";
            contentLabel.Font = new Font("Segoe UI", 12F, FontStyle.Bold);
            contentLabel.ForeColor = Color.FromArgb(68, 68, 68);
            contentLabel.AutoSize = true;
            contentLabel.Location = new Point(20, 20);
            page.Controls.Add(contentLabel);
            
            if (isPrimary)
            {
                this.tabSplitterContainer1.PrimaryPages.Add(page);
            }
            else
            {
                this.tabSplitterContainer1.SecondaryPages.Add(page);
            }
        }
    }
}
```

## Best Practices

### Consistency Across Themes

Maintain visual consistency when applying themes:

```csharp
private void ApplyConsistentTheming()
{
    // Choose a theme
    TabSplitterContainerStyle selectedStyle = TabSplitterContainerStyle.Office2016DarkGray;
    this.tabSplitterContainer1.Style = selectedStyle;
    
    // Apply consistent colors to child controls
    Color textColor = GetThemeTextColor(selectedStyle);
    Color backgroundColor = GetThemeBackgroundColor(selectedStyle);
    
    ApplyThemeToAllPages(textColor, backgroundColor);
}

private Color GetThemeTextColor(TabSplitterContainerStyle style)
{
    switch (style)
    {
        case TabSplitterContainerStyle.Office2016DarkGray:
        case TabSplitterContainerStyle.Office2016Black:
            return Color.White;
        default:
            return Color.Black;
    }
}

private Color GetThemeBackgroundColor(TabSplitterContainerStyle style)
{
    switch (style)
    {
        case TabSplitterContainerStyle.Office2016Black:
            return Color.FromArgb(37, 37, 38);
        case TabSplitterContainerStyle.Office2016DarkGray:
            return Color.FromArgb(62, 62, 66);
        default:
            return Color.White;
    }
}

private void ApplyThemeToAllPages(Color textColor, Color backgroundColor)
{
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        page.BackColor = backgroundColor;
        page.ForeColor = textColor;
        
        // Apply to child controls
        ApplyThemeToControls(page.Controls, textColor, backgroundColor);
    }
    
    foreach (TabSplitterPage page in this.tabSplitterContainer1.SecondaryPages)
    {
        page.BackColor = backgroundColor;
        page.ForeColor = textColor;
        
        ApplyThemeToControls(page.Controls, textColor, backgroundColor);
    }
}

private void ApplyThemeToControls(Control.ControlCollection controls, Color textColor, Color backgroundColor)
{
    foreach (Control control in controls)
    {
        control.ForeColor = textColor;
        
        // Apply background to appropriate controls
        if (control is TextBox || control is RichTextBox || control is ListBox)
        {
            control.BackColor = backgroundColor;
        }
        
        // Recursively apply to nested controls
        if (control.HasChildren)
        {
            ApplyThemeToControls(control.Controls, textColor, backgroundColor);
        }
    }
}
```

### Responsive Splitter Styling

Ensure splitter visibility across different themes:

```csharp
private void EnsureSplitterVisibility()
{    
    // Ensure color contrast with background
    Color containerBack = this.tabSplitterContainer1.BackColor;
    Color splitterBack = this.tabSplitterContainer1.SplitterBackColor;
    
    if (GetColorBrightness(containerBack) == GetColorBrightness(splitterBack))
    {
        // Adjust splitter color for better contrast
        this.tabSplitterContainer1.SplitterBackColor = 
            InvertColor(containerBack);
    }
}

private int GetColorBrightness(Color color)
{
    return (color.R + color.G + color.B) / 3;
}

private Color InvertColor(Color color)
{
    return Color.FromArgb(255 - color.R, 255 - color.G, 255 - color.B);
}
```

### Theme Persistence

Save and restore user theme preferences:

```csharp
using System.Configuration;

private void SaveThemePreference()
{
    // Save current theme
    Configuration config = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
    config.AppSettings.Settings.Remove("TabSplitterTheme");
    config.AppSettings.Settings.Add("TabSplitterTheme", 
        this.tabSplitterContainer1.Style.ToString());
    config.Save(ConfigurationSaveMode.Modified);
}

private void LoadThemePreference()
{
    string themeSetting = ConfigurationManager.AppSettings["TabSplitterTheme"];
    
    if (!string.IsNullOrEmpty(themeSetting))
    {
        if (Enum.TryParse(themeSetting, out TabSplitterContainerStyle style))
        {
            this.tabSplitterContainer1.Style = style;
            ApplyThemedSplitterColors();
            ApplyThemeConsistentBackgrounds();
        }
    }
}
```

## Edge Cases and Troubleshooting

### Theme Not Applied

If theme changes don't appear immediately:

```csharp
private void ForceThemeRefresh()
{
    this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Default;
    this.tabSplitterContainer1.Refresh();
    Application.DoEvents();
    
    this.tabSplitterContainer1.Style = TabSplitterContainerStyle.Office2016Colorful;
    this.tabSplitterContainer1.Refresh();
}
```

### Color Inconsistencies

Handle color inheritance issues:

```csharp
private void FixColorInheritance()
{
    // Explicitly set colors instead of relying on inheritance
    Color targetBackColor = Color.FromArgb(37, 37, 38);
    Color targetForeColor = Color.White;
    
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        page.BackColor = targetBackColor;
        page.ForeColor = targetForeColor;
        
        // Set for all descendants
        SetColorsRecursively(page, targetBackColor, targetForeColor);
    }
}

private void SetColorsRecursively(Control parent, Color backColor, Color foreColor)
{
    foreach (Control child in parent.Controls)
    {
        child.BackColor = backColor;
        child.ForeColor = foreColor;
        
        if (child.HasChildren)
        {
            SetColorsRecursively(child, backColor, foreColor);
        }
    }
}
```

### Custom Controls Theme Compatibility

Ensure custom controls respect theme settings:

```csharp
private void EnsureCustomControlTheming()
{
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        foreach (Control control in page.Controls)
        {
            // Check if control supports theming
            if (control is IThemeable themeable)
            {
                themeable.ApplyTheme(this.tabSplitterContainer1.Style);
            }
            else
            {
                // Apply manual theming
                control.BackColor = page.BackColor;
                control.ForeColor = page.ForeColor;
            }
        }
    }
}

// Example themeable interface
public interface IThemeable
{
    void ApplyTheme(TabSplitterContainerStyle style);
}
```

### High DPI Scaling Issues

Handle DPI scaling for consistent appearance:

```csharp
private void AdjustForDPI()
{
    using (Graphics graphics = this.CreateGraphics())
    {
        float dpiX = graphics.DpiX;
        float scaleFactor = dpiX / 96f; // 96 DPI is standard
        
        if (scaleFactor > 1.0f)
        {
            // Adjust splitter position based on DPI
            this.tabSplitterContainer1.SplitterPosition = 
                (int)(this.tabSplitterContainer1.SplitterPosition * scaleFactor);

        }
    }
}
```

### Runtime Theme Switching Performance

Optimize theme switching for better performance:

```csharp
private void OptimizedThemeSwitch(TabSplitterContainerStyle newStyle)
{
    // Suspend layout during theme change
    this.tabSplitterContainer1.SuspendLayout();
    
    try
    {
        // Apply theme
        this.tabSplitterContainer1.Style = newStyle;
        
        // Apply related styling
        ApplyThemedSplitterColors();
        ApplyThemeConsistentBackgrounds();
    }
    finally
    {
        // Resume layout
        this.tabSplitterContainer1.ResumeLayout(true);
    }
}
```

## Summary

The TabSplitterContainer provides comprehensive styling options:

- **Visual Styles**: Five built-in themes including Office2016 variants
- **Splitter Customization**: Control color, width, and appearance
- **Page Styling**: Customize backgrounds, borders, and content appearance
- **Theme Consistency**: Apply unified theming across all components
- **Custom Rendering**: Implement advanced visual effects when needed

Apply these styling techniques to create professional, visually appealing layouts that match your application's design requirements.
