# Appearance and Customization

This guide covers visual customization options for the Pivot Chart control.

## Table of Contents
- [Overview](#overview)
- [Chart Area Customization](#chart-area-customization)
- [Series Customization](#series-customization)
- [Color Palette](#color-palette)
- [Themes](#themes)
- [Print Support](#print-support)

## Overview

The Pivot Chart control provides extensive customization options for colors, fonts, borders, and overall appearance.

## Chart Area Customization

### Background and Borders

```csharp
// Chart area background
pivotChart1.BackColor = Color.White;
pivotChart1.ChartArea.BackColor = Color.WhiteSmoke;

// Border customization
pivotChart1.ChartArea.Border.Color = Color.Gray;
pivotChart1.ChartArea.BorderWidth = 1;
```

### Title Customization

```csharp
// Chart title
pivotChart1.Title.Text = "Sales Analysis by Product";
pivotChart1.Title.Font = new Font("Arial", 14, FontStyle.Bold);
pivotChart1.Title.ForeColor = Color.DarkBlue;
pivotChart1.Title.Alignment = StringAlignment.Center;
```

### Axis Customization

```csharp
// X-Axis customization
pivotChart1.PrimaryXAxis.Title = "Products";
pivotChart1.PrimaryXAxis.TitleFont = new Font("Arial", 10);
pivotChart1.PrimaryXAxis.LabelFont = new Font("Arial", 9);

// Y-Axis customization
pivotChart1.PrimaryYAxis.Title = "Revenue ($)";
pivotChart1.PrimaryYAxis.TitleFont = new Font("Arial", 10);
pivotChart1.PrimaryYAxis.LabelFont = new Font("Arial", 9);
pivotChart1.PrimaryYAxis.FormatLabel += (s, args) =>
{
    args.Label = "$" + args.Label;  // Add currency symbol
};
```

## Series Customization

### Individual Series Styling

```csharp
// Access and customize series
foreach (ChartSeries series in pivotChart1.Series)
{
    series.Style.Border.Width = 2;
    series.Style.DisplayText = true;
    series.Style.Font.Size = 8;
}
```

### Data Labels

```csharp
// Configure data labels
pivotChart1.ShowDataLabels = true;
pivotChart1.DataLabels.Font = new Font("Arial", 8);
pivotChart1.DataLabels.ForeColor = Color.Black;
pivotChart1.DataLabels.Format = "{0:C0}";  // Currency format
```

## Color Palette

### Setting Custom Colors

```csharp
// Define custom color palette
Color[] customPalette = new Color[]
{
    Color.FromArgb(0, 120, 215),   // Blue
    Color.FromArgb(232, 17, 35),   // Red
    Color.FromArgb(16, 124, 16),   // Green
    Color.FromArgb(247, 99, 12),   // Orange
    Color.FromArgb(118, 118, 118)  // Gray
};

// Apply palette
pivotChart1.Palette = ChartColorPalette.Custom;
pivotChart1.CustomPalette = customPalette;
```

### Predefined Palettes

```csharp
// Use built-in color palettes
pivotChart1.Palette = ChartColorPalette.Metro;       // Metro style
pivotChart1.Palette = ChartColorPalette.Nature;      // Nature colors
pivotChart1.Palette = ChartColorPalette.EarthTones;  // Earth tones
pivotChart1.Palette = ChartColorPalette.Pastel;      // Pastel colors
```

### Complete Color Customization Example

```csharp
using System.Drawing;

private void SetColorPalette()
{
    // Corporate color scheme
    Color[] corporateColors = new Color[]
    {
        Color.FromArgb(0, 114, 198),    // Corporate Blue
        Color.FromArgb(255, 127, 14),   // Corporate Orange
        Color.FromArgb(44, 160, 44),    // Corporate Green
        Color.FromArgb(214, 39, 40),    // Corporate Red
        Color.FromArgb(148, 103, 189),  // Corporate Purple
        Color.FromArgb(140, 86, 75),    // Corporate Brown
        Color.FromArgb(227, 119, 194)   // Corporate Pink
    };
    
    pivotChart1.Palette = ChartColorPalette.Custom;
    pivotChart1.CustomPalette = corporateColors;
}
```

## Themes

### Applying Office Themes

```csharp
// Apply Office 2016 theme
pivotChart1.ChartTheme = ChartTheme.Office2016Colorful;

// Other theme options
pivotChart1.ChartTheme = ChartTheme.Office2016White;
pivotChart1.ChartTheme = ChartTheme.Office2016DarkGray;
pivotChart1.ChartTheme = ChartTheme.Office2016Black;
```

### Custom Theme

```csharp
private void ApplyCustomTheme()
{
    // Background
    pivotChart1.BackColor = Color.FromArgb(245, 245, 245);
    pivotChart1.ChartArea.BackColor = Color.White;
    
    // Grid lines
    pivotChart1.PrimaryXAxis.GridLineType.ForeColor = Color.LightGray;
    pivotChart1.PrimaryYAxis.GridLineType.ForeColor = Color.LightGray;
    
    // Fonts
    Font chartFont = new Font("Segoe UI", 9);
    pivotChart1.PrimaryXAxis.LabelFont = chartFont;
    pivotChart1.PrimaryYAxis.LabelFont = chartFont;
    
    // Border
    pivotChart1.ChartArea.Border.Color = Color.FromArgb(200, 200, 200);
}
```

## Print Support

### Basic Print

```csharp
// Print chart
pivotChart1.Print();
```

### Print with Settings

```csharp
using System.Drawing.Printing;

private void PrintChartWithSettings()
{
    PrintDocument printDoc = new PrintDocument();
    printDoc.DocumentName = "Pivot Chart Report";
    
    // Print settings
    printDoc.DefaultPageSettings.Landscape = true;
    printDoc.DefaultPageSettings.Margins = new Margins(50, 50, 50, 50);
    
    // Show print dialog
    PrintDialog printDialog = new PrintDialog();
    printDialog.Document = printDoc;
    
    if (printDialog.ShowDialog() == DialogResult.OK)
    {
        pivotChart1.Print(printDoc);
    }
}
```

### Print Preview

```csharp
// Show print preview dialog
private void ShowPrintPreview()
{
    PrintPreviewDialog previewDialog = new PrintPreviewDialog();
    PrintDocument printDoc = new PrintDocument();
    
    printDoc.PrintPage += (s, e) =>
    {
        // Draw chart on print page
        Bitmap chartImage = new Bitmap(pivotChart1.Width, pivotChart1.Height);
        pivotChart1.DrawToBitmap(chartImage, new Rectangle(0, 0, pivotChart1.Width, pivotChart1.Height));
        e.Graphics.DrawImage(chartImage, e.MarginBounds);
    };
    
    previewDialog.Document = printDoc;
    previewDialog.ShowDialog();
}
```

## Complete Customization Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.PivotChart;

public partial class CustomizedPivotChart : Form
{
    private PivotChart pivotChart1;
    
    public CustomizedPivotChart()
    {
        InitializeComponent();
        SetupCustomizedChart();
    }
    
    private void SetupCustomizedChart()
    {
        pivotChart1 = new PivotChart { Dock = DockStyle.Fill };
        
        // Apply comprehensive customization
        CustomizeAppearance();
        CustomizeColors();
        CustomizeDataLabels();
        
        this.Controls.Add(pivotChart1);
    }
    
    private void CustomizeAppearance()
    {
        // Chart title
        pivotChart1.Title.Text = "Q4 Sales Performance Dashboard";
        pivotChart1.Title.Font = new Font("Segoe UI", 16, FontStyle.Bold);
        pivotChart1.Title.ForeColor = Color.FromArgb(0, 114, 198);
        
        // Chart area
        pivotChart1.BackColor = Color.FromArgb(250, 250, 250);
        pivotChart1.ChartArea.BackColor = Color.White;
        pivotChart1.ChartArea.Border.Color = Color.FromArgb(230, 230, 230);
        pivotChart1.ChartArea.BorderWidth = 1;
        
        // Axes
        pivotChart1.PrimaryXAxis.Title = "Product Categories";
        pivotChart1.PrimaryXAxis.TitleFont = new Font("Segoe UI", 11, FontStyle.Bold);
        pivotChart1.PrimaryXAxis.LabelFont = new Font("Segoe UI", 9);
        
        pivotChart1.PrimaryYAxis.Title = "Revenue (USD)";
        pivotChart1.PrimaryYAxis.TitleFont = new Font("Segoe UI", 11, FontStyle.Bold);
        pivotChart1.PrimaryYAxis.LabelFont = new Font("Segoe UI", 9);
        
        // Grid lines
        pivotChart1.PrimaryXAxis.GridLineType.ForeColor = Color.FromArgb(240, 240, 240);
        pivotChart1.PrimaryYAxis.GridLineType.ForeColor = Color.FromArgb(240, 240, 240);
    }
    
    private void CustomizeColors()
    {
        // Corporate color palette
        Color[] colors = new Color[]
        {
            Color.FromArgb(0, 114, 198),   // Blue
            Color.FromArgb(255, 127, 14),  // Orange
            Color.FromArgb(44, 160, 44),   // Green
            Color.FromArgb(214, 39, 40),   // Red
            Color.FromArgb(148, 103, 189)  // Purple
        };
        
        pivotChart1.Palette = ChartColorPalette.Custom;
        pivotChart1.CustomPalette = colors;
    }
    
    private void CustomizeDataLabels()
    {
        pivotChart1.ShowDataLabels = true;
        pivotChart1.DataLabels.Font = new Font("Segoe UI", 8);
        pivotChart1.DataLabels.ForeColor = Color.Black;
        pivotChart1.DataLabels.Format = "${0:N0}";
    }
}
```

## Best Practices

1. **Consistent Branding:** Use company colors and fonts
2. **Accessibility:** Ensure sufficient color contrast
3. **Data Ink Ratio:** Minimize non-data elements
4. **Readable Fonts:** Use clear, legible fonts (11pt+ for titles)
5. **Color Blind Friendly:** Test with color blind simulators
6. **Print Testing:** Verify appearance in print/PDF
7. **Theme Consistency:** Apply consistent styling across all charts

## Common Customization Scenarios

### Scenario 1: Dashboard Chart
```csharp
// Clean, minimal design for dashboards
pivotChart1.BackColor = Color.White;
pivotChart1.ChartArea.Border.Visible = false;
pivotChart1.ShowLegend = true;
pivotChart1.Legend.Position = ChartDock.Bottom;
```

### Scenario 2: Report Chart
```csharp
// Professional report styling
pivotChart1.ChartArea.Border.Color = Color.Black;
pivotChart1.ChartArea.BorderWidth = 2;
pivotChart1.Title.Font = new Font("Times New Roman", 14, FontStyle.Bold);
```

### Scenario 3: Presentation Chart
```csharp
// High contrast for presentations
pivotChart1.BackColor = Color.White;
pivotChart1.Palette = ChartColorPalette.Metro;
pivotChart1.ShowDataLabels = true;
pivotChart1.DataLabels.Font = new Font("Arial", 10, FontStyle.Bold);
```

## Next Steps

- Combine with **Drill Operations** for interactive exploration
- Configure **Export** to save customized charts
- Add **Touch Support** for tablet presentations
- Implement **Zooming** for detailed data exploration
