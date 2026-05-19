# Export in WinForms DataGrid

Comprehensive guide for exporting data from the Syncfusion WinForms DataGrid (SfDataGrid) to Excel and PDF formats, including export options, customization, styling, and saving methods.

## Table of Contents
- [Overview](#overview)
- [Required Assemblies](#required-assemblies)
- [Export to Excel](#export-to-excel)
  - [Basic Excel Export](#basic-excel-export)
  - [Excel Export Options](#excel-export-options)
  - [Excel Cell Customization](#excel-cell-customization)
  - [Excel Saving Options](#excel-saving-options)
- [Export to PDF](#export-to-pdf)
  - [Basic PDF Export](#basic-pdf-export)
  - [PDF Export Options](#pdf-export-options)
  - [PDF Cell Customization](#pdf-cell-customization)
  - [PDF Saving Options](#pdf-saving-options)
- [Export Format Comparison](#export-format-comparison)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

SfDataGrid provides comprehensive support for exporting grid data to Excel and PDF formats, preserving grouping, filtering, sorting, paging, unbound rows, and stacked headers.

**Export Capabilities:**
- Export to Excel (.xlsx, .xls)
- Export to PDF
- Export selected items only
- Export with custom styling
- Export with images
- Export to various formats (HTML, CSV, XML, Image)

## Required Assemblies

### For Excel Export

**Assemblies:**
- `Syncfusion.SfDataGridConverter.WinForms`
- `Syncfusion.XlsIO.Base`

**NuGet Package:**
```
Install-Package Syncfusion.DataGridExport.WinForms
```

### For PDF Export

**Assemblies:**
- `Syncfusion.SfDataGridConverter.WinForms`
- `Syncfusion.Pdf.Base`

**NuGet Package:**
```
Install-Package Syncfusion.DataGridExport.WinForms
```

## Export to Excel

### Basic Excel Export

```csharp
using Syncfusion.WinForms.DataGridConverter;

var options = new ExcelExportingOptions();
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

```vb
Imports Syncfusion.WinForms.DataGridConverter

Dim options = New ExcelExportingOptions()
Dim excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options)
Dim workBook = excelEngine.Excel.Workbooks(0)
workBook.SaveAs("Sample.xlsx")
```

### Excel Export Options

#### Export Mode: Value vs Text

```csharp
var options = new ExcelExportingOptions();
options.ExportMode = ExportMode.Text; // Export display text instead of actual value
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Export Groups with Outlines

```csharp
var options = new ExcelExportingOptions();
options.AllowOutlining = true; // Enable grouping outlines in Excel
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Exclude Columns

```csharp
var options = new ExcelExportingOptions();
options.ExcludeColumns.Add("CustomerID");
options.ExcludeColumns.Add("ProductName");
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Set Excel Version

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Export Stacked Headers

```csharp
var options = new ExcelExportingOptions();
options.ExportStackedHeaders = true;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Export Unbound Rows

```csharp
var options = new ExcelExportingOptions();
options.ExportUnboundRows = true;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Change Start Row and Column

```csharp
var options = new ExcelExportingOptions();
options.StartRowIndex = 3; // Start exporting from row 3
options.StartColumnIndex = 3; // Start exporting from column 3
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

### Excel Cell Customization

#### Style Cells Based on Cell Type

```csharp
var options = new ExcelExportingOptions();
options.Exporting += options_Exporting;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");

void options_Exporting(object sender, DataGridExcelExportingEventArgs e)
{
    if (e.CellType == ExportCellType.HeaderCell)
    {
        e.CellStyle.BackGroundColor = Color.LightPink;
        e.CellStyle.ForeGroundColor = Color.White;
        e.Handled = true;
    }
    else if (e.CellType == ExportCellType.RecordCell)
    {
        e.CellStyle.BackGroundColor = Color.LightSkyBlue;
        e.Handled = true;
    }
}
```

#### Customize Cell Values

```csharp
var options = new ExcelExportingOptions();
options.CellExporting += CellExporting;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");

void CellExporting(object sender, DataGridCellExcelExportingEventArgs e)
{
    if (e.CellType == ExportCellType.RecordCell && e.ColumnName == "OrderID")
    {
        if ((int)e.CellValue % 2 == 0)
            e.Range.Cells[0].Value = "Y";
        else
            e.Range.Cells[0].Value = "N";
        e.Handled = true;
    }
}
```

#### Change Row Style Based on Data

```csharp
var options = new ExcelExportingOptions();
options.CellExporting += options_CellExporting;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");

void options_CellExporting(object sender, DataGridCellExcelExportingEventArgs e)
{
    if (!(e.NodeEntry is OrderInfo))
        return;
    
    var record = e.NodeEntry as OrderInfo;
    if (record.CustomerID == "FRANS")
    {
        e.Range.CellStyle.ColorIndex = ExcelKnownColors.Green;
        e.Range.CellStyle.Font.Color = ExcelKnownColors.White;
    }
}
```

#### Customize Columns

```csharp
var options = new ExcelExportingOptions();
options.CellExporting += Options_CellExporting;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");

void Options_CellExporting(object sender, DataGridCellExcelExportingEventArgs e)
{
    if (e.ColumnName != "OrderID")
        return;
    
    e.Range.CellStyle.Font.Size = 12;
    e.Range.CellStyle.Font.Color = ExcelKnownColors.Pink;
    e.Range.CellStyle.Font.FontName = "Segoe UI";
}
```

#### Set Border Color

```csharp
var options = new ExcelExportingOptions();
options.CellExporting += OnCellExporting;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");

void OnCellExporting(object sender, DataGridCellExcelExportingEventArgs e)
{
    // Set border color for Excel cell
    e.Range.BorderAround(ExcelLineStyle.Medium, ExcelKnownColors.Yellow);
}
```

### Excel Saving Options

#### Save Directly to File

```csharp
var options = new ExcelExportingOptions();
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.xlsx");
```

#### Save as Stream

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
FileStream fileStream = new FileStream("Output.xlsx", FileMode.Create);
workBook.SaveAs(fileStream);
fileStream.Close();
```

#### Save Using File Dialog

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];

SaveFileDialog saveFilterDialog = new SaveFileDialog
{
    FilterIndex = 2,
    Filter = "Excel 97 to 2003 Files(*.xls)|*.xls|Excel 2007 to 2010 Files(*.xlsx)|*.xlsx|Excel 2013 File(*.xlsx)|*.xlsx"
};

if (saveFilterDialog.ShowDialog() == System.Windows.Forms.DialogResult.OK)
{
    using (Stream stream = saveFilterDialog.OpenFile())
    {
        if (saveFilterDialog.FilterIndex == 1)
            workBook.Version = ExcelVersion.Excel97to2003;
        else if (saveFilterDialog.FilterIndex == 2)
            workBook.Version = ExcelVersion.Excel2010;
        else
            workBook.Version = ExcelVersion.Excel2013;
        workBook.SaveAs(stream);
    }
}
```

### Export Selected Items to Excel

```csharp
var options = new ExcelExportingOptions();
ExcelEngine excelEngine = new ExcelEngine();
IWorkbook workBook = excelEngine.Excel.Workbooks.Create();
workBook.Worksheets.Create();
sfDataGrid.ExportToExcel(sfDataGrid.SelectedItems, options, workBook.Worksheets[0]);
workBook.Version = ExcelVersion.Excel2013;
workBook.SaveAs("Sample.xlsx");
```

### Export to Other Formats

#### Export to HTML

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAsHtml("Sample.html", HtmlSaveOptions.Default);
```

#### Export to CSV

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAs("Sample.csv", ",");
```

#### Export to XML

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.SaveAsXml("Sample.xml", ExcelXmlSaveType.MSExcel);
```

#### Export to Image

```csharp
var excelExportingOptions = new ExcelExportingOptions();
ExcelEngine excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, excelExportingOptions);
IWorkbook workbook = excelEngine.Excel.Workbooks[0];
IWorksheet sheet = workbook.Worksheets[0];
sheet.UsedRangeIncludesFormatting = false;
int lastRow = sheet.UsedRange.LastRow + 1;
int lastColumn = sheet.UsedRange.LastColumn;
System.Drawing.Image img = sheet.ConvertToImage(1, 1, lastRow, lastColumn, ImageType.Bitmap, null);
img.Save("Sample.png", ImageFormat.Png);
```

### Worksheet Customization

#### Enable Filters

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.Worksheets[0].AutoFilters.FilterRange = workBook.Worksheets[0].UsedRange;
workBook.SaveAs("Sample.xlsx");
```

#### Set Borders

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.Worksheets[0].UsedRange.BorderInside(ExcelLineStyle.Dash_dot, ExcelKnownColors.Black);
workBook.Worksheets[0].UsedRange.BorderAround(ExcelLineStyle.Dash_dot, ExcelKnownColors.Black);
workBook.SaveAs("Sample.xlsx");
```

#### Customize Range of Cells

```csharp
var options = new ExcelExportingOptions();
options.ExcelVersion = ExcelVersion.Excel2013;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.Worksheets[0].Range["A2:A6"].CellStyle.Color = System.Drawing.Color.LightSlateGray;
workBook.Worksheets[0].Range["A2:A6"].CellStyle.Font.Color = ExcelKnownColors.White;
workBook.SaveAs("Sample.xlsx");
```

#### Row Height and Column Width

```csharp
var options = new ExcelExportingOptions();
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];
workBook.Worksheets[0].SetRowHeight(2, 50);
workBook.Worksheets[0].SetColumnWidth(2, 50);
workBook.SaveAs("Sample.xlsx");
```

## Export to PDF

### Basic PDF Export

```csharp
using Syncfusion.WinForms.DataGridConverter;

var document = sfDataGrid.ExportToPdf();
document.Save("Sample.pdf");
```

```vb
Imports Syncfusion.WinForms.DataGridConverter

Dim document = sfDataGrid.ExportToPdf()
document.Save("Sample.pdf")
```

### PDF Export Options

#### Auto Column Width

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.AutoColumnWidth = true;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Auto Row Height

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.AutoRowHeight = true;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Exclude Columns

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExcludeColumns.Add("CustomerID");
options.ExcludeColumns.Add("ProductName");
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Export Format

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportFormat = false; // Export actual value instead of display text
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Repeat Headers on Each Page

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.RepeatHeaders = true;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Fit All Columns in One Page

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.FitAllColumnsInOnePage = true;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Exclude Groups

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportGroups = false;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Exclude Summaries

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportGroupSummary = false; // Exclude group summaries
options.ExportTableSummary = false; // Exclude table summaries
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Export Stacked Headers

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportStackedHeaders = true;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

#### Export Unbound Rows

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.ExportUnboundRows = true;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");
```

### PDF Cell Customization

#### Style Cells Based on Cell Type

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.Exporting += options_Exporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void options_Exporting(object sender, DataGridPdfExportingEventArgs e)
{
    if (e.CellType == ExportCellType.HeaderCell)
        e.CellStyle.BackgroundBrush = PdfBrushes.LightSteelBlue;
    else if (e.CellType == ExportCellType.GroupCaptionCell)
        e.CellStyle.BackgroundBrush = PdfBrushes.LightGray;
    else if (e.CellType == ExportCellType.RecordCell)
        e.CellStyle.BackgroundBrush = PdfBrushes.Wheat;
}
```

#### Customize Cell Values

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.CellExporting += OnCellExporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void OnCellExporting(object sender, DataGridCellPdfExportingEventArgs e)
{
    if (e.CellType == ExportCellType.RecordCell && e.ColumnName == "OrderID")
    {
        if (Convert.ToInt16(e.CellValue) % 2 == 0)
            e.CellValue = "Even";
        else
            e.CellValue = "Odd";
    }
}
```

#### Change Row Style Based on Data

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.CellExporting += options_CellExporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void options_CellExporting(object sender, DataGridCellPdfExportingEventArgs e)
{
    if (!(e.NodeEntry is OrderInfo))
        return;
    
    if ((e.NodeEntry as OrderInfo).CustomerID == "MEREP")
    {
        var cellStyle = new PdfGridCellStyle();
        cellStyle.BackgroundBrush = PdfBrushes.LightPink;
        cellStyle.Borders.All = new PdfPen(PdfBrushes.DarkGray, 0.2f);
        e.PdfGridCell.Style = cellStyle;
    }
}
```

#### Set Border Color

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.CellExporting += OnCellExporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void OnCellExporting(object sender, DataGridCellPdfExportingEventArgs e)
{
    if (e.CellValue == null)
        e.CellValue = string.Empty;
    
    // Set border color for PDF cell
    e.PdfGridCell.Style.Borders.All = new PdfPen(Color.Blue, 0.2f);
}
```

#### Embed Fonts (Unicode Support)

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.Exporting += OnPdfExporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void OnPdfExporting(object sender, DataGridPdfExportingEventArgs e)
{
    if (e.CellType != ExportCellType.RecordCell)
        return;
    
    // Create font from file
    var font = new PdfTrueTypeFont(@"..\..\Resources\SegoeUI.ttf", 9f, PdfFontStyle.Regular);
    e.CellStyle.Font = font;
}
```

#### Export Images to PDF

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.CellExporting += cellExporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void cellExporting(object sender, DataGridCellPdfExportingEventArgs e)
{
    if (e.CellType == ExportCellType.RecordCell && e.ColumnName == "OrderID")
    {
        var style = new PdfGridCellStyle();
        PdfPen normalBorder = new PdfPen(PdfBrushes.DarkGray, 0.2f);
        System.Drawing.Image image = null;
        
        if (Convert.ToInt16(e.CellValue) % 2 == 0)
            image = SystemIcons.Information.ToBitmap();
        else
            image = SystemIcons.Shield.ToBitmap();
        
        style.BackgroundImage = PdfImage.FromImage(image);
        e.PdfGridCell.ImagePosition = PdfGridImagePosition.Center;
        e.PdfGridCell.Style = style;
        e.PdfGridCell.Style.Borders.All = normalBorder;
        e.CellValue = string.Empty;
    }
}
```

#### Right-to-Left (Arabic, Hebrew) Support

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.CellExporting += options_CellExporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void options_CellExporting(object sender, DataGridCellPdfExportingEventArgs e)
{
    if (e.CellType != ExportCellType.RecordCell)
        return;
    
    PdfStringFormat format = new PdfStringFormat();
    format.TextDirection = PdfTextDirection.RightToLeft;
    format.Alignment = PdfTextAlignment.Right;
    e.PdfGridCell.StringFormat = format;
}
```

### PDF Saving Options

#### Save Directly to File

```csharp
var document = sfDataGrid.ExportToPdf();
document.Save("Sample.pdf");
```

#### Save as Stream

```csharp
FileStream fileStream = new FileStream("Sample.pdf", FileMode.Create);
var document = sfDataGrid.ExportToPdf();
document.Save(fileStream);
fileStream.Close();
```

#### Save Using File Dialog

```csharp
var document = sfDataGrid.ExportToPdf();
SaveFileDialog saveFileDialog = new SaveFileDialog
{
    Filter = "PDF Files(*.pdf)|*.pdf"
};

if (saveFileDialog.ShowDialog() == DialogResult.OK)
{
    using (Stream stream = saveFileDialog.OpenFile())
    {
        document.Save(stream);
    }
}
```

### Header and Footer in PDF

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.HeaderFooterExporting += options_HeaderFooterExporting;
var document = sfDataGrid.ExportToPdf(options);
document.Save("Sample.pdf");

void options_HeaderFooterExporting(object sender, PdfHeaderFooterEventArgs e)
{
    PdfFont font = new PdfStandardFont(PdfFontFamily.TimesRoman, 20f, PdfFontStyle.Bold);
    var width = e.PdfPage.GetClientSize().Width;
    PdfPageTemplateElement header = new PdfPageTemplateElement(width, 38);
    header.Graphics.DrawString("Order Details", font, PdfPens.Black, 70, 3);
    e.PdfDocumentTemplate.Top = header;
}
```

### Change PDF Page Orientation

```csharp
var options = new PdfExportingOptions();
var document = new PdfDocument();
document.PageSettings.Orientation = PdfPageOrientation.Landscape;
var page = document.Pages.Add();
var PDFGrid = sfDataGrid.ExportToPdfGrid(sfDataGrid.View, options);
var format = new PdfGridLayoutFormat()
{
    Layout = PdfLayoutType.Paginate,
    Break = PdfLayoutBreakType.FitPage
};
PDFGrid.Draw(page, new PointF(), format);
document.Save("Sample.pdf");
```

### Export Selected Items to PDF

```csharp
PdfExportingOptions options = new PdfExportingOptions();
options.AutoColumnWidth = true;
var document = sfDataGrid.ExportToPdf(sfDataGrid.SelectedItems, options);
document.Save("Sample.pdf");
```

## Export Format Comparison

| Feature | Excel Export | PDF Export |
|---------|-------------|-----------|
| Editable output | ✓ | ✗ |
| Fixed layout | ✗ | ✓ |
| Multiple sheets | ✓ | ✓ (multi-page) |
| Formulas | ✓ | ✗ |
| Images | ✓ | ✓ |
| Custom fonts | ✓ | ✓ |
| Right-to-left text | ✓ | ✓ |
| File size | Smaller | Larger |
| Print-ready | Depends | ✓ |
| Grouping outlines | ✓ | ✗ |
| Auto-filter | ✓ | ✗ |

## Edge Cases and Troubleshooting

### Issue: Export takes too long

**Cause:** Using `CellExporting` event for large datasets

**Solution:** Perform customization after export using XlsIO or PDF APIs:

```csharp
// Instead of using CellExporting for each cell
var options = new ExcelExportingOptions();
options.ExportMode = ExportMode.Value;
var excelEngine = sfDataGrid.ExportToExcel(sfDataGrid.View, options);
var workBook = excelEngine.Excel.Workbooks[0];

// Apply formatting after export
workBook.ActiveSheet.Columns[5].NumberFormat = "0.0";
workBook.SaveAs("Sample.xlsx");
```

### Issue: Unicode characters not displaying in PDF

**Solution:** Embed custom font using `PdfTrueTypeFont`:

```csharp
options.Exporting += (sender, e) =>
{
    var font = new PdfTrueTypeFont(@"path\to\font.ttf", 9f, PdfFontStyle.Regular);
    e.CellStyle.Font = font;
};
```

### Issue: Images not exporting

**Solution:** Handle `CellExporting` event and set `BackgroundImage`:

```csharp
options.CellExporting += (sender, e) =>
{
    if (e.ColumnName == "ImageColumn")
    {
        var style = new PdfGridCellStyle();
        style.BackgroundImage = PdfImage.FromImage(yourImage);
        e.PdfGridCell.Style = style;
    }
};
```

### Issue: Stacked headers not aligned in Excel with filters

**Solution:** Set filter range based on stacked header count:

```csharp
var range = "A" + (sfDataGrid.StackedHeaderRows.Count + 1).ToString() + ":" + 
            workBook.Worksheets[0].UsedRange.End.AddressLocal;
workBook.Worksheets[0].AutoFilters.FilterRange = workBook.Worksheets[0].Range[range];
```

### Issue: Export doesn't include hidden columns

**Behavior:** By default, all columns—including hidden columns—are exported to Excel and PDF.

**Solution:** Explicitly exclude columns if needed:

```csharp
options.ExcludeColumns.Add("HiddenColumn1");
```

### Issue: PDF columns too narrow with FitAllColumnsInOnePage

**Solution:** Customize column widths after export:

```csharp
var pdfGrid = sfDataGrid.ExportToPdfGrid(sfDataGrid.View, options);
foreach (PdfGridCell headerCell in pdfGrid.Headers[0].Cells)
{
    if (headerCell.Value.ToString() == "OrderID")
    {
        var index = pdfGrid.Headers[0].Cells.IndexOf(headerCell);
        pdfGrid.Columns[index].Width = 50; // Set custom width
    }
}
```

### Issue: Alternate row coloring impacts performance

**Solution:** Use conditional formatting instead of `CellExporting`:

```csharp
IConditionalFormats condition = workBook.ActiveSheet.Range[2, 1, rowCount, columnCount].ConditionalFormats;
IConditionalFormat condition1 = condition.AddCondition();
condition1.FormatType = ExcelCFType.Formula;
condition1.FirstFormula = "MOD(ROW(),2)=0";
condition1.BackColorRGB = System.Drawing.Color.Pink;
```
