# Exporting and Printing

## Table of Contents
- [Overview](#overview)
- [Exporting to Excel](#exporting-to-excel)
- [Exporting to PDF](#exporting-to-pdf)
- [Exporting to Word](#exporting-to-word)
- [Printing](#printing)
- [Custom Export Options](#custom-export-options)

## Overview

The Pivot Grid supports exporting data to Excel, PDF, and Word formats, plus comprehensive printing capabilities for generating reports and sharing analysis results.

## Exporting to Excel

Export pivot grid data to Excel workbooks with formatting preserved.

### Required Assemblies

Add references to your project:
- `Syncfusion.PivotConverter.Windows.dll`
- `Syncfusion.XlsIO.Base.dll`

### Basic Excel Export

```csharp
using Syncfusion.PivotConverter;
using Syncfusion.XlsIO;

// Create exporter
ExcelExport excelExport = new ExcelExport(pivotGridControl1, ExcelVersion.Excel2016);

// Export to file
excelExport.Export(@"C:\Reports\PivotGrid.xlsx");
```

### Export Modes

#### Cell-by-Cell Export

Exports the visual representation exactly as displayed:

```csharp
ExcelExport excelExport = new ExcelExport(pivotGridControl1, ExcelVersion.Excel2016);
excelExport.ExportMode = ExportModes.Cell;
excelExport.Export(@"C:\Reports\PivotGrid_CellMode.xlsx");
```

**Features:**
- Preserves all formatting
- Exports as static values
- No pivot table functionality in Excel

#### Pivot Table Export

Creates a native Excel pivot table:

```csharp
ExcelExport excelExport = new ExcelExport(pivotGridControl1, ExcelVersion.Excel2016);
excelExport.ExportMode = ExportModes.PivotTable;
excelExport.Export(@"C:\Reports\PivotGrid_PivotMode.xlsx");
```

**Features:**
- Creates interactive Excel pivot table
- Users can reorganize fields in Excel
- Preserves drill-down capability
- Maintains sorting and filtering

### Export with Progress Tracking

```csharp
private void ExportToExcelWithProgress()
{
    ExcelExport excelExport = new ExcelExport(pivotGridControl1, ExcelVersion.Excel2016);
    
    // Show progress
    ProgressBar progressBar = new ProgressBar { Visible = true };
    
    try
    {
        excelExport.Export(@"C:\Reports\PivotGrid.xlsx");
        MessageBox.Show("Export completed successfully!", "Success", 
                       MessageBoxButtons.OK, MessageBoxIcon.Information);
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Export failed: {ex.Message}", "Error", 
                       MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
    finally
    {
        progressBar.Visible = false;
    }
}
```

## Exporting to PDF

Export pivot grid to PDF format for sharing and archival.

### Required Assemblies

- `Syncfusion.PivotConverter.Windows.dll`
- `Syncfusion.Pdf.Base.dll`

### Basic PDF Export

```csharp
using Syncfusion.PivotConverter;

// Create PDF exporter
PivotPdfExport pdfExport = new PivotPdfExport(pivotGridControl1);

// Export to file
pdfExport.Export(@"C:\Reports\PivotGrid.pdf");
```

### PDF Export with Options

```csharp
using Syncfusion.Pdf;

PivotPdfExport pdfExport = new PivotPdfExport(pivotGridControl1);

// Set page orientation
pdfExport.PageOrientation = PdfPageOrientation.Landscape;

// Set page size
pdfExport.PageSize = new SizeF(842, 595);  // A4 Landscape

// Export
pdfExport.Export(@"C:\Reports\PivotGrid_Landscape.pdf");
```

### PDF with Custom Margins

```csharp
PivotPdfExport pdfExport = new PivotPdfExport(pivotGridControl1);

// Set margins (in points, 72 points = 1 inch)
pdfExport.Margins = new PdfMargins
{
    Left = 36,    // 0.5 inch
    Right = 36,
    Top = 72,     // 1 inch
    Bottom = 72
};

pdfExport.Export(@"C:\Reports\PivotGrid_CustomMargins.pdf");
```

## Exporting to Word

Export pivot grid data to Microsoft Word documents.

### Required Assemblies

- `Syncfusion.PivotConverter.Windows.dll`
- `Syncfusion.DocIO.Base.dll`

### Basic Word Export

```csharp
using Syncfusion.PivotConverter;

// Create Word exporter
PivotWordExport wordExport = new PivotWordExport(pivotGridControl1);

// Export to file
wordExport.Export(@"C:\Reports\PivotGrid.docx");
```

### Word Export with Formatting

```csharp
PivotWordExport wordExport = new PivotWordExport(pivotGridControl1);

// Configure export options
wordExport.ExportWithFormatting = true;  // Preserve colors and fonts

// Export
wordExport.Export(@"C:\Reports\PivotGrid_Formatted.docx");
```

## Printing

Print pivot grid directly or with custom settings.

### Basic Printing

```csharp
using Syncfusion.Windows.Forms.PivotAnalysis;

// Simple print
pivotGridControl1.Print();
```

### Print with Dialog

```csharp
// Show print dialog first
PrintDialog printDialog = new PrintDialog();

if (printDialog.ShowDialog() == DialogResult.OK)
{
    pivotGridControl1.Print(printDialog.PrinterSettings);
}
```

### Print Preview

```csharp
// Show print preview dialog
PrintPreviewDialog previewDialog = new PrintPreviewDialog();
previewDialog.Document = pivotGridControl1.GetPrintDocument();

if (previewDialog.ShowDialog() == DialogResult.OK)
{
    pivotGridControl1.Print();
}
```

### Custom Print Settings

```csharp
using System.Drawing.Printing;

// Configure print settings
PrintDocument printDoc = pivotGridControl1.GetPrintDocument();

printDoc.DefaultPageSettings.Landscape = true;
printDoc.DefaultPageSettings.Margins = new Margins(50, 50, 50, 50);

// Add header/footer
printDoc.PrintPage += (s, e) =>
{
    // Draw header
    string header = "Pivot Grid Report - " + DateTime.Now.ToShortDateString();
    Font headerFont = new Font("Arial", 12, FontStyle.Bold);
    e.Graphics.DrawString(header, headerFont, Brushes.Black, 50, 20);
    
    // Draw footer (page number)
    string footer = $"Page {e.PageSettings.PrinterSettings.FromPage}";
    Font footerFont = new Font("Arial", 9);
    e.Graphics.DrawString(footer, footerFont, Brushes.Gray, 50, 
                         e.PageBounds.Height - 50);
};

// Print
printDoc.Print();
```

## Custom Export Options

### Save FileDialog Integration

```csharp
private void ExportWithSaveDialog()
{
    SaveFileDialog saveDialog = new SaveFileDialog();
    saveDialog.Filter = "Excel Files (*.xlsx)|*.xlsx|" +
                       "PDF Files (*.pdf)|*.pdf|" +
                       "Word Files (*.docx)|*.docx";
    saveDialog.DefaultExt = "xlsx";
    saveDialog.FileName = $"PivotReport_{DateTime.Now:yyyyMMdd}";
    
    if (saveDialog.ShowDialog() == DialogResult.OK)
    {
        string extension = Path.GetExtension(saveDialog.FileName).ToLower();
        
        try
        {
            switch (extension)
            {
                case ".xlsx":
                    ExportToExcel(saveDialog.FileName);
                    break;
                case ".pdf":
                    ExportToPdf(saveDialog.FileName);
                    break;
                case ".docx":
                    ExportToWord(saveDialog.FileName);
                    break;
            }
            
            MessageBox.Show("Export completed successfully!", "Success");
            
            // Open file
            if (MessageBox.Show("Open exported file?", "Export Complete",
                               MessageBoxButtons.YesNo) == DialogResult.Yes)
            {
                System.Diagnostics.Process.Start(saveDialog.FileName);
            }
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Export failed: {ex.Message}", "Error");
        }
    }
}

private void ExportToExcel(string fileName)
{
    ExcelExport excelExport = new ExcelExport(pivotGridControl1, ExcelVersion.Excel2016);
    excelExport.ExportMode = ExportModes.PivotTable;
    excelExport.Export(fileName);
}

private void ExportToPdf(string fileName)
{
    PivotPdfExport pdfExport = new PivotPdfExport(pivotGridControl1);
    pdfExport.PageOrientation = PdfPageOrientation.Landscape;
    pdfExport.Export(fileName);
}

private void ExportToWord(string fileName)
{
    PivotWordExport wordExport = new PivotWordExport(pivotGridControl1);
    wordExport.ExportWithFormatting = true;
    wordExport.Export(fileName);
}
```

### Batch Export

```csharp
private void ExportAllFormats(string baseFileName)
{
    string directory = Path.GetDirectoryName(baseFileName);
    string fileNameWithoutExt = Path.GetFileNameWithoutExtension(baseFileName);
    
    try
    {
        // Export to Excel
        string excelPath = Path.Combine(directory, fileNameWithoutExt + ".xlsx");
        ExportToExcel(excelPath);
        
        // Export to PDF
        string pdfPath = Path.Combine(directory, fileNameWithoutExt + ".pdf");
        ExportToPdf(pdfPath);
        
        // Export to Word
        string wordPath = Path.Combine(directory, fileNameWithoutExt + ".docx");
        ExportToWord(wordPath);
        
        MessageBox.Show($"Exported to:\n{excelPath}\n{pdfPath}\n{wordPath}", 
                       "Batch Export Complete");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Batch export failed: {ex.Message}", "Error");
    }
}
```

### Email Export

```csharp
using System.Net.Mail;

private void EmailExport()
{
    // Export to temp file
    string tempFile = Path.Combine(Path.GetTempPath(), 
                                   $"PivotReport_{DateTime.Now:yyyyMMdd_HHmmss}.pdf");
    
    PivotPdfExport pdfExport = new PivotPdfExport(pivotGridControl1);
    pdfExport.Export(tempFile);
    
    // Create email
    try
    {
        MailMessage mail = new MailMessage();
        mail.To.Add("recipient@example.com");
        mail.Subject = "Pivot Grid Report";
        mail.Body = "Please find the attached pivot grid report.";
        mail.Attachments.Add(new Attachment(tempFile));
        
        SmtpClient smtp = new SmtpClient("smtp.example.com");
        smtp.Send(mail);
        
        MessageBox.Show("Report emailed successfully!", "Success");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Email failed: {ex.Message}", "Error");
    }
    finally
    {
        // Clean up temp file
        if (File.Exists(tempFile))
            File.Delete(tempFile);
    }
}
```

## Complete Example with UI

```csharp
public class ExportPrintForm : Form
{
    private PivotGridControl pivotGridControl1;
    
    private void AddExportButtons()
    {
        // Excel button
        Button btnExcel = new Button { Text = "Export to Excel", Location = new Point(10, 10) };
        btnExcel.Click += (s, e) => ExportWithDialog("Excel");
        
        // PDF button
        Button btnPdf = new Button { Text = "Export to PDF", Location = new Point(120, 10) };
        btnPdf.Click += (s, e) => ExportWithDialog("PDF");
        
        // Word button
        Button btnWord = new Button { Text = "Export to Word", Location = new Point(230, 10) };
        btnWord.Click += (s, e) => ExportWithDialog("Word");
        
        // Print button
        Button btnPrint = new Button { Text = "Print", Location = new Point(340, 10) };
        btnPrint.Click += (s, e) => PrintWithPreview();
        
        this.Controls.AddRange(new Control[] { btnExcel, btnPdf, btnWord, btnPrint });
    }
    
    private void ExportWithDialog(string format)
    {
        SaveFileDialog dlg = new SaveFileDialog();
        
        switch (format)
        {
            case "Excel":
                dlg.Filter = "Excel Files (*.xlsx)|*.xlsx";
                dlg.DefaultExt = "xlsx";
                break;
            case "PDF":
                dlg.Filter = "PDF Files (*.pdf)|*.pdf";
                dlg.DefaultExt = "pdf";
                break;
            case "Word":
                dlg.Filter = "Word Files (*.docx)|*.docx";
                dlg.DefaultExt = "docx";
                break;
        }
        
        if (dlg.ShowDialog() == DialogResult.OK)
        {
            Cursor = Cursors.WaitCursor;
            try
            {
                switch (format)
                {
                    case "Excel":
                        ExportToExcel(dlg.FileName);
                        break;
                    case "PDF":
                        ExportToPdf(dlg.FileName);
                        break;
                    case "Word":
                        ExportToWord(dlg.FileName);
                        break;
                }
                MessageBox.Show("Export completed!", "Success");
            }
            finally
            {
                Cursor = Cursors.Default;
            }
        }
    }
    
    private void PrintWithPreview()
    {
        PrintPreviewDialog preview = new PrintPreviewDialog();
        preview.Document = pivotGridControl1.GetPrintDocument();
        preview.ShowDialog();
    }
}
```

## Best Practices

1. **Choose Appropriate Format:**
   - Excel: For further analysis and manipulation
   - PDF: For sharing and archival
   - Word: For reports and documentation

2. **Use Pivot Table Mode:** For Excel exports when users need interactivity

3. **Handle Large Data:** Consider pagination or data limits for exports

4. **Provide Progress Feedback:** Show progress bars for large exports

5. **Clean Up Resources:** Dispose of export objects properly

6. **Test Print Layouts:** Always use print preview before final printing
