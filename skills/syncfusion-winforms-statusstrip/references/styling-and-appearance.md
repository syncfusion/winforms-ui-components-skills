# Styling and Appearance

The StatusStripEx control provides extensive styling options to match various Microsoft Office themes and custom color schemes. This guide covers all available styling options including Office2007 color schemes, Office2016 visual styles, and custom color management.

## Table of Contents

- [Overview](#overview)
- [Office2007 Color Schemes](#office2007-color-schemes)
  - [Silver Scheme](#silver-scheme)
  - [Blue Scheme](#blue-scheme)
  - [Black Scheme](#black-scheme)
- [Office2016 Visual Styles](#office2016-visual-styles)
  - [Office2016Colorful](#office2016colorful)
  - [Office2016White](#office2016white)
  - [Office2016Black](#office2016black)
  - [Office2016DarkGray](#office2016darkgray)
- [VisualStyle Property](#visualstyle-property)
- [OfficeColorScheme Property](#officecolorscheme-property)
- [Custom Managed Colors](#custom-managed-colors)
- [ApplyManagedColors Method](#applymanagedcolors-method)
- [StatusString for Context Menu](#statusstring-for-context-menu)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

The StatusStripEx control supports multiple styling approaches to integrate seamlessly with your application's visual design:

- **Office2007 Color Schemes** - Classic Office themes (Silver, Blue, Black)
- **Office2016 Visual Styles** - Modern Office themes (Colorful, White, Black, DarkGray)
- **Custom Managed Colors** - Apply custom colors for unique branding

## Office2007 Color Schemes

The StatusStripEx supports three classic Office2007 color schemes that provide a professional, polished look familiar to Microsoft Office users.

### OfficeColorScheme Property

The `OfficeColorScheme` property controls which Office2007 theme is applied. This property is of type `ToolStripEx.ColorScheme`.

### Available Color Schemes

| Scheme | Description |
|--------|-------------|
| `Silver` | Light gray theme with silver accents |
| `Blue` | Blue-tinted theme, the classic Office look |
| `Black` | Dark theme with black and gray tones |
| `Managed` | Allows custom color management |

### Silver Scheme

The Silver scheme provides a light, neutral appearance suitable for most applications.

#### Setting Silver Scheme Through Designer

1. **Select the StatusStripEx control**
2. **Locate the OfficeColorScheme property** in the Properties window
3. **Select Silver** from the dropdown

#### Setting Silver Scheme Through Code

```csharp
using Syncfusion.Windows.Forms.Tools;

// Apply Silver color scheme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Silver;
```

```vb
Imports Syncfusion.Windows.Forms.Tools

' Apply Silver color scheme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Silver
```

### Blue Scheme

The Blue scheme offers the traditional Office look with blue accents.

#### Setting Blue Scheme Through Code

```csharp
// Apply Blue color scheme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Blue;
```

```vb
' Apply Blue color scheme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Blue
```

### Black Scheme

The Black scheme provides a sophisticated dark appearance.

#### Setting Black Scheme Through Code

```csharp
// Apply Black color scheme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Black;
```

```vb
' Apply Black color scheme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Black
```

### Complete Office2007 Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Office2007Form : Form
{
    private StatusStripEx statusStripEx1;
    private ComboBox schemeComboBox;

    public Office2007Form()
    {
        InitializeComponent();
        InitializeStatusBar();
        InitializeSchemeSelector();
    }

    private void InitializeStatusBar()
    {
        // Create StatusStripEx
        this.statusStripEx1 = new StatusStripEx();
        
        // Apply default Silver scheme
        this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Silver;
        
        // Add some status items
        ToolStripStatusLabel label = new ToolStripStatusLabel("Office2007 Themes");
        this.statusStripEx1.Items.Add(label);
        
        // Dock to bottom
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
        this.Controls.Add(this.statusStripEx1);
    }

    private void InitializeSchemeSelector()
    {
        // Create a combo box to switch schemes
        this.schemeComboBox = new ComboBox();
        this.schemeComboBox.Items.AddRange(new object[] { "Silver", "Blue", "Black" });
        this.schemeComboBox.SelectedIndex = 0;
        this.schemeComboBox.SelectedIndexChanged += SchemeComboBox_SelectedIndexChanged;
        
        // Add to form (for demonstration)
        this.schemeComboBox.Location = new System.Drawing.Point(10, 10);
        this.Controls.Add(this.schemeComboBox);
    }

    private void SchemeComboBox_SelectedIndexChanged(object sender, EventArgs e)
    {
        switch (this.schemeComboBox.SelectedItem.ToString())
        {
            case "Silver":
                this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Silver;
                break;
            case "Blue":
                this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Blue;
                break;
            case "Black":
                this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Black;
                break;
        }
    }
}
```

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Office2007Form
    Inherits Form
    
    Private statusStripEx1 As StatusStripEx
    Private schemeComboBox As ComboBox
    
    Public Sub New()
        InitializeComponent()
        InitializeStatusBar()
        InitializeSchemeSelector()
    End Sub
    
    Private Sub InitializeStatusBar()
        ' Create StatusStripEx
        Me.statusStripEx1 = New StatusStripEx()
        
        ' Apply default Silver scheme
        Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Silver
        
        ' Add some status items
        Dim label As New ToolStripStatusLabel("Office2007 Themes")
        Me.statusStripEx1.Items.Add(label)
        
        ' Dock to bottom
        Me.statusStripEx1.Dock = DockStyleEx.Bottom
        Me.Controls.Add(Me.statusStripEx1)
    End Sub
    
    Private Sub InitializeSchemeSelector()
        ' Create a combo box to switch schemes
        Me.schemeComboBox = New ComboBox()
        Me.schemeComboBox.Items.AddRange(New Object() {"Silver", "Blue", "Black"})
        Me.schemeComboBox.SelectedIndex = 0
        AddHandler Me.schemeComboBox.SelectedIndexChanged, AddressOf SchemeComboBox_SelectedIndexChanged
        
        ' Add to form (for demonstration)
        Me.schemeComboBox.Location = New System.Drawing.Point(10, 10)
        Me.Controls.Add(Me.schemeComboBox)
    End Sub
    
    Private Sub SchemeComboBox_SelectedIndexChanged(sender As Object, e As EventArgs)
        Select Case Me.schemeComboBox.SelectedItem.ToString()
            Case "Silver"
                Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Silver
            Case "Blue"
                Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Blue
            Case "Black"
                Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Black
        End Select
    End Sub
End Class
```

## Office2016 Visual Styles

The StatusStripEx supports modern Office2016 visual styles that provide a contemporary, flat design aesthetic.

### VisualStyle Property

The `VisualStyle` property controls which Office2016 theme is applied. This property is of type `StatusStripExStyle`.

### Available Visual Styles

| Style | Description |
|-------|-------------|
| `Office2016Colorful` | Vibrant, colorful theme with accent colors |
| `Office2016White` | Clean white theme with minimal colors |
| `Office2016Black` | Dark black theme for low-light environments |
| `Office2016DarkGray` | Medium-dark gray theme |

### Office2016Colorful

The Colorful style provides a vibrant, modern appearance with colored accents.

#### Setting Through Designer

1. **Select the StatusStripEx control**
2. **Locate the VisualStyle property** in the Properties window
3. **Select Office2016Colorful** from the dropdown

#### Setting Through Code

```csharp
using Syncfusion.Windows.Forms.Tools;

// Apply Office2016Colorful style
this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful;
```

```vb
Imports Syncfusion.Windows.Forms.Tools

' Apply Office2016Colorful style
Me.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful
```

### Office2016White

The White style offers a clean, minimalist appearance.

#### Setting Through Code

```csharp
// Apply Office2016White style
this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016White;
```

```vb
' Apply Office2016White style
Me.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016White
```

### Office2016Black

The Black style provides a dark interface ideal for reducing eye strain.

#### Setting Through Code

```csharp
// Apply Office2016Black style
this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Black;
```

```vb
' Apply Office2016Black style
Me.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Black
```

### Office2016DarkGray

The DarkGray style offers a balanced dark theme.

#### Setting Through Code

```csharp
// Apply Office2016DarkGray style
this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016DarkGray;
```

```vb
' Apply Office2016DarkGray style
Me.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016DarkGray
```

### Complete Office2016 Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Office2016Form : Form
{
    private StatusStripEx statusStripEx1;
    private ToolStripStatusLabel statusLabel;

    public Office2016Form()
    {
        InitializeComponent();
        InitializeStatusBar();
        CreateStyleButtons();
    }

    private void InitializeStatusBar()
    {
        // Create StatusStripEx
        this.statusStripEx1 = new StatusStripEx();
        
        // Apply default Office2016Colorful style
        this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful;
        
        // Add status label
        this.statusLabel = new ToolStripStatusLabel();
        this.statusLabel.Text = "Office2016 Colorful Theme";
        this.statusLabel.Spring = true;
        this.statusLabel.TextAlign = ContentAlignment.MiddleLeft;
        
        this.statusStripEx1.Items.Add(this.statusLabel);
        
        // Dock to bottom
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
        this.Controls.Add(this.statusStripEx1);
    }

    private void CreateStyleButtons()
    {
        int buttonY = 10;
        
        // Colorful button
        Button btnColorful = new Button();
        btnColorful.Text = "Colorful";
        btnColorful.Location = new System.Drawing.Point(10, buttonY);
        btnColorful.Click += (s, e) => ApplyStyle(StatusStripExStyle.Office2016Colorful, "Colorful");
        this.Controls.Add(btnColorful);
        
        // White button
        Button btnWhite = new Button();
        btnWhite.Text = "White";
        btnWhite.Location = new System.Drawing.Point(100, buttonY);
        btnWhite.Click += (s, e) => ApplyStyle(StatusStripExStyle.Office2016White, "White");
        this.Controls.Add(btnWhite);
        
        // Black button
        Button btnBlack = new Button();
        btnBlack.Text = "Black";
        btnBlack.Location = new System.Drawing.Point(190, buttonY);
        btnBlack.Click += (s, e) => ApplyStyle(StatusStripExStyle.Office2016Black, "Black");
        this.Controls.Add(btnBlack);
        
        // DarkGray button
        Button btnDarkGray = new Button();
        btnDarkGray.Text = "DarkGray";
        btnDarkGray.Location = new System.Drawing.Point(280, buttonY);
        btnDarkGray.Click += (s, e) => ApplyStyle(StatusStripExStyle.Office2016DarkGray, "DarkGray");
        this.Controls.Add(btnDarkGray);
    }

    private void ApplyStyle(StatusStripExStyle style, string name)
    {
        this.statusStripEx1.VisualStyle = style;
        this.statusLabel.Text = $"Office2016 {name} Theme";
    }
}
```

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class Office2016Form
    Inherits Form
    
    Private statusStripEx1 As StatusStripEx
    Private statusLabel As ToolStripStatusLabel
    
    Public Sub New()
        InitializeComponent()
        InitializeStatusBar()
        CreateStyleButtons()
    End Sub
    
    Private Sub InitializeStatusBar()
        ' Create StatusStripEx
        Me.statusStripEx1 = New StatusStripEx()
        
        ' Apply default Office2016Colorful style
        Me.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful
        
        ' Add status label
        Me.statusLabel = New ToolStripStatusLabel()
        Me.statusLabel.Text = "Office2016 Colorful Theme"
        Me.statusLabel.Spring = True
        Me.statusLabel.TextAlign = ContentAlignment.MiddleLeft
        
        Me.statusStripEx1.Items.Add(Me.statusLabel)
        
        ' Dock to bottom
        Me.statusStripEx1.Dock = DockStyleEx.Bottom
        Me.Controls.Add(Me.statusStripEx1)
    End Sub
    
    Private Sub CreateStyleButtons()
        Dim buttonY As Integer = 10
        
        ' Colorful button
        Dim btnColorful As New Button()
        btnColorful.Text = "Colorful"
        btnColorful.Location = New System.Drawing.Point(10, buttonY)
        AddHandler btnColorful.Click, Sub(s, e) ApplyStyle(StatusStripExStyle.Office2016Colorful, "Colorful")
        Me.Controls.Add(btnColorful)
        
        ' White button
        Dim btnWhite As New Button()
        btnWhite.Text = "White"
        btnWhite.Location = New System.Drawing.Point(100, buttonY)
        AddHandler btnWhite.Click, Sub(s, e) ApplyStyle(StatusStripExStyle.Office2016White, "White")
        Me.Controls.Add(btnWhite)
        
        ' Black button
        Dim btnBlack As New Button()
        btnBlack.Text = "Black"
        btnBlack.Location = New System.Drawing.Point(190, buttonY)
        AddHandler btnBlack.Click, Sub(s, e) ApplyStyle(StatusStripExStyle.Office2016Black, "Black")
        Me.Controls.Add(btnBlack)
        
        ' DarkGray button
        Dim btnDarkGray As New Button()
        btnDarkGray.Text = "DarkGray"
        btnDarkGray.Location = New System.Drawing.Point(280, buttonY)
        AddHandler btnDarkGray.Click, Sub(s, e) ApplyStyle(StatusStripExStyle.Office2016DarkGray, "DarkGray")
        Me.Controls.Add(btnDarkGray)
    End Sub
    
    Private Sub ApplyStyle(style As StatusStripExStyle, name As String)
        Me.statusStripEx1.VisualStyle = style
        Me.statusLabel.Text = $"Office2016 {name} Theme"
    End Sub
End Class
```

## VisualStyle Property

The `VisualStyle` property is the primary way to set Office2016-style themes. It overrides the `OfficeColorScheme` property when set.

### Property Details

| Property | Type | Description |
|----------|------|-------------|
| `VisualStyle` | `StatusStripExStyle` | Gets or sets the Office2016 visual style |

### Setting VisualStyle

```csharp
// Set the visual style
this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful;
```

```vb
' Set the visual style
Me.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful
```

### Reading Current VisualStyle

```csharp
// Get the current visual style
StatusStripExStyle currentStyle = this.statusStripEx1.VisualStyle;
Console.WriteLine($"Current style: {currentStyle}");
```

```vb
' Get the current visual style
Dim currentStyle As StatusStripExStyle = Me.statusStripEx1.VisualStyle
Console.WriteLine($"Current style: {currentStyle}")
```

## OfficeColorScheme Property

The `OfficeColorScheme` property is used for Office2007-style themes. When using Office2016 themes, this property is typically not used.

### Property Details

| Property | Type | Description |
|----------|------|-------------|
| `OfficeColorScheme` | `ToolStripEx.ColorScheme` | Gets or sets the Office2007 color scheme |

### Setting OfficeColorScheme

```csharp
// Set the Office2007 color scheme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Blue;
```

```vb
' Set the Office2007 color scheme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Blue
```

### Reading Current OfficeColorScheme

```csharp
// Get the current color scheme
ToolStripEx.ColorScheme currentScheme = this.statusStripEx1.OfficeColorScheme;
Console.WriteLine($"Current scheme: {currentScheme}");
```

```vb
' Get the current color scheme
Dim currentScheme As ToolStripEx.ColorScheme = Me.statusStripEx1.OfficeColorScheme
Console.WriteLine($"Current scheme: {currentScheme}")
```

## Custom Managed Colors

For unique branding or custom color schemes, use the Managed color scheme with the `ApplyManagedColors` method.

### Enabling Managed Colors

First, set the `OfficeColorScheme` to `Managed`:

```csharp
// Enable managed colors
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
```

```vb
' Enable managed colors
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed
```

### Available Base Colors

You can use any `System.Drawing.Color` value as the base for your custom theme.

## ApplyManagedColors Method

The `Office2007Colors.ApplyManagedColors` method applies a custom color scheme to all Office2007-styled controls in the form.

### Method Signature

```csharp
public static void ApplyManagedColors(Form form, Color baseColor)
```

### Applying Custom Colors

```csharp
using System.Drawing;
using Syncfusion.Windows.Forms.Tools;

// Set to Managed mode
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;

// Apply custom dark green color
Office2007Colors.ApplyManagedColors(this, Color.DarkGreen);
```

```vb
Imports System.Drawing
Imports Syncfusion.Windows.Forms.Tools

' Set to Managed mode
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed

' Apply custom dark green color
Office2007Colors.ApplyManagedColors(Me, Color.DarkGreen)
```

### Examples with Different Colors

```csharp
// Custom purple theme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.Purple);

// Custom teal theme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.Teal);

// Custom orange theme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.DarkOrange);

// Custom red theme
this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.DarkRed);
```

```vb
' Custom purple theme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed
Office2007Colors.ApplyManagedColors(Me, Color.Purple)

' Custom teal theme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed
Office2007Colors.ApplyManagedColors(Me, Color.Teal)

' Custom orange theme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed
Office2007Colors.ApplyManagedColors(Me, Color.DarkOrange)

' Custom red theme
Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed
Office2007Colors.ApplyManagedColors(Me, Color.DarkRed)
```

### Complete Custom Color Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class CustomColorForm : Form
{
    private StatusStripEx statusStripEx1;
    private ColorDialog colorDialog1;
    private Button btnChangeColor;

    public CustomColorForm()
    {
        InitializeComponent();
        InitializeStatusBar();
        InitializeColorPicker();
    }

    private void InitializeStatusBar()
    {
        // Create StatusStripEx
        this.statusStripEx1 = new StatusStripEx();
        
        // Enable managed colors
        this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
        
        // Apply default custom color
        Office2007Colors.ApplyManagedColors(this, Color.DarkGreen);
        
        // Add status label
        ToolStripStatusLabel label = new ToolStripStatusLabel("Custom Colors Applied");
        this.statusStripEx1.Items.Add(label);
        
        // Dock to bottom
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
        this.Controls.Add(this.statusStripEx1);
    }

    private void InitializeColorPicker()
    {
        // Create color dialog
        this.colorDialog1 = new ColorDialog();
        
        // Create button to open color picker
        this.btnChangeColor = new Button();
        this.btnChangeColor.Text = "Choose Custom Color";
        this.btnChangeColor.Location = new System.Drawing.Point(10, 10);
        this.btnChangeColor.Size = new System.Drawing.Size(150, 30);
        this.btnChangeColor.Click += BtnChangeColor_Click;
        
        this.Controls.Add(this.btnChangeColor);
    }

    private void BtnChangeColor_Click(object sender, EventArgs e)
    {
        if (this.colorDialog1.ShowDialog() == DialogResult.OK)
        {
            // Apply selected color
            Office2007Colors.ApplyManagedColors(this, this.colorDialog1.Color);
        }
    }
}
```

```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class CustomColorForm
    Inherits Form
    
    Private statusStripEx1 As StatusStripEx
    Private colorDialog1 As ColorDialog
    Private btnChangeColor As Button
    
    Public Sub New()
        InitializeComponent()
        InitializeStatusBar()
        InitializeColorPicker()
    End Sub
    
    Private Sub InitializeStatusBar()
        ' Create StatusStripEx
        Me.statusStripEx1 = New StatusStripEx()
        
        ' Enable managed colors
        Me.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed
        
        ' Apply default custom color
        Office2007Colors.ApplyManagedColors(Me, Color.DarkGreen)
        
        ' Add status label
        Dim label As New ToolStripStatusLabel("Custom Colors Applied")
        Me.statusStripEx1.Items.Add(label)
        
        ' Dock to bottom
        Me.statusStripEx1.Dock = DockStyleEx.Bottom
        Me.Controls.Add(Me.statusStripEx1)
    End Sub
    
    Private Sub InitializeColorPicker()
        ' Create color dialog
        Me.colorDialog1 = New ColorDialog()
        
        ' Create button to open color picker
        Me.btnChangeColor = New Button()
        Me.btnChangeColor.Text = "Choose Custom Color"
        Me.btnChangeColor.Location = New System.Drawing.Point(10, 10)
        Me.btnChangeColor.Size = New System.Drawing.Size(150, 30)
        AddHandler Me.btnChangeColor.Click, AddressOf BtnChangeColor_Click
        
        Me.Controls.Add(Me.btnChangeColor)
    End Sub
    
    Private Sub BtnChangeColor_Click(sender As Object, e As EventArgs)
        If Me.colorDialog1.ShowDialog() = DialogResult.OK Then
            ' Apply selected color
            Office2007Colors.ApplyManagedColors(Me, Me.colorDialog1.Color)
        End If
    End Sub
End Class
```

## StatusString for Context Menu

The `StatusString` property is used to customize the appearance of status items in a Word-like context menu. This creates a professional, Office-style context menu display.

### Setting StatusString

```csharp
// Create a status label with StatusString
ToolStripStatusLabel pageLabel = new ToolStripStatusLabel();
pageLabel.Text = "Pages";
pageLabel.StatusString = "1/5";  // Displayed in context menu
```

```vb
' Create a status label with StatusString
Dim pageLabel As New ToolStripStatusLabel()
pageLabel.Text = "Pages"
pageLabel.StatusString = "1/5"  ' Displayed in context menu
```

### Multiple Items with StatusString

```csharp
// Create multiple status items with StatusString
ToolStripStatusLabel pageLabel = new ToolStripStatusLabel();
pageLabel.Text = "Pages";
pageLabel.StatusString = "1/5";

ToolStripStatusLabel wordLabel = new ToolStripStatusLabel();
wordLabel.Text = "Words";
wordLabel.StatusString = "1,234";

ToolStripStatusLabel charLabel = new ToolStripStatusLabel();
charLabel.Text = "Characters";
charLabel.StatusString = "5,678";

// Add all items
this.statusStripEx1.Items.AddRange(new ToolStripItem[]
{
    pageLabel,
    wordLabel,
    charLabel
});
```

```vb
' Create multiple status items with StatusString
Dim pageLabel As New ToolStripStatusLabel()
pageLabel.Text = "Pages"
pageLabel.StatusString = "1/5"

Dim wordLabel As New ToolStripStatusLabel()
wordLabel.Text = "Words"
wordLabel.StatusString = "1,234"

Dim charLabel As New ToolStripStatusLabel()
charLabel.Text = "Characters"
charLabel.StatusString = "5,678"

' Add all items
Me.statusStripEx1.Items.AddRange(New ToolStripItem() {
    pageLabel,
    wordLabel,
    charLabel
})
```

### Complete Word-Style Status Bar Example

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class WordStyleStatusBar
{
    private StatusStripEx statusStripEx1;
    private ToolStripStatusLabel pageLabel;
    private ToolStripStatusLabel wordLabel;
    private ToolStripStatusLabel charLabel;

    public void Initialize(Form form)
    {
        // Create StatusStripEx
        this.statusStripEx1 = new StatusStripEx();
        
        // Apply Office2016Colorful style
        this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful;
        
        // Create page label
        this.pageLabel = new ToolStripStatusLabel();
        this.pageLabel.Text = "Page";
        this.pageLabel.StatusString = "1 of 10";
        this.pageLabel.BorderSides = ToolStripStatusLabelBorderSides.Right;
        
        // Create word count label
        this.wordLabel = new ToolStripStatusLabel();
        this.wordLabel.Text = "Words";
        this.wordLabel.StatusString = "1,234";
        this.wordLabel.BorderSides = ToolStripStatusLabelBorderSides.Right;
        
        // Create character count label
        this.charLabel = new ToolStripStatusLabel();
        this.charLabel.Text = "Characters";
        this.charLabel.StatusString = "5,678";
        
        // Add all items
        this.statusStripEx1.Items.AddRange(new ToolStripItem[]
        {
            this.pageLabel,
            this.wordLabel,
            this.charLabel
        });
        
        // Dock and add to form
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
        form.Controls.Add(this.statusStripEx1);
    }

    public void UpdatePageInfo(int currentPage, int totalPages)
    {
        this.pageLabel.StatusString = $"{currentPage} of {totalPages}";
    }

    public void UpdateWordCount(int wordCount)
    {
        this.wordLabel.StatusString = wordCount.ToString("N0");
    }

    public void UpdateCharacterCount(int charCount)
    {
        this.charLabel.StatusString = charCount.ToString("N0");
    }
}
```

```vb
Imports System
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class WordStyleStatusBar
    Private statusStripEx1 As StatusStripEx
    Private pageLabel As ToolStripStatusLabel
    Private wordLabel As ToolStripStatusLabel
    Private charLabel As ToolStripStatusLabel
    
    Public Sub Initialize(form As Form)
        ' Create StatusStripEx
        Me.statusStripEx1 = New StatusStripEx()
        
        ' Apply Office2016Colorful style
        Me.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful
        
        ' Create page label
        Me.pageLabel = New ToolStripStatusLabel()
        Me.pageLabel.Text = "Page"
        Me.pageLabel.StatusString = "1 of 10"
        Me.pageLabel.BorderSides = ToolStripStatusLabelBorderSides.Right
        
        ' Create word count label
        Me.wordLabel = New ToolStripStatusLabel()
        Me.wordLabel.Text = "Words"
        Me.wordLabel.StatusString = "1,234"
        Me.wordLabel.BorderSides = ToolStripStatusLabelBorderSides.Right
        
        ' Create character count label
        Me.charLabel = New ToolStripStatusLabel()
        Me.charLabel.Text = "Characters"
        Me.charLabel.StatusString = "5,678"
        
        ' Add all items
        Me.statusStripEx1.Items.AddRange(New ToolStripItem() {
            Me.pageLabel,
            Me.wordLabel,
            Me.charLabel
        })
        
        ' Dock and add to form
        Me.statusStripEx1.Dock = DockStyleEx.Bottom
        form.Controls.Add(Me.statusStripEx1)
    End Sub
    
    Public Sub UpdatePageInfo(currentPage As Integer, totalPages As Integer)
        Me.pageLabel.StatusString = $"{currentPage} of {totalPages}"
    End Sub
    
    Public Sub UpdateWordCount(wordCount As Integer)
        Me.wordLabel.StatusString = wordCount.ToString("N0")
    End Sub
    
    Public Sub UpdateCharacterCount(charCount As Integer)
        Me.charLabel.StatusString = charCount.ToString("N0")
    End Sub
End Class
```

## Complete Examples

### Example: Theme Switcher Application

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ThemeSwitcherForm : Form
{
    private StatusStripEx statusStripEx1;
    private ToolStripStatusLabel themeLabel;
    private GroupBox office2007Group;
    private GroupBox office2016Group;
    private GroupBox customGroup;

    public ThemeSwitcherForm()
    {
        InitializeComponent();
        InitializeStatusBar();
        InitializeThemeControls();
    }

    private void InitializeStatusBar()
    {
        this.statusStripEx1 = new StatusStripEx();
        
        this.themeLabel = new ToolStripStatusLabel();
        this.themeLabel.Text = "Current Theme: Office2016Colorful";
        this.themeLabel.Spring = true;
        this.themeLabel.TextAlign = ContentAlignment.MiddleLeft;
        
        this.statusStripEx1.Items.Add(this.themeLabel);
        this.statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful;
        this.statusStripEx1.Dock = DockStyleEx.Bottom;
        
        this.Controls.Add(this.statusStripEx1);
    }

    private void InitializeThemeControls()
    {
        int groupY = 10;
        
        // Office2007 themes
        this.office2007Group = new GroupBox();
        this.office2007Group.Text = "Office2007 Themes";
        this.office2007Group.Location = new Point(10, groupY);
        this.office2007Group.Size = new Size(200, 120);
        
        AddOffice2007Button("Silver", 10, ToolStripEx.ColorScheme.Silver);
        AddOffice2007Button("Blue", 50, ToolStripEx.ColorScheme.Blue);
        AddOffice2007Button("Black", 90, ToolStripEx.ColorScheme.Black);
        
        this.Controls.Add(this.office2007Group);
        
        // Office2016 themes
        this.office2016Group = new GroupBox();
        this.office2016Group.Text = "Office2016 Themes";
        this.office2016Group.Location = new Point(220, groupY);
        this.office2016Group.Size = new Size(200, 160);
        
        AddOffice2016Button("Colorful", 10, StatusStripExStyle.Office2016Colorful);
        AddOffice2016Button("White", 45, StatusStripExStyle.Office2016White);
        AddOffice2016Button("Black", 80, StatusStripExStyle.Office2016Black);
        AddOffice2016Button("DarkGray", 115, StatusStripExStyle.Office2016DarkGray);
        
        this.Controls.Add(this.office2016Group);
        
        // Custom colors
        this.customGroup = new GroupBox();
        this.customGroup.Text = "Custom Colors";
        this.customGroup.Location = new Point(430, groupY);
        this.customGroup.Size = new Size(200, 120);
        
        AddCustomColorButton("Dark Green", 10, Color.DarkGreen);
        AddCustomColorButton("Purple", 50, Color.Purple);
        AddCustomColorButton("Dark Orange", 90, Color.DarkOrange);
        
        this.Controls.Add(this.customGroup);
    }

    private void AddOffice2007Button(string name, int y, ToolStripEx.ColorScheme scheme)
    {
        Button btn = new Button();
        btn.Text = name;
        btn.Location = new Point(10, 20 + y);
        btn.Size = new Size(180, 30);
        btn.Click += (s, e) =>
        {
            this.statusStripEx1.OfficeColorScheme = scheme;
            this.themeLabel.Text = $"Current Theme: Office2007 {name}";
        };
        this.office2007Group.Controls.Add(btn);
    }

    private void AddOffice2016Button(string name, int y, StatusStripExStyle style)
    {
        Button btn = new Button();
        btn.Text = name;
        btn.Location = new Point(10, 20 + y);
        btn.Size = new Size(180, 30);
        btn.Click += (s, e) =>
        {
            this.statusStripEx1.VisualStyle = style;
            this.themeLabel.Text = $"Current Theme: Office2016 {name}";
        };
        this.office2016Group.Controls.Add(btn);
    }

    private void AddCustomColorButton(string name, int y, Color color)
    {
        Button btn = new Button();
        btn.Text = name;
        btn.Location = new Point(10, 20 + y);
        btn.Size = new Size(180, 30);
        btn.Click += (s, e) =>
        {
            this.statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
            Office2007Colors.ApplyManagedColors(this, color);
            this.themeLabel.Text = $"Current Theme: Custom {name}";
        };
        this.customGroup.Controls.Add(btn);
    }
}
```

## Best Practices

1. **Choose one styling approach** - Use either Office2007 schemes OR Office2016 styles, not both simultaneously
2. **Match application theme** - Ensure StatusStripEx style matches your overall application design
3. **Use Office2016 styles for modern apps** - Office2016 styles provide a contemporary look
4. **Test with all themes** - Verify your status items look good with all supported color schemes
5. **Use StatusString property** - Implement Word-like context menus for professional appearance
6. **Apply managed colors carefully** - Custom colors should maintain readability and accessibility
7. **Consider user preferences** - Allow users to switch themes if your application supports it
8. **Update theme consistently** - When changing themes, update all Office-styled controls in the form
9. **Persist theme selection** - Save user's theme preference in application settings
10. **Test accessibility** - Ensure sufficient contrast in all color schemes for readability

## Summary

The StatusStripEx control offers comprehensive styling options:

- **Office2007 Color Schemes** - Silver, Blue, Black (via `OfficeColorScheme` property)
- **Office2016 Visual Styles** - Colorful, White, Black, DarkGray (via `VisualStyle` property)
- **Custom Managed Colors** - Apply any custom color using `ApplyManagedColors` method
- **StatusString Property** - Create Word-like context menus for professional appearance

Choose the styling approach that best matches your application's design requirements and maintain consistency across all Office-styled controls in your application.
