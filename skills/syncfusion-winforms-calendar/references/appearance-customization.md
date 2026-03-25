# Appearance Customization

## Table of Contents
- [Overview](#overview)
- [Visual Styles and Themes](#visual-styles-and-themes)
- [Cell Customization](#cell-customization)
- [Color Customization](#color-customization)
- [Font Customization](#font-customization)
- [Border and Padding](#border-and-padding)
- [Weekend Highlighting](#weekend-highlighting)
- [Current Date Highlighting](#current-date-highlighting)

## When to Read This

Read this guide when you need to:
- Apply visual styles and themes (Office2016, Metro, etc.)
- Customize cell appearance and colors
- Change fonts for different calendar elements
- Customize weekend and today highlighting
- Modify borders, padding, and spacing
- Create custom cell templates
- Match calendar appearance to application branding

## Overview

SfCalendar provides extensive customization options through the `Style` property, allowing you to control colors, fonts, borders, and cell appearance to match your application's design.

## Visual Styles and Themes

### Built-in Visual Styles

**C#:**
```csharp
using Syncfusion.WinForms.Input.Enums;

SfCalendar calendar = new SfCalendar();

// Apply Office 2016 Colorful theme
calendar.Style.VisualStyle = CalendarVisualStyle.Office2016Colorful;

// Other available styles:
// calendar.Style.VisualStyle = CalendarVisualStyle.Office2016Black;
// calendar.Style.VisualStyle = CalendarVisualStyle.Office2016DarkGray;
// calendar.Style.VisualStyle = CalendarVisualStyle.Office2016White;
// calendar.Style.VisualStyle = CalendarVisualStyle.Metro;
```

**VB.NET:**
```vb
Dim calendar As New SfCalendar()

' Apply Office 2016 Colorful theme
calendar.Style.VisualStyle = CalendarVisualStyle.Office2016Colorful
```

### Theme Examples

**C#:**
```csharp
// Office 2016 Colorful (modern, colorful)
calendar.Style.VisualStyle = CalendarVisualStyle.Office2016Colorful;

// Office 2016 White (clean, minimal)
calendar.Style.VisualStyle = CalendarVisualStyle.Office2016White;

// Office 2016 Dark Gray (dark theme)
calendar.Style.VisualStyle = CalendarVisualStyle.Office2016DarkGray;

// Office 2016 Black (high contrast)
calendar.Style.VisualStyle = CalendarVisualStyle.Office2016Black;

// Metro (flat, modern)
calendar.Style.VisualStyle = CalendarVisualStyle.Metro;
```

## Cell Customization

### Basic Cell Appearance

**C#:**
```csharp
// Customize normal cell appearance
calendar.Style.Cell.BackColor = Color.White;
calendar.Style.Cell.ForeColor = Color.Black;
calendar.Style.Cell.Font = new Font("Segoe UI", 9F);

// Cell border
calendar.Style.Cell.BorderColor = Color.LightGray;
```

**VB.NET:**
```vb
' Customize normal cell appearance
calendar.Style.Cell.BackColor = Color.White
calendar.Style.Cell.ForeColor = Color.Black
calendar.Style.Cell.Font = New Font("Segoe UI", 9F)
```

### Selected Cell Appearance

**C#:**
```csharp
// Customize selected cell
calendar.Style.SelectedCell.BackColor = Color.FromArgb(0, 120, 215);  // Blue
calendar.Style.SelectedCell.ForeColor = Color.White;
calendar.Style.SelectedCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
calendar.Style.SelectedCell.BorderColor = Color.FromArgb(0, 90, 158);
```

### Hovered Cell Appearance

**C#:**
```csharp
// Customize hovered cell (mouse over)
calendar.Style.HoverCell.BackColor = Color.FromArgb(229, 243, 255);  // Light blue
calendar.Style.HoverCell.ForeColor = Color.Black;
calendar.Style.HoverCell.BorderColor = Color.FromArgb(0, 120, 215);
```

### Disabled Cell Appearance

**C#:**
```csharp
// Customize disabled/inactive cells (other month days)
calendar.Style.DisabledCell.BackColor = Color.WhiteSmoke;
calendar.Style.DisabledCell.ForeColor = Color.LightGray;
calendar.Style.DisabledCell.Font = new Font("Segoe UI", 9F, FontStyle.Italic);
```

## Color Customization

### Background Colors

**C#:**
```csharp
// Calendar background
calendar.BackColor = Color.White;

// Header background
calendar.Style.Header.BackColor = Color.FromArgb(240, 240, 240);

// Footer background (Today/None buttons area)
calendar.Style.Footer.BackColor = Color.FromArgb(250, 250, 250);
```

**VB.NET:**
```vb
' Calendar background
calendar.BackColor = Color.White

' Header background
calendar.Style.Header.BackColor = Color.FromArgb(240, 240, 240)
```

### Foreground Colors

**C#:**
```csharp
// Header text color
calendar.Style.Header.ForeColor = Color.Black;

// Day names (Mon, Tue, Wed, etc.) color
calendar.Style.DayNames.ForeColor = Color.DarkGray;

// Cell text color
calendar.Style.Cell.ForeColor = Color.Black;
```

### Border Colors

**C#:**
```csharp
// Outer border
calendar.BorderColor = Color.Gray;

// Cell borders
calendar.Style.Cell.BorderColor = Color.LightGray;

// Selected cell border
calendar.Style.SelectedCell.BorderColor = Color.Blue;
```

### Complete Color Scheme Example

**C#:**
```csharp
public void ApplyCustomColorScheme(SfCalendar calendar)
{
    // Define color palette
    Color primaryColor = Color.FromArgb(0, 120, 215);
    Color lightPrimary = Color.FromArgb(229, 243, 255);
    Color darkText = Color.FromArgb(51, 51, 51);
    Color lightText = Color.FromArgb(153, 153, 153);
    
    // Background
    calendar.BackColor = Color.White;
    calendar.Style.Header.BackColor = Color.FromArgb(240, 240, 240);
    calendar.Style.Footer.BackColor = Color.FromArgb(250, 250, 250);
    
    // Normal cells
    calendar.Style.Cell.BackColor = Color.White;
    calendar.Style.Cell.ForeColor = darkText;
    calendar.Style.Cell.BorderColor = Color.FromArgb(230, 230, 230);
    
    // Selected cell
    calendar.Style.SelectedCell.BackColor = primaryColor;
    calendar.Style.SelectedCell.ForeColor = Color.White;
    calendar.Style.SelectedCell.BorderColor = Color.FromArgb(0, 90, 158);
    
    // Hovered cell
    calendar.Style.HoverCell.BackColor = lightPrimary;
    calendar.Style.HoverCell.ForeColor = darkText;
    calendar.Style.HoverCell.BorderColor = primaryColor;
    
    // Disabled cells
    calendar.Style.DisabledCell.BackColor = Color.WhiteSmoke;
    calendar.Style.DisabledCell.ForeColor = lightText;
    
    // Weekend cells
    calendar.Style.WeekendCell.ForeColor = Color.FromArgb(192, 0, 0);  // Dark red
    
    // Today cell
    calendar.Style.TodayCell.BackColor = Color.FromArgb(255, 250, 205);  // Light yellow
    calendar.Style.TodayCell.ForeColor = darkText;
    calendar.Style.TodayCell.BorderColor = Color.FromArgb(255, 215, 0);  // Gold
}
```

## Font Customization

### Header Font

**C#:**
```csharp
// Header font (Month/Year display)
calendar.Style.Header.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
```

### Day Names Font

**C#:**
```csharp
// Day names font (Mon, Tue, Wed, etc.)
calendar.Style.DayNames.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
```

### Cell Fonts

**C#:**
```csharp
// Normal cell font
calendar.Style.Cell.Font = new Font("Segoe UI", 9F);

// Selected cell font
calendar.Style.SelectedCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);

// Today cell font
calendar.Style.TodayCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);

// Weekend cell font
calendar.Style.WeekendCell.Font = new Font("Segoe UI", 9F, FontStyle.Italic);
```

**VB.NET:**
```vb
' Normal cell font
calendar.Style.Cell.Font = New Font("Segoe UI", 9F)

' Selected cell font
calendar.Style.SelectedCell.Font = New Font("Segoe UI", 9F, FontStyle.Bold)
```

### Font Family Examples

**C#:**
```csharp
// Modern fonts
calendar.Style.Cell.Font = new Font("Segoe UI", 9F);
calendar.Style.Cell.Font = new Font("Calibri", 9F);
calendar.Style.Cell.Font = new Font("Arial", 9F);

// Monospace fonts (for alignment)
calendar.Style.Cell.Font = new Font("Consolas", 9F);
calendar.Style.Cell.Font = new Font("Courier New", 9F);
```

## Border and Padding

### Border Styles

**C#:**
```csharp
// Outer border
calendar.BorderStyle = BorderStyle.FixedSingle;  // Or Fixed3D, None

// Border color
calendar.BorderColor = Color.Gray;
```

### Cell Padding

**C#:**
```csharp
// Cell padding (space inside cells)
calendar.Style.Cell.CellPadding = new Padding(5);

// Different padding for each side
calendar.Style.Cell.CellPadding = new Padding(5, 3, 5, 3);  // Left, Top, Right, Bottom
```

**VB.NET:**
```vb
' Cell padding
calendar.Style.Cell.CellPadding = New Padding(5)
```

### Cell Spacing

**C#:**
```csharp
// Space between cells
calendar.Style.CellSpacing = 2;
```

## Weekend Highlighting

### Customize Weekend Appearance

**C#:**
```csharp
// Weekend cell colors
calendar.Style.WeekendCell.ForeColor = Color.Red;
calendar.Style.WeekendCell.BackColor = Color.FromArgb(255, 240, 240);  // Light pink
calendar.Style.WeekendCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
```

**VB.NET:**
```vb
' Weekend cell colors
calendar.Style.WeekendCell.ForeColor = Color.Red
calendar.Style.WeekendCell.BackColor = Color.FromArgb(255, 240, 240)
```

### Weekend Example

**C#:**
```csharp
public void HighlightWeekends(SfCalendar calendar)
{
    // Red text for weekends
    calendar.Style.WeekendCell.ForeColor = Color.FromArgb(192, 0, 0);
    
    // Light red background
    calendar.Style.WeekendCell.BackColor = Color.FromArgb(255, 245, 245);
    
    // Bold font
    calendar.Style.WeekendCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
    
    // Distinct border
    calendar.Style.WeekendCell.BorderColor = Color.FromArgb(255, 200, 200);
}
```

## Current Date Highlighting

### Customize Today Cell

**C#:**
```csharp
// Today cell appearance
calendar.Style.TodayCell.BackColor = Color.FromArgb(255, 250, 205);  // Light yellow
calendar.Style.TodayCell.ForeColor = Color.Black;
calendar.Style.TodayCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
calendar.Style.TodayCell.BorderColor = Color.FromArgb(255, 215, 0);  // Gold
```

**VB.NET:**
```vb
' Today cell appearance
calendar.Style.TodayCell.BackColor = Color.FromArgb(255, 250, 205)
calendar.Style.TodayCell.ForeColor = Color.Black
calendar.Style.TodayCell.Font = New Font("Segoe UI", 9F, FontStyle.Bold)
```

### Today Cell Example

**C#:**
```csharp
public void HighlightToday(SfCalendar calendar)
{
    // Bright background
    calendar.Style.TodayCell.BackColor = Color.FromArgb(255, 255, 200);
    
    // Dark text
    calendar.Style.TodayCell.ForeColor = Color.Black;
    
    // Bold font
    calendar.Style.TodayCell.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    
    // Prominent border
    calendar.Style.TodayCell.BorderColor = Color.Orange;
}
```

## Complete Appearance Customization Example

**C#:**
```csharp
using Syncfusion.WinForms.Input;
using System;
using System.Drawing;
using System.Windows.Forms;

public class AppearanceCustomizationForm : Form
{
    private SfCalendar calendar;
    private GroupBox grpThemes;
    private RadioButton rbOffice2016;
    private RadioButton rbMetro;
    private RadioButton rbCustom;
    private GroupBox grpColors;
    private Button btnPrimaryColor;
    private Button btnBackColor;
    
    private Color primaryColor = Color.FromArgb(0, 120, 215);
    
    public AppearanceCustomizationForm()
    {
        SetupCalendar();
        SetupThemeControls();
        SetupColorControls();
        
        this.Text = "Appearance Customization";
        this.Size = new Size(620, 520);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
    
    private void SetupCalendar()
    {
        calendar = new SfCalendar
        {
            Location = new Point(20, 20),
            Size = new Size(350, 360),
            ShowToday = true,
            SelectedDate = DateTime.Today
        };
        
        this.Controls.Add(calendar);
    }
    
    private void SetupThemeControls()
    {
        grpThemes = new GroupBox
        {
            Location = new Point(390, 20),
            Size = new Size(200, 120),
            Text = "Visual Style"
        };
        
        rbOffice2016 = new RadioButton
        {
            Location = new Point(15, 25),
            Size = new Size(170, 25),
            Text = "Office 2016 Colorful",
            Checked = true
        };
        rbOffice2016.CheckedChanged += (s, e) =>
        {
            if (rbOffice2016.Checked)
                calendar.Style.VisualStyle = CalendarVisualStyle.Office2016Colorful;
        };
        
        rbMetro = new RadioButton
        {
            Location = new Point(15, 55),
            Size = new Size(170, 25),
            Text = "Metro"
        };
        rbMetro.CheckedChanged += (s, e) =>
        {
            if (rbMetro.Checked)
                calendar.Style.VisualStyle = CalendarVisualStyle.Metro;
        };
        
        rbCustom = new RadioButton
        {
            Location = new Point(15, 85),
            Size = new Size(170, 25),
            Text = "Custom"
        };
        rbCustom.CheckedChanged += (s, e) =>
        {
            if (rbCustom.Checked) ApplyCustomStyle();
        };
        
        grpThemes.Controls.AddRange(new Control[] {
            rbOffice2016, rbMetro, rbCustom
        });
        
        this.Controls.Add(grpThemes);
    }
    
    private void SetupColorControls()
    {
        grpColors = new GroupBox
        {
            Location = new Point(390, 150),
            Size = new Size(200, 100),
            Text = "Custom Colors"
        };
        
        btnPrimaryColor = new Button
        {
            Location = new Point(15, 30),
            Size = new Size(170, 30),
            Text = "Primary Color",
            BackColor = primaryColor,
            ForeColor = Color.White
        };
        btnPrimaryColor.Click += BtnPrimaryColor_Click;
        
        btnBackColor = new Button
        {
            Location = new Point(15, 65),
            Size = new Size(170, 30),
            Text = "Background Color"
        };
        btnBackColor.Click += BtnBackColor_Click;
        
        grpColors.Controls.AddRange(new Control[] {
            btnPrimaryColor, btnBackColor
        });
        
        this.Controls.Add(grpColors);
    }
    
    private void ApplyCustomStyle()
    {
        // Reset to default first
        calendar.ResetStyle();
        
        // Apply custom colors
        calendar.BackColor = Color.White;
        
        // Header
        calendar.Style.Header.BackColor = Color.FromArgb(240, 240, 240);
        calendar.Style.Header.ForeColor = Color.Black;
        calendar.Style.Header.Font = new Font("Segoe UI", 11F, FontStyle.Bold);
        
        // Normal cells
        calendar.Style.Cell.BackColor = Color.White;
        calendar.Style.Cell.ForeColor = Color.Black;
        calendar.Style.Cell.Font = new Font("Segoe UI", 9F);
        calendar.Style.Cell.BorderColor = Color.FromArgb(230, 230, 230);
        
        // Selected cell
        calendar.Style.SelectedCell.BackColor = primaryColor;
        calendar.Style.SelectedCell.ForeColor = Color.White;
        calendar.Style.SelectedCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
        
        // Hover cell
        calendar.Style.HoverCell.BackColor = Color.FromArgb(229, 243, 255);
        calendar.Style.HoverCell.ForeColor = Color.Black;
        calendar.Style.HoverCell.BorderColor = primaryColor;
        
        // Weekend cells
        calendar.Style.WeekendCell.ForeColor = Color.FromArgb(192, 0, 0);
        calendar.Style.WeekendCell.BackColor = Color.FromArgb(255, 245, 245);
        
        // Today cell
        calendar.Style.TodayCell.BackColor = Color.FromArgb(255, 250, 205);
        calendar.Style.TodayCell.ForeColor = Color.Black;
        calendar.Style.TodayCell.BorderColor = Color.Orange;
        calendar.Style.TodayCell.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
        
        // Disabled cells
        calendar.Style.DisabledCell.ForeColor = Color.LightGray;
    }
    
    private void BtnPrimaryColor_Click(object sender, EventArgs e)
    {
        using (ColorDialog dialog = new ColorDialog())
        {
            dialog.Color = primaryColor;
            if (dialog.ShowDialog() == DialogResult.OK)
            {
                primaryColor = dialog.Color;
                btnPrimaryColor.BackColor = primaryColor;
                
                if (rbCustom.Checked)
                {
                    ApplyCustomStyle();
                }
            }
        }
    }
    
    private void BtnBackColor_Click(object sender, EventArgs e)
    {
        using (ColorDialog dialog = new ColorDialog())
        {
            dialog.Color = calendar.BackColor;
            if (dialog.ShowDialog() == DialogResult.OK)
            {
                calendar.BackColor = dialog.Color;
            }
        }
    }
}
```

## Next Steps

- **[Globalization](globalization.md)** - Localize calendar for different cultures and languages
- **[Date Selection](date-selection.md)** - Configure selection modes and date restrictions
- **[Views and Navigation](views-and-navigation.md)** - Work with different calendar views
