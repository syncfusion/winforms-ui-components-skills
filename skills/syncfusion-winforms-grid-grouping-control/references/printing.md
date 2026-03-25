# Printing

## Overview

GridGroupingControl provides comprehensive printing support with preview dialogs, page setup, headers/footers, and multi-page printing. Supports hierarchical grids and nested tables.

### Key Components

- **GridPrintDocument** - Basic print document for grid
- **GridPrintDocumentAdv** - Advanced printing with scaling and headers/footers
- **PrintDialog** - Standard Windows print dialog
- **PrintPreviewDialog** - Print preview interface
- **PageSetup** - Configure margins, orientation, paper size

## Basic Printing

### Print with Dialog

```csharp
using Syncfusion.Windows.Forms.Grid;
using System.Windows.Forms;

// Create print document
GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);

// Show print dialog
PrintDialog printDialog = new PrintDialog();
printDialog.Document = pd;

if (printDialog.ShowDialog() == DialogResult.OK)
{
    pd.Print();
}
```

### Direct Print

```csharp
// Print without dialog
GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);
pd.Print();
```

## Print Preview

### Show Print Preview

```csharp
// Create print document
GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl, true);

// Show preview dialog
PrintPreviewDialog printPreviewDialog = new PrintPreviewDialog();
printPreviewDialog.Document = pd;
printPreviewDialog.ShowDialog();
```

### Print via Keyboard (Ctrl+P)

```csharp
// Handle Ctrl+P keyboard shortcut
gridGroupingControl1.TableControl.KeyDown += TableControl_KeyDown;

void TableControl_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Control && e.KeyCode == Keys.P)
    {
        GridPrintDocument printDocument = new GridPrintDocument(gridGroupingControl1.TableControl);
        PrintPreviewDialog previewDialog = new PrintPreviewDialog();
        previewDialog.Document = printDocument;
        previewDialog.Show();
        
        e.Handled = true;  // Prevent default handling
    }
}
```

## Advanced Print Document

Provides scaling, headers/footers, and enhanced options.

### Enable Advanced Printing

```csharp
using Syncfusion.GridHelperClasses;

// Create advanced print document
GridPrintDocumentAdv printDocument = new GridPrintDocumentAdv(gridGroupingControl1.TableControl);

// Show preview
PrintPreviewDialog previewDialog = new PrintPreviewDialog();
previewDialog.Document = printDocument;
previewDialog.Show();
```

### Scale Columns to Fit Page

```csharp
GridPrintDocumentAdv printDocument = new GridPrintDocumentAdv(gridGroupingControl1.TableControl);

// Scale all columns to fit within one page width
printDocument.ScaleColumnsToFitPage = true;

printDocument.Print();
```

### Fit to Specific Number of Pages

```csharp
GridPrintDocumentAdv printDocument = new GridPrintDocumentAdv(gridGroupingControl1.TableControl);

// Fit columns to page
printDocument.PrintColumnToFitPage = true;

// Fit all content into minimum number of pages (e.g., 1 page)
printDocument.PagesToFit = 1;

printDocument.Print();
```

## Page Setup

### Page Orientation

```csharp
GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);

// Landscape orientation
pd.DefaultPageSettings.Landscape = true;

// Portrait orientation
pd.DefaultPageSettings.Landscape = false;

PrintDialog printDialog = new PrintDialog();
printDialog.Document = pd;
printDialog.ShowDialog();
```

### Page Margins

```csharp
using System.Drawing.Printing;

GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);

// Set margins (in hundredths of an inch)
pd.DefaultPageSettings.Margins = new Margins(50, 50, 50, 50);  // Left, Right, Top, Bottom

pd.Print();
```

### Paper Size

```csharp
GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);

// Set paper size
pd.DefaultPageSettings.PaperSize = new PaperSize("A4", 827, 1169);  // Width, Height in hundredths of inch

// Or use standard sizes
foreach (PaperSize ps in pd.PrinterSettings.PaperSizes)
{
    if (ps.Kind == PaperKind.A4)
    {
        pd.DefaultPageSettings.PaperSize = ps;
        break;
    }
}

pd.Print();
```

## Print Options

### Print with Frame

```csharp
// Print grid with outer border frame
gridGroupingControl1.TableModel.Properties.PrintFrame = true;

GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);
pd.Print();
```

### Black and White Printing

```csharp
// Print in black and white (no colors)
gridGroupingControl1.TableModel.Properties.BlackWhite = true;

GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);
pd.Print();
```

### Print Specific Columns

```csharp
// Hide columns before printing
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("InternalID");
gridGroupingControl1.TableDescriptor.VisibleColumns.Remove("Notes");

GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);
pd.Print();

// Restore columns after printing
gridGroupingControl1.TableDescriptor.VisibleColumns.Add("InternalID");
gridGroupingControl1.TableDescriptor.VisibleColumns.Add("Notes");
```

## Headers and Footers

### Add Page Headers

```csharp
using Syncfusion.GridHelperClasses;

GridPrintDocumentAdv printDocument = new GridPrintDocumentAdv(gridGroupingControl1.TableControl);

// Enable headers
printDocument.ShowHeaderRowOnAllPages = true;

// Set header text
printDocument.HeaderText = "Company Name - Sales Report";

// Header height
printDocument.HeaderHeight = 40;

// Header style
printDocument.HeaderBackColor = Color.LightBlue;
printDocument.HeaderForeColor = Color.DarkBlue;
printDocument.HeaderFont = new Font("Arial", 14, FontStyle.Bold);

printDocument.Print();
```

### Add Page Footers

```csharp
GridPrintDocumentAdv printDocument = new GridPrintDocumentAdv(gridGroupingControl1.TableControl);

// Enable footers
printDocument.ShowFooterOnAllPages = true;

// Set footer text with page number
printDocument.FooterText = "Page {page} of {pagecount}";

// Footer height
printDocument.FooterHeight = 30;

// Footer style
printDocument.FooterBackColor = Color.LightGray;
printDocument.FooterForeColor = Color.Black;
printDocument.FooterFont = new Font("Arial", 10);

printDocument.Print();
```

### Custom Header/Footer Drawing

```csharp
GridPrintDocumentAdv printDocument = new GridPrintDocumentAdv(gridGroupingControl1.TableControl);

// Draw custom header
printDocument.DrawHeader += PrintDocument_DrawHeader;

void PrintDocument_DrawHeader(object sender, DrawRectangleEventArgs e)
{
    Graphics g = e.Graphics;
    Rectangle rect = e.Rectangle;
    
    // Draw company logo
    Image logo = Image.FromFile("logo.png");
    g.DrawImage(logo, rect.X, rect.Y, 100, 40);
    
    // Draw title
    Font font = new Font("Arial", 16, FontStyle.Bold);
    string title = "Sales Report";
    SizeF textSize = g.MeasureString(title, font);
    g.DrawString(title, font, Brushes.Black, 
        rect.X + rect.Width / 2 - textSize.Width / 2, 
        rect.Y + rect.Height / 2 - textSize.Height / 2);
    
    // Draw date
    Font dateFont = new Font("Arial", 10);
    string date = DateTime.Now.ToString("yyyy-MM-dd");
    g.DrawString(date, dateFont, Brushes.Black, 
        rect.Right - 100, rect.Y + 10);
}

printDocument.Print();
```

## Multiple Grid Printing

Print multiple grids across pages.

### Print Multiple Grids

```csharp
using Syncfusion.GridHelperClasses;
using System.Collections.Generic;

// Collect grids to print
List<Control> grids = new List<Control>();

foreach (Control control in panel1.Controls)
{
    if (control is GridGroupingControl)
    {
        grids.Add((control as GridGroupingControl).TableControl);
    }
}

// Create multi-grid print document
MultiGridPrintDocument multiPrint = new MultiGridPrintDocument(grids);

// Print options
multiPrint.GridPrintOption = MultiGridPrintDocument.GridPrintOptions.MultipleGridPrint;
multiPrint.ShowHeaderFooterOnAllPages = true;

// Show preview
PrintPreviewDialog previewDialog = new PrintPreviewDialog();
previewDialog.Document = multiPrint;
previewDialog.Show();
```

### MultiGrid Print Options

```csharp
// Each grid starts on new page
multiPrint.GridPrintOption = MultiGridPrintDocument.GridPrintOptions.PrintGridInNewPage;

// Print continuously without page breaks
multiPrint.GridPrintOption = MultiGridPrintDocument.GridPrintOptions.DefaultGridPrint;

// Scale columns to fit page
multiPrint.GridPrintOption = MultiGridPrintDocument.GridPrintOptions.ScaleColumnsToFit;
```

## Common Scenarios

### Scenario 1: Print with Company Header

```csharp
void PrintWithHeader()
{
    GridPrintDocumentAdv printDoc = new GridPrintDocumentAdv(gridGroupingControl1.TableControl);
    
    // Header configuration
    printDoc.ShowHeaderRowOnAllPages = true;
    printDoc.HeaderText = "Acme Corporation\nEmployee Directory";
    printDoc.HeaderHeight = 60;
    printDoc.HeaderBackColor = Color.DarkBlue;
    printDoc.HeaderForeColor = Color.White;
    printDoc.HeaderFont = new Font("Arial", 14, FontStyle.Bold);
    
    // Footer with page numbers
    printDoc.ShowFooterOnAllPages = true;
    printDoc.FooterText = $"Printed: {DateTime.Now:yyyy-MM-dd HH:mm} | Page {{page}} of {{pagecount}}";
    printDoc.FooterHeight = 30;
    
    // Landscape for wide grid
    printDoc.DefaultPageSettings.Landscape = true;
    
    // Show preview
    PrintPreviewDialog preview = new PrintPreviewDialog();
    preview.Document = printDoc;
    preview.ShowDialog();
}
```

### Scenario 2: Print Selected Records Only

```csharp
void PrintSelectedRecords()
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
    
    // Create temporary grid
    GridGroupingControl tempGrid = new GridGroupingControl();
    tempGrid.DataSource = selectedData;
    tempGrid.Size = gridGroupingControl1.Size;
    
    // Apply same column settings
    foreach (GridColumnDescriptor col in gridGroupingControl1.TableDescriptor.Columns)
    {
        if (tempGrid.TableDescriptor.Columns.Contains(col.MappingName))
        {
            tempGrid.TableDescriptor.Columns[col.MappingName].Width = col.Width;
            tempGrid.TableDescriptor.Columns[col.MappingName].HeaderText = col.HeaderText;
        }
    }
    
    // Print temporary grid
    GridPrintDocument pd = new GridPrintDocument(tempGrid.TableControl);
    PrintPreviewDialog preview = new PrintPreviewDialog();
    preview.Document = pd;
    preview.ShowDialog();
    
    tempGrid.Dispose();
}
```

### Scenario 3: Print with Page Setup Dialog

```csharp
void PrintWithPageSetup()
{
    GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);
    
    // Show page setup dialog
    PageSetupDialog pageSetup = new PageSetupDialog();
    pageSetup.Document = pd;
    
    if (pageSetup.ShowDialog() == DialogResult.OK)
    {
        // Show print preview with user's page settings
        PrintPreviewDialog preview = new PrintPreviewDialog();
        preview.Document = pd;
        
        if (preview.ShowDialog() == DialogResult.OK)
        {
            pd.Print();
        }
    }
}
```

### Scenario 4: Batch Print Multiple Reports

```csharp
void BatchPrintReports()
{
    // Configure printer (no dialog)
    GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl);
    pd.PrinterSettings.PrinterName = "Microsoft Print to PDF";  // Or specific printer
    pd.DefaultPageSettings.Landscape = true;
    
    // Print multiple configurations
    string[] reportNames = { "Sales", "Inventory", "Customers" };
    
    foreach (string reportName in reportNames)
    {
        // Load data for report
        LoadReportData(reportName);
        
        // Set output file (for PDF printer)
        pd.PrinterSettings.PrintFileName = $"C:\\Reports\\{reportName}_{DateTime.Now:yyyyMMdd}.pdf";
        
        // Print
        pd.Print();
    }
    
    MessageBox.Show("Batch printing complete");
}
```

## Best Practices

### Page Layout

1. **Test with Real Data**: Print preview with actual data volumes to check pagination.

2. **Column Width**: Ensure columns fit page width or use `ScaleColumnsToFitPage`.

3. **Landscape for Wide Grids**: Use landscape orientation for grids with many columns:
   ```csharp
   pd.DefaultPageSettings.Landscape = true;
   ```

4. **Page Breaks**: Control page breaks for grouped data to keep groups together.

### Headers and Footers

1. **Consistent Branding**: Include company name, logo, report title in headers.

2. **Page Numbers**: Always include page numbers in footers:
   ```csharp
   printDoc.FooterText = "Page {page} of {pagecount}";
   ```

3. **Timestamps**: Add print date/time to identify report version:
   ```csharp
   printDoc.FooterText = $"Printed: {DateTime.Now:yyyy-MM-dd HH:mm}";
   ```

4. **Header Height**: Set adequate height for multi-line headers (60-80 pixels).

### Performance

1. **Large Grids**: Show progress indicator for grids with 1000+ rows.

2. **Print Preview First**: Encourage users to preview before printing (saves paper/ink).

3. **Dispose Documents**: Dispose print documents after use:
   ```csharp
   using (GridPrintDocument pd = new GridPrintDocument(gridGroupingControl1.TableControl))
   {
       pd.Print();
   }
   ```

### User Experience

1. **Print Button**: Provide toolbar/menu button for printing:
   ```csharp
   btnPrint.Click += (s, e) => ShowPrintPreview();
   ```

2. **Keyboard Shortcut**: Support Ctrl+P for quick printing.

3. **Save Settings**: Remember user's page setup preferences across sessions.

4. **Print Selection**: Offer option to print selected records only.

### Quality

- Test print output on actual printers (not just preview)
- Verify fonts are readable at typical print sizes
- Check that colors print clearly (or use black/white mode)
- Ensure borders and gridlines are visible when printed
