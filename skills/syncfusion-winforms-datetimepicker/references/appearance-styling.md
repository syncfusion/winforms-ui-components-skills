# Appearance and Styling

Customize the visual appearance of the SfDateTimeEdit control including themes, colors, fonts, borders, buttons, and drop-down calendar styling.

## Table of Contents

- [Style Property Overview](#style-property-overview)
- [Basic Appearance Customization](#basic-appearance-customization)
- [Border Styling](#border-styling)
- [Drop-Down Button Customization](#drop-down-button-customization)
- [Up-Down Button Customization](#up-down-button-customization)
- [Calendar Customization](#calendar-customization)
- [Size and Layout](#size-and-layout)
- [Built-in Themes](#built-in-themes)
- [Complete Examples](#complete-examples)
- [Next Steps](#next-steps)

## Style Property Overview

The `Style` property provides access to all appearance-related settings. Use it to customize colors, fonts, borders, and button styles.

**C#:**
```csharp
// Access style properties
dateTimeEdit.Style.BackColor = Color.White;
dateTimeEdit.Style.ForeColor = Color.Black;
dateTimeEdit.Style.BorderColor = Color.Gray;
```

**VB.NET:**
```vb
' Access style properties
dateTimeEdit.Style.BackColor = Color.White
dateTimeEdit.Style.ForeColor = Color.Black
dateTimeEdit.Style.BorderColor = Color.Gray
```

## Basic Appearance Customization

### Background and Foreground Colors

**C#:**
```csharp
// Set background color
dateTimeEdit.Style.BackColor = Color.LightYellow;

// Set text color
dateTimeEdit.Style.ForeColor = Color.DarkBlue;

// Set watermark color (for null values)
dateTimeEdit.Style.WatermarkForeColor = Color.Gray;
```

**VB.NET:**
```vb
' Set background color
dateTimeEdit.Style.BackColor = Color.LightYellow

' Set text color
dateTimeEdit.Style.ForeColor = Color.DarkBlue

' Set watermark color (for null values)
dateTimeEdit.Style.WatermarkForeColor = Color.Gray
```

### Font Customization

**C#:**
```csharp
// Set font
dateTimeEdit.Font = new Font("Segoe UI", 12, FontStyle.Regular);

// Or use specific font properties
dateTimeEdit.Font = new Font("Arial", 10, FontStyle.Bold);
```

**VB.NET:**
```vb
' Set font
dateTimeEdit.Font = New Font("Segoe UI", 12, FontStyle.Regular)

' Or use specific font properties
dateTimeEdit.Font = New Font("Arial", 10, FontStyle.Bold)
```

### Disabled State Appearance

**C#:**
```csharp
// Customize disabled state colors
dateTimeEdit.Style.DisabledBackColor = Color.LightGray;
dateTimeEdit.Style.DisabledForeColor = Color.DarkGray;

// Disable the control
dateTimeEdit.Enabled = false;
```

**VB.NET:**
```vb
' Customize disabled state colors
dateTimeEdit.Style.DisabledBackColor = Color.LightGray
dateTimeEdit.Style.DisabledForeColor = Color.DarkGray

' Disable the control
dateTimeEdit.Enabled = False
```

## Border Styling

### Border Colors for Different States

**C#:**
```csharp
// Normal state border
dateTimeEdit.Style.BorderColor = Color.Gray;

// Focused state border
dateTimeEdit.Style.FocusedBorderColor = Color.Blue;

// Hover state border
dateTimeEdit.Style.HoverBorderColor = Color.DodgerBlue;
```

**VB.NET:**
```vb
' Normal state border
dateTimeEdit.Style.BorderColor = Color.Gray

' Focused state border
dateTimeEdit.Style.FocusedBorderColor = Color.Blue

' Hover state border
dateTimeEdit.Style.HoverBorderColor = Color.DodgerBlue
```

### Complete Border Customization Example

**C#:**
```csharp
SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();
dateTimeEdit.Location = new System.Drawing.Point(50, 50);
dateTimeEdit.Size = new System.Drawing.Size(250, 30);

// Border customization
dateTimeEdit.Style.BorderColor = Color.FromArgb(200, 200, 200);
dateTimeEdit.Style.FocusedBorderColor = Color.FromArgb(0, 120, 215);
dateTimeEdit.Style.HoverBorderColor = Color.FromArgb(100, 180, 230);

this.Controls.Add(dateTimeEdit);
```

**VB.NET:**
```vb
Dim dateTimeEdit As New SfDateTimeEdit()
dateTimeEdit.Location = New System.Drawing.Point(50, 50)
dateTimeEdit.Size = New System.Drawing.Size(250, 30)

' Border customization
dateTimeEdit.Style.BorderColor = Color.FromArgb(200, 200, 200)
dateTimeEdit.Style.FocusedBorderColor = Color.FromArgb(0, 120, 215)
dateTimeEdit.Style.HoverBorderColor = Color.FromArgb(100, 180, 230)

Me.Controls.Add(dateTimeEdit)
```

## Drop-Down Button Customization

### Show or Hide Drop-Down Button

**C#:**
```csharp
// Show drop-down button (default)
dateTimeEdit.ShowDropDown = true;

// Hide drop-down button
dateTimeEdit.ShowDropDown = false;
```

**VB.NET:**
```vb
' Show drop-down button (default)
dateTimeEdit.ShowDropDown = True

' Hide drop-down button
dateTimeEdit.ShowDropDown = False
```

### Drop-Down Button Colors

**C#:**
```csharp
// Normal state
dateTimeEdit.Style.DropDown.BackColor = Color.White;
dateTimeEdit.Style.DropDown.ForeColor = Color.Black;

// Hover state
dateTimeEdit.Style.DropDown.HoverBackColor = Color.LightBlue;
dateTimeEdit.Style.DropDown.HoverForeColor = Color.DarkBlue;

// Pressed state
dateTimeEdit.Style.DropDown.PressedBackColor = Color.Blue;
dateTimeEdit.Style.DropDown.PressedForeColor = Color.White;
```

**VB.NET:**
```vb
' Normal state
dateTimeEdit.Style.DropDown.BackColor = Color.White
dateTimeEdit.Style.DropDown.ForeColor = Color.Black

' Hover state
dateTimeEdit.Style.DropDown.HoverBackColor = Color.LightBlue
dateTimeEdit.Style.DropDown.HoverForeColor = Color.DarkBlue

' Pressed state
dateTimeEdit.Style.DropDown.PressedBackColor = Color.Blue
dateTimeEdit.Style.DropDown.PressedForeColor = Color.White
```

### Custom Calendar Icon

**C#:**
```csharp
// Set custom calendar icon (recommended size: 16x16 pixels)
dateTimeEdit.DateTimeIcon = Image.FromFile(@"path\to\calendar_icon.png");

// Or use embedded resource
dateTimeEdit.DateTimeIcon = Properties.Resources.CustomCalendarIcon;
```

**VB.NET:**
```vb
' Set custom calendar icon (recommended size: 16x16 pixels)
dateTimeEdit.DateTimeIcon = Image.FromFile("path\to\calendar_icon.png")

' Or use embedded resource
dateTimeEdit.DateTimeIcon = My.Resources.CustomCalendarIcon
```

### Drop-Down Alignment

**C#:**
```csharp
// Align drop-down to the left
dateTimeEdit.DropDownPopupAlignment = DropDownPopupAlignment.Left;

// Align drop-down to the right
dateTimeEdit.DropDownPopupAlignment = DropDownPopupAlignment.Right;
```

**VB.NET:**
```vb
' Align drop-down to the left
dateTimeEdit.DropDownPopupAlignment = DropDownPopupAlignment.Left

' Align drop-down to the right
dateTimeEdit.DropDownPopupAlignment = DropDownPopupAlignment.Right
```

## Up-Down Button Customization

### Show or Hide Up-Down Buttons

**C#:**
```csharp
// Enable mask mode (required for up-down buttons)
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;

// Show up-down buttons
dateTimeEdit.ShowUpDown = true;

// Hide up-down buttons
dateTimeEdit.ShowUpDown = false;
```

**VB.NET:**
```vb
' Enable mask mode (required for up-down buttons)
dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask

' Show up-down buttons
dateTimeEdit.ShowUpDown = True

' Hide up-down buttons
dateTimeEdit.ShowUpDown = False
```

### Up-Down Button Colors

**C#:**
```csharp
// Normal state
dateTimeEdit.Style.UpDownBackColor = Color.White;
dateTimeEdit.Style.UpDownForeColor = Color.Black;

// Hover state
dateTimeEdit.Style.UpDownHoverBackColor = Color.LightGray;
dateTimeEdit.Style.UpDownHoverForeColor = Color.Blue;
```

**VB.NET:**
```vb
' Normal state
dateTimeEdit.Style.UpDownBackColor = Color.White
dateTimeEdit.Style.UpDownForeColor = Color.Black

' Hover state
dateTimeEdit.Style.UpDownHoverBackColor = Color.LightGray
dateTimeEdit.Style.UpDownHoverForeColor = Color.Blue
```

## Calendar Customization

### Drop-Down Calendar Size

**C#:**
```csharp
// Set custom drop-down calendar size
dateTimeEdit.DropDownSize = new Size(300, 280);

// Adjust control width to match
dateTimeEdit.Width = 300;
```

**VB.NET:**
```vb
' Set custom drop-down calendar size
dateTimeEdit.DropDownSize = New Size(300, 280)

' Adjust control width to match
dateTimeEdit.Width = 300
```

### Calendar Properties

**C#:**
```csharp
// Access calendar through MonthCalendar property
var calendar = dateTimeEdit.MonthCalendar;

// Hide footer (Today button)
calendar.ShowFooter = false;

// Show week numbers
calendar.ShowWeekNumbers = true;

// Customize calendar colors
calendar.Style.HeaderBackColor = Color.DarkBlue;
calendar.Style.HeaderForeColor = Color.White;
```

**VB.NET:**
```vb
' Access calendar through MonthCalendar property
Dim calendar = dateTimeEdit.MonthCalendar

' Hide footer (Today button)
calendar.ShowFooter = False

' Show week numbers
calendar.ShowWeekNumbers = True

' Customize calendar colors
calendar.Style.HeaderBackColor = Color.DarkBlue
calendar.Style.HeaderForeColor = Color.White
```

## Size and Layout

### Control Size

**C#:**
```csharp
// Set specific size
dateTimeEdit.Size = new Size(250, 30);

// Or set dimensions separately
dateTimeEdit.Width = 250;
dateTimeEdit.Height = 30;
```

**VB.NET:**
```vb
' Set specific size
dateTimeEdit.Size = New Size(250, 30)

' Or set dimensions separately
dateTimeEdit.Width = 250
dateTimeEdit.Height = 30
```

### Layout Properties

**C#:**
```csharp
// Set location
dateTimeEdit.Location = new Point(50, 50);

// Set anchor for responsive layout
dateTimeEdit.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;

// Set dock for full container fill
dateTimeEdit.Dock = DockStyle.Top;
```

**VB.NET:**
```vb
' Set location
dateTimeEdit.Location = New Point(50, 50)

' Set anchor for responsive layout
dateTimeEdit.Anchor = AnchorStyles.Top Or AnchorStyles.Left Or AnchorStyles.Right

' Set dock for full container fill
dateTimeEdit.Dock = DockStyle.Top
```

### ToolTip Support

**C#:**
```csharp
// Set tooltip text
dateTimeEdit.ToolTipText = "Select a date from the calendar or type directly";
```

**VB.NET:**
```vb
' Set tooltip text
dateTimeEdit.ToolTipText = "Select a date from the calendar or type directly"
```

## Built-in Themes

The SfDateTimeEdit supports four Office 2016 themes: Colorful, White, DarkGray, and Black.

### Loading Theme Assembly

First, load the theme assembly in your Program.cs (or Main):

**C#:**
```csharp
using Syncfusion.WinForms.Controls;

static class Program
{
    static void Main()
    {
        // Load theme assembly
        SfSkinManager.LoadAssembly(typeof(Office2016Theme).Assembly);
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.WinForms.Controls

Module Program
    Sub Main()
        ' Load theme assembly
        SfSkinManager.LoadAssembly(GetType(Office2016Theme).Assembly)
        
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Module
```

### Office2016Colorful Theme

**C#:**
```csharp
dateTimeEdit.ThemeName = "Office2016Colorful";
```

**VB.NET:**
```vb
dateTimeEdit.ThemeName = "Office2016Colorful"
```

### Office2016White Theme

**C#:**
```csharp
dateTimeEdit.ThemeName = "Office2016White";
```

**VB.NET:**
```vb
dateTimeEdit.ThemeName = "Office2016White"
```

### Office2016DarkGray Theme

**C#:**
```csharp
dateTimeEdit.ThemeName = "Office2016DarkGray";
```

**VB.NET:**
```vb
dateTimeEdit.ThemeName = "Office2016DarkGray"
```

### Office2016Black Theme

**C#:**
```csharp
dateTimeEdit.ThemeName = "Office2016Black";
```

**VB.NET:**
```vb
dateTimeEdit.ThemeName = "Office2016Black"
```

## Complete Examples

### Example 1: Custom Color Scheme

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class CustomColorForm : Form
{
    public CustomColorForm()
    {
        this.Text = "Custom Color Scheme";
        this.Size = new Size(400, 150);
        this.BackColor = Color.FromArgb(240, 240, 245);
        
        SfDateTimeEdit dateTimeEdit = new SfDateTimeEdit();
        dateTimeEdit.Location = new Point(50, 50);
        dateTimeEdit.Size = new Size(280, 35);
        dateTimeEdit.Value = DateTime.Now;
        
        // Custom color scheme
        dateTimeEdit.Style.BackColor = Color.White;
        dateTimeEdit.Style.ForeColor = Color.FromArgb(50, 50, 50);
        dateTimeEdit.Style.BorderColor = Color.FromArgb(180, 180, 190);
        dateTimeEdit.Style.FocusedBorderColor = Color.FromArgb(0, 120, 215);
        dateTimeEdit.Style.HoverBorderColor = Color.FromArgb(100, 160, 220);
        
        // Drop-down button colors
        dateTimeEdit.Style.DropDown.ForeColor = Color.FromArgb(100, 100, 100);
        dateTimeEdit.Style.DropDown.HoverBackColor = Color.FromArgb(230, 240, 250);
        dateTimeEdit.Style.DropDown.HoverForeColor = Color.FromArgb(0, 120, 215);
        dateTimeEdit.Style.DropDown.PressedBackColor = Color.FromArgb(200, 220, 240);
        dateTimeEdit.Style.DropDown.PressedForeColor = Color.FromArgb(0, 90, 180);
        
        // Font
        dateTimeEdit.Font = new Font("Segoe UI", 11);
        
        this.Controls.Add(dateTimeEdit);
    }
}
```

### Example 2: Theme Selector

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class ThemeSelectorForm : Form
{
    private SfDateTimeEdit dateTimeEdit;
    private ComboBox cmbTheme;
    
    public ThemeSelectorForm()
    {
        this.Text = "Theme Selector";
        this.Size = new Size(450, 200);
        
        // Theme selector
        Label lblTheme = new Label();
        lblTheme.Text = "Select Theme:";
        lblTheme.Location = new Point(20, 20);
        this.Controls.Add(lblTheme);
        
        cmbTheme = new ComboBox();
        cmbTheme.Location = new Point(120, 20);
        cmbTheme.Size = new Size(280, 25);
        cmbTheme.DropDownStyle = ComboBoxStyle.DropDownList;
        cmbTheme.Items.AddRange(new object[]
        {
            "Default (No Theme)",
            "Office2016Colorful",
            "Office2016White",
            "Office2016DarkGray",
            "Office2016Black"
        });
        cmbTheme.SelectedIndex = 0;
        cmbTheme.SelectedIndexChanged += Theme_Changed;
        this.Controls.Add(cmbTheme);
        
        // DateTime control
        Label lblEdit = new Label();
        lblEdit.Text = "Date/Time:";
        lblEdit.Location = new Point(20, 60);
        this.Controls.Add(lblEdit);
        
        dateTimeEdit = new SfDateTimeEdit();
        dateTimeEdit.Location = new Point(120, 60);
        dateTimeEdit.Size = new Size(280, 30);
        dateTimeEdit.Value = DateTime.Now;
        dateTimeEdit.DateTimePattern = DateTimePattern.LongDate;
        this.Controls.Add(dateTimeEdit);
    }
    
    private void Theme_Changed(object sender, EventArgs e)
    {
        string theme = cmbTheme.SelectedItem.ToString();
        
        if (theme == "Default (No Theme)")
        {
            dateTimeEdit.ThemeName = string.Empty;
        }
        else
        {
            dateTimeEdit.ThemeName = theme;
        }
    }
}
```

### Example 3: Comprehensive Styling

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class ComprehensiveStylingForm : Form
{
    private SfDateTimeEdit dateTimeEdit;
    
    public ComprehensiveStylingForm()
    {
        this.Text = "Comprehensive Styling Example";
        this.Size = new Size(500, 300);
        this.BackColor = Color.FromArgb(45, 45, 48);
        
        Label lblTitle = new Label();
        lblTitle.Text = "Styled DateTime Editor";
        lblTitle.Location = new Point(20, 20);
        lblTitle.Size = new Size(400, 30);
        lblTitle.Font = new Font("Segoe UI", 14, FontStyle.Bold);
        lblTitle.ForeColor = Color.White;
        this.Controls.Add(lblTitle);
        
        // DateTime edit with comprehensive styling
        dateTimeEdit = new SfDateTimeEdit();
        dateTimeEdit.Location = new Point(20, 70);
        dateTimeEdit.Size = new Size(420, 40);
        dateTimeEdit.Value = DateTime.Now;
        dateTimeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
        dateTimeEdit.DateTimePattern = DateTimePattern.FullDateTime;
        dateTimeEdit.ShowUpDown = true;
        
        // Control colors
        dateTimeEdit.Style.BackColor = Color.FromArgb(30, 30, 30);
        dateTimeEdit.Style.ForeColor = Color.White;
        dateTimeEdit.Style.BorderColor = Color.FromArgb(100, 100, 100);
        dateTimeEdit.Style.FocusedBorderColor = Color.FromArgb(0, 122, 204);
        dateTimeEdit.Style.HoverBorderColor = Color.FromArgb(50, 150, 220);
        
        // Drop-down button
        dateTimeEdit.Style.DropDown.BackColor = Color.FromArgb(40, 40, 40);
        dateTimeEdit.Style.DropDown.ForeColor = Color.FromArgb(200, 200, 200);
        dateTimeEdit.Style.DropDown.HoverBackColor = Color.FromArgb(60, 60, 60);
        dateTimeEdit.Style.DropDown.HoverForeColor = Color.White;
        dateTimeEdit.Style.DropDown.PressedBackColor = Color.FromArgb(0, 122, 204);
        dateTimeEdit.Style.DropDown.PressedForeColor = Color.White;
        
        // Up-down buttons
        dateTimeEdit.Style.UpDownBackColor = Color.FromArgb(40, 40, 40);
        dateTimeEdit.Style.UpDownForeColor = Color.FromArgb(200, 200, 200);
        dateTimeEdit.Style.UpDownHoverBackColor = Color.FromArgb(60, 60, 60);
        dateTimeEdit.Style.UpDownHoverForeColor = Color.White;
        
        // Font
        dateTimeEdit.Font = new Font("Consolas", 12);
        
        // Calendar customization
        dateTimeEdit.MonthCalendar.Style.HeaderBackColor = Color.FromArgb(0, 122, 204);
        dateTimeEdit.MonthCalendar.Style.HeaderForeColor = Color.White;
        dateTimeEdit.MonthCalendar.ShowWeekNumbers = true;
        
        // Tooltip
        dateTimeEdit.ToolTipText = "Use arrow keys to navigate, up/down to change values";
        
        this.Controls.Add(dateTimeEdit);
        
        // Information label
        Label lblInfo = new Label();
        lblInfo.Text = "Features:\n" +
                      "• Dark theme with custom colors\n" +
                      "• Mask editing mode with up-down buttons\n" +
                      "• Hover and focus effects\n" +
                      "• Customized calendar with week numbers";
        lblInfo.Location = new Point(20, 130);
        lblInfo.Size = new Size(420, 120);
        lblInfo.ForeColor = Color.LightGray;
        lblInfo.Font = new Font("Segoe UI", 10);
        this.Controls.Add(lblInfo);
    }
}
```

### Example 4: Compact vs Expanded Styles

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Input;

public class SizeComparisonForm : Form
{
    public SizeComparisonForm()
    {
        this.Text = "Size Comparison";
        this.Size = new Size(450, 250);
        
        // Compact style
        Label lblCompact = new Label();
        lblCompact.Text = "Compact Style:";
        lblCompact.Location = new Point(20, 20);
        this.Controls.Add(lblCompact);
        
        SfDateTimeEdit compactEdit = new SfDateTimeEdit();
        compactEdit.Location = new Point(130, 20);
        compactEdit.Size = new Size(250, 25);
        compactEdit.Font = new Font("Segoe UI", 9);
        compactEdit.Value = DateTime.Today;
        this.Controls.Add(compactEdit);
        
        // Regular style
        Label lblRegular = new Label();
        lblRegular.Text = "Regular Style:";
        lblRegular.Location = new Point(20, 70);
        this.Controls.Add(lblRegular);
        
        SfDateTimeEdit regularEdit = new SfDateTimeEdit();
        regularEdit.Location = new Point(130, 70);
        regularEdit.Size = new Size(280, 30);
        regularEdit.Font = new Font("Segoe UI", 10);
        regularEdit.Value = DateTime.Today;
        this.Controls.Add(regularEdit);
        
        // Large style
        Label lblLarge = new Label();
        lblLarge.Text = "Large Style:";
        lblLarge.Location = new Point(20, 120);
        this.Controls.Add(lblLarge);
        
        SfDateTimeEdit largeEdit = new SfDateTimeEdit();
        largeEdit.Location = new Point(130, 120);
        largeEdit.Size = new Size(300, 40);
        largeEdit.Font = new Font("Segoe UI", 12, FontStyle.Bold);
        largeEdit.Value = DateTime.Today;
        largeEdit.DateTimeEditingMode = DateTimeEditingMode.Mask;
        largeEdit.ShowUpDown = true;
        this.Controls.Add(largeEdit);
    }
}
```

## Next Steps

- **[Getting Started](getting-started.md)** - Basic setup and usage
- **[Date Range and Value](date-range-value.md)** - Manage date-time values
- **[Display Patterns](display-patterns.md)** - Format date-time display
- **[Editing Modes](editing-modes.md)** - Configure editing behavior
- **[Validation Features](validation-features.md)** - Implement validation and globalization
