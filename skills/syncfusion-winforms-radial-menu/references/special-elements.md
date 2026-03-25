# Special Elements in RadialMenu

## Table of Contents
- [Special Elements Overview](#special-elements-overview)
- [RadialColorPalette](#radialcolorpalette)
- [RadialFontListBox](#radialfontlistbox)
- [RadialMenuSlider](#radialmenuslider)
- [ImageListAdv Configuration](#imagelistadv-configuration)
- [Integration Patterns](#integration-patterns)
- [Complete Working Examples](#complete-working-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Special Elements Overview

In addition to standard RadialMenuItem objects, RadialMenu supports three specialized elements designed for specific input scenarios:

1. **RadialColorPalette** - Color selection from a customizable palette
2. **RadialFontListBox** - Font family selection from installed fonts
3. **RadialMenuSlider** - Numeric value selection within a range

These specialized elements integrate seamlessly with RadialMenu and provide intuitive touch-friendly interfaces for common formatting and configuration tasks.

**Key Characteristics:**
- All special elements inherit from RadialMenuItem base class
- Support standard properties (Text, ImageIndex)
- Provide specific event handlers for their data types
- Can be mixed with regular menu items
- Require ImageListAdv for custom icons

**When to Use Special Elements:**
- Text formatting toolbars (font, size, color)
- Graphics editing applications (brush color, size)
- Document editors (highlighting, font selection)
- Data visualization tools (color coding, sizing)

## RadialColorPalette

`RadialColorPalette` provides a circular color picker interface within the RadialMenu, allowing users to select colors from a predefined or custom palette.

### Creating a RadialColorPalette

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create the color palette instance
RadialColorPalette colorPalette = new RadialColorPalette();
colorPalette.Text = "Color";
colorPalette.ImageIndex = 0;  // Icon for color palette item

// Add to RadialMenu
this.radialMenu1.Items.Add(colorPalette);
```

### Configuring Custom Icon

Color palette items typically need a visual icon to indicate their purpose.

```csharp
// Set up ImageListAdv with color palette icon
ImageListAdv imageList = new ImageListAdv(this.components);
imageList.Images.Add(Image.FromFile("icons/color-palette.png"));
imageList.Images.Add(Image.FromFile("icons/font.png"));
imageList.Images.Add(Image.FromFile("icons/size.png"));

// Attach to RadialMenu
this.radialMenu1.ImageList = imageList;

// Create color palette with icon
RadialColorPalette colorPalette = new RadialColorPalette();
colorPalette.Text = "Color";
colorPalette.ImageIndex = 0;  // First image (color-palette.png)

this.radialMenu1.Items.Add(colorPalette);
```

### Handling ColorSelected Event

The `ColorSelected` event fires when a user picks a color from the palette.

```csharp
private void SetupColorPalette()
{
    RadialColorPalette colorPalette = new RadialColorPalette();
    colorPalette.Text = "Text Color";
    colorPalette.ImageIndex = 0;
    
    // Attach event handler
    colorPalette.ColorSelected += ColorPalette_ColorSelected;
    
    this.radialMenu1.Items.Add(colorPalette);
}

private void ColorPalette_ColorSelected(object sender, ColorSelectedEventArgs e)
{
    // Get the selected color
    Color selectedColor = e.Color;
    
    // Apply to text selection
    if (richTextBox1.SelectionLength > 0)
    {
        richTextBox1.SelectionColor = selectedColor;
    }
    else
    {
        // Apply to future text if no selection
        richTextBox1.ForeColor = selectedColor;
    }
    
    // Update status bar
    statusLabel.Text = $"Color changed to: {selectedColor.Name}";
}
```

**Result:**
When users select a color, it's immediately applied to the selected text in the RichTextBox.

### Complete Color Palette Example

```csharp
public class ColorFormattingDemo : Form
{
    private RichTextBox editor;
    private RadialMenu radialMenu;
    private Label statusLabel;

    private void InitializeColorPalette()
    {
        // Create RadialMenu
        this.radialMenu = new RadialMenu();
        this.radialMenu.Style = RadialMenuStyle.Office2016Colorful;
        this.radialMenu.Size = new Size(320, 320);
        this.radialMenu.Location = new Point(50, 50);
        this.radialMenu.Visible = true;
        this.radialMenu.MenuVisibility = true;

        // Set up image list
        ImageListAdv imageList = new ImageListAdv(this.components);
        // Add color palette icon (you would use actual image file)
        imageList.Images.Add(CreateColorIcon());
        this.radialMenu.ImageList = imageList;

        // Create text color palette
        RadialColorPalette textColorPalette = new RadialColorPalette();
        textColorPalette.Text = "Text Color";
        textColorPalette.ImageIndex = 0;
        textColorPalette.ColorSelected += (s, e) =>
        {
            if (editor.SelectionLength > 0)
                editor.SelectionColor = e.Color;
            else
                editor.ForeColor = e.Color;
            
            statusLabel.Text = $"Text color: {e.Color.Name}";
        };

        // Create background color palette
        RadialColorPalette backColorPalette = new RadialColorPalette();
        backColorPalette.Text = "Highlight";
        backColorPalette.ImageIndex = 0;
        backColorPalette.ColorSelected += (s, e) =>
        {
            if (editor.SelectionLength > 0)
                editor.SelectionBackColor = e.Color;
            else
                editor.BackColor = e.Color;
            
            statusLabel.Text = $"Background color: {e.Color.Name}";
        };

        // Add to menu
        this.radialMenu.Items.Add(textColorPalette);
        this.radialMenu.Items.Add(backColorPalette);

        this.Controls.Add(this.radialMenu);
    }

    private Image CreateColorIcon()
    {
        // Create a simple color icon programmatically
        Bitmap bmp = new Bitmap(32, 32);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            g.FillEllipse(Brushes.Red, 4, 4, 24, 24);
            g.DrawEllipse(Pens.Black, 4, 4, 24, 24);
        }
        return bmp;
    }
}
```

**Result:**
A fully functional color selection menu with separate palettes for text color and background highlighting.

## RadialFontListBox

`RadialFontListBox` displays a list of all installed fonts on the system, allowing users to select a font family for text formatting.

### Creating a RadialFontListBox

```csharp
// Create the font list box instance
RadialFontListBox fontListBox = new RadialFontListBox();
fontListBox.Text = "Font";
fontListBox.ImageIndex = 1;  // Icon for font selection

// Add to RadialMenu
this.radialMenu1.Items.Add(fontListBox);
```

### Handling SelectedFontChanged Event

The `SelectedFontChanged` event fires when a user selects a font from the list.

```csharp
private void SetupFontListBox()
{
    // Set up image list with font icon
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Image.FromFile("icons/color.png"));      // Index 0
    imageList.Images.Add(Image.FromFile("icons/font.png"));       // Index 1
    imageList.Images.Add(Image.FromFile("icons/size.png"));       // Index 2
    this.radialMenu1.ImageList = imageList;

    // Create font list box
    RadialFontListBox fontListBox = new RadialFontListBox();
    fontListBox.Text = "Font";
    fontListBox.ImageIndex = 1;  // Font icon
    
    // Attach event handler
    fontListBox.SelectedFontChanged += FontListBox_SelectedFontChanged;
    
    this.radialMenu1.Items.Add(fontListBox);
}

private void FontListBox_SelectedFontChanged(object sender, SelectedFontChangedEventArgs e)
{
    // Get the selected font family
    string selectedFont = e.FontName;
    
    // Apply to text selection
    if (richTextBox1.SelectionLength > 0)
    {
        Font currentFont = richTextBox1.SelectionFont;
        if (currentFont != null)
        {
            // Create new font with selected family, keeping current size and style
            Font newFont = new Font(selectedFont, currentFont.Size, currentFont.Style);
            richTextBox1.SelectionFont = newFont;
        }
    }
    else
    {
        // Apply to entire control if no selection
        Font currentFont = richTextBox1.Font;
        richTextBox1.Font = new Font(selectedFont, currentFont.Size, currentFont.Style);
    }
    
    // Update status
    statusLabel.Text = $"Font changed to: {selectedFont}";
}
```

**Result:**
Users can browse and select from all installed fonts, with immediate application to selected text.

### Complete Font Selection Example

```csharp
private void CreateFontFormattingMenu()
{
    // Initialize RadialMenu
    this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
    this.radialMenu1.Size = new Size(320, 320);
    this.radialMenu1.Visible = true;
    this.radialMenu1.MenuVisibility = true;
    this.radialMenu1.WedgeCount = 4;

    // Set up icons
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(CreateFontIcon());
    imageList.Images.Add(CreateSizeIcon());
    this.radialMenu1.ImageList = imageList;

    // Create font list box
    RadialFontListBox fontList = new RadialFontListBox();
    fontList.Text = "Font Family";
    fontList.ImageIndex = 0;
    fontList.SelectedFontChanged += (s, e) =>
    {
        ApplyFontFamily(e.FontName);
    };

    // Create formatting options as regular menu items
    RadialMenuItem formatItem = new RadialMenuItem();
    formatItem.Text = "Style";
    
    RadialMenuItem boldItem = new RadialMenuItem();
    boldItem.Text = "Bold";
    boldItem.CheckMode = CheckMode.Check;
    boldItem.Click += (s, e) => ToggleBold(boldItem.Checked);
    
    RadialMenuItem italicItem = new RadialMenuItem();
    italicItem.Text = "Italic";
    italicItem.CheckMode = CheckMode.Check;
    italicItem.Click += (s, e) => ToggleItalic(italicItem.Checked);
    
    formatItem.Items.Add(boldItem);
    formatItem.Items.Add(italicItem);

    // Add all items
    this.radialMenu1.Items.Add(fontList);
    this.radialMenu1.Items.Add(formatItem);
}

private void ApplyFontFamily(string fontName)
{
    try
    {
        if (richTextBox1.SelectionLength > 0)
        {
            Font current = richTextBox1.SelectionFont ?? richTextBox1.Font;
            richTextBox1.SelectionFont = new Font(fontName, current.Size, current.Style);
        }
        else
        {
            Font current = richTextBox1.Font;
            richTextBox1.Font = new Font(fontName, current.Size, current.Style);
        }
    }
    catch (ArgumentException)
    {
        MessageBox.Show($"Font '{fontName}' is not available.", "Font Error");
    }
}

private Image CreateFontIcon()
{
    Bitmap bmp = new Bitmap(32, 32);
    using (Graphics g = Graphics.FromImage(bmp))
    {
        g.Clear(Color.Transparent);
        g.DrawString("F", new Font("Arial", 20, FontStyle.Bold), Brushes.Black, 2, 2);
    }
    return bmp;
}
```

**Result:**
A complete font formatting menu with font family selection and style toggles.

## RadialMenuSlider

`RadialMenuSlider` provides a circular slider interface for selecting numeric values within a specified range, ideal for size, opacity, or other numeric settings.

### Creating a RadialMenuSlider

```csharp
// Create the slider instance
RadialMenuSlider menuSlider = new RadialMenuSlider();
menuSlider.Text = "Size";
menuSlider.ImageIndex = 2;
menuSlider.MinimumValue = 8;    // Minimum value
menuSlider.MaximumValue = 72;   // Maximum value

// Add to RadialMenu
this.radialMenu1.Items.Add(menuSlider);
```

### Configuring Value Range

Set appropriate minimum and maximum values based on your use case.

```csharp
// Font size slider (8pt to 72pt)
RadialMenuSlider fontSizeSlider = new RadialMenuSlider();
fontSizeSlider.Text = "Font Size";
fontSizeSlider.MinimumValue = 8;
fontSizeSlider.MaximumValue = 72;

// Opacity slider (0% to 100%)
RadialMenuSlider opacitySlider = new RadialMenuSlider();
opacitySlider.Text = "Opacity";
opacitySlider.MinimumValue = 0;
opacitySlider.MaximumValue = 100;

// Line thickness slider (1px to 50px)
RadialMenuSlider thicknessSlider = new RadialMenuSlider();
thicknessSlider.Text = "Line Width";
thicknessSlider.MinimumValue = 1;
thicknessSlider.MaximumValue = 50;
```

### Handling SliderValueChanged Event

The `SliderValueChanged` event fires when the user adjusts the slider value.

```csharp
private void SetupFontSizeSlider()
{
    // Set up image list
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Image.FromFile("icons/size.png"));
    this.radialMenu1.ImageList = imageList;

    // Create font size slider
    RadialMenuSlider sizeSlider = new RadialMenuSlider();
    sizeSlider.Text = "Font Size";
    sizeSlider.ImageIndex = 0;
    sizeSlider.MinimumValue = 8;
    sizeSlider.MaximumValue = 72;
    
    // Attach event handler
    sizeSlider.SliderValueChanged += SizeSlider_ValueChanged;
    
    this.radialMenu1.Items.Add(sizeSlider);
}

private void SizeSlider_ValueChanged(object sender, SliderValueChangedEventArgs e)
{
    // Get the selected value
    int fontSize = e.Value;
    
    // Apply to text selection
    if (richTextBox1.SelectionLength > 0)
    {
        Font currentFont = richTextBox1.SelectionFont;
        if (currentFont != null)
        {
            // Create new font with selected size
            Font newFont = new Font(
                currentFont.FontFamily,
                fontSize,
                currentFont.Style
            );
            richTextBox1.SelectionFont = newFont;
        }
    }
    else
    {
        // Apply to entire control if no selection
        Font currentFont = richTextBox1.Font;
        richTextBox1.Font = new Font(
            currentFont.FontFamily,
            fontSize,
            currentFont.Style
        );
    }
    
    // Update status
    statusLabel.Text = $"Font size: {fontSize}pt";
}
```

**Result:**
Users can adjust font size smoothly using the circular slider interface, with immediate visual feedback.

### Complete Slider Example

```csharp
public class SliderFormattingDemo : Form
{
    private RichTextBox editor;
    private RadialMenu radialMenu;
    private Panel previewPanel;

    private void InitializeSliders()
    {
        // Create RadialMenu
        this.radialMenu = new RadialMenu();
        this.radialMenu.Style = RadialMenuStyle.Office2016Colorful;
        this.radialMenu.Size = new Size(320, 320);
        this.radialMenu.Location = new Point(50, 50);
        this.radialMenu.Visible = true;
        this.radialMenu.MenuVisibility = true;

        // Set up image list
        ImageListAdv imageList = new ImageListAdv(this.components);
        imageList.Images.Add(CreateSizeIcon());       // Index 0
        imageList.Images.Add(CreateOpacityIcon());    // Index 1
        this.radialMenu.ImageList = imageList;

        // Font size slider
        RadialMenuSlider fontSizeSlider = new RadialMenuSlider();
        fontSizeSlider.Text = "Font Size";
        fontSizeSlider.ImageIndex = 0;
        fontSizeSlider.MinimumValue = 8;
        fontSizeSlider.MaximumValue = 72;
        fontSizeSlider.SliderValueChanged += (s, e) =>
        {
            if (editor.SelectionLength > 0)
            {
                Font current = editor.SelectionFont ?? editor.Font;
                editor.SelectionFont = new Font(
                    current.FontFamily,
                    e.Value,
                    current.Style
                );
            }
            statusLabel.Text = $"Font size: {e.Value}pt";
        };

        // Opacity slider
        RadialMenuSlider opacitySlider = new RadialMenuSlider();
        opacitySlider.Text = "Opacity";
        opacitySlider.ImageIndex = 1;
        opacitySlider.MinimumValue = 0;
        opacitySlider.MaximumValue = 100;
        opacitySlider.SliderValueChanged += (s, e) =>
        {
            // Apply opacity to preview panel
            int alpha = (int)(e.Value * 2.55);  // Convert 0-100 to 0-255
            previewPanel.BackColor = Color.FromArgb(
                alpha,
                previewPanel.BackColor.R,
                previewPanel.BackColor.G,
                previewPanel.BackColor.B
            );
            statusLabel.Text = $"Opacity: {e.Value}%";
        };

        // Add to menu
        this.radialMenu.Items.Add(fontSizeSlider);
        this.radialMenu.Items.Add(opacitySlider);

        this.Controls.Add(this.radialMenu);
    }

    private Image CreateSizeIcon()
    {
        Bitmap bmp = new Bitmap(32, 32);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            g.DrawString("A", new Font("Arial", 20), Brushes.Black, 2, 2);
        }
        return bmp;
    }

    private Image CreateOpacityIcon()
    {
        Bitmap bmp = new Bitmap(32, 32);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            g.FillRectangle(new SolidBrush(Color.FromArgb(128, 0, 0, 255)), 8, 8, 16, 16);
        }
        return bmp;
    }
}
```

**Result:**
A professional formatting menu with smooth slider controls for font size and opacity adjustments.

## ImageListAdv Configuration

All special elements require proper ImageListAdv configuration to display custom icons.

### Setting Up ImageListAdv

```csharp
private void ConfigureImageList()
{
    // Create ImageListAdv component
    ImageListAdv imageList = new ImageListAdv(this.components);
    
    // Configure image size (32x32 recommended for RadialMenu)
    imageList.ImageSize = new Size(32, 32);
    
    // Add images from files
    imageList.Images.Add("color", Image.FromFile("icons/color-palette.png"));
    imageList.Images.Add("font", Image.FromFile("icons/font.png"));
    imageList.Images.Add("size", Image.FromFile("icons/size.png"));
    
    // OR add images from resources
    imageList.Images.Add("color", Properties.Resources.ColorIcon);
    imageList.Images.Add("font", Properties.Resources.FontIcon);
    imageList.Images.Add("size", Properties.Resources.SizeIcon);
    
    // Attach to RadialMenu
    this.radialMenu1.ImageList = imageList;
}
```

### Using Image Indices

```csharp
// Create special elements with correct image indices
RadialColorPalette colorPalette = new RadialColorPalette();
colorPalette.ImageIndex = 0;  // First image (color)

RadialFontListBox fontList = new RadialFontListBox();
fontList.ImageIndex = 1;  // Second image (font)

RadialMenuSlider sizeSlider = new RadialMenuSlider();
sizeSlider.ImageIndex = 2;  // Third image (size)
```

## Integration Patterns

### Pattern 1: Complete Text Formatting Toolbar

```csharp
private void CreateCompleteFormattingMenu()
{
    // Initialize menu
    this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
    this.radialMenu1.WedgeCount = 6;
    this.radialMenu1.Size = new Size(350, 350);

    // Set up image list
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Properties.Resources.ColorIcon);
    imageList.Images.Add(Properties.Resources.FontIcon);
    imageList.Images.Add(Properties.Resources.SizeIcon);
    this.radialMenu1.ImageList = imageList;

    // 1. Text color palette
    RadialColorPalette textColor = new RadialColorPalette();
    textColor.Text = "Text Color";
    textColor.ImageIndex = 0;
    textColor.ColorSelected += (s, e) =>
    {
        if (richTextBox1.SelectionLength > 0)
            richTextBox1.SelectionColor = e.Color;
    };

    // 2. Highlight color palette
    RadialColorPalette highlight = new RadialColorPalette();
    highlight.Text = "Highlight";
    highlight.ImageIndex = 0;
    highlight.ColorSelected += (s, e) =>
    {
        if (richTextBox1.SelectionLength > 0)
            richTextBox1.SelectionBackColor = e.Color;
    };

    // 3. Font family list
    RadialFontListBox fontList = new RadialFontListBox();
    fontList.Text = "Font";
    fontList.ImageIndex = 1;
    fontList.SelectedFontChanged += (s, e) =>
    {
        ApplyFontFamily(e.FontName);
    };

    // 4. Font size slider
    RadialMenuSlider fontSize = new RadialMenuSlider();
    fontSize.Text = "Size";
    fontSize.ImageIndex = 2;
    fontSize.MinimumValue = 8;
    fontSize.MaximumValue = 72;
    fontSize.SliderValueChanged += (s, e) =>
    {
        ApplyFontSize(e.Value);
    };

    // 5. Format toggles (submenu)
    RadialMenuItem formatItem = new RadialMenuItem();
    formatItem.Text = "Format";
    
    string[] formats = { "Bold", "Italic", "Underline", "Strikeout" };
    foreach (string format in formats)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = format;
        item.CheckMode = CheckMode.Check;
        item.Click += FormatToggle_Click;
        formatItem.Items.Add(item);
    }

    // 6. Alignment (submenu)
    RadialMenuItem alignItem = new RadialMenuItem();
    alignItem.Text = "Align";
    
    string[] alignments = { "Left", "Center", "Right", "Justify" };
    foreach (string align in alignments)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = align;
        item.CheckMode = CheckMode.Option;
        item.GroupName = "alignment";
        item.Checked = (align == "Left");
        item.Click += Alignment_Click;
        alignItem.Items.Add(item);
    }

    // Add all to menu
    this.radialMenu1.Items.Add(textColor);
    this.radialMenu1.Items.Add(highlight);
    this.radialMenu1.Items.Add(fontList);
    this.radialMenu1.Items.Add(fontSize);
    this.radialMenu1.Items.Add(formatItem);
    this.radialMenu1.Items.Add(alignItem);
}
```

**Result:**
A professional-grade text formatting toolbar with all essential features integrated.

### Pattern 2: Graphics Editor Toolbar

```csharp
private void CreateGraphicsEditorMenu()
{
    // Set up menu
    this.radialMenu1.Style = RadialMenuStyle.Office2016DarkGray;
    this.radialMenu1.WedgeCount = 5;

    // Configure icons
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Properties.Resources.BrushColorIcon);
    imageList.Images.Add(Properties.Resources.BrushSizeIcon);
    this.radialMenu1.ImageList = imageList;

    // Brush color
    RadialColorPalette brushColor = new RadialColorPalette();
    brushColor.Text = "Brush Color";
    brushColor.ImageIndex = 0;
    brushColor.ColorSelected += (s, e) =>
    {
        currentBrushColor = e.Color;
        UpdateBrushPreview();
    };

    // Brush size
    RadialMenuSlider brushSize = new RadialMenuSlider();
    brushSize.Text = "Brush Size";
    brushSize.ImageIndex = 1;
    brushSize.MinimumValue = 1;
    brushSize.MaximumValue = 50;
    brushSize.SliderValueChanged += (s, e) =>
    {
        currentBrushSize = e.Value;
        UpdateBrushPreview();
    };

    // Brush opacity
    RadialMenuSlider brushOpacity = new RadialMenuSlider();
    brushOpacity.Text = "Opacity";
    brushOpacity.MinimumValue = 0;
    brushOpacity.MaximumValue = 100;
    brushOpacity.SliderValueChanged += (s, e) =>
    {
        currentOpacity = e.Value;
        UpdateBrushPreview();
    };

    // Tool selection
    RadialMenuItem toolsItem = new RadialMenuItem();
    toolsItem.Text = "Tools";
    string[] tools = { "Brush", "Pencil", "Eraser", "Fill" };
    foreach (string tool in tools)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = tool;
        item.CheckMode = CheckMode.Option;
        item.GroupName = "tools";
        item.Click += Tool_Selected;
        toolsItem.Items.Add(item);
    }

    this.radialMenu1.Items.Add(brushColor);
    this.radialMenu1.Items.Add(brushSize);
    this.radialMenu1.Items.Add(brushOpacity);
    this.radialMenu1.Items.Add(toolsItem);
}
```

**Result:**
A complete graphics editor toolbar with color, size, and opacity controls.

## Complete Working Examples

### Example: Rich Text Editor Menu

```csharp
public partial class RichTextEditorForm : Form
{
    private RichTextBox editor;
    private RadialMenu formattingMenu;
    private StatusStrip statusStrip;
    private ToolStripStatusLabel statusLabel;

    public RichTextEditorForm()
    {
        InitializeComponent();
        InitializeEditor();
        InitializeFormattingMenu();
    }

    private void InitializeEditor()
    {
        this.editor = new RichTextBox();
        this.editor.Dock = DockStyle.Fill;
        this.editor.Font = new Font("Arial", 12);
        this.Controls.Add(this.editor);

        this.statusStrip = new StatusStrip();
        this.statusLabel = new ToolStripStatusLabel();
        this.statusStrip.Items.Add(this.statusLabel);
        this.Controls.Add(this.statusStrip);
    }

    private void InitializeFormattingMenu()
    {
        // Create menu
        this.formattingMenu = new RadialMenu();
        this.formattingMenu.Style = RadialMenuStyle.Office2016Colorful;
        this.formattingMenu.Size = new Size(360, 360);
        this.formattingMenu.Location = new Point(100, 100);
        this.formattingMenu.Visible = true;
        this.formattingMenu.MenuVisibility = false;
        this.formattingMenu.WedgeCount = 4;

        // Set up icons
        ImageListAdv images = new ImageListAdv(this.components);
        images.Images.Add("color", CreateColorIcon());
        images.Images.Add("font", CreateFontIcon());
        images.Images.Add("size", CreateSizeIcon());
        this.formattingMenu.ImageList = images;

        // Add text color
        RadialColorPalette textColor = new RadialColorPalette();
        textColor.Text = "Color";
        textColor.ImageIndex = 0;
        textColor.ColorSelected += TextColor_Selected;

        // Add font family
        RadialFontListBox fontFamily = new RadialFontListBox();
        fontFamily.Text = "Font";
        fontFamily.ImageIndex = 1;
        fontFamily.SelectedFontChanged += FontFamily_Changed;

        // Add font size
        RadialMenuSlider fontSize = new RadialMenuSlider();
        fontSize.Text = "Size";
        fontSize.ImageIndex = 2;
        fontSize.MinimumValue = 8;
        fontSize.MaximumValue = 72;
        fontSize.SliderValueChanged += FontSize_Changed;

        // Add style toggles
        RadialMenuItem styleItem = new RadialMenuItem();
        styleItem.Text = "Style";
        CreateStyleOptions(styleItem);

        // Add all items
        this.formattingMenu.Items.Add(textColor);
        this.formattingMenu.Items.Add(fontFamily);
        this.formattingMenu.Items.Add(fontSize);
        this.formattingMenu.Items.Add(styleItem);

        // Show menu on right-click
        this.editor.MouseUp += Editor_MouseUp;

        this.Controls.Add(this.formattingMenu);
    }

    private void CreateStyleOptions(RadialMenuItem parent)
    {
        string[] styles = { "Bold", "Italic", "Underline" };
        foreach (string style in styles)
        {
            RadialMenuItem item = new RadialMenuItem();
            item.Text = style;
            item.CheckMode = CheckMode.Check;
            item.Click += Style_Click;
            parent.Items.Add(item);
        }
    }

    private void Editor_MouseUp(object sender, MouseEventArgs e)
    {
        if (e.Button == MouseButtons.Right)
        {
            this.formattingMenu.Location = editor.PointToScreen(e.Location);
            this.formattingMenu.MenuVisibility = true;
        }
    }

    private void TextColor_Selected(object sender, ColorSelectedEventArgs e)
    {
        if (editor.SelectionLength > 0)
            editor.SelectionColor = e.Color;
        statusLabel.Text = $"Color: {e.Color.Name}";
    }

    private void FontFamily_Changed(object sender, SelectedFontChangedEventArgs e)
    {
        if (editor.SelectionLength > 0)
        {
            Font current = editor.SelectionFont ?? editor.Font;
            editor.SelectionFont = new Font(e.FontName, current.Size, current.Style);
        }
        statusLabel.Text = $"Font: {e.FontName}";
    }

    private void FontSize_Changed(object sender, SliderValueChangedEventArgs e)
    {
        if (editor.SelectionLength > 0)
        {
            Font current = editor.SelectionFont ?? editor.Font;
            editor.SelectionFont = new Font(current.FontFamily, e.Value, current.Style);
        }
        statusLabel.Text = $"Size: {e.Value}pt";
    }

    private void Style_Click(object sender, EventArgs e)
    {
        RadialMenuItem item = sender as RadialMenuItem;
        if (editor.SelectionLength == 0) return;

        Font current = editor.SelectionFont ?? editor.Font;
        FontStyle style = current.Style;

        switch (item.Text)
        {
            case "Bold":
                style = item.Checked ? style | FontStyle.Bold : style & ~FontStyle.Bold;
                break;
            case "Italic":
                style = item.Checked ? style | FontStyle.Italic : style & ~FontStyle.Italic;
                break;
            case "Underline":
                style = item.Checked ? style | FontStyle.Underline : style & ~FontStyle.Underline;
                break;
        }

        editor.SelectionFont = new Font(current.FontFamily, current.Size, style);
    }

    private Image CreateColorIcon()
    {
        Bitmap bmp = new Bitmap(32, 32);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            using (SolidBrush brush = new SolidBrush(Color.FromArgb(200, 255, 0, 0)))
            {
                g.FillEllipse(brush, 4, 4, 24, 24);
            }
            g.DrawEllipse(Pens.Black, 4, 4, 24, 24);
        }
        return bmp;
    }

    private Image CreateFontIcon()
    {
        Bitmap bmp = new Bitmap(32, 32);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            g.DrawString("F", new Font("Arial", 22, FontStyle.Bold), Brushes.Black, 4, 2);
        }
        return bmp;
    }

    private Image CreateSizeIcon()
    {
        Bitmap bmp = new Bitmap(32, 32);
        using (Graphics g = Graphics.FromImage(bmp))
        {
            g.Clear(Color.Transparent);
            g.DrawString("A", new Font("Arial", 10), Brushes.Black, 8, 14);
            g.DrawString("A", new Font("Arial", 18), Brushes.Black, 14, 4);
        }
        return bmp;
    }
}
```

**Result:**
A fully functional rich text editor with context-menu formatting using all three special elements.

## Best Practices

**1. Always Set ImageIndex**
```csharp
// Special elements need icons for recognition
colorPalette.ImageIndex = 0;  // REQUIRED
fontList.ImageIndex = 1;      // REQUIRED
sizeSlider.ImageIndex = 2;    // REQUIRED
```

**2. Use Descriptive Text Labels**
```csharp
// Good: Clear and specific
colorPalette.Text = "Text Color";
sizeSlider.Text = "Font Size";

// Avoid: Too generic
colorPalette.Text = "Color";  // Color of what?
sizeSlider.Text = "Value";    // Value of what?
```

**3. Set Appropriate Slider Ranges**
```csharp
// Font size: typical range
fontSizeSlider.MinimumValue = 8;
fontSizeSlider.MaximumValue = 72;

// Percentage: 0-100
opacitySlider.MinimumValue = 0;
opacitySlider.MaximumValue = 100;

// Don't use unnecessarily large ranges
brushSizeSlider.MaximumValue = 50;  // Not 1000
```

**4. Provide User Feedback**
```csharp
// Always update status or preview
colorPalette.ColorSelected += (s, e) =>
{
    ApplyColor(e.Color);
    statusLabel.Text = $"Color: {e.Color.Name}";  // Feedback
};
```

**5. Handle Edge Cases**
```csharp
// Check for valid selection before applying
if (richTextBox1.SelectionLength > 0)
{
    richTextBox1.SelectionColor = selectedColor;
}
else
{
    // Provide feedback or apply to future text
    MessageBox.Show("Please select text first");
}
```

## Troubleshooting

**Problem: Special element doesn't appear in menu**
```csharp
// Solution: Ensure ImageList is set and ImageIndex is valid
this.radialMenu1.ImageList = imageListAdv1;
colorPalette.ImageIndex = 0;  // Valid index

// Check ImageList has images
if (imageListAdv1.Images.Count == 0)
{
    // Add images first!
}
```

**Problem: ColorSelected event doesn't fire**
```csharp
// Problem: Event handler not attached
RadialColorPalette palette = new RadialColorPalette();
// Missing event handler attachment

// Solution: Always attach event
palette.ColorSelected += Palette_ColorSelected;
```

**Problem: Font selection doesn't apply**
```csharp
// Problem: Font name might not exist on system
try
{
    Font newFont = new Font(fontName, 12);
    richTextBox1.SelectionFont = newFont;
}
catch (ArgumentException ex)
{
    MessageBox.Show($"Font '{fontName}' not available: {ex.Message}");
    // Fallback to default font
    richTextBox1.SelectionFont = new Font("Arial", 12);
}
```

**Problem: Slider value seems incorrect**
```csharp
// Problem: Not handling SliderValueChanged event
RadialMenuSlider slider = new RadialMenuSlider();
slider.MinimumValue = 10;
slider.MaximumValue = 100;
// Missing: slider.SliderValueChanged += Handler;

// Solution: Attach event handler
slider.SliderValueChanged += (s, e) =>
{
    int value = e.Value;  // Get actual value
    ApplySize(value);
};
```

**Problem: Images appear too small/large**
```csharp
// Solution: Set appropriate image size in ImageListAdv
imageList.ImageSize = new Size(32, 32);  // Recommended for RadialMenu

// OR adjust individual item size
colorPalette.ImageSize = new Size(40, 40);  // Larger for emphasis
```
