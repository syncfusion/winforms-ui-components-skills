# Exporting

## Table of Contents
- [Overview](#overview)
- [Excel Export](#excel-export)
- [PDF Export](#pdf-export)
- [Word Export](#word-export)
- [CSV Export](#csv-export)
- [Export Options](#export-options)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl supports exporting data to multiple formats including Excel, PDF, Word, and CSV. Exports preserve formatting, grouping, summaries, and hierarchical structure.

### Key Components

- **GroupingGridExcelConverterControl** - Excel export converter
- **GroupingGridPDFConverter** - PDF export converter
- **GroupingGridWordConverter** - Word export converter
- **ConverterOptions** - Control what elements to export
- **ExportStyle** - Include/exclude cell styles

## Excel Export

### Basic Excel Export

```csharp
using Syncfusion.GroupingGridExcelConverter;

// Create converter
GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();

// Export to Excel
converter.GroupingGridToExcel(gridGroupingControl1, "Output.xls", ConverterOptions.Visible);
```

### ConverterOptions

Control which grid elements to export:

```csharp
// Export everything
converter.GroupingGridToExcel(gridGroupingControl1, "Output.xls", ConverterOptions.Default);

// Export only visible rows/columns (collapsed groups hidden)
converter.GroupingGridToExcel(gridGroupingControl1, "Output.xls", ConverterOptions.Visible);

// Export with column headers
converter.GroupingGridToExcel(gridGroupingControl1, "Output.xls", ConverterOptions.ColumnHeaders);

// Export with row headers
converter.GroupingGridToExcel(gridGroupingControl1, "Output.xls", ConverterOptions.RowHeaders);

// Combined options
converter.GroupingGridToExcel(gridGroupingControl1, "Output.xls", 
    ConverterOptions.Visible | ConverterOptions.ColumnHeaders | ConverterOptions.RowHeaders);
```

### Export with Styles

```csharp
GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();

// Export cell styles (colors, fonts, borders)
converter.ExportStyle = true;

// Export borders
converter.ExportBorders = true;

// Export images
converter.ExportImage = true;

converter.GroupingGridToExcel(gridGroupingControl1, "Styled.xls", ConverterOptions.Default);
```

### Export Without Styles

```csharp
GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();

// Export data only, no formatting
converter.ExportStyle = false;

converter.GroupingGridToExcel(gridGroupingControl1, "DataOnly.xls", ConverterOptions.Default);
```

### Column Width and Row Height

```csharp
using Syncfusion.GroupingGridExcelConverter;

GridGroupingExcelConverterControl excelConverter = new GridGroupingExcelConverterControl();

// Export with original widths/heights
excelConverter.CanExportColumnWidth = true;
excelConverter.CanExportRowHeight = true;

// Or set default sizes
excelConverter.CanExportColumnWidth = false;
excelConverter.CanExportRowHeight = false;
excelConverter.DefaultColumnWidth = 20;
excelConverter.DefaultRowHeight = 20;

ExcelExportingOptions options = new ExcelExportingOptions();
excelConverter.ExportToExcel(gridGroupingControl1, "Sized.xls", options);
```

### Export to Specific Excel Version

```csharp
GridGroupingExcelConverterControl excelConverter = new GridGroupingExcelConverterControl();

// Excel 2007 (.xlsx)
excelConverter.ExcelVersion = Syncfusion.XlsIO.ExcelVersion.Excel2007;

// Excel 2010
excelConverter.ExcelVersion = Syncfusion.XlsIO.ExcelVersion.Excel2010;

// Excel 2013
excelConverter.ExcelVersion = Syncfusion.XlsIO.ExcelVersion.Excel2013;

// Excel 97-2003 (.xls)
excelConverter.ExcelVersion = Syncfusion.XlsIO.ExcelVersion.Excel97to2003;

excelConverter.ExportToExcel(gridGroupingControl1, "Output.xlsx", new ExcelExportingOptions());
```

### Export Groups

```csharp
GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();

// Export groups as Excel groups (collapsible)
converter.ExportGroupPlusMinus = true;

// Export nested tables with expand/collapse
converter.ExportRecordPlusMinus = true;

converter.GroupingGridToExcel(gridGroupingControl1, "Grouped.xls", ConverterOptions.Default);
```

### Export Preview Rows

```csharp
// Enable preview rows in grid
gridGroupingControl1.TableOptions.ShowRecordPreviewRow = true;

GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();

// Export preview rows
converter.ExportPreviewRows = true;

converter.GroupingGridToExcel(gridGroupingControl1, "WithPreview.xls", ConverterOptions.Default);
```

### Customize Preview Row Export

```csharp
GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();
converter.ExportPreviewRows = true;

// Customize preview row appearance
converter.QueryExportPreviewRowInfo += Converter_QueryExportPreviewRowInfo;

void Converter_QueryExportPreviewRowInfo(object sender, GroupingGridExportPreviewRowQueryInfoEventArgs e)
{
    if (e.Element.Kind == DisplayElementKind.RecordPreview)
    {
        Element el = e.Element;
        Record record = el.ParentRecord;
        
        // Custom preview content
        e.Style.CellValue = $"Preview: {record.GetValue("Notes")}";
        e.Style.BackColor = Color.LightYellow;
        e.Handled = true;
    }
}

converter.GroupingGridToExcel(gridGroupingControl1, "CustomPreview.xls", ConverterOptions.Default);
```

## PDF Export

### Basic PDF Export

```csharp
using Syncfusion.GroupingGridPDFConverter;

// Create PDF converter
GridGroupingPDFConverter pdfConverter = new GridGroupingPDFConverter();

// Export to PDF
pdfConverter.ExportToPdf("Output.pdf", gridGroupingControl1);
```

### PDF Export Options

```csharp
GridGroupingPDFConverter pdfConverter = new GridGroupingPDFConverter();

// Export with styles
pdfConverter.ExportStyle = true;

// Export borders
pdfConverter.ExportBorders = true;

// Fit columns to page
pdfConverter.FitColumnWidthToPage = true;

pdfConverter.ExportToPdf("Formatted.pdf", gridGroupingControl1);
```

### PDF Page Setup

```csharp
using Syncfusion.Pdf;

GridGroupingPDFConverter pdfConverter = new GridGroupingPDFConverter();

// Page orientation
pdfConverter.PageOrientation = PdfPageOrientation.Landscape;

// Page size
pdfConverter.PageSize = PdfPageSize.A4;

// Margins
pdfConverter.Margins = new PdfMargins { Left = 40, Right = 40, Top = 40, Bottom = 40 };

pdfConverter.ExportToPdf("CustomPage.pdf", gridGroupingControl1);
```

## Word Export

### Basic Word Export

```csharp
using Syncfusion.GroupingGridWordConverter;

// Create Word converter
GridGroupingWordConverter wordConverter = new GridGroupingWordConverter();

// Export to Word
wordConverter.ExportToWord("Output.doc", gridGroupingControl1);
```

### Word Export Options

```csharp
GridGroupingWordConverter wordConverter = new GridGroupingWordConverter();

// Export styles
wordConverter.ExportStyle = true;

// Export borders
wordConverter.ExportBorders = true;

// Table style
wordConverter.TableStyle = Syncfusion.DocIO.DLS.TableStyle.TableGrid;

wordConverter.ExportToWord("Styled.docx", gridGroupingControl1);
```

## CSV Export

### Basic CSV Export

```csharp
using System.IO;
using System.Text;

// Manual CSV export
void ExportToCSV(string filePath)
{
    StringBuilder csv = new StringBuilder();
    
    // Headers
    foreach (GridColumnDescriptor col in gridGroupingControl1.TableDescriptor.Columns)
    {
        if (gridGroupingControl1.TableDescriptor.VisibleColumns.Contains(col.Name))
        {
            csv.Append($"\"{col.HeaderText}\",");
        }
    }
    csv.AppendLine();
    
    // Data rows
    foreach (Record record in gridGroupingControl1.Table.Records)
    {
        foreach (GridColumnDescriptor col in gridGroupingControl1.TableDescriptor.Columns)
        {
            if (gridGroupingControl1.TableDescriptor.VisibleColumns.Contains(col.Name))
            {
                string value = record.GetValue(col.MappingName)?.ToString() ?? "";
                value = value.Replace("\"", "\"\"");  // Escape quotes
                csv.Append($"\"{value}\",");
            }
        }
        csv.AppendLine();
    }
    
    File.WriteAllText(filePath, csv.ToString(), Encoding.UTF8);
}

// Call export
ExportToCSV("Output.csv");
```

### CSV with Grouping

```csharp
void ExportToCSVWithGroups(string filePath)
{
    StringBuilder csv = new StringBuilder();
    
    // Headers
    csv.AppendLine("Group,Column1,Column2,Column3");
    
    // Export groups
    foreach (Group group in gridGroupingControl1.Table.TopLevelGroup.Groups)
    {
        string groupName = $"{group.Name}: {group.Category}";
        
        foreach (Record record in group.Records)
        {
            csv.Append($"\"{groupName}\",");
            
            foreach (FieldDescriptor field in gridGroupingControl1.TableDescriptor.Fields)
            {
                string value = record.GetValue(field.Name)?.ToString() ?? "";
                value = value.Replace("\"", "\"\"");
                csv.Append($"\"{value}\",");
            }
            csv.AppendLine();
        }
    }
    
    File.WriteAllText(filePath, csv.ToString(), Encoding.UTF8);
}
```

## Export Options

### Export Events

Customize export with events:

```csharp
GridGroupingExcelConverterControl converter = new GridGroupingExcelConverterControl();

// Customize cell export
converter.QueryImportExportCellInfo += Converter_QueryImportExportCellInfo;

void Converter_QueryImportExportCellInfo(object sender, GridImportExportCellInfoEventArgs e)
{
    if (e.Action == GridConverterAction.Export)
    {
        GridTableCellStyleInfoIdentity id = e.GridCell.CellIdentity as GridTableCellStyleInfoIdentity;
        
        if (id != null && id.Column != null && id.Column.Name == "Salary")
        {
            // Mask salary data
            e.ExcelCell.Value = "****";
            e.Handled = true;
        }
    }
}

converter.GroupingGridToExcel(gridGroupingControl1, "Masked.xls", ConverterOptions.Default);
```

### Export Summary Rows

```csharp
GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();

// Export summaries
converter.ExportSummary = true;

// Summary styles
converter.SummaryBackColor = Color.LightGray;
converter.SummaryFontBold = true;

converter.GroupingGridToExcel(gridGroupingControl1, "WithSummaries.xls", ConverterOptions.Default);
```

## Common Scenarios

### Scenario 1: Export with Progress Indicator

```csharp
void ExportWithProgress()
{
    ProgressDialog progress = new ProgressDialog();
    progress.Show();
    progress.Status = "Exporting to Excel...";
    
    Task.Run(() =>
    {
        try
        {
            GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();
            converter.ExportBorders = true;
            converter.ExportStyle = true;
            
            converter.GroupingGridToExcel(gridGroupingControl1, "Export.xls", ConverterOptions.Visible);
            
            Invoke((Action)(() =>
            {
                progress.Close();
                MessageBox.Show("Export completed successfully");
            }));
        }
        catch (Exception ex)
        {
            Invoke((Action)(() =>
            {
                progress.Close();
                MessageBox.Show($"Export failed: {ex.Message}");
            }));
        }
    });
}
```

### Scenario 2: Export Selected Records Only

```csharp
void ExportSelectedRecords()
{
    if (gridGroupingControl1.Table.SelectedRecords.Count == 0)
    {
        MessageBox.Show("No records selected");
        return;
    }
    
    // Create temporary table with selected records
    DataTable selectedData = gridGroupingControl1.Table.Records[0].ParentTable.UnderlyingTable.Clone();
    
    foreach (Record record in gridGroupingControl1.Table.SelectedRecords)
    {
        DataRowView drv = record.GetData() as DataRowView;
        if (drv != null)
        {
            selectedData.ImportRow(drv.Row);
        }
    }
    
    // Bind to temporary grid
    GridGroupingControl tempGrid = new GridGroupingControl();
    tempGrid.DataSource = selectedData;
    
    // Export temporary grid
    GroupingGridExcelConverterControl converter = new GroupingGridExcelConverterControl();
    converter.GroupingGridToExcel(tempGrid, "SelectedRecords.xls", ConverterOptions.Default);
    
    tempGrid.Dispose();
    
    MessageBox.Show("Selected records exported");
}
```

### Scenario 3: Export to Multiple Formats

```csharp
void ExportMultipleFormats(string basePath)
{
    try
    {
        Cursor = Cursors.WaitCursor;
        
        // Excel
        GroupingGridExcelConverterControl excelConverter = new GroupingGridExcelConverterControl();
        excelConverter.ExportStyle = true;
        excelConverter.GroupingGridToExcel(gridGroupingControl1, $"{basePath}.xls", ConverterOptions.Visible);
        
        // PDF
        GridGroupingPDFConverter pdfConverter = new GridGroupingPDFConverter();
        pdfConverter.ExportStyle = true;
        pdfConverter.ExportToPdf($"{basePath}.pdf", gridGroupingControl1);
        
        // CSV
        ExportToCSV($"{basePath}.csv");
        
        MessageBox.Show("Exported to Excel, PDF, and CSV");
    }
    finally
    {
        Cursor = Cursors.Default;
    }
}
```

### Scenario 4: Export with Custom Headers/Footers

```csharp
void ExportWithHeaderFooter()
{
    GridGroupingExcelConverterControl converter = new GridGroupingExcelConverterControl();
    
    using (ExcelEngine excelEngine = new ExcelEngine())
    {
        IWorkbook workbook = excelEngine.Excel.Workbooks.Create(1);
        IWorksheet worksheet = workbook.Worksheets[0];
        
        // Add header
        worksheet.Range["A1"].Text = "Company Name: Acme Corp";
        worksheet.Range["A1"].CellStyle.Font.Bold = true;
        worksheet.Range["A2"].Text = $"Export Date: {DateTime.Now:yyyy-MM-dd}";
        
        // Export grid starting at row 4
        converter.ExportToWorksheet(gridGroupingControl1, worksheet, 4, 1, ConverterOptions.Visible);
        
        // Add footer
        int lastRow = worksheet.UsedRange.LastRow + 2;
        worksheet.Range[$"A{lastRow}"].Text = "End of Report";
        
        // Save
        workbook.SaveAs("WithHeaders.xlsx");
    }
    
    MessageBox.Show("Export complete with headers/footers");
}
```

## Best Practices

### Performance

1. **Large Datasets**: Show progress indicator for exports exceeding 10,000 records.

2. **Async Export**: Run export on background thread to keep UI responsive:
   ```csharp
   Task.Run(() => ExportToExcel());
   ```

3. **Dispose Converters**: Dispose converter objects after use to free resources.

4. **Limit Styling**: For very large exports, disable `ExportStyle` for faster processing.

### File Management

1. **Unique Filenames**: Add timestamp to avoid overwriting:
   ```csharp
   string filename = $"Export_{DateTime.Now:yyyyMMdd_HHmmss}.xlsx";
   ```

2. **Check Permissions**: Verify write permissions before exporting:
   ```csharp
   string directory = Path.GetDirectoryName(filePath);
   if (!Directory.Exists(directory))
   {
       Directory.CreateDirectory(directory);
   }
   ```

3. **Error Handling**: Wrap exports in try-catch with meaningful messages.

### User Experience

1. **Format Choice**: Let users select export format:
   ```csharp
   SaveFileDialog dlg = new SaveFileDialog();
   dlg.Filter = "Excel Files|*.xlsx|PDF Files|*.pdf|CSV Files|*.csv";
   if (dlg.ShowDialog() == DialogResult.OK)
   {
       // Export based on selected filter
   }
   ```

2. **Success Feedback**: Confirm export completion:
   ```csharp
   MessageBox.Show($"Data exported to {filePath}");
   ```

3. **Open File**: Offer to open exported file:
   ```csharp
   if (MessageBox.Show("Export complete. Open file?", "Export", MessageBoxButtons.YesNo) == DialogResult.Yes)
   {
       System.Diagnostics.Process.Start(filePath);
   }
   ```

### Data Integrity

- Validate data before export (check for null values, invalid formats)
- Escape special characters in CSV exports (quotes, commas, newlines)
- Test exports with edge cases (empty grid, single record, maximum records)
- Preserve number formats (don't let Excel auto-format as dates)
