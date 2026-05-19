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
    GridExcelConverterControl excelConverter = new GridExcelConverterControl();
    
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
    GridExcelConverterControl converter = new GridExcelConverterControl();
    converter.ExportStyle = true;  // Include cell styles
    converter.ExportBorders = true;  // Include borders
    
    ExcelEngine excelEngine = new ExcelEngine();
    IWorkbook workbook = excelEngine.Excel.Workbooks.Create(1);
    IWorksheet worksheet = workbook.Worksheets[0];
    
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
    
    GridExcelConverterControl converter = new GridExcelConverterControl();
    ExcelEngine excelEngine = new ExcelEngine();
    IWorkbook workbook = excelEngine.Excel.Workbooks.Create(1);
    IWorksheet worksheet = workbook.Worksheets[0];
    
    // Export only selected range
    converter.SelectedExport(this.gridControl1.Model, "FileName", ConverterOptions.Default);
    
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
this.gridControl1.TextDataExchange.ExportTabDelim = ",";

GridCSVConverter csvConverter = new GridCSVConverter();

//Exporting to CSV format.
csvConverter.GridToCSV(this.gridControl1.Model, "Sample.csv");
```

### Exporting the Range of Cells

```csharp
GridCSVConverter csvConverter = new GridCSVConverter();

//Exporting the range of cells to CSV.
csvConverter.ExportRange(GridRangeInfo.Rows(4, 8), this.gridControl1.Model, "Sample.csv");
```
### Exporting the Selected Ranges
```csharp
GridCSVConverter csvConverter = new GridCSVConverter();

//Exporting the selected ranges.
csvConverter.SelectedExport(this.gridControl1.Model, "Sample.csv");
```

## HTML Export
**Step - 1**

Converting the GridControl range to the HTML Tags by building the strings.

```csharp
StringBuilder ExportAsHTML(GridRangeInfoList rangeList)
{

    GridRangeInfoList expandedRange = rangeList.ExpandRanges(0, 0, this.gridControl1.RowCount, this.gridControl1.ColCount);
    StringBuilder html = new StringBuilder();
    foreach (GridRangeInfo r in expandedRange)
    {
        html.Append("<table border=\"0\">");
        for (int i = r.Top; i <= r.Bottom; i++)
        {
            html.Append("<tr>");
            for (int j = r.Left; j <= r.Right; j++)
            {
                GridStyleInfo style = this.gridControl1.Model[i, j];

                string align = style.VerticalAlignment.ToString();
                string backColor = ColorTranslator.ToHtml(Color.FromArgb(style.BackColor.A, style.BackColor.R, style.BackColor.G, style.BackColor.B));
                string foreColor = ColorTranslator.ToHtml(Color.FromArgb(style.TextColor.A, style.TextColor.R, style.TextColor.G, style.TextColor.B));
                string htmlStyle = BordersAsStyle(style.Borders);
                htmlStyle += " " + FontAsStyle(style.Font, style.TextColor, style.HorizontalAlignment);

                object o = (object)style.FormattedText;
                string tag = "td";

                //Add a non-breaking space (&nbsp;) to empty cells, to make the borders visible.
                if (!style.HasText)
                    o = (object)"&nbsp;";

                if (style.CellType == GridCellTypeName.Header)
                {
                    if (j > this.gridControl1.Cols.HeaderCount && i == 0 && !style.HasText)
                        o = (object)GridRangeInfo.GetAlphaLabel(j);
                    else
                        if (j == 0 && i > this.gridControl1.Rows.HeaderCount && !style.HasText)
                            o = (object)i;
                    tag = "th";
                }

                html.AppendFormat("<" + tag + " width=\"{0}\" height = \"{1}\" valign =\"{2}\" bgcolor=\"{3}\" style=\"{4}\">",
                    this.gridControl1.ColWidths[j], this.gridControl1.RowHeights[i], align, backColor, htmlStyle);

                if (style.CellType == GridCellTypeName.CheckBox || style.CellType == GridCellTypeName.PushButton ||
                    style.CellType == GridCellTypeName.RadioButton || style.CellType == GridCellTypeName.Image ||
                    style.CellType == GridCellTypeName.ComboBox)
                {
                    switch (style.CellType)
                    {
                        case "CheckBox":
                            html.AppendFormat("<input type=\"checkbox\" id=\"checkboxR{0}C{1}\" name=\"checkbox1\" {2}>", i, j, (style.CheckBoxOptions.HasCheckedValue ? (style.CheckBoxOptions.CheckedValue == style.CellValue.ToString() ? "checked" : "") : (style.CellValue.ToString() == "1") ? "checked" : ""));
                            html.AppendFormat(style.HasDescription ? style.Description : "");
                            break;
                        case "Image":
                            if (style.ImageIndex != -1 && style.ImageList != null && style.ImageList.Images.Count > style.ImageIndex)
                            {
                                string srcFile = System.IO.Path.Combine(System.IO.Path.GetTempPath(), System.IO.Path.GetTempFileName() + ".jpg");
                                style.ImageList.Images[style.ImageIndex].Save(srcFile, System.Drawing.Imaging.ImageFormat.Jpeg);
                                html.AppendFormat("<img src=\"{0}\">", srcFile);
                            }
                            break;
                        case "PushButton":

                        //To show button uncomment below.
                            
                        //html.AppendFormat("<input type=\"button\" value=\"{0}\">",style.Description);
                            html.Append(style.Description);
                            break;
                        case "RadioButton":
                            for (int rc = 0; rc < style.ChoiceList.Count; rc++)
                                html.AppendFormat("{0}<input type=\"radio\" id=\"radio{1}R{2}C{3}\" value=\"radio{1}\" name=\"RadioGroup{4}\" {5}>", style.ChoiceList[rc], rc, i, j, i * this.gridControl1.ColCount + j, rc.ToString() == style.CellValue.ToString() ? "checked" : "");
                            break;
                        case "ComboBox":
                            if (style.ChoiceList != null)
                            {
                                html.Append("<select>");
                                html.Append("<OPTION></OPTION>");
                                for (int l = 0; l < style.ChoiceList.Count; l++)
                                    html.AppendFormat("<option value=\"{0}\" {1}>{0}</option>", style.ChoiceList[l], style.ChoiceList[l] == style.CellValue.ToString() ? "selected" : "");
                                html.Append("</select>");
                            }
                            else
                                html.Append(style.FormattedText);
                            break;
                    }
                }
                else
                    html.AppendFormat("{0}", o);

                html.AppendFormat("</" + tag + ">");
            }
            html.Append("</tr>");
        }
        html.Append("</table>");
    }
    return html;
}
```


**Step – 2**

Convert the string which is formed as HTML tags to the HTML file,

```csharp
private void ExportToHTML(object sender, EventArgs e)
{

    //Getting the GridControl table range.
    GridRangeInfoList range = new GridRangeInfoList();
    range.Add(GridRangeInfo.Table());

    //Exporting the GridControl content to HTML.
    System.Diagnostics.Process.Start(CopyHtmlToClipBoard(ExportAsHTML(range).ToString(), true));
}

public static string CopyHtmlToClipBoard(string html, bool e)
{
  if (html != "")
  {
       Encoding enc = Encoding.UTF8;
       string begin = e ? "<!--Syncfusion Essential Grid-->" : "Version:0.9\r\nStartHTML:{0:000000}\r\nEndHTML:{1:000000}"
                + "\r\nStartFragment:{2:000000}\r\nEndFragment:{3:000000}\r\n";
            string html_begin = "<html>\r\n<head>\r\n"
                + "<meta http-equiv=\"Content-Type\""
                + " content=\"text/html; charset=" + enc.WebName + "\">\r\n"
                + "<title>Syncfusion Essential Grid</title>\r\n</head>\r\n<body>\r\n"
                + "<!--StartFragment-->";

       string html_end = "<!--EndFragment-->\r\n</body>\r\n</html>\r\n";

       string begin_sample = String.Format(begin, 0, 0, 0, 0);

       int count_begin = enc.GetByteCount(begin_sample);
       int count_html_begin = enc.GetByteCount(html_begin);
       int count_html = enc.GetByteCount(html);
       int count_html_end = enc.GetByteCount(html_end);

       string html_total = String.Format( begin
                , count_begin
                , count_begin + count_html_begin + count_html + count_html_end
                , count_begin + count_html_begin
                , count_begin + count_html_begin + count_html
                ) + html_begin + html + html_end;

        DataObject obj = new DataObject();
        obj.SetData(DataFormats.Html, new System.IO.MemoryStream(
            enc.GetBytes(html_total)));
        obj.SetData(DataFormats.Text, true, html_total);
        Clipboard.SetDataObject(obj, true);
        string htmlFile = System.IO.Path.Combine(System.IO.Path.GetTempPath(), System.IO.Path.GetTempFileName() + ".html");
        System.IO.StreamWriter streamWriter = System.IO.File.CreateText(htmlFile);
        streamWriter.Write(html_total);
        streamWriter.Close();
        return htmlFile;
    }
    return "";
}
```

## Word Export

```csharp
//Create Converter to export the contents of Grid to Excel.
GridWordConverter wordConverter = new GridWordConverter();
wordConverter.GridToWord("Sample.doc", gridControl1);
```
### Displaying the Header and Footer

```csharp
// “true” defines the ShowHeader and ShowFooter to export.

// i.e. GridWordConverter(bool showHeader, bool showFooter).
GridWordConverter converter = new GridWordConverter(true,true);

//To Set the Header and Footer for the Exported word document.
wordConverter.DrawHeader += new GridWordConverterBase.DrawDocHeaderFooterEventHandler(converter_DrawHeader);
wordConverter.DrawFooter += new GridWordConverterBase.DrawDocHeaderFooterEventHandler(converter_DrawFooter);
converter.GridToWord("Sample.doc", gridControl1);
void converter_DrawFooter(object sender, DocHeaderFooterEventArgs e)
{
     e.Footer.AddParagraph().AppendText("Copyright 2001-2015");
}
void converter_DrawHeader(object sender, DocHeaderFooterEventArgs e)
{
     e.Header.AddParagraph().AppendText("Syncfusion Inc.");
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
