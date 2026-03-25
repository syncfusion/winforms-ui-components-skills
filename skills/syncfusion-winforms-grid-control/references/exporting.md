# Exporting

This guide covers exporting GridControl data to various formats including Excel, PDF, CSV, HTML, and Word documents.

## Table of Contents
- [Overview](#overview)
- [Excel Export](#excel-export)
- [PDF Export](#pdf-export)
- [CSV Export](#csv-export)
- [HTML Export](#html-export)
- [Word Export](#word-export)
- [Export Options](#export-options)
- [Custom Export](#custom-export)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

GridControl supports exporting to multiple formats:
- **Excel** - XLSX, XLS formats with formatting
- **PDF** - Portable documents with styling
- **CSV** - Comma-separated values
- **HTML** - Web-ready tables
- **Word** - DOCX format

## Excel Export

### Basic Export:

```csharp
using Syncfusion.GridExcelConverter;
using Syncfusion.XlsIO;

// Export to Excel
private void ExportToExcel()
{
    // Create converter
    GridExcelConverter converter = new GridExcelConverter();
    
    // Export grid to Excel
    ExcelEngine excelEngine = new ExcelEngine();
    IWorkbook workbook = excelEngine.Excel.Workbooks.Create(1);
    IWorksheet worksheet = workbook.Worksheets[0];
    
    // Convert grid to Excel
    converter.GridToExcel(gridControl1.Model, worksheet);
    
    // Save
    workbook.SaveAs("output.xlsx");
    workbook.Close();
    excelEngine.Dispose();
}
```

### Export with Formatting:

```csharp
private void ExportToExcelWithFormatting()
{
    GridExcelConverter converter = new GridExcelConverter();
    converter.ExportStyle = true;  // Include cell styles
    converter.ExportBorders = true;  // Include borders
    
    ExcelEngine excelEngine = new ExcelEngine();
    IWorkbook workbook = excelEngine.Excel.Workbooks.Create(1);
    IWorksheet worksheet = workbook.Worksheets[0];
    
    // Convert with formatting
    converter.GridToExcel(gridControl1.Model, worksheet, 
        GridExcelConverterFlags.Default | GridExcelConverterFlags.Formulas);
    
    // Auto-fit columns
    worksheet.UsedRange.AutofitColumns();
    
    // Save
    workbook.SaveAs("formatted_output.xlsx");
    workbook.Close();
    excelEngine.Dispose();
}
```

### Export Selected Range:

```csharp
private void ExportSelectedRangeToExcel()
{
    GridRangeInfo range = gridControl1.Selections.Ranges[0];
    
    GridExcelConverter converter = new GridExcelConverter();
    ExcelEngine excelEngine = new ExcelEngine();
    IWorkbook workbook = excelEngine.Excel.Workbooks.Create(1);
    IWorksheet worksheet = workbook.Worksheets[0];
    
    // Export only selected range
    converter.GridToExcel(gridControl1.Model, worksheet, range);
    
    workbook.SaveAs("selected_range.xlsx");
    workbook.Close();
    excelEngine.Dispose();
}
```

## PDF Export

### Basic PDF Export:

```csharp
using Syncfusion.Pdf;
using Syncfusion.Pdf.Grid;

private void ExportToPDF()
{
    // Create PDF document
    PdfDocument document = new PdfDocument();
    PdfPage page = document.Pages.Add();
    
    // Create PDF grid
    PdfGrid pdfGrid = new PdfGrid();
    
    // Add columns
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        pdfGrid.Columns.Add();
    }
    
    // Add rows
    for (int row = 1; row <= gridControl1.RowCount; row++)
    {
        PdfGridRow pdfRow = pdfGrid.Rows.Add();
        
        for (int col = 1; col <= gridControl1.ColCount; col++)
        {
            string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
            pdfRow.Cells[col - 1].Value = cellValue;
        }
    }
    
    // Draw grid on page
    pdfGrid.Draw(page, new PointF(10, 10));
    
    // Save
    document.Save("output.pdf");
    document.Close(true);
}
```

### PDF with Styling:

```csharp
private void ExportToPDFWithStyling()
{
    PdfDocument document = new PdfDocument();
    PdfPage page = document.Pages.Add();
    PdfGrid pdfGrid = new PdfGrid();
    
    // Add columns
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        pdfGrid.Columns.Add();
    }
    
    // Header row with styling
    PdfGridRow headerRow = pdfGrid.Rows.Add();
    headerRow.Style.BackgroundBrush = PdfBrushes.LightBlue;
    headerRow.Style.Font = new PdfStandardFont(PdfFontFamily.Helvetica, 12, PdfFontStyle.Bold);
    
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        string headerValue = gridControl1[0, col].CellValue?.ToString() ?? $"Column {col}";
        headerRow.Cells[col - 1].Value = headerValue;
    }
    
    // Data rows
    for (int row = 1; row <= gridControl1.RowCount; row++)
    {
        PdfGridRow pdfRow = pdfGrid.Rows.Add();
        
        // Alternate row colors
        if (row % 2 == 0)
        {
            pdfRow.Style.BackgroundBrush = PdfBrushes.WhiteSmoke;
        }
        
        for (int col = 1; col <= gridControl1.ColCount; col++)
        {
            string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
            pdfRow.Cells[col - 1].Value = cellValue;
            
            // Apply cell formatting
            GridStyleInfo style = gridControl1[row, col];
            if (style.Font.Bold)
            {
                pdfRow.Cells[col - 1].Style.Font = new PdfStandardFont(PdfFontFamily.Helvetica, 10, PdfFontStyle.Bold);
            }
        }
    }
    
    // Grid styling
    pdfGrid.Style.CellPadding = new PdfPaddings(5, 5, 5, 5);
    pdfGrid.Style.BorderOverlapStyle = PdfBorderOverlapStyle.Inside;
    
    // Draw
    pdfGrid.Draw(page, new PointF(10, 10));
    
    document.Save("styled_output.pdf");
    document.Close(true);
}
```

## CSV Export

### Basic CSV Export:

```csharp
private void ExportToCSV()
{
    using (StreamWriter writer = new StreamWriter("output.csv"))
    {
        // Export all rows
        for (int row = 1; row <= gridControl1.RowCount; row++)
        {
            List<string> rowValues = new List<string>();
            
            for (int col = 1; col <= gridControl1.ColCount; col++)
            {
                string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
                
                // Escape commas and quotes
                if (cellValue.Contains(",") || cellValue.Contains("\""))
                {
                    cellValue = $"\"{cellValue.Replace("\"", "\"\"")}\"";
                }
                
                rowValues.Add(cellValue);
            }
            
            writer.WriteLine(string.Join(",", rowValues));
        }
    }
}
```

### CSV with Headers:

```csharp
private void ExportToCSVWithHeaders()
{
    using (StreamWriter writer = new StreamWriter("output.csv"))
    {
        // Write headers
        List<string> headers = new List<string>();
        for (int col = 1; col <= gridControl1.ColCount; col++)
        {
            string header = gridControl1[0, col].CellValue?.ToString() ?? $"Column{col}";
            headers.Add(EscapeCSV(header));
        }
        writer.WriteLine(string.Join(",", headers));
        
        // Write data rows
        for (int row = 1; row <= gridControl1.RowCount; row++)
        {
            List<string> rowValues = new List<string>();
            
            for (int col = 1; col <= gridControl1.ColCount; col++)
            {
                string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
                rowValues.Add(EscapeCSV(cellValue));
            }
            
            writer.WriteLine(string.Join(",", rowValues));
        }
    }
}

private string EscapeCSV(string value)
{
    if (value.Contains(",") || value.Contains("\"") || value.Contains("\n"))
    {
        return $"\"{value.Replace("\"", "\"\"")}\"";
    }
    return value;
}
```

## HTML Export

### Basic HTML Export:

```csharp
private void ExportToHTML()
{
    StringBuilder html = new StringBuilder();
    html.AppendLine("<html><body>");
    html.AppendLine("<table border='1' cellpadding='5' cellspacing='0'>");
    
    // Headers
    html.AppendLine("<thead><tr>");
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        string header = gridControl1[0, col].CellValue?.ToString() ?? $"Column {col}";
        html.AppendLine($"<th>{System.Web.HttpUtility.HtmlEncode(header)}</th>");
    }
    html.AppendLine("</tr></thead>");
    
    // Data rows
    html.AppendLine("<tbody>");
    for (int row = 1; row <= gridControl1.RowCount; row++)
    {
        html.AppendLine("<tr>");
        for (int col = 1; col <= gridControl1.ColCount; col++)
        {
            string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
            html.AppendLine($"<td>{System.Web.HttpUtility.HtmlEncode(cellValue)}</td>");
        }
        html.AppendLine("</tr>");
    }
    html.AppendLine("</tbody>");
    
    html.AppendLine("</table>");
    html.AppendLine("</body></html>");
    
    File.WriteAllText("output.html", html.ToString());
}
```

### HTML with Styling:

```csharp
private void ExportToHTMLWithStyling()
{
    StringBuilder html = new StringBuilder();
    html.AppendLine("<html>");
    html.AppendLine("<head><style>");
    html.AppendLine("table { border-collapse: collapse; font-family: Arial; }");
    html.AppendLine("th { background-color: #4CAF50; color: white; padding: 8px; }");
    html.AppendLine("td { padding: 8px; border: 1px solid #ddd; }");
    html.AppendLine("tr:nth-child(even) { background-color: #f2f2f2; }");
    html.AppendLine("</style></head>");
    html.AppendLine("<body>");
    html.AppendLine("<table>");
    
    // Headers
    html.AppendLine("<thead><tr>");
    for (int col = 1; col <= gridControl1.ColCount; col++)
    {
        string header = gridControl1[0, col].CellValue?.ToString() ?? $"Column {col}";
        html.AppendLine($"<th>{System.Web.HttpUtility.HtmlEncode(header)}</th>");
    }
    html.AppendLine("</tr></thead>");
    
    // Data rows with cell styling
    html.AppendLine("<tbody>");
    for (int row = 1; row <= gridControl1.RowCount; row++)
    {
        html.AppendLine("<tr>");
        for (int col = 1; col <= gridControl1.ColCount; col++)
        {
            GridStyleInfo style = gridControl1[row, col];
            string cellValue = style.CellValue?.ToString() ?? "";
            
            // Build inline style
            List<string> styles = new List<string>();
            if (style.Font.Bold)
                styles.Add("font-weight: bold");
            if (style.BackColor != Color.White)
                styles.Add($"background-color: {ColorTranslator.ToHtml(style.BackColor)}");
            if (style.TextColor != Color.Black)
                styles.Add($"color: {ColorTranslator.ToHtml(style.TextColor)}");
            
            string styleAttr = styles.Count > 0 ? $" style='{string.Join("; ", styles)}'" : "";
            html.AppendLine($"<td{styleAttr}>{System.Web.HttpUtility.HtmlEncode(cellValue)}</td>");
        }
        html.AppendLine("</tr>");
    }
    html.AppendLine("</tbody>");
    
    html.AppendLine("</table>");
    html.AppendLine("</body></html>");
    
    File.WriteAllText("styled_output.html", html.ToString());
}
```

## Word Export

### Export to Word Document:

```csharp
using Syncfusion.DocIO;
using Syncfusion.DocIO.DLS;

private void ExportToWord()
{
    // Create Word document
    WordDocument document = new WordDocument();
    IWSection section = document.AddSection();
    
    // Add table
    IWTable table = section.AddTable();
    table.ResetCells(gridControl1.RowCount + 1, gridControl1.ColCount);
    
    // Add headers
    for (int col = 0; col < gridControl1.ColCount; col++)
    {
        string header = gridControl1[0, col + 1].CellValue?.ToString() ?? $"Column {col + 1}";
        table[0, col].AddParagraph().AppendText(header);
        table[0, col].CellFormat.BackColor = Color.LightBlue;
    }
    
    // Add data rows
    for (int row = 1; row <= gridControl1.RowCount; row++)
    {
        for (int col = 1; col <= gridControl1.ColCount; col++)
        {
            string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
            table[row, col - 1].AddParagraph().AppendText(cellValue);
        }
    }
    
    // Save
    document.Save("output.docx", FormatType.Docx);
    document.Close();
}
```

## Export Options

### Export Dialog:

```csharp
private void ShowExportDialog()
{
    using (SaveFileDialog dialog = new SaveFileDialog())
    {
        dialog.Filter = "Excel Files (*.xlsx)|*.xlsx|" +
                       "PDF Files (*.pdf)|*.pdf|" +
                       "CSV Files (*.csv)|*.csv|" +
                       "HTML Files (*.html)|*.html|" +
                       "Word Files (*.docx)|*.docx";
        
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            string extension = Path.GetExtension(dialog.FileName).ToLower();
            
            switch (extension)
            {
                case ".xlsx":
                    ExportToExcel();
                    break;
                case ".pdf":
                    ExportToPDF();
                    break;
                case ".csv":
                    ExportToCSV();
                    break;
                case ".html":
                    ExportToHTML();
                    break;
                case ".docx":
                    ExportToWord();
                    break;
            }
            
            MessageBox.Show($"Grid exported to {dialog.FileName}", "Export Complete");
        }
    }
}
```

### Export Selected Cells Only:

```csharp
private void ExportSelectedCells()
{
    if (gridControl1.Selections.Ranges.Count == 0)
    {
        MessageBox.Show("Please select cells to export");
        return;
    }
    
    GridRangeInfo range = gridControl1.Selections.Ranges[0];
    
    using (StreamWriter writer = new StreamWriter("selected_cells.csv"))
    {
        for (int row = range.Top; row <= range.Bottom; row++)
        {
            List<string> rowValues = new List<string>();
            
            for (int col = range.Left; col <= range.Right; col++)
            {
                string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
                rowValues.Add(EscapeCSV(cellValue));
            }
            
            writer.WriteLine(string.Join(",", rowValues));
        }
    }
}
```

## Custom Export

### Export with Progress:

```csharp
private async Task ExportWithProgress()
{
    ProgressBar progressBar = new ProgressBar();
    progressBar.Maximum = gridControl1.RowCount;
    
    await Task.Run(() =>
    {
        using (StreamWriter writer = new StreamWriter("output.csv"))
        {
            for (int row = 1; row <= gridControl1.RowCount; row++)
            {
                List<string> rowValues = new List<string>();
                
                for (int col = 1; col <= gridControl1.ColCount; col++)
                {
                    string cellValue = gridControl1[row, col].CellValue?.ToString() ?? "";
                    rowValues.Add(EscapeCSV(cellValue));
                }
                
                writer.WriteLine(string.Join(",", rowValues));
                
                // Update progress
                this.Invoke((Action)(() => progressBar.Value = row));
            }
        }
    });
    
    MessageBox.Show("Export complete!");
}
```

### Custom Format Export:

```csharp
private void ExportWithCustomFormat()
{
    using (StreamWriter writer = new StreamWriter("custom_output.txt"))
    {
        // Custom format: Key=Value pairs
        for (int row = 1; row <= gridControl1.RowCount; row++)
        {
            writer.WriteLine($"[Record {row}]");
            
            for (int col = 1; col <= gridControl1.ColCount; col++)
            {
                string header = gridControl1[0, col].CellValue?.ToString() ?? $"Field{col}";
                string value = gridControl1[row, col].CellValue?.ToString() ?? "";
                writer.WriteLine($"{header}={value}");
            }
            
            writer.WriteLine();  // Blank line between records
        }
    }
}
```

## Best Practices

1. **Handle large datasets** - Use streaming or batch export
2. **Preserve formatting** - Export styles when needed
3. **Validate data** - Check for null/empty values
4. **Show progress** - For large exports
5. **Allow format selection** - Let users choose format
6. **Escape special characters** - Especially in CSV/HTML
7. **Test exports** - Verify output in target applications

## Troubleshooting

### Out of memory errors
- Export in batches
- Use streaming writers
- Dispose objects properly
- Clear unused references

### Formatting not preserved
- Enable `ExportStyle` option
- Manually apply formatting
- Check converter flags

### Special characters broken
- Use proper encoding (UTF-8)
- Escape HTML/CSV characters
- Test with international characters

### Large file size
- Compress output
- Remove unnecessary formatting
- Export selected data only

## Next Steps

- Implement batch export
- Add export templates
- Create scheduled exports
- Implement export presets
- Add export validation
