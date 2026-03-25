# Conditional Styling and Custom Rendering in WinForms DataGrid

Comprehensive guide for applying conditional styling, custom rendering, and visual customization to cells, rows, headers, and summaries in the Syncfusion WinForms DataGrid (SfDataGrid).

## Table of Contents
- [Overview](#overview)
- [Cell Styling](#cell-styling)
- [Row Styling](#row-styling)
- [Summary Cell Styling](#summary-cell-styling)
- [Header and Special Cell Styling](#header-and-special-cell-styling)
- [Custom Rendering](#custom-rendering)
- [Advanced Styling Scenarios](#advanced-styling-scenarios)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Overview

SfDataGrid provides rich conditional styling capabilities through events and custom renderers, allowing complete control over visual appearance based on data values, row indices, cell types, and custom conditions.

**Key Styling Events:**
- `QueryCellStyle` - Style individual cells based on content
- `QueryRowStyle` - Style entire rows based on row data
- `DrawCell` - Custom rendering for specific cells
- Custom Renderers - Override cell rendering logic

## Cell Styling

### Styling Based on Content

Use `QueryCellStyle` event to apply styles based on cell values:

```csharp
this.sfDataGrid.QueryCellStyle += SfDataGrid_QueryCellStyle;

void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    if (e.Column.MappingName == "ProductName")
    {
        if (e.DisplayText == "NuNuCa Nub-Nougat-Creme")
        {
            e.Style.BackColor = Color.Coral;
            e.Style.TextColor = Color.White;
        }
        else if (e.DisplayText == "Alice Mutton")
        {
            e.Style.BackColor = Color.LightSkyBlue;
            e.Style.TextColor = Color.DarkSlateBlue;
        }
    }
}
```

```vb
AddHandler sfDataGrid.QueryCellStyle, AddressOf SfDataGrid_QueryCellStyle

Private Sub SfDataGrid_QueryCellStyle(ByVal sender As Object, ByVal e As QueryCellStyleEventArgs)
    If e.Column.MappingName = "ProductName" Then
        If e.DisplayText = "NuNuCa Nub-Nougat-Creme" Then
            e.Style.BackColor = Color.Coral
            e.Style.TextColor = Color.White
        ElseIf e.DisplayText = "Alice Mutton" Then
            e.Style.BackColor = Color.LightSkyBlue
            e.Style.TextColor = Color.DarkSlateBlue
        End If
    End If
End Sub
```

### Styling Alternate Cells

Apply alternating styles to cells in a specific column:

```csharp
this.sfDataGrid.QueryCellStyle += SfDataGrid_QueryCellStyle;

void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    if (e.Column.MappingName == "OrderID")
    {
        if (e.RowIndex % 2 == 0)
            e.Style.BackColor = Color.PaleTurquoise;
        else
            e.Style.BackColor = Color.Snow;
    }
}
```

### Styling Based on Numeric Conditions

```csharp
void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    if (e.Column.MappingName == "UnitPrice")
    {
        if (e.CellValue != null)
        {
            double price = Convert.ToDouble(e.CellValue);
            
            if (price < 20)
                e.Style.BackColor = Color.LightGreen;
            else if (price >= 20 && price < 50)
                e.Style.BackColor = Color.LightYellow;
            else
            {
                e.Style.BackColor = Color.LightCoral;
                e.Style.Font.Bold = true;
            }
        }
    }
}
```

### Alignment Customization

```csharp
this.sfDataGrid.QueryCellStyle += SfDataGrid_QueryCellStyle;

void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    if (e.DataRow.RowType == RowType.DefaultRow && e.Column.MappingName == "ProductName")
    {
        e.Style.VerticalAlignment = System.Windows.Forms.VisualStyles.VerticalAlignment.Top;
        e.Style.HorizontalAlignment = HorizontalAlignment.Center;
    }
}
```

## Row Styling

### Styling Based on Row Content (Observable Collection)

Use `QueryRowStyle` event to style entire rows:

```csharp
this.sfDataGrid.QueryRowStyle += SfDataGrid_QueryRowStyle;

void SfDataGrid_QueryRowStyle(object sender, QueryRowStyleEventArgs e)
{
    if (e.RowType == RowType.DefaultRow)
    {
        if ((e.RowData as OrderInfo).CustomerID == "FRANS")
            e.Style.BackColor = Color.Bisque;
        else if ((e.RowData as OrderInfo).CustomerID == "MEREP")
            e.Style.BackColor = Color.LightBlue;
    }
}
```

```vb
AddHandler sfDataGrid.QueryRowStyle, AddressOf SfDataGrid_QueryRowStyle

Private Sub SfDataGrid_QueryRowStyle(ByVal sender As Object, ByVal e As QueryRowStyleEventArgs)
    If e.RowType = RowType.DefaultRow Then
        If (TryCast(e.RowData, OrderInfo)).CustomerID = "FRANS" Then
            e.Style.BackColor = Color.Bisque
        ElseIf (TryCast(e.RowData, OrderInfo)).CustomerID = "MEREP" Then
            e.Style.BackColor = Color.LightBlue
        End If
    End If
End Sub
```

### Styling Based on Row Content (DataTable)

```csharp
this.sfDataGrid.QueryRowStyle += SfDataGrid_QueryRowStyle;

void SfDataGrid_QueryRowStyle(object sender, QueryRowStyleEventArgs e)
{
    if (e.RowType == RowType.DefaultRow)
    {
        var dataRowView = e.RowData as DataRowView;
        if (dataRowView != null)
        {
            var dataRow = dataRowView.Row;
            var cellValue = dataRow["Country"].ToString();

            if (cellValue == "UK")
                e.Style.BackColor = Color.PaleTurquoise;
            else if (cellValue == "US")
                e.Style.BackColor = Color.CornflowerBlue;
            else
                e.Style.BackColor = Color.Wheat;
        }
    }
}
```

### Styling Alternate Rows

```csharp
this.sfDataGrid.QueryRowStyle += SfDataGrid_QueryRowStyle;

void SfDataGrid_QueryRowStyle(object sender, QueryRowStyleEventArgs e)
{
    if (e.RowType == RowType.DefaultRow)
    {
        if (e.RowIndex % 2 == 0)
            e.Style.BackColor = Color.Lavender;
        else
            e.Style.BackColor = Color.AliceBlue;
    }
}
```

### Highlighting Newly Added Rows

Add a Boolean property to track new rows:

```csharp
// In AddNewRowInitiating event
this.sfDataGrid.AddNewRowInitiating += SfDataGrid_AddNewRowInitiating;

void SfDataGrid_AddNewRowInitiating(object sender, AddNewRowInitiatingEventArgs e)
{
    (e.NewObject as OrderInfo).isNewlyAdded = true;
}

// In QueryCellStyle event
this.sfDataGrid.QueryCellStyle += SfDataGrid_QueryCellStyle;

void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    if (e.DataRow == null || e.DataRow.RowData == null)
        return;

    if (e.DataRow.RowData != null && (e.DataRow.RowData as OrderInfo).isNewlyAdded)
        e.Style.BackColor = Color.LightBlue;
}
```

## Summary Cell Styling

### Caption Summary Cell Styling

Style caption summary cells based on summary values:

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.DataRow.RowType == RowType.CaptionRow)
    {
        int result;
        int.TryParse((e.DataRow.RowData as Group).SummaryDetails.SummaryValues[0]
            .AggregateValues.ElementAt(0).Value.ToString(), out result);
        
        if (result < 50 && e.Column.MappingName == "Quantity")
        {
            e.Style.Font.Bold = true;
            e.Style.TextColor = Color.Red;
        }
    }
}
```

### Caption Summary Row Styling

Style entire caption summary rows:

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.DataRow.RowType == RowType.CaptionCoveredRow)
    {
        int result;
        int.TryParse((e.DataRow.RowData as Group).SummaryDetails.SummaryValues[0]
            .AggregateValues.ElementAt(0).Value.ToString(), out result);
        
        if (result < 50)
        {
            e.Style.Font.Bold = true;
            e.Style.TextColor = Color.Red;
        }
    }
}
```

### Group Summary Cell Styling

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.DataRow.RowType == RowType.SummaryRow)
    {
        if (e.Column.MappingName == "UnitPrice")
            e.Style.BackColor = Color.RosyBrown;
    }
}
```

### Group Summary Row Styling

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.DataRow.RowType == RowType.SummaryCoveredRow)
    {
        float result;
        float.TryParse((e.DataRow.RowData as SummaryRecordEntry).SummaryValues[0]
            .AggregateValues.ElementAt(0).Value.ToString(), out result);

        if (result < 200)
            e.Style.BackColor = Color.BlanchedAlmond;
    }
}
```

### Table Summary Cell Styling

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.DataRow.RowType == RowType.TableSummaryRow)
    {
        if (e.Column.MappingName == "UnitPrice")
            e.Style.BackColor = Color.Aquamarine;
    }
}
```

### Table Summary Row Styling

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.DataRow.RowType == RowType.TableSummaryCoveredRow)
    {
        double result;
        double.TryParse((e.DataRow.RowData as SummaryRecordEntry).SummaryValues[0]
            .AggregateValues.ElementAt(0).Value.ToString(), out result);
        
        if (result > 10000)
            e.Style.BackColor = Color.Beige;
        else
            e.Style.BackColor = Color.Bisque;
    }
}
```

## Header and Special Cell Styling

### Stacked Header Styling

Override `GridStackedHeaderCellRenderer` for custom stacked header styling:

```csharp
this.sfDataGrid.CellRenderers.Remove("StackedHeader");
this.sfDataGrid.CellRenderers.Add("StackedHeader", new CustomStackedHeaderCellRenderer());

public class CustomStackedHeaderCellRenderer : GridStackedHeaderCellRenderer
{
    protected override void OnRender(Graphics paint, Rectangle cellRect, string cellValue, 
        CellStyleInfo style, DataColumnBase column, RowColumnIndex rowColumnIndex)
    {
        if (column.ColumnIndex == 0)
        {
            style.BackColor = Color.LightSkyBlue;
        }
        else
        {
            style.BackColor = Color.BlanchedAlmond;
        }
        base.OnRender(paint, cellRect, cellValue, style, column, rowColumnIndex);
    }
}
```

```vb
Me.sfDataGrid.CellRenderers.Remove("StackedHeader")
Me.sfDataGrid.CellRenderers.Add("StackedHeader", New CustomStackedHeaderCellRenderer())

Public Class CustomStackedHeaderCellRenderer
    Inherits GridStackedHeaderCellRenderer
    Protected Overrides Sub OnRender(ByVal paint As Graphics, ByVal cellRect As Rectangle, 
        ByVal cellValue As String, ByVal style As CellStyleInfo, ByVal column As DataColumnBase, 
        ByVal rowColumnIndex As RowColumnIndex)
        If column.ColumnIndex = 0 Then
            style.BackColor = Color.LightSkyBlue
        Else
            style.BackColor = Color.BlanchedAlmond
        End If
        MyBase.OnRender(paint, cellRect, cellValue, style, column, rowColumnIndex)
    End Sub
End Class
```

### Row Header Styling

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (this.sfDataGrid.ShowRowHeader && e.ColumnIndex == 0 && e.DataRow.RowIndex != 0)
    {
        if (e.RowIndex % 2 == 0)
        {
            e.Style.BackColor = Color.LightBlue;
        }
        else
        {
            e.Style.BackColor = Color.CadetBlue;
        }
    }
}
```

### Indent Cell Styling

Override `GridIndentCellRenderer` for indent cell styling:

```csharp
public Form1()
{
    InitializeComponent();
    this.sfDataGrid.CellRenderers.Remove("Indent");
    this.sfDataGrid.CellRenderers.Add("Indent", new CustomIndentCellRenderer());
}

public class CustomIndentCellRenderer : GridIndentCellRenderer
{
    protected override void OnRender(Graphics paint, Rectangle cellRect, string cellValue, 
        CellStyleInfo style, DataColumnBase column, RowColumnIndex rowColumnIndex)
    {
        if (rowColumnIndex.RowIndex % 2 == 0)
            style.BackColor = Color.Lavender;
        else
            style.BackColor = Color.AliceBlue;

        base.OnRender(paint, cellRect, cellValue, style, column, rowColumnIndex);
    }
}
```

## Custom Rendering

### Adding Images to Cells

Use `DrawCell` event to render custom graphics:

```csharp
this.sfDataGrid.DrawCell += SfDataGrid_DrawCell;

void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.RowIndex == 1 && e.Column.MappingName == "ShipCountry")
    {
        e.Handled = true;
        e.Graphics.DrawImage(Image.FromFile(@"../../US.jpg"), e.Bounds.X + 20, e.Bounds.Y);
        
        Pen borderPen = new Pen(Color.LightGray);
        e.Graphics.DrawLine(borderPen, e.Bounds.Right, e.Bounds.Top, 
            e.Bounds.Right, e.Bounds.Bottom);
        e.Graphics.DrawLine(borderPen, e.Bounds.Left, e.Bounds.Bottom, 
            e.Bounds.Right, e.Bounds.Bottom);
    }
}
```

### Custom Cell Renderer

Create custom cell renderer for advanced scenarios:

```csharp
public class CustomCellRenderer : GridCellRendererBase
{
    protected override void OnRender(Graphics paint, Rectangle cellRect, string cellValue, 
        CellStyleInfo style, DataColumnBase column, RowColumnIndex rowColumnIndex)
    {
        // Custom rendering logic
        using (Brush brush = new SolidBrush(style.BackColor))
        {
            paint.FillRectangle(brush, cellRect);
        }
        
        // Draw custom content
        using (Brush textBrush = new SolidBrush(style.TextColor))
        {
            StringFormat format = new StringFormat();
            format.Alignment = StringAlignment.Center;
            format.LineAlignment = StringAlignment.Center;
            paint.DrawString(cellValue, style.Font.GetFont(), textBrush, cellRect, format);
        }
    }
}

// Register custom renderer
sfDataGrid.CellRenderers.Add("CustomRenderer", new CustomCellRenderer());
```

### Drawing Custom Shapes

```csharp
void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.Column.MappingName == "Status")
    {
        e.Handled = true;
        
        // Draw background
        using (Brush backBrush = new SolidBrush(e.Style.BackColor))
        {
            e.Graphics.FillRectangle(backBrush, e.Bounds);
        }
        
        // Draw circle indicator
        int diameter = Math.Min(e.Bounds.Width, e.Bounds.Height) - 10;
        int x = e.Bounds.X + (e.Bounds.Width - diameter) / 2;
        int y = e.Bounds.Y + (e.Bounds.Height - diameter) / 2;
        
        Color indicatorColor = e.DisplayText == "Active" ? Color.Green : Color.Red;
        using (Brush brush = new SolidBrush(indicatorColor))
        {
            e.Graphics.FillEllipse(brush, x, y, diameter, diameter);
        }
    }
}
```

## Advanced Styling Scenarios

### Conditional Formatting Based on Multiple Columns

```csharp
void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    if (e.DataRow.RowType == RowType.DefaultRow)
    {
        var record = e.DataRow.RowData as OrderInfo;
        if (record != null)
        {
            // Highlight expensive items with high quantity
            if (record.UnitPrice > 50 && record.Quantity > 10)
            {
                e.Style.BackColor = Color.Gold;
                e.Style.Font.Bold = true;
            }
            // Highlight low stock
            else if (record.Quantity < 5)
            {
                e.Style.BackColor = Color.LightCoral;
                e.Style.TextColor = Color.DarkRed;
            }
        }
    }
}
```

### Gradient Background

```csharp
void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.Column.MappingName == "Progress")
    {
        e.Handled = true;
        
        using (LinearGradientBrush brush = new LinearGradientBrush(
            e.Bounds, Color.LightGreen, Color.DarkGreen, LinearGradientMode.Horizontal))
        {
            e.Graphics.FillRectangle(brush, e.Bounds);
        }
        
        // Draw text
        using (Brush textBrush = new SolidBrush(Color.White))
        {
            StringFormat format = new StringFormat();
            format.Alignment = StringAlignment.Center;
            format.LineAlignment = StringAlignment.Center;
            e.Graphics.DrawString(e.DisplayText, e.Style.Font.GetFont(), 
                textBrush, e.Bounds, format);
        }
    }
}
```

### Progress Bar in Cell

```csharp
void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (e.Column.MappingName == "Completion")
    {
        e.Handled = true;
        
        // Draw background
        e.Graphics.FillRectangle(Brushes.White, e.Bounds);
        
        // Calculate progress width
        int value = Convert.ToInt32(e.CellValue);
        int progressWidth = (int)((value / 100.0) * e.Bounds.Width);
        
        // Draw progress bar
        Rectangle progressRect = new Rectangle(e.Bounds.X, e.Bounds.Y, 
            progressWidth, e.Bounds.Height);
        e.Graphics.FillRectangle(Brushes.LightBlue, progressRect);
        
        // Draw text
        string text = value.ToString() + "%";
        StringFormat format = new StringFormat();
        format.Alignment = StringAlignment.Center;
        format.LineAlignment = StringAlignment.Center;
        e.Graphics.DrawString(text, e.Style.Font.GetFont(), 
            Brushes.Black, e.Bounds, format);
        
        // Draw border
        e.Graphics.DrawRectangle(Pens.Gray, e.Bounds);
    }
}
```

## Edge Cases and Troubleshooting

### Issue: Styling not applied

**Cause:** Event handler not registered or wrong row/cell type checked

**Solution:** Verify event registration and row type:

```csharp
// Check row type
if (e.RowType == RowType.DefaultRow)  // For data rows
if (e.RowType == RowType.HeaderRow)   // For header rows

// Verify event registration
this.sfDataGrid.QueryCellStyle += SfDataGrid_QueryCellStyle;
```

### Issue: Performance degradation with styling

**Cause:** Complex logic in styling events for large datasets

**Solution:** Optimize conditional checks:

```csharp
void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    // Early exit for non-styled columns
    if (e.Column.MappingName != "TargetColumn")
        return;
    
    // Cache frequently used values
    if (e.CellValue == null)
        return;
    
    // Minimal conditional logic
    int value = (int)e.CellValue;
    if (value > 100)
        e.Style.BackColor = Color.Red;
}
```

### Issue: Custom renderer not applied

**Cause:** Renderer not registered or wrong cell type

**Solution:** Register renderer correctly:

```csharp
// Remove existing renderer
sfDataGrid.CellRenderers.Remove("RendererType");

// Add custom renderer
sfDataGrid.CellRenderers.Add("RendererType", new CustomRenderer());
```

### Issue: Image not displaying in DrawCell

**Cause:** `e.Handled = true` not set or image path incorrect

**Solution:** Set Handled and verify path:

```csharp
void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    if (condition)
    {
        e.Handled = true; // MUST set to prevent default rendering
        
        // Verify image exists
        if (File.Exists(imagePath))
        {
            e.Graphics.DrawImage(Image.FromFile(imagePath), e.Bounds);
        }
    }
}
```

### Issue: Styling conflicts between QueryCellStyle and QueryRowStyle

**Behavior:** `QueryCellStyle` takes precedence over `QueryRowStyle`

**Solution:** Use one approach or coordinate between them:

```csharp
// Option 1: Use only QueryRowStyle for row-level styling
// Option 2: Use QueryCellStyle and check if row-level styling is needed
void SfDataGrid_QueryCellStyle(object sender, QueryCellStyleEventArgs e)
{
    // Apply row-level style first
    var record = e.DataRow.RowData as OrderInfo;
    if (record.Status == "Inactive")
        e.Style.BackColor = Color.LightGray;
    
    // Then apply column-specific overrides
    if (e.Column.MappingName == "ImportantColumn")
        e.Style.BackColor = Color.Yellow;
}
```

### Issue: Text not visible after custom background

**Solution:** Always set text color when changing background:

```csharp
e.Style.BackColor = Color.DarkBlue;
e.Style.TextColor = Color.White; // Ensure contrast
```

### Issue: Borders missing in custom rendering

**Solution:** Draw borders explicitly:

```csharp
void SfDataGrid_DrawCell(object sender, DrawCellEventArgs e)
{
    e.Handled = true;
    
    // Custom rendering
    e.Graphics.FillRectangle(Brushes.White, e.Bounds);
    
    // Draw borders
    using (Pen pen = new Pen(Color.LightGray))
    {
        e.Graphics.DrawLine(pen, e.Bounds.Right, e.Bounds.Top, 
            e.Bounds.Right, e.Bounds.Bottom); // Right border
        e.Graphics.DrawLine(pen, e.Bounds.Left, e.Bounds.Bottom, 
            e.Bounds.Right, e.Bounds.Bottom); // Bottom border
    }
}
```
