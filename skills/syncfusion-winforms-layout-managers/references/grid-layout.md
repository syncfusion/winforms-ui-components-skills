# GridLayout

## Table of Contents

- [GridLayout](#gridlayout)
  - [Table of Contents](#table-of-contents)
  - [What is GridLayout](#what-is-gridlayout)
  - [Key Features](#key-features)
  - [Rows and Columns Configuration](#rows-and-columns-configuration)
  - [Automatic Child Placement](#automatic-child-placement)
  - [Spacing Configuration](#spacing-configuration)
  - [Adding Controls via Designer](#adding-controls-via-designer)
  - [Adding Controls via Code](#adding-controls-via-code)
  - [Grid Sizing Behavior](#grid-sizing-behavior)
  - [Complete Examples](#complete-examples)
    - [Calculator Layout (4x4 Grid)](#calculator-layout-4x4-grid)
    - [Icon Grid (Uniform Icons)](#icon-grid-uniform-icons)
    - [Color Palette Grid](#color-palette-grid)
  - [Common Patterns](#common-patterns)
  - [Best Practices](#best-practices)
  - [Troubleshooting](#troubleshooting)

GridLayout is a layout manager that arranges child controls in a uniform grid with fixed rows and columns. All cells in the grid are equal-sized, and controls are automatically placed in sequential order from left to right, top to bottom.

## What is GridLayout

GridLayout divides the available container space into a uniform grid containing rows and columns. Each cell in the grid has the same size, determined by dividing the container dimensions by the number of rows and columns.

**Purpose**: Arrange controls in a uniform grid where all cells are equal-sized.

**Key Characteristics**:
- Fixed number of rows and columns
- All cells have equal size
- Automatic sequential placement (left-to-right, then top-to-bottom)
- Simple and straightforward configuration

**When to Use GridLayout**:
- Creating calculator keypads
- Building uniform icon panels
- Designing color picker grids
- Creating uniform button panels
- Arranging thumbnail grids

**When NOT to Use GridLayout**:
- Need variable cell sizes (use GridBagLayout instead)
- Need controls spanning multiple cells (use GridBagLayout instead)
- Need flexible, wrapping layouts (use FlowLayout instead)

## Key Features

GridLayout provides the following features:

1. **Rows and Columns Properties**: Define the grid dimensions
2. **Automatic Child Placement**: Controls are placed sequentially without manual positioning
3. **HGap and VGap**: Control spacing between grid cells
4. **Uniform Cell Sizing**: All cells are automatically sized equally
5. **Simple Configuration**: Minimal properties to set for basic layouts

## Rows and Columns Configuration

The grid structure is defined using the Rows and Columns properties.

**Properties**:
- **Rows**: Number of rows in the grid
- **Columns**: Number of columns in the grid

**Configuration Rules**:
- If both Rows and Columns are set, the Rows property typically dictates the layout
- If Rows is set to 0 or less, the Columns property determines the number of rows based on child count
- The grid automatically calculates dimensions based on the number of child controls

**Code Examples**:

```csharp
// Create a 3x3 grid
GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = this;
gridLayout1.Rows = 3;
gridLayout1.Columns = 3;

// Create a grid with 2 rows (columns auto-calculated)
gridLayout1.Rows = 2;
gridLayout1.Columns = 0;  // Auto-calculate based on child count

// Create a grid with 4 columns (rows auto-calculated)
gridLayout1.Rows = 0;  // Auto-calculate based on child count
gridLayout1.Columns = 4;
```

```vbnet
' Create a 3x3 grid
Dim gridLayout1 As New GridLayout()
gridLayout1.ContainerControl = Me
gridLayout1.Rows = 3
gridLayout1.Columns = 3

' Create a grid with 2 rows (columns auto-calculated)
gridLayout1.Rows = 2
gridLayout1.Columns = 0  ' Auto-calculate based on child count

' Create a grid with 4 columns (rows auto-calculated)
gridLayout1.Rows = 0  ' Auto-calculate based on child count
gridLayout1.Columns = 4
```

## Automatic Child Placement

GridLayout automatically places child controls in sequential order:

1. **Left-to-Right**: Controls are placed from left to right across each row
2. **Top-to-Bottom**: After filling a row, placement continues on the next row below
3. **Order-Based**: Placement order is determined by the order controls are added to the container

**No Manual Positioning Required**: Unlike GridBagLayout, you don't need to specify cell positions - GridLayout handles this automatically.

**Code Example**:

```csharp
// Create 3x3 grid
GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = this;
gridLayout1.Rows = 3;
gridLayout1.Columns = 3;

// Add 9 buttons - they will be placed automatically
// Row 1: Button1, Button2, Button3
// Row 2: Button4, Button5, Button6
// Row 3: Button7, Button8, Button9
for (int i = 1; i <= 9; i++)
{
    Button button = new Button();
    button.Text = "Button " + i;
    this.Controls.Add(button);
}
```

```vbnet
' Create 3x3 grid
Dim gridLayout1 As New GridLayout()
gridLayout1.ContainerControl = Me
gridLayout1.Rows = 3
gridLayout1.Columns = 3

' Add 9 buttons - they will be placed automatically
' Row 1: Button1, Button2, Button3
' Row 2: Button4, Button5, Button6
' Row 3: Button7, Button8, Button9
For i As Integer = 1 To 9
    Dim button As New Button()
    button.Text = "Button " & i
    Me.Controls.Add(button)
Next
```

## Spacing Configuration

Control the spacing between grid cells using HGap (horizontal gap) and VGap (vertical gap) properties.

**Properties**:
- **HGap**: Horizontal spacing between cells (in pixels)
- **VGap**: Vertical spacing between cells (in pixels)

**Code Example**:

{% tabs %}

{% highlight C# %}

// Set spacing between grid cells
gridLayout1.HGap = 5;   // 5 pixels between columns
gridLayout1.VGap = 5;   // 5 pixels between rows

// No spacing (cells touch)
gridLayout1.HGap = 0;
gridLayout1.VGap = 0;

// Large spacing for visual separation
gridLayout1.HGap = 15;
gridLayout1.VGap = 15;

{% endhighlight %}

{% highlight VB %}

' Set spacing between grid cells
gridLayout1.HGap = 5   ' 5 pixels between columns
gridLayout1.VGap = 5   ' 5 pixels between rows

' No spacing (cells touch)
gridLayout1.HGap = 0
gridLayout1.VGap = 0

' Large spacing for visual separation
gridLayout1.HGap = 15
gridLayout1.VGap = 15

{% endhighlight %}

{% endtabs %}

## Adding Controls via Designer

**Step-by-Step Instructions**:

1. **Open your Windows Forms project** in Visual Studio
2. **Locate GridLayout** in the Toolbox (under Syncfusion Controls)
3. **Drag and drop** GridLayout onto the form
4. **Click "Yes"** in the popup to set the form as the container control
5. **Set Rows and Columns**:
   - Select the GridLayout component
   - In Properties window, set `Rows` (e.g., 3)
   - Set `Columns` (e.g., 3)
6. **Add child controls**:
   - Drag controls from Toolbox onto the form
   - Controls are automatically placed in grid order
7. **Configure spacing**:
   - Set `HGap` for horizontal spacing
   - Set `VGap` for vertical spacing
8. **Rearrange controls** (if needed):
   - Right-click control and select "Bring To Front" or "Send To Back"
   - Or drag-and-drop controls to reorder

## Adding Controls via Code

**Step-by-Step Instructions**:

**Step 1**: Add assembly reference:
- `Syncfusion.Tools.Windows.dll`

**Step 2**: Include namespace:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vbnet
Imports Syncfusion.Windows.Forms.Tools
```

**Step 3**: Create GridLayout and add controls:

```csharp
// Create GridLayout instance
GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = this;
gridLayout1.Rows = 3;
gridLayout1.Columns = 3;
gridLayout1.HGap = 5;
gridLayout1.VGap = 5;

// Create and add button controls
for (int i = 1; i <= 9; i++)
{
    Button button = new Button();
    button.Text = i.ToString();
    button.Font = new Font("Arial", 12, FontStyle.Bold);
    
    // Add to container (automatically placed in grid)
    this.Controls.Add(button);
}
```

```vbnet
' Create GridLayout instance
Dim gridLayout1 As New GridLayout()
gridLayout1.ContainerControl = Me
gridLayout1.Rows = 3
gridLayout1.Columns = 3
gridLayout1.HGap = 5
gridLayout1.VGap = 5

' Create and add button controls
For i As Integer = 1 To 9
    Dim button As New Button()
    button.Text = i.ToString()
    button.Font = New Font("Arial", 12, FontStyle.Bold)
    
    ' Add to container (automatically placed in grid)
    Me.Controls.Add(button)
Next
```

## Grid Sizing Behavior

GridLayout automatically calculates cell sizes based on:

1. **Container Size**: The available space in the container control
2. **Number of Rows and Columns**: Divides container space evenly
3. **Gap Settings**: HGap and VGap are subtracted from available space

**Cell Size Calculation**:
```
Cell Width = (Container Width - (Columns - 1) * HGap) / Columns
Cell Height = (Container Height - (Rows - 1) * VGap) / Rows
```

**All Controls Sized to Fit Cells**: Child controls are automatically resized to fit their cells

**Resizing Behavior**:
- When container resizes, cells resize proportionally
- All cells remain equal-sized
- Control sizes adjust automatically to fill cells

```csharp
// Set spacing between grid cells
gridLayout1.HGap = 5;   // 5 pixels between columns
gridLayout1.VGap = 5;   // 5 pixels between rows

// No spacing (cells touch)
gridLayout1.HGap = 0;
gridLayout1.VGap = 0;

// Large spacing for visual separation
gridLayout1.HGap = 15;
gridLayout1.VGap = 15;
```

```vbnet
' Set spacing between grid cells
gridLayout1.HGap = 5   ' 5 pixels between columns
gridLayout1.VGap = 5   ' 5 pixels between rows

' No spacing (cells touch)
gridLayout1.HGap = 0
gridLayout1.VGap = 0

' Large spacing for visual separation
gridLayout1.HGap = 15
gridLayout1.VGap = 15
```

' Container: 300x300 pixels
' Grid: 3x3
' HGap: 10, VGap: 10

' Cell Width = (300 - 2*10) / 3 = 280 / 3 ≈ 93 pixels
' Cell Height = (300 - 2*10) / 3 = 280 / 3 ≈ 93 pixels

```csharp
// Container: 300x300 pixels
// Grid: 3x3
// HGap: 10, VGap: 10

// Cell Width = (300 - 2*10) / 3 = 280 / 3 ≈ 93 pixels
// Cell Height = (300 - 2*10) / 3 = 280 / 3 ≈ 93 pixels

Panel panel1 = new Panel();
panel1.Size = new Size(300, 300);
this.Controls.Add(panel1);

GridLayout gridLayout1 = new GridLayout();
gridLayout1.ContainerControl = panel1;
gridLayout1.Rows = 3;
gridLayout1.Columns = 3;
gridLayout1.HGap = 10;
gridLayout1.VGap = 10;

// Each button will be approximately 93x93 pixels
for (int i = 1; i <= 9; i++)
{
    Button button = new Button();
    button.Text = i.ToString();
    panel1.Controls.Add(button);
}
```

```vbnet
' Container: 300x300 pixels
' Grid: 3x3
' HGap: 10, VGap: 10
'
' Cell Width = (300 - 2*10) / 3 = 280 / 3 ≈ 93 pixels
' Cell Height = (300 - 2*10) / 3 = 280 / 3 ≈ 93 pixels
'
Dim panel1 As New Panel()
panel1.Size = New Size(300, 300)
Me.Controls.Add(panel1)
'
Dim gridLayout1 As New GridLayout()
gridLayout1.ContainerControl = panel1
gridLayout1.Rows = 3
gridLayout1.Columns = 3
gridLayout1.HGap = 10
gridLayout1.VGap = 10

' Each button will be approximately 93x93 pixels
For i As Integer = 1 To 9
    Dim button As New Button()
    button.Text = i.ToString()
    panel1.Controls.Add(button)
Next
```
        gridLayout1.VGap = 2;

        // Calculator button labels
        string[] buttons = {
            "7", "8", "9", "/",
            "4", "5", "6", "*",
            "1", "2", "3", "-",
            "0", ".", "=", "+"
        };

        // Add buttons
        foreach (string label in buttons)
        {
            ButtonAdv button = new ButtonAdv();
            button.Text = label;
            button.Font = new Font("Arial", 14, FontStyle.Bold);
            button.Click += Button_Click;
            buttonPanel.Controls.Add(button);
        }

        // Form settings
        this.Text = "Calculator Grid Layout";
        this.Size = new Size(300, 350);
        this.FormBorderStyle = FormBorderStyle.FixedDialog;
        this.MaximizeBox = false;
    }

    private void Button_Click(object sender, EventArgs e)
    {
        ButtonAdv button = sender as ButtonAdv;
        MessageBox.Show("Button clicked: " + button.Text);
    ```csharp
    using System;
    using System.Drawing;
    using System.Windows.Forms;
    using Syncfusion.Windows.Forms.Tools;

    public class CalculatorForm : Form
    {
        private GridLayout gridLayout1;
        private Panel buttonPanel;

        public CalculatorForm()
        {
            InitializeComponent();
        }

        private void InitializeComponent()
        {
            // Create button panel
            buttonPanel = new Panel();
            buttonPanel.Location = new Point(20, 60);
            buttonPanel.Size = new Size(240, 240);
            buttonPanel.BorderStyle = BorderStyle.FixedSingle;
            this.Controls.Add(buttonPanel);

            // Create GridLayout
            gridLayout1 = new GridLayout();
            gridLayout1.ContainerControl = buttonPanel;
            gridLayout1.Rows = 4;
            gridLayout1.Columns = 4;
            gridLayout1.HGap = 2;
            gridLayout1.VGap = 2;

            // Calculator button labels
            string[] buttons = {
                "7", "8", "9", "/",
                "4", "5", "6", "*",
                "1", "2", "3", "-",
                "0", ".", "=", "+"
            };

            // Add buttons
            foreach (string label in buttons)
            {
                Button button = new Button();
                button.Text = label;
                button.Font = new Font("Arial", 14, FontStyle.Bold);
                button.Click += Button_Click;
                buttonPanel.Controls.Add(button);
            }

            // Form settings
            this.Text = "Calculator Grid Layout";
            this.Size = new Size(300, 350);
            this.FormBorderStyle = FormBorderStyle.FixedDialog;
            this.MaximizeBox = false;
        }

        private void Button_Click(object sender, EventArgs e)
        {
            Button button = sender as Button;
            MessageBox.Show("Button clicked: " + button.Text);
        }
    }
    ```

    ```vbnet
    Imports System
    Imports System.Drawing
    Imports System.Windows.Forms
    Imports Syncfusion.Windows.Forms.Tools

    Public Class CalculatorForm
        Inherits Form

        Private gridLayout1 As GridLayout
        Private buttonPanel As Panel

        Public Sub New()
            InitializeComponent()
        End Sub

        Private Sub InitializeComponent()
            ' Create button panel
            buttonPanel = New Panel()
            buttonPanel.Location = New Point(20, 60)
            buttonPanel.Size = New Size(240, 240)
            buttonPanel.BorderStyle = BorderStyle.FixedSingle
            Me.Controls.Add(buttonPanel)

            ' Create GridLayout
            gridLayout1 = New GridLayout()
            gridLayout1.ContainerControl = buttonPanel
            gridLayout1.Rows = 4
            gridLayout1.Columns = 4
            gridLayout1.HGap = 2
            gridLayout1.VGap = 2

            ' Calculator button labels
            Dim buttons() As String = {
                "7", "8", "9", "/",
                "4", "5", "6", "*",
                "1", "2", "3", "-",
                "0", ".", "=", "+"
            }

            ' Add buttons
            For Each label As String In buttons
                Dim button As New Button()
                button.Text = label
                button.Font = New Font("Arial", 14, FontStyle.Bold)
                AddHandler button.Click, AddressOf Button_Click
                buttonPanel.Controls.Add(button)
            Next

            ' Form settings
            Me.Text = "Calculator Grid Layout"
            Me.Size = New Size(300, 350)
            Me.FormBorderStyle = FormBorderStyle.FixedDialog
            Me.MaximizeBox = False
        End Sub

        Private Sub Button_Click(sender As Object, e As EventArgs)
            Dim button As Button = TryCast(sender, Button)
            MessageBox.Show("Button clicked: " & button.Text)
        End Sub
    End Class
    ```
{% endhighlight %}

{% highlight VB %}

Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class IconGridForm
    Inherits Form

    Private gridLayout1 As GridLayout
    Private iconPanel As Panel

    Public Sub New()
        InitializeComponent()
    End Sub

    Private Sub InitializeComponent()
        ' Create icon panel
        iconPanel = New Panel()
        iconPanel.Dock = DockStyle.Fill
        iconPanel.Padding = New Padding(10)
        iconPanel.BackColor = Color.White
        Me.Controls.Add(iconPanel)

        ' Create GridLayout
        gridLayout1 = New GridLayout()
        gridLayout1.ContainerControl = iconPanel
        gridLayout1.Rows = 3
        gridLayout1.Columns = 4
        gridLayout1.HGap = 10
        gridLayout1.VGap = 10

        ' Icon labels and emojis
        Dim icons(,) As String = {
            {"Home", "🏠"}, {"Settings", "⚙"}, {"Search", "🔍"}, {"Mail", "✉"},
            {"Calendar", "📅"}, {"Photos", "📷"}, {"Music", "🎵"}, {"Videos", "🎬"},
            {"Documents", "📄"}, {"Downloads", "⬇"}, {"Cloud", "☁"}, {"Help", "❓"}
        }

        ' Add icon buttons
        For i As Integer = 0 To icons.GetLength(0) - 1
            Dim button As New ButtonAdv()
            button.Text = icons(i, 1) & vbLf & icons(i, 0)
            button.Font = New Font("Segoe UI", 10)
            button.TextAlign = ContentAlignment.MiddleCenter
            iconPanel.Controls.Add(button)
        Next

        ' Form settings
        Me.Text = "Icon Grid"
        Me.Size = New Size(600, 400)
        Me.MinimumSize = New Size(400, 300)
    End Sub
End Class

{% endhighlight %}

{% endtabs %}

### Color Palette Grid

Create a color picker grid with uniform color swatches.

{% tabs %}

{% highlight C# %}

using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class ColorPaletteForm : Form
{
    private GridLayout gridLayout1;
    private Panel palettePanel;
    private Label selectedColorLabel;

    public ColorPaletteForm()
    {
        InitializeComponent();
    }

    private void InitializeComponent()
    {
        // Status label
        selectedColorLabel = new Label();
        selectedColorLabel.Text = "Select a color";
        selectedColorLabel.Dock = DockStyle.Top;
        selectedColorLabel.Height = 30;
        selectedColorLabel.TextAlign = ContentAlignment.MiddleCenter;
        selectedColorLabel.Font = new Font("Arial", 10, FontStyle.Bold);
        this.Controls.Add(selectedColorLabel);

        // Create palette panel
        palettePanel = new Panel();
        palettePanel.Dock = DockStyle.Fill;
        palettePanel.Padding = new Padding(10);
        this.Controls.Add(palettePanel);

        // Create GridLayout
        gridLayout1 = new GridLayout();
        gridLayout1.ContainerControl = palettePanel;
        gridLayout1.Rows = 8;
        gridLayout1.Columns = 8;
        gridLayout1.HGap = 3;
        gridLayout1.VGap = 3;

        // Create color palette
        Color[] baseColors = {
            Color.Red, Color.Orange, Color.Yellow, Color.Green,
            Color.Cyan, Color.Blue, Color.Purple, Color.Magenta
        };

        // Generate color swatches (8x8 grid with shades)
        for (int row = 0; row < 8; row++)
        {
            Color baseColor = baseColors[row];
            
            for (int col = 0; col < 8; col++)
            {
                // Create lighter to darker shades
                float factor = 1.0f - (col * 0.12f);
                int r = (int)(baseColor.R * factor);
                int g = (int)(baseColor.G * factor);
                int b = (int)(baseColor.B * factor);
                Color swatchColor = Color.FromArgb(r, g, b);

                // Create color swatch button
                Button swatch = new Button();
                swatch.BackColor = swatchColor;
                swatch.FlatStyle = FlatStyle.Flat;
                swatch.FlatAppearance.BorderSize = 1;
                swatch.Tag = swatchColor;
                swatch.Click += Swatch_Click;
                palettePanel.Controls.Add(swatch);
            }
        }

        // Form settings
        this.Text = "Color Palette Grid";
        this.Size = new Size(500, 500);
        this.MinimumSize = new Size(300, 300);
    }

    private void Swatch_Click(object sender, EventArgs e)
    {
        Button swatch = sender as Button;
        Color selectedColor = (Color)swatch.Tag;
        selectedColorLabel.Text = $"Selected: RGB({selectedColor.R}, {selectedColor.G}, {selectedColor.B})";
        selectedColorLabel.BackColor = selectedColor;
        selectedColorLabel.ForeColor = selectedColor.GetBrightness() > 0.5 ? Color.Black : Color.White;
    }
}

{% endhighlight %}

{% highlight VB %}

Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class ColorPaletteForm
    Inherits Form

    Private gridLayout1 As GridLayout
    Private palettePanel As Panel
    Private selectedColorLabel As Label

    Public Sub New()
        InitializeComponent()
    End Sub

    Private Sub InitializeComponent()
        ' Status label
        selectedColorLabel = New Label()
        selectedColorLabel.Text = "Select a color"
        selectedColorLabel.Dock = DockStyle.Top
        selectedColorLabel.Height = 30
        selectedColorLabel.TextAlign = ContentAlignment.MiddleCenter
        selectedColorLabel.Font = New Font("Arial", 10, FontStyle.Bold)
        Me.Controls.Add(selectedColorLabel)

        ' Create palette panel
        palettePanel = New Panel()
        palettePanel.Dock = DockStyle.Fill
        palettePanel.Padding = New Padding(10)
        Me.Controls.Add(palettePanel)

        ' Create GridLayout
        gridLayout1 = New GridLayout()
        gridLayout1.ContainerControl = palettePanel
        gridLayout1.Rows = 8
        gridLayout1.Columns = 8
        gridLayout1.HGap = 3
        gridLayout1.VGap = 3

        ' Create color palette
        Dim baseColors() As Color = {
            Color.Red, Color.Orange, Color.Yellow, Color.Green,
            Color.Cyan, Color.Blue, Color.Purple, Color.Magenta
        }

        ' Generate color swatches (8x8 grid with shades)
        For row As Integer = 0 To 7
            Dim baseColor As Color = baseColors(row)
            
            For col As Integer = 0 To 7
                ' Create lighter to darker shades
                Dim factor As Single = 1.0F - (col * 0.12F)
                Dim r As Integer = CInt(baseColor.R * factor)
                Dim g As Integer = CInt(baseColor.G * factor)
                Dim b As Integer = CInt(baseColor.B * factor)
                Dim swatchColor As Color = Color.FromArgb(r, g, b)

                ' Create color swatch button
                Dim swatch As New Button()
                swatch.BackColor = swatchColor
                swatch.FlatStyle = FlatStyle.Flat
                swatch.FlatAppearance.BorderSize = 1
                swatch.Tag = swatchColor
                AddHandler swatch.Click, AddressOf Swatch_Click
                palettePanel.Controls.Add(swatch)
            Next
        Next

        ' Form settings
        Me.Text = "Color Palette Grid"
        Me.Size = New Size(500, 500)
        Me.MinimumSize = New Size(300, 300)
    End Sub

    Private Sub Swatch_Click(sender As Object, e As EventArgs)
        Dim swatch As Button = TryCast(sender, Button)
        Dim selectedColor As Color = CType(swatch.Tag, Color)
        selectedColorLabel.Text = $"Selected: RGB({selectedColor.R}, {selectedColor.G}, {selectedColor.B})"
        selectedColorLabel.BackColor = selectedColor
        selectedColorLabel.ForeColor = If(selectedColor.GetBrightness() > 0.5, Color.Black, Color.White)
    End Sub
End Class

{% endhighlight %}

{% endtabs %}

## Common Patterns

**Calculator Keypad**:
- 4x4 or 5x4 grid
- Equal-sized buttons
- Minimal spacing (HGap=2, VGap=2)
- Fixed container size

**Icon Panel**:
- 3x4 or 4x4 grid
- Icon buttons with labels
- Medium spacing (HGap=10, VGap=10)
- Resizable container

**Color Picker Grid**:
- 8x8 or 16x16 grid
- Small color swatches
- Minimal spacing
- Fixed or resizable

**Uniform Button Panel**:
- Variable grid size (e.g., 2x3, 3x3)
- Equal-sized buttons
- Medium spacing
- Responsive to container resize

**Thumbnail Grid**:
- 3x3 or 4x4 grid
- Image thumbnails
- Medium spacing
- Scrollable container

## Best Practices

1. **Use for Uniform Layouts**: GridLayout is ideal when all controls should be the same size

2. **Set Rows and Columns Based on Content**: Calculate grid size based on the number of items you need to display

3. **Use HGap/VGap for Visual Separation**: Always add spacing between cells for better visual clarity (typically 2-10 pixels)

4. **Consider GridBagLayout for Variable Sizes**: If you need different sized cells or spanning, use GridBagLayout instead

5. **Ensure Container Has Appropriate Size**: Set container size large enough for desired cell sizes

6. **Test Resize Behavior**: Verify that controls resize appropriately when container is resized

7. **Use SetParticipateInLayout for Hidden Controls**: Exclude controls from layout rather than making them invisible

8. **Set Fixed Container Size for Calculators**: Use fixed size for calculator-style layouts to maintain button proportions

9. **Use Panels for Complex Layouts**: Nest GridLayouts in panels for more complex overall layouts

10. **Add Controls in Correct Order**: Remember that order matters - controls are placed sequentially

## Troubleshooting

**Controls Too Small**:
- Increase container size
- Reduce number of rows or columns
- Reduce HGap and VGap values

**Unexpected Layout**:
- Check Rows and Columns settings
- Verify correct number of controls added
- Check control addition order

**Spacing Issues**:
- Adjust HGap and VGap properties
- Check container padding
- Verify no conflicting margin settings on controls

**Controls in Wrong Order**:
- Check the order controls were added to container
- Use "Bring To Front" / "Send To Back" to reorder
- Remove and re-add controls in correct order

**Uneven Cell Sizes**:
- GridLayout always creates equal cells - if sizes appear uneven, check:
  - Container size calculations
  - Border and padding settings
  - HGap/VGap values

**Controls Not Filling Cells**:
- GridLayout automatically sizes controls to fill cells
- Check if controls have fixed size constraints
- Verify Dock or Anchor properties aren't interfering

**Layout Not Updating After Adding Controls**:
- Call `container.PerformLayout()` to force layout update
- Verify GridLayout's ContainerControl is set correctly
- Check that new controls are added to the correct container
