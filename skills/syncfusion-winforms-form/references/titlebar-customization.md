# Title Bar Customization

This guide covers all aspects of customizing the SfForm title bar, including height, text alignment, button styling, rich text support, and loading custom user controls.

## Table of Contents
- [Title Bar Height](#title-bar-height)
- [Text Alignment](#text-alignment)
- [Button Customization](#button-customization)
- [Hiding Title Bar Buttons](#hiding-title-bar-buttons)
- [Rich Text Formatting](#rich-text-formatting)
- [Loading User Control to Title Bar](#loading-user-control-to-title-bar)
- [Appearance Customization](#appearance-customization)
- [Icon Customization](#icon-customization)
- [Caption Image](#caption-image)

## Title Bar Height

The height of the title bar can be adjusted to accommodate larger fonts, icons, or custom controls.

### Property
`Style.TitleBar.Height` (int)

### Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Set title bar height to 45 pixels
    this.Style.TitleBar.Height = 45;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Set title bar height to 45 pixels
    Me.Style.TitleBar.Height = 45
End Sub
```

### Recommended Heights
- **Standard:** 30-32 pixels (default Windows style)
- **Comfortable:** 35-40 pixels (better for touch)
- **Large:** 45-50 pixels (for custom controls or branding)
- **Compact:** 25-28 pixels (space-saving)

### Important Notes
- Ensure custom controls fit within the specified height
- Consider button spacing when setting height
- Larger heights provide better touch target areas
- Height affects the overall form appearance

## Text Alignment

Title bar text can be aligned both horizontally and vertically within the title bar space.

### Properties
- `Style.TitleBar.TextHorizontalAlignment` - Left, Center, Right
- `Style.TitleBar.TextVerticalAlignment` - Top, Center, Bottom

### Horizontal Alignment Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Center the title text horizontally
    this.Style.TitleBar.TextHorizontalAlignment = HorizontalAlignment.Center;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Center the title text horizontally
    Me.Style.TitleBar.TextHorizontalAlignment = HorizontalAlignment.Center
End Sub
```

### Vertical Alignment Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Align text to top of title bar
    this.Style.TitleBar.TextVerticalAlignment = VerticalAlignment.Top;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Align text to top of title bar
    Me.Style.TitleBar.TextVerticalAlignment = VerticalAlignment.Top
End Sub
```

### Combined Alignment Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Style.TitleBar.Height = 40;
    this.Style.TitleBar.TextHorizontalAlignment = HorizontalAlignment.Center;
    this.Style.TitleBar.TextVerticalAlignment = VerticalAlignment.Center;
}
```

### Alignment Guidelines
- **Left alignment:** Standard Windows convention, familiar to users
- **Center alignment:** Good for simple forms or dialogs
- **Right alignment:** Uncommon, use sparingly
- **Vertical centering:** Works best with standard height title bars

## Button Customization

Title bar buttons (Close, Minimize, Maximize) can be fully customized including colors, icons, and state-specific appearances.

### Button State Properties

Each button has three visual states:
1. **Normal** - Default appearance
2. **Hover** - When mouse is over the button
3. **Pressed** - When button is clicked

### Color Customization

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Normal state foreground colors
    this.Style.TitleBar.CloseButtonForeColor = Color.White;
    this.Style.TitleBar.MinimizeButtonForeColor = Color.White;
    this.Style.TitleBar.MaximizeButtonForeColor = Color.White;
    
    // Hover state background colors
    this.Style.TitleBar.CloseButtonHoverBackColor = Color.Red;
    this.Style.TitleBar.MinimizeButtonHoverBackColor = Color.DarkGray;
    this.Style.TitleBar.MaximizeButtonHoverBackColor = Color.DarkGray;
    
    // Pressed state background colors
    this.Style.TitleBar.CloseButtonPressedBackColor = Color.DarkRed;
    this.Style.TitleBar.MinimizeButtonPressedBackColor = Color.Gray;
    this.Style.TitleBar.MaximizeButtonPressedBackColor = Color.Gray;
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Normal state foreground colors
    Me.Style.TitleBar.CloseButtonForeColor = Color.White
    Me.Style.TitleBar.MinimizeButtonForeColor = Color.White
    Me.Style.TitleBar.MaximizeButtonForeColor = Color.White
    
    ' Hover state background colors
    Me.Style.TitleBar.CloseButtonHoverBackColor = Color.Red
    Me.Style.TitleBar.MinimizeButtonHoverBackColor = Color.DarkGray
    Me.Style.TitleBar.MaximizeButtonHoverBackColor = Color.DarkGray
    
    ' Pressed state background colors
    Me.Style.TitleBar.CloseButtonPressedBackColor = Color.DarkRed
    Me.Style.TitleBar.MinimizeButtonPressedBackColor = Color.Gray
    Me.Style.TitleBar.MaximizeButtonPressedBackColor = Color.Gray
End Sub
```

### Custom Button Icons

You can replace the default button icons with custom images for each state.

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Normal state icons
    this.Style.TitleBar.CloseButtonImage = Image.FromFile("close.png");
    this.Style.TitleBar.MaximizeButtonImage = Image.FromFile("maximize.png");
    this.Style.TitleBar.MinimizeButtonImage = Image.FromFile("minimize.png");
    
    // Hover state icons
    this.Style.TitleBar.CloseButtonHoverImage = Image.FromFile("close_hover.png");
    this.Style.TitleBar.MaximizeButtonHoverImage = Image.FromFile("maximize_hover.png");
    this.Style.TitleBar.MinimizeButtonHoverImage = Image.FromFile("minimize_hover.png");
    
    // Pressed state icons
    this.Style.TitleBar.CloseButtonPressedImage = Image.FromFile("close_pressed.png");
    this.Style.TitleBar.MaximizeButtonPressedImage = Image.FromFile("maximize_pressed.png");
    this.Style.TitleBar.MinimizeButtonPressedImage = Image.FromFile("minimize_pressed.png");
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Normal state icons
    Me.Style.TitleBar.CloseButtonImage = Image.FromFile("close.png")
    Me.Style.TitleBar.MaximizeButtonImage = Image.FromFile("maximize.png")
    Me.Style.TitleBar.MinimizeButtonImage = Image.FromFile("minimize.png")
    
    ' Hover state icons
    Me.Style.TitleBar.CloseButtonHoverImage = Image.FromFile("close_hover.png")
    Me.Style.TitleBar.MaximizeButtonHoverImage = Image.FromFile("maximize_hover.png")
    Me.Style.TitleBar.MinimizeButtonHoverImage = Image.FromFile("minimize_hover.png")
    
    ' Pressed state icons
    Me.Style.TitleBar.CloseButtonPressedImage = Image.FromFile("close_pressed.png")
    Me.Style.TitleBar.MaximizeButtonPressedImage = Image.FromFile("maximize_pressed.png")
    Me.Style.TitleBar.MinimizeButtonPressedImage = Image.FromFile("minimize_pressed.png")
End Sub
```

### Button Icon Guidelines
- Use 16x16 or 24x24 pixel images for clarity
- Ensure sufficient contrast with button background
- Provide all three states for consistent experience
- Use PNG format with transparency for best results
- Test icons at different DPI settings

### Complete Button Styling Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Dark theme button styling
    this.Style.TitleBar.BackColor = Color.FromArgb(30, 30, 30);
    
    // Close button - Red on hover (Windows style)
    this.Style.TitleBar.CloseButtonForeColor = Color.White;
    this.Style.TitleBar.CloseButtonHoverBackColor = Color.FromArgb(232, 17, 35);
    this.Style.TitleBar.CloseButtonHoverForeColor = Color.White;
    this.Style.TitleBar.CloseButtonPressedBackColor = Color.FromArgb(180, 10, 25);
    
    // Minimize button
    this.Style.TitleBar.MinimizeButtonForeColor = Color.White;
    this.Style.TitleBar.MinimizeButtonHoverBackColor = Color.FromArgb(60, 60, 60);
    this.Style.TitleBar.MinimizeButtonPressedBackColor = Color.FromArgb(90, 90, 90);
    
    // Maximize button
    this.Style.TitleBar.MaximizeButtonForeColor = Color.White;
    this.Style.TitleBar.MaximizeButtonHoverBackColor = Color.FromArgb(60, 60, 60);
    this.Style.TitleBar.MaximizeButtonPressedBackColor = Color.FromArgb(90, 90, 90);
}
```

## Hiding Title Bar Buttons

You can selectively hide title bar buttons based on your application needs.

### Properties
- `MinimizeBox` (bool) - Show/hide minimize button
- `MaximizeBox` (bool) - Show/hide maximize button
- `CloseButtonVisible` (bool) - Show/hide close button

### Example: Hide Minimize and Maximize

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Hide minimize and maximize buttons
    this.MinimizeBox = false;
    this.MaximizeBox = false;
    
    // Close button remains visible by default
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Hide minimize and maximize buttons
    Me.MinimizeBox = False
    Me.MaximizeBox = False
    
    ' Close button remains visible by default
End Sub
```

### Example: Hide All Buttons

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Hide all buttons
    this.MinimizeBox = false;
    this.MaximizeBox = false;
    this.CloseButtonVisible = false;
    
    // Note: Provide alternative way to close form!
}
```

### Use Cases for Hiding Buttons
- **Dialog boxes:** Hide minimize/maximize, keep close
- **Splash screens:** Hide all buttons
- **Fixed-size windows:** Hide maximize button only
- **Child windows:** May hide all buttons depending on context

### Important Considerations
- Always provide an alternative way to close the form if hiding close button
- Users expect standard window controls - hide sparingly
- Consider keyboard shortcuts (Alt+F4) still work
- Hiding buttons affects user experience expectations

## Rich Text Formatting

SfForm supports displaying rich text (RTF) in the title bar, allowing formatted text with colors, fonts, and styles.

### Enabling Rich Text

**Property:** `Style.TitleBar.AllowRichText` (bool)

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Enable rich text support
    this.Style.TitleBar.AllowRichText = true;
    
    // Set RTF formatted text
    this.Text = @"{\rtf1\ansi\deff0{\colortbl;\red255\green0\blue0;\red0\green0\blue255;}
                  {\fonttbl{\f0 Segoe UI;}}
                  \f0\fs20 {\cf1 Important} {\cf2 Document}}";
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Enable rich text support
    Me.Style.TitleBar.AllowRichText = True
    
    ' Set RTF formatted text
    Me.Text = "{\rtf1\ansi\deff0{\colortbl;\red255\green0\blue0;\red0\green0\blue255;}" & _
              "{\fonttbl{\f0 Segoe UI;}}" & _
              "\f0\fs20 {\cf1 Important} {\cf2 Document}}"
End Sub
```

### RTF Example: Text Editor Title

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Style.TitleBar.AllowRichText = true;
    
    // Create title showing filename and modified status
    string rtfText = @"{\rtf1\ansi\deff0
        {\colortbl;\red150\green0\blue20;\red100\green0\blue150;}
        {\fonttbl{\f0 Segoe UI;}}
        \qc\f0\fs23 {\cf1 Untitled* \cf2 - \b Custom Text Editor}}";
    
    this.Text = rtfText;
}
```

### RTF Color Table Example

**C#:**
```csharp
// Create a method to generate RTF title with custom colors
private string CreateRtfTitle(string filename, bool isModified)
{
    string modifiedIndicator = isModified ? "*" : "";
    
    return $@"{{\rtf1\ansi\deff0
        {{\colortbl;\red0\green0\blue0;\red255\green0\blue0;\red100\green100\blue100;}}
        {{\fonttbl{{\f0 Segoe UI;}}}}
        \f0\fs22 {{\cf1 {filename}{{\cf2 {modifiedIndicator}}} \cf3 - My Application}}}}";
}

public Form1()
{
    InitializeComponent();
    
    this.Style.TitleBar.AllowRichText = true;
    this.Text = CreateRtfTitle("Document1.txt", true);
}
```

### Important Notes
- Rich text only works when `AllowRichText` is `true`
- RTF format must be valid or text won't display
- Consider performance with complex RTF
- Test RTF rendering at different title bar heights
- Rich text doesn't work with `TitleBarTextControl` set

## Loading User Control to Title Bar

One of the most powerful features of SfForm is the ability to load any Windows Forms user control into the title bar.

### Property
`TitleBarTextControl` - Control to display in title bar

### Basic Example: Label and Button

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create a panel
    FlowLayoutPanel titlePanel = new FlowLayoutPanel();
    titlePanel.FlowDirection = FlowDirection.LeftToRight;
    titlePanel.Size = new Size(300, 28);
    
    // Add label
    Label appLabel = new Label();
    appLabel.Text = "My Application";
    appLabel.ForeColor = Color.White;
    appLabel.Font = new Font("Segoe UI", 10, FontStyle.Bold);
    appLabel.AutoSize = true;
    appLabel.Padding = new Padding(5, 5, 10, 0);
    
    // Add button
    Button actionButton = new Button();
    actionButton.Text = "Action";
    actionButton.Size = new Size(60, 23);
    actionButton.FlatStyle = FlatStyle.Flat;
    
    titlePanel.Controls.Add(appLabel);
    titlePanel.Controls.Add(actionButton);
    
    // Load to title bar
    this.TitleBarTextControl = titlePanel;
    this.Style.TitleBar.BackColor = Color.FromArgb(0, 120, 215);
    this.Style.TitleBar.Height = 35;
}
```

### Advanced Example: Search Bar with Icon

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create main panel
    TableLayoutPanel titlePanel = new TableLayoutPanel();
    titlePanel.ColumnCount = 3;
    titlePanel.RowCount = 1;
    titlePanel.Size = new Size(400, 30);
    titlePanel.ColumnStyles.Add(new ColumnStyle(SizeType.AutoSize));
    titlePanel.ColumnStyles.Add(new ColumnStyle(SizeType.Absolute, 250));
    titlePanel.ColumnStyles.Add(new ColumnStyle(SizeType.AutoSize));
    
    // App icon/logo
    PictureBox logo = new PictureBox();
    logo.Size = new Size(24, 24);
    logo.SizeMode = PictureBoxSizeMode.StretchImage;
    logo.Image = Image.FromFile("logo.png");
    logo.Margin = new Padding(5, 3, 10, 0);
    
    // Search box
    TextBox searchBox = new TextBox();
    searchBox.Width = 240;
    searchBox.PlaceholderText = "Search...";
    searchBox.Margin = new Padding(0, 5, 0, 0);
    
    // Search button
    Button searchButton = new Button();
    searchButton.Text = "🔍";
    searchButton.Size = new Size(30, 24);
    searchButton.FlatStyle = FlatStyle.Flat;
    searchButton.Margin = new Padding(5, 3, 0, 0);
    
    titlePanel.Controls.Add(logo, 0, 0);
    titlePanel.Controls.Add(searchBox, 1, 0);
    titlePanel.Controls.Add(searchButton, 2, 0);
    
    // Apply to title bar
    this.TitleBarTextControl = titlePanel;
    this.Style.TitleBar.BackColor = Color.White;
    this.Style.TitleBar.Height = 40;
}
```

### Example: Navigation Buttons

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    FlowLayoutPanel navPanel = new FlowLayoutPanel();
    navPanel.FlowDirection = FlowDirection.LeftToRight;
    navPanel.Size = new Size(250, 28);
    navPanel.Padding = new Padding(5, 2, 0, 0);
    
    // Back button
    Button backButton = new Button();
    backButton.Text = "◀";
    backButton.Size = new Size(35, 24);
    backButton.FlatStyle = FlatStyle.Flat;
    backButton.FlatAppearance.BorderSize = 0;
    
    // Forward button
    Button forwardButton = new Button();
    forwardButton.Text = "▶";
    forwardButton.Size = new Size(35, 24);
    forwardButton.FlatStyle = FlatStyle.Flat;
    forwardButton.FlatAppearance.BorderSize = 0;
    
    // Title label
    Label titleLabel = new Label();
    titleLabel.Text = "Browse";
    titleLabel.AutoSize = true;
    titleLabel.Font = new Font("Segoe UI", 10);
    titleLabel.Padding = new Padding(10, 3, 0, 0);
    
    navPanel.Controls.AddRange(new Control[] { backButton, forwardButton, titleLabel });
    
    this.TitleBarTextControl = navPanel;
    this.Style.TitleBar.Height = 35;
}
```

### Best Practices for Custom Title Bar Controls

1. **Size Appropriately**
   - Control height should be 5-8 pixels less than title bar height
   - Leave space for window buttons on the right
   - Consider margins and padding

2. **Visual Consistency**
   - Match control colors to title bar theme
   - Use appropriate fonts and sizes
   - Consider high DPI displays

3. **Performance**
   - Avoid heavy controls (e.g., web browsers)
   - Don't add too many controls
   - Use lightweight controls for better performance

4. **Layout**
   - Use `FlowLayoutPanel` or `TableLayoutPanel` for organization
   - Set explicit sizes to avoid layout issues
   - Test with different title bar heights

5. **Functionality**
   - Ensure controls are actually usable (not just decorative)
   - Handle events appropriately
   - Provide tooltips for buttons without text

## Appearance Customization

Comprehensive appearance customization for the title bar beyond basic colors.

### Complete Styling Example

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Title bar background and text
    this.Style.TitleBar.BackColor = Color.Black;
    this.Style.TitleBar.ForeColor = Color.White;
    this.Style.TitleBar.Font = new Font("Segoe UI", 10, FontStyle.Bold);
    
    // Title bar buttons - normal state
    this.Style.TitleBar.CloseButtonForeColor = Color.White;
    this.Style.TitleBar.MinimizeButtonForeColor = Color.White;
    this.Style.TitleBar.MaximizeButtonForeColor = Color.White;
    
    // Title bar buttons - hover state
    this.Style.TitleBar.CloseButtonHoverBackColor = Color.DarkGray;
    this.Style.TitleBar.MinimizeButtonHoverBackColor = Color.DarkGray;
    this.Style.TitleBar.MaximizeButtonHoverBackColor = Color.DarkGray;
    
    // Title bar buttons - pressed state
    this.Style.TitleBar.CloseButtonPressedBackColor = Color.Gray;
    this.Style.TitleBar.MaximizeButtonPressedBackColor = Color.Gray;
    this.Style.TitleBar.MinimizeButtonPressedBackColor = Color.Gray;
    
    // Client area (form background)
    this.Style.BackColor = Color.DarkGray;
}
```

## Icon Customization

Customize the appearance and position of the icon in the title bar.

### Icon Back Color

Set a background color for the form icon area:

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Set icon background color
    this.Style.TitleBar.IconBackColor = Color.Olive;
    
    // Set form icon
    this.Icon = new Icon("myicon.ico");
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Set icon background color
    Me.Style.TitleBar.IconBackColor = Color.Olive
    
    ' Set form icon
    Me.Icon = New Icon("myicon.ico")
End Sub
```

## Caption Image

Replace or supplement the standard form icon with a custom caption image.

### Setting Caption Image

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Set custom caption image
    this.Style.TitleBar.CaptionImage = SystemIcons.Error.ToBitmap();
    
    // Or load from file
    // this.Style.TitleBar.CaptionImage = Image.FromFile("caption.png");
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Set custom caption image
    Me.Style.TitleBar.CaptionImage = SystemIcons.Error.ToBitmap()
    
    ' Or load from file
    ' Me.Style.TitleBar.CaptionImage = Image.FromFile("caption.png")
End Sub
```

### Caption Image Location

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Set caption image
    this.Style.TitleBar.CaptionImage = Image.FromFile("logo.png");
    
    // Position the image (X, Y coordinates relative to title bar)
    this.Style.TitleBar.CaptionImageLocation = new Point(40, 5);
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    
    ' Set caption image
    Me.Style.TitleBar.CaptionImage = Image.FromFile("logo.png")
    
    ' Position the image (X, Y coordinates relative to title bar)
    Me.Style.TitleBar.CaptionImageLocation = New Point(40, 5)
End Sub
```

### Caption Image Best Practices
- Use 16x16 or 24x24 pixel images for standard title bars
- Ensure image contrasts with title bar background
- PNG format with transparency works best
- Test positioning at different title bar heights
- Consider icon and caption image together

## Complete Examples

### Example 1: Modern Dark Theme Title Bar

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    this.Text = "Modern Application";
    this.Size = new Size(1000, 700);
    
    // Title bar
    this.Style.TitleBar.BackColor = Color.FromArgb(30, 30, 30);
    this.Style.TitleBar.ForeColor = Color.White;
    this.Style.TitleBar.Height = 32;
    this.Style.TitleBar.Font = new Font("Segoe UI", 9);
    
    // Buttons
    this.Style.TitleBar.CloseButtonForeColor = Color.White;
    this.Style.TitleBar.CloseButtonHoverBackColor = Color.FromArgb(232, 17, 35);
    this.Style.TitleBar.CloseButtonPressedBackColor = Color.FromArgb(180, 10, 25);
    
    this.Style.TitleBar.MinimizeButtonForeColor = Color.White;
    this.Style.TitleBar.MinimizeButtonHoverBackColor = Color.FromArgb(50, 50, 50);
    
    this.Style.TitleBar.MaximizeButtonForeColor = Color.White;
    this.Style.TitleBar.MaximizeButtonHoverBackColor = Color.FromArgb(50, 50, 50);
}
```

### Example 2: Custom Control with Breadcrumb

**C#:**
```csharp
public Form1()
{
    InitializeComponent();
    
    // Create breadcrumb panel
    FlowLayoutPanel breadcrumb = new FlowLayoutPanel();
    breadcrumb.FlowDirection = FlowDirection.LeftToRight;
    breadcrumb.AutoSize = true;
    breadcrumb.Padding = new Padding(5, 7, 0, 0);
    
    Label home = new Label { Text = "Home", ForeColor = Color.White, AutoSize = true };
    Label sep1 = new Label { Text = ">", ForeColor = Color.Gray, AutoSize = true, Padding = new Padding(5, 0, 5, 0) };
    Label folder = new Label { Text = "Documents", ForeColor = Color.White, AutoSize = true };
    Label sep2 = new Label { Text = ">", ForeColor = Color.Gray, AutoSize = true, Padding = new Padding(5, 0, 5, 0) };
    Label file = new Label { Text = "Report.docx", ForeColor = Color.LightBlue, AutoSize = true };
    
    breadcrumb.Controls.AddRange(new Control[] { home, sep1, folder, sep2, file });
    
    this.TitleBarTextControl = breadcrumb;
    this.Style.TitleBar.BackColor = Color.FromArgb(0, 90, 160);
    this.Style.TitleBar.Height = 36;
}
```
