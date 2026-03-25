# Export Functionality

This guide covers exporting Pivot Charts to Excel and other formats.

## Overview

The Pivot Chart control supports exporting to Excel format for reporting and distribution.

## Export to Excel

```csharp
// Export to Excel file
pivotChart1.ExportToExcel("PivotChartReport.xlsx");
```

## Export with Options

```csharp
using Syncfusion.ExcelExport;

// Configure export options
ExcelExportingOptions options = new ExcelExportingOptions
{
    ExcelVersion = ExcelVersion.Excel2016,
    ExportAsImage = false
};

// Export with options
pivotChart1.ExportToExcel("Report.xlsx", options);
```

## Export Methods

```csharp
// Method 1: Export to file path
pivotChart1.ExportToExcel(@"C:\Reports\Chart.xlsx");

// Method 2: Export to stream
using (FileStream stream = new FileStream("Chart.xlsx", FileMode.Create))
{
    pivotChart1.ExportToExcel(stream);
}
```

## Export with User Dialog

```csharp
private void ExportChart()
{
    SaveFileDialog saveDialog = new SaveFileDialog
    {
        Filter = "Excel Files|*.xlsx",
        Title = "Export Pivot Chart",
        FileName = "PivotChart_" + DateTime.Now.ToString("yyyyMMdd")
    };
    
    if (saveDialog.ShowDialog() == DialogResult.OK)
    {
        try
        {
            pivotChart1.ExportToExcel(saveDialog.FileName);
            MessageBox.Show("Chart exported successfully!", "Export", 
                            MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Export failed: {ex.Message}", "Error", 
                            MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
}
```

## Export as Image

```csharp
// Export chart as image
pivotChart1.SaveAsImage("PivotChart.png");

// With image format
pivotChart1.SaveAsImage("Chart.jpg", System.Drawing.Imaging.ImageFormat.Jpeg);
```

## Best Practices

1. Provide meaningful file names with dates
2. Handle export exceptions gracefully
3. Show progress for large exports
4. Validate file path before exporting
5. Consider user permissions for file locations
