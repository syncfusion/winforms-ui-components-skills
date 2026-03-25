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
chartControl1.SaveImage("chart.png", ChartImageFormat.Png);

// JPEG export
chartControl1.SaveImage("chart.jpg", ChartImageFormat.Jpeg);

// BMP export
chartControl1.SaveImage("chart.bmp", ChartImageFormat.Bmp);

// GIF export
chartControl1.SaveImage("chart.gif", ChartImageFormat.Gif);
```

### Export to Stream
```csharp
using (MemoryStream stream = new MemoryStream())
{
    chartControl1.SaveImage(stream, ChartImageFormat.Png);
    byte[] imageBytes = stream.ToArray();
}
```

### Export with Custom Size
```csharp
Image image = chartControl1.GetChartImage(new Size(800, 600));
image.Save("chart.png", ImageFormat.Png);
```

## PDF Export

```csharp
chartControl1.SaveImage("chart.pdf", ChartImageFormat.PDF);
```

## Excel/Word Export

Requires Syncfusion.XlsIO and Syncfusion.DocIO assemblies.

### Export to Excel
```csharp
using (ExcelEngine excelEngine = new ExcelEngine())
{
    IApplication application = excelEngine.Excel;
    IWorkbook workbook = application.Workbooks.Create(1);
    IWorksheet worksheet = workbook.Worksheets[0];
    
    // Export chart as image to Excel
    Image chartImage = chartControl1.GetChartImage();
    worksheet.Pictures.AddPicture(1, 1, chartImage);
    
    workbook.SaveAs("chart.xlsx");
}
```

### Export to Word
```csharp
using (WordDocument document = new WordDocument())
{
    IWSection section = document.AddSection();
    IWParagraph paragraph = section.AddParagraph();
    
    // Export chart as image to Word
    Image chartImage = chartControl1.GetChartImage();
    WPicture picture = paragraph.AppendPicture(chartImage) as WPicture;
    
    document.Save("chart.docx", FormatType.Docx);
}
```

## Printing

### Simple Print
```csharp
chartControl1.Print();
```

### Print Preview
```csharp
chartControl1.ShowPrintPreview();
```

### Print with Settings
```csharp
using (PrintDocument printDoc = new PrintDocument())
{
    printDoc.DocumentName = "Chart Document";
    
    printDoc.PrintPage += (sender, e) =>
    {
        Image chartImage = chartControl1.GetChartImage(e.PageBounds.Size);
        e.Graphics.DrawImage(chartImage, e.PageBounds);
    };
    
    // Show print dialog
    PrintDialog printDialog = new PrintDialog();
    printDialog.Document = printDoc;
    
    if (printDialog.ShowDialog() == DialogResult.OK)
    {
        printDoc.Print();
    }
}
```

### Multi-Page Printing
For large charts split across multiple pages:

```csharp
private int currentPage = 0;
private int totalPages = 2;

printDoc.PrintPage += (sender, e) =>
{
    Image chartImage = chartControl1.GetChartImage();
    int pageHeight = e.PageBounds.Height;
    int sourceY = currentPage * pageHeight;
    
    Rectangle destRect = e.PageBounds;
    Rectangle srcRect = new Rectangle(0, sourceY, chartImage.Width, pageHeight);
    
    e.Graphics.DrawImage(chartImage, destRect, srcRect, GraphicsUnit.Pixel);
    
    currentPage++;
    e.HasMorePages = (currentPage < totalPages);
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
chartControl1.ChartArea.AutoScale = false;
Image highResImage = chartControl1.GetChartImage(new Size(2400, 1800));  // 300 DPI at 8x6 inches
highResImage.Save("chart_hires.png", ImageFormat.Png);
```

## Batch Export Example

```csharp
// Export in multiple formats
string baseName = "sales_chart";
chartControl1.SaveImage($"{baseName}.png", ChartImageFormat.Png);
chartControl1.SaveImage($"{baseName}.pdf", ChartImageFormat.PDF);
chartControl1.SaveImage($"{baseName}.jpg", ChartImageFormat.Jpeg);
```
