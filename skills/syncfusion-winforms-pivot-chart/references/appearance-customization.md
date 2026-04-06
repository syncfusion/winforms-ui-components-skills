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
pivotChart1.ChartArea.BackInterior = new BrushInfo(Color.WhiteSmoke);
// Border customization
pivotChart1.ChartArea.BorderColor = Color.Gray;
pivotChart1.ChartArea.BorderWidth = 1;
```

### Title Customization

```csharp
// Chart title
pivotChart1.ChartControl.Title.Text = "Sales Analysis by Product";
pivotChart1.ChartControl.Title.Font = new System.Drawing.Font("Arial", 14, System.Drawing.FontStyle.Bold);
pivotChart1.ChartControl.Title.ForeColor = Color.DarkBlue;
pivotChart1.ChartControl.Title.Alignment = ChartAlignment.Center;
```

### Axis Customization

```csharp
// X-Axis customization
pivotChart1.ChartControl.PrimaryXAxis.Title = "Products";
pivotChart1.ChartControl.PrimaryXAxis.TitleFont = new System.Drawing.Font("Arial", 10);
pivotChart1.ChartControl.PrimaryXAxis.Font = new System.Drawing.Font("Arial", 9);

// Y-Axis customization
pivotChart1.ChartControl.PrimaryYAxis.Title = "Revenue ($)";
pivotChart1.ChartControl.PrimaryYAxis.TitleFont = new System.Drawing.Font("Arial", 10);
pivotChart1.ChartControl.PrimaryYAxis.Font = new System.Drawing.Font("Arial", 9);
pivotChart1.ChartControl.PrimaryYAxis.FormatLabel += (s, args) =>
{
    args.Label = "$" + args.Label;  // Add currency symbol
};
```

## Series Customization

### Individual Series Styling

```csharp
// Access and customize series
foreach (ChartSeries series in pivotChart1.ChartControl.Series)
{
    series.Style.Border.Width = 2;
    series.Style.DisplayText = true;
    series.Style.Font.Size = 8;
}
```

### Data Labels

```csharp
// Configure data labels
foreach (ChartSeries series in pivotChart1.ChartControl.Series)
{
    series.Style.DisplayText = true;
    series.Style.Font.Size = 8;
    series.Style.Font.Facename = "Arial";
    series.Style.TextColor = Color.Black;
    series.Style.TextFormat = "{0:C0}";  // Currency format
}
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
pivotChart1.ChartControl.Palette = ChartColorPalette.Custom;
pivotChart1.CustomPalette = customPalette;
```

### Predefined Palettes

```csharp
// Use built-in color palettes
pivotChart1.ChartControl.Palette = ChartColorPalette.Metro;       // Metro style
pivotChart1.ChartControl.Palette = ChartColorPalette.Nature;      // Nature colors
pivotChart1.ChartControl.Palette = ChartColorPalette.EarthTone;  // Earth tone
pivotChart1.ChartControl.Palette = ChartColorPalette.Pastel;      // Pastel colors
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
    
    pivotChart1.ChartControl.Palette = ChartColorPalette.Custom;
    pivotChart1.CustomPalette = corporateColors;
}
```

## Themes

### Applying Office Themes

```csharp
// Apply Office 2016 theme
pivotChart1.Skins = Skins.Office2016Colorful;

// Other theme options
pivotChart1.Skins = Skins.Office2016White;
pivotChart1.Skins = Skins.Office2016DarkGray;
pivotChart1.Skins = Skins.Office2016Black;
```

### Custom Theme

```csharp
private void ApplyCustomTheme()
{
    // Background
    pivotChart1.BackColor = Color.FromArgb(245, 245, 245);
    pivotChart1.ChartArea.BackInterior = new BrushInfo(Color.White);

    // Grid lines
    pivotChart1.PrimaryXAxis.GridLineType.ForeColor = Color.LightGray;
    pivotChart1.PrimaryYAxis.GridLineType.ForeColor = Color.LightGray;

    // Fonts
    System.Drawing.Font chartFont = new System.Drawing.Font("Segoe UI", 9);
    pivotChart1.ChartControl.PrimaryXAxis.Font = chartFont;
    pivotChart1.ChartControl.PrimaryYAxis.Font = chartFont;

    // Border
    pivotChart1.ChartArea.BorderColor = Color.FromArgb(200, 200, 200);
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
        pivotChart1.ChartControl.Title.Text = "Q4 Sales Performance Dashboard";
        pivotChart1.ChartControl.Title.Font = new System.Drawing.Font("Segoe UI", 16, FontStyle.Bold);
        pivotChart1.ChartControl.Title.ForeColor = Color.FromArgb(0, 114, 198);

        // Chart area
        pivotChart1.BackColor = Color.FromArgb(250, 250, 250);
        pivotChart1.ChartArea.BackInterior = new BrushInfo(Color.White);
        pivotChart1.ChartArea.BorderColor = Color.FromArgb(230, 230, 230);
        pivotChart1.ChartArea.BorderWidth = 1;

        // Axes
        pivotChart1.ChartControl.PrimaryXAxis.Title = "Product Categories";
        pivotChart1.ChartControl.PrimaryXAxis.TitleFont = new System.Drawing.Font("Segoe UI", 11, FontStyle.Bold);
        pivotChart1.ChartControl.PrimaryXAxis.Font = new System.Drawing.Font("Segoe UI", 9);

        pivotChart1.ChartControl.PrimaryYAxis.Title = "Revenue (USD)";
        pivotChart1.ChartControl.PrimaryYAxis.TitleFont = new System.Drawing.Font("Segoe UI", 11, FontStyle.Bold);
        pivotChart1.ChartControl.PrimaryYAxis.Font = new System.Drawing.Font("Segoe UI", 9);

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
        
        pivotChart1.ChartControl.Palette = ChartColorPalette.Custom;
        pivotChart1.CustomPalette = colors;
    }
    
    private void CustomizeDataLabels()
    {
        foreach (ChartSeries series in pivotChart1.ChartControl.Series)
        {
            series.Style.DisplayText = true;
            series.Style.Font.Size = 8;
            series.Style.Font.Facename = "Segoe UI";
            series.Style.TextColor = Color.Black;
            series.Style.TextFormat = "{0:N0}";
        }
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

## Common Customization Scenarios

### Scenario 1: Dashboard Chart
```csharp
// Clean, minimal design for dashboards
pivotChart1.BackColor = Color.White;
pivotChart1.ShowLegend = true;
pivotChart1.ChartControl.LegendPosition = ChartDock.Bottom;
```

### Scenario 2: Report Chart
```csharp
// Professional report styling
pivotChart1.ChartArea.BorderColor = Color.Black;
pivotChart1.ChartArea.BorderWidth = 2;
pivotChart1.ChartControl.Title.Font = new System.Drawing.Font("Times New Roman", 14, FontStyle.Bold);
```

### Scenario 3: Presentation Chart
```csharp
// High contrast for presentations
 pivotChart1.BackColor = Color.White;
 pivotChart1.ChartControl.Palette = ChartColorPalette.Metro;

 foreach (ChartSeries series in pivotChart1.ChartControl.Series)
 {
     series.Style.DisplayText = true;
     series.Style.Font.Size = 10;
     series.Style.Font.Facename = "Arial";
     series.Style.Font.FontStyle = FontStyle.Bold;
 }
```

## Next Steps

- Combine with **Drill Operations** for interactive exploration
- Configure **Export** to save customized charts
- Add **Touch Support** for tablet presentations
- Implement **Zooming** for detailed data exploration
