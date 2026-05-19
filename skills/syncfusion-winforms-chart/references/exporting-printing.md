# Exporting and Printing

## Table of Contents
- [Image Export](#image-export)
- [PDF Export](#pdf-export)
- [Excel/Word Export](#excelword-export)
- [Printing](#printing)

## Image Export

Export chart as image file.

```csharp
// PNG export
chartControl1.SaveImage("chart.png");

// JPEG export
chartControl1.SaveImage("chart.jpg");

// BMP export
chartControl1.SaveImage("chart.bmp");

// GIF export
chartControl1.SaveImage("chart.gif");
```

### Export to Stream
```csharp
using (MemoryStream stream = new MemoryStream())
{
    chartControl1.SaveImage(stream);
    byte[] imageBytes = stream.ToArray();
}
```

## PDF Export

```csharp
chartControl1.SaveImage("chart.pdf");
```

## Excel/Word Export

Requires Syncfusion.XlsIO and Syncfusion.DocIO assemblies.

### Export to Excel
```csharp
using (ExcelEngine excelEngine = new ExcelEngine())
{
    IApplication application = excelEngine.Excel;
    IWorkbook workbook = application.Workbooks.Create(1);
    IWorksheet sheet = workbook.Worksheets[0];

    // Fill data from WinForms ChartControl
    for (int i = 0; i < chartControl1.Series[0].Points.Count; i++)
    {
        sheet.Range[i + 1, 1].Number =
            chartControl1.Series[0].Points[i].X;

        sheet.Range[i + 1, 2].Number =
            chartControl1.Series[0].Points[i].YValues[0];
    }

    // Create Excel chart
    IChart chart = workbook.Charts.Add();

    chart.Series.Add("Series1");
    chart.Series[0].SerieType = ExcelChartType.Column_Clustered;

    chart.Series[0].Values =
        sheet.Range[1, 2, chartControl1.Series[0].Points.Count, 2];

    chart.Series[0].CategoryLabels =
        sheet.Range[1, 1, chartControl1.Series[0].Points.Count, 1];

    // Save file
    workbook.SaveAs("chart.xlsx");
    workbook.Close();
    excelEngine.Dispose();
}
```

### Export to Word
```csharp
using (WordDocument document = new WordDocument())
{
    IWSection section = document.AddSection();

    // Title
    IWParagraph paragraph = section.AddParagraph();
    paragraph.AppendText("Essential Chart");

    // Add image (chart)
    paragraph = section.AddParagraph();
    paragraph.ParagraphFormat.HorizontalAlignment =
        Syncfusion.DocIO.DLS.HorizontalAlignment.Center;

    paragraph.AppendPicture(Image.FromFile(imageFile));

    // Save document
    document.Save(exportFileName, Syncfusion.DocIO.FormatType.Doc);
}

OpenFile("DocIO", exportFileName);
```

## Printing

### Simple Print
```csharp
chartControl1.Print();
```

### Print Preview
```csharp
PrintPreviewDialog printPreviewDialog1 = new PrintPreviewDialog();

//Customizing the icon of print preview dialog
(printPreviewDialog1 as Form).Icon = new Icon(@"..\..\App.ico");

printPreviewDialog1.Document = this.chartControl1.PrintDocument;
printPreviewDialog1.ShowDialog();
```

### Print with Settings
```csharp
using (PrintDocument printDoc = chartControl1.PrintDocument)
{
    printDoc.DocumentName = "Chart Document";

    printDoc.PrintPage += (sender, e) =>
    {
        // Draw chart directly using Syncfusion API
        chartControl1.Draw(e.Graphics, e.MarginBounds);
    };

    PrintDialog printDialog = new PrintDialog
    {
        Document = printDoc
    };

    if (printDialog.ShowDialog() == DialogResult.OK)
    {
        printDoc.Print();
    }
}
```

### Multi-Page Printing
For large charts split across multiple pages:

```csharp
private double currentMin = 0;
private double currentMax = 0;
private double pageRange = 20;
private double totalMax;

printDoc.PrintPage += (sender, e) =>
{
    // Initialize range on first page
    if (currentMin == 0 && currentMax == 0)
    {
        currentMin = chartControl1.ChartArea.PrimaryXAxis.Range.Min;
        totalMax = chartControl1.ChartArea.PrimaryXAxis.Range.Max;
        currentMax = currentMin + pageRange;
    }

    // Set range for current page
    chartControl1.ChartArea.PrimaryXAxis.Range.Min = currentMin;
    chartControl1.ChartArea.PrimaryXAxis.Range.Max = currentMax;

    chartControl1.ChartArea.PrimaryXAxis.Range.Interval =
        (currentMax - currentMin) /
        chartControl1.ChartArea.PrimaryXAxis.Range.NumberOfIntervals;

    // Draw chart (NO image API needed)
    chartControl1.Draw(e.Graphics, e.MarginBounds);

    // Move to next page
    currentMin = currentMax;
    currentMax += pageRange;

    // More pages
    if (currentMin < totalMax)
    {
        e.HasMorePages = true;
    }
    else
    {
        e.HasMorePages = false;

        // Reset chart range after printing
        chartControl1.ChartArea.PrimaryXAxis.Range.Min = 0;
        chartControl1.ChartArea.PrimaryXAxis.Range.Max = totalMax;
    }
};
```

### Page Setup
```csharp
PageSetupDialog setupDialog = new PageSetupDialog();
setupDialog.Document = printDoc;
setupDialog.ShowDialog();
```

## Export Quality Settings

### High Resolution Export
```csharp
// Disable autoscaling for consistent rendering
chartControl1.ChartArea.AutoScale = false;

// Create high-resolution bitmap
using (Bitmap bitmap = new Bitmap(2400, 1800))
{
    using (Graphics g = Graphics.FromImage(bitmap))
    {
        // Optional: improve quality
        g.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.HighQuality;
        g.InterpolationMode = System.Drawing.Drawing2D.InterpolationMode.HighQualityBicubic;
        g.PixelOffsetMode = System.Drawing.Drawing2D.PixelOffsetMode.HighQuality;

        // Draw chart into bitmap
        chartControl1.Draw(g, new Rectangle(0, 0, 2400, 1800));
    }

    // Save image
    bitmap.Save("chart_hires.png", System.Drawing.Imaging.ImageFormat.Png);
}
```

## Batch Export Example

```csharp
// Export in multiple formats
string baseName = "sales_chart";
chartControl1.SaveImage($"{baseName}.png");
chartControl1.SaveImage($"{baseName}.pdf");
chartControl1.SaveImage($"{baseName}.jpg");
```
