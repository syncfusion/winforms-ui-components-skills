# DataGrid Summaries Reference

Complete reference guide for implementing summary rows in Syncfusion WinForms DataGrid (SfDataGrid).

## Table of Contents

1. [Summary Types Overview](#summary-types-overview)
2. [Aggregate Types](#aggregate-types)
3. [Table Summary](#table-summary)
4. [Group Summary](#group-summary)
5. [Caption Summary](#caption-summary)
6. [Custom Summaries](#custom-summaries)
7. [Summary Formatting](#summary-formatting)
8. [Summary Calculation Options](#summary-calculation-options)
9. [Summary Appearance](#summary-appearance)
10. [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

---

## Summary Types Overview

SfDataGrid provides three types of summary rows to display aggregated information:

- **Table Summary**: Summary information for entire table (top or bottom)
- **Group Summary**: Summary information within each group
- **Caption Summary**: Summary information in group caption row

---

## Aggregate Types

Built-in aggregate functions available for summary calculations:

| Aggregate Type | Available Functions |
|---------------|-------------------|
| CountAggregate | Count |
| Int32Aggregate | Count, Max, Min, Average, Sum |
| DoubleAggregate | Count, Max, Min, Average, Sum |
| Custom | User-defined custom aggregates |

---

## Table Summary

### Adding Table Summary for Column

Display summary in specific columns.

**C# Example:**
```csharp
// Create table summary row
GridTableSummaryRow tableSummaryRow = new GridTableSummaryRow();
tableSummaryRow.Name = "TableSummary";
tableSummaryRow.ShowSummaryInRow = false;
tableSummaryRow.Position = VerticalPosition.Bottom;

// Create summary column
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "TotalProduct";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
summaryColumn.Format = "Total Product Count: {Sum:c}";
summaryColumn.MappingName = "ProductName";

// Add summary column
tableSummaryRow.SummaryColumns.Add(summaryColumn);

// Add table summary row
sfDataGrid.TableSummaryRows.Add(tableSummaryRow);
```

**VB.NET Example:**
```vb
' Create table summary row
Dim tableSummaryRow As New GridTableSummaryRow()
tableSummaryRow.Name = "TableSummary"
tableSummaryRow.ShowSummaryInRow = False
tableSummaryRow.Position = VerticalPosition.Bottom

' Create summary column
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "TotalProduct"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
summaryColumn.Format = "Total Product Count: {Sum:c}"
summaryColumn.MappingName = "ProductName"

' Add summary column
tableSummaryRow.SummaryColumns.Add(summaryColumn)

' Add table summary row
sfDataGrid.TableSummaryRows.Add(tableSummaryRow)
```

### Adding Table Summary for Row

Display summary information across the entire row.

**C# Example:**
```csharp
// Create table summary row
GridTableSummaryRow tableSummaryRow = new GridTableSummaryRow();
tableSummaryRow.Name = "TableSummary";
tableSummaryRow.ShowSummaryInRow = true;
tableSummaryRow.Title = "Total Product Count: {TotalProduct}";
tableSummaryRow.Position = VerticalPosition.Bottom;

// Create summary column
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "TotalProduct";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
summaryColumn.Format = "{Count}";
summaryColumn.MappingName = "ProductName";

tableSummaryRow.SummaryColumns.Add(summaryColumn);
sfDataGrid.TableSummaryRows.Add(tableSummaryRow);
```

**VB.NET Example:**
```vb
' Create table summary row
Dim tableSummaryRow As New GridTableSummaryRow()
tableSummaryRow.Name = "TableSummary"
tableSummaryRow.ShowSummaryInRow = True
tableSummaryRow.Title = "Total Product Count: {TotalProduct}"
tableSummaryRow.Position = VerticalPosition.Bottom

' Create summary column
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "TotalProduct"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
summaryColumn.Format = "{Count}"
summaryColumn.MappingName = "ProductName"

tableSummaryRow.SummaryColumns.Add(summaryColumn)
sfDataGrid.TableSummaryRows.Add(tableSummaryRow)
```

### Positioning Table Summary

**C# Example:**
```csharp
// Table summary at top
GridTableSummaryRow topSummary = new GridTableSummaryRow();
topSummary.Position = VerticalPosition.Top;
topSummary.ShowSummaryInRow = true;
topSummary.Title = "Total: {ProductCount}";

// Table summary at bottom
GridTableSummaryRow bottomSummary = new GridTableSummaryRow();
bottomSummary.Position = VerticalPosition.Bottom;
bottomSummary.ShowSummaryInRow = true;
bottomSummary.Title = "Total: {ProductCount}";

// Create summary column
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "ProductCount";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
summaryColumn.Format = "{Count:c}";
summaryColumn.MappingName = "ProductName";

topSummary.SummaryColumns.Add(summaryColumn);
bottomSummary.SummaryColumns.Add(summaryColumn);

sfDataGrid.TableSummaryRows.Add(topSummary);
sfDataGrid.TableSummaryRows.Add(bottomSummary);
```

**VB.NET Example:**
```vb
' Table summary at top
Dim topSummary As New GridTableSummaryRow()
topSummary.Position = VerticalPosition.Top
topSummary.ShowSummaryInRow = True
topSummary.Title = "Total: {ProductCount}"

' Table summary at bottom
Dim bottomSummary As New GridTableSummaryRow()
bottomSummary.Position = VerticalPosition.Bottom
bottomSummary.ShowSummaryInRow = True
bottomSummary.Title = "Total: {ProductCount}"

' Create summary column
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "ProductCount"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
summaryColumn.Format = "{Count:c}"
summaryColumn.MappingName = "ProductName"

topSummary.SummaryColumns.Add(summaryColumn)
bottomSummary.SummaryColumns.Add(summaryColumn)

sfDataGrid.TableSummaryRows.Add(topSummary)
sfDataGrid.TableSummaryRows.Add(bottomSummary)
```

### Display Column Summary with Title

**C# Example:**
```csharp
// Create table summary with title and columns
GridTableSummaryRow tableSummaryRow = new GridTableSummaryRow();
tableSummaryRow.ShowSummaryInRow = false;
tableSummaryRow.TitleColumnCount = 3;
tableSummaryRow.Title = "Total Price: {PriceAmount} for {ProductCount} products";
tableSummaryRow.Position = VerticalPosition.Top;

// Add multiple summary columns
GridSummaryColumn priceColumn = new GridSummaryColumn();
priceColumn.Name = "PriceAmount";
priceColumn.Format = "{Sum:c}";
priceColumn.MappingName = "UnitPrice";
priceColumn.SummaryType = SummaryType.DoubleAggregate;

GridSummaryColumn countColumn = new GridSummaryColumn();
countColumn.Name = "ProductCount";
countColumn.Format = "{Count:d}";
countColumn.MappingName = "Quantity";
countColumn.SummaryType = SummaryType.CountAggregate;

tableSummaryRow.SummaryColumns.Add(priceColumn);
tableSummaryRow.SummaryColumns.Add(countColumn);

sfDataGrid.TableSummaryRows.Add(tableSummaryRow);
```

**VB.NET Example:**
```vb
' Create table summary with title and columns
Dim tableSummaryRow As New GridTableSummaryRow()
tableSummaryRow.ShowSummaryInRow = False
tableSummaryRow.TitleColumnCount = 3
tableSummaryRow.Title = "Total Price: {PriceAmount} for {ProductCount} products"
tableSummaryRow.Position = VerticalPosition.Top

' Add multiple summary columns
Dim priceColumn As New GridSummaryColumn()
priceColumn.Name = "PriceAmount"
priceColumn.Format = "{Sum:c}"
priceColumn.MappingName = "UnitPrice"
priceColumn.SummaryType = SummaryType.DoubleAggregate

Dim countColumn As New GridSummaryColumn()
countColumn.Name = "ProductCount"
countColumn.Format = "{Count:d}"
countColumn.MappingName = "Quantity"
countColumn.SummaryType = SummaryType.CountAggregate

tableSummaryRow.SummaryColumns.Add(priceColumn)
tableSummaryRow.SummaryColumns.Add(countColumn)

sfDataGrid.TableSummaryRows.Add(tableSummaryRow)
```

---

## Group Summary

### Adding Group Summary for Column

**C# Example:**
```csharp
// Create group summary row
GridSummaryRow groupSummaryRow = new GridSummaryRow();
groupSummaryRow.Name = "GroupSummary";
groupSummaryRow.ShowSummaryInRow = false;

// Create summary column
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "UnitPrice";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
summaryColumn.Format = "Total Price: {Sum:c}";
summaryColumn.MappingName = "UnitPrice";

groupSummaryRow.SummaryColumns.Add(summaryColumn);
sfDataGrid.GroupSummaryRows.Add(groupSummaryRow);
```

**VB.NET Example:**
```vb
' Create group summary row
Dim groupSummaryRow As New GridSummaryRow()
groupSummaryRow.Name = "GroupSummary"
groupSummaryRow.ShowSummaryInRow = False

' Create summary column
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "UnitPrice"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
summaryColumn.Format = "Total Price: {Sum:c}"
summaryColumn.MappingName = "UnitPrice"

groupSummaryRow.SummaryColumns.Add(summaryColumn)
sfDataGrid.GroupSummaryRows.Add(groupSummaryRow)
```

### Adding Group Summary for Row

**C# Example:**
```csharp
// Create group summary row
GridSummaryRow groupSummaryRow = new GridSummaryRow();
groupSummaryRow.Name = "GroupSummary";
groupSummaryRow.ShowSummaryInRow = true;
groupSummaryRow.Title = "Total Price: {UnitPrice}";

// Create summary column
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "UnitPrice";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
summaryColumn.Format = "{Sum:c}";
summaryColumn.MappingName = "UnitPrice";

groupSummaryRow.SummaryColumns.Add(summaryColumn);
sfDataGrid.GroupSummaryRows.Add(groupSummaryRow);
```

**VB.NET Example:**
```vb
' Create group summary row
Dim groupSummaryRow As New GridSummaryRow()
groupSummaryRow.Name = "GroupSummary"
groupSummaryRow.ShowSummaryInRow = True
groupSummaryRow.Title = "Total Price: {UnitPrice}"

' Create summary column
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "UnitPrice"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
summaryColumn.Format = "{Sum:c}"
summaryColumn.MappingName = "UnitPrice"

groupSummaryRow.SummaryColumns.Add(summaryColumn)
sfDataGrid.GroupSummaryRows.Add(groupSummaryRow)
```

### Display Group Summary with Title

**C# Example:**
```csharp
// Create group summary with title and columns
GridSummaryRow groupSummaryRow = new GridSummaryRow();
groupSummaryRow.Name = "GroupSummary";
groupSummaryRow.ShowSummaryInRow = false;
groupSummaryRow.TitleColumnCount = 3;
groupSummaryRow.Title = "Total Price: {PriceAmount} for {ProductCount} Products";

// Add multiple summary columns
GridSummaryColumn priceColumn = new GridSummaryColumn();
priceColumn.Name = "PriceAmount";
priceColumn.Format = "{Sum:c}";
priceColumn.MappingName = "UnitPrice";
priceColumn.SummaryType = SummaryType.DoubleAggregate;

GridSummaryColumn countColumn = new GridSummaryColumn();
countColumn.Name = "ProductCount";
countColumn.Format = "{Count:d}";
countColumn.MappingName = "Quantity";
countColumn.SummaryType = SummaryType.CountAggregate;

groupSummaryRow.SummaryColumns.Add(priceColumn);
groupSummaryRow.SummaryColumns.Add(countColumn);

sfDataGrid.GroupSummaryRows.Add(groupSummaryRow);
```

**VB.NET Example:**
```vb
' Create group summary with title and columns
Dim groupSummaryRow As New GridSummaryRow()
groupSummaryRow.Name = "GroupSummary"
groupSummaryRow.ShowSummaryInRow = False
groupSummaryRow.TitleColumnCount = 3
groupSummaryRow.Title = "Total Price: {PriceAmount} for {ProductCount} Products"

' Add multiple summary columns
Dim priceColumn As New GridSummaryColumn()
priceColumn.Name = "PriceAmount"
priceColumn.Format = "{Sum:c}"
priceColumn.MappingName = "UnitPrice"
priceColumn.SummaryType = SummaryType.DoubleAggregate

Dim countColumn As New GridSummaryColumn()
countColumn.Name = "ProductCount"
countColumn.Format = "{Count:d}"
countColumn.MappingName = "Quantity"
countColumn.SummaryType = SummaryType.CountAggregate

groupSummaryRow.SummaryColumns.Add(priceColumn)
groupSummaryRow.SummaryColumns.Add(countColumn)

sfDataGrid.GroupSummaryRows.Add(groupSummaryRow)
```

---

## Caption Summary

### Formatting Built-in Caption Summary

**C# Example:**
```csharp
// Customize group caption format
// Default: {ColumnName}: {Key} - {ItemsCount} Items
sfDataGrid.GroupCaptionTextFormat = "{Key} : {ItemsCount}";
```

**VB.NET Example:**
```vb
' Customize group caption format
' Default: {ColumnName}: {Key} - {ItemsCount} Items
sfDataGrid.GroupCaptionTextFormat = "{Key} : {ItemsCount}"
```

### Adding Caption Summary for Column

**C# Example:**
```csharp
// Create caption summary row
GridSummaryRow captionSummaryRow = new GridSummaryRow();
captionSummaryRow.Name = "CaptionSummary";
captionSummaryRow.ShowSummaryInRow = false;

// Add summary columns
GridSummaryColumn priceColumn = new GridSummaryColumn();
priceColumn.Name = "Column1";
priceColumn.SummaryType = SummaryType.DoubleAggregate;
priceColumn.Format = "{Sum:c}";
priceColumn.MappingName = "UnitPrice";

GridSummaryColumn countColumn = new GridSummaryColumn();
countColumn.Name = "Column2";
countColumn.SummaryType = SummaryType.Int32Aggregate;
countColumn.Format = "{Count}";
countColumn.MappingName = "OrderID";

captionSummaryRow.SummaryColumns.Add(priceColumn);
captionSummaryRow.SummaryColumns.Add(countColumn);

sfDataGrid.CaptionSummaryRow = captionSummaryRow;
```

**VB.NET Example:**
```vb
' Create caption summary row
Dim captionSummaryRow As New GridSummaryRow()
captionSummaryRow.Name = "CaptionSummary"
captionSummaryRow.ShowSummaryInRow = False

' Add summary columns
Dim priceColumn As New GridSummaryColumn()
priceColumn.Name = "Column1"
priceColumn.SummaryType = SummaryType.DoubleAggregate
priceColumn.Format = "{Sum:c}"
priceColumn.MappingName = "UnitPrice"

Dim countColumn As New GridSummaryColumn()
countColumn.Name = "Column2"
countColumn.SummaryType = SummaryType.Int32Aggregate
countColumn.Format = "{Count}"
countColumn.MappingName = "OrderID"

captionSummaryRow.SummaryColumns.Add(priceColumn)
captionSummaryRow.SummaryColumns.Add(countColumn)

sfDataGrid.CaptionSummaryRow = captionSummaryRow
```

### Adding Caption Summary for Row

**C# Example:**
```csharp
// Create caption summary row
GridSummaryRow captionSummaryRow = new GridSummaryRow();
captionSummaryRow.Name = "CaptionSummary";
captionSummaryRow.ShowSummaryInRow = true;
captionSummaryRow.Title = "Total Quantity of {Key}: {Quantity}";

// Create summary column
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "Quantity";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
summaryColumn.Format = "{Sum:c}";
summaryColumn.MappingName = "Quantity";

captionSummaryRow.SummaryColumns.Add(summaryColumn);
sfDataGrid.CaptionSummaryRow = captionSummaryRow;
```

**VB.NET Example:**
```vb
' Create caption summary row
Dim captionSummaryRow As New GridSummaryRow()
captionSummaryRow.Name = "CaptionSummary"
captionSummaryRow.ShowSummaryInRow = True
captionSummaryRow.Title = "Total Quantity of {Key}: {Quantity}"

' Create summary column
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "Quantity"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
summaryColumn.Format = "{Sum:c}"
summaryColumn.MappingName = "Quantity"

captionSummaryRow.SummaryColumns.Add(summaryColumn)
sfDataGrid.CaptionSummaryRow = captionSummaryRow
```

### Display Caption Summary with Title

**C# Example:**
```csharp
// Create caption summary with title and columns
GridSummaryRow captionSummaryRow = new GridSummaryRow();
captionSummaryRow.Name = "CaptionSummary";
captionSummaryRow.ShowSummaryInRow = false;
captionSummaryRow.TitleColumnCount = 3;
captionSummaryRow.Title = "Total Price: {PriceAmount} for {ProductCount} Products";

// Add multiple summary columns
GridSummaryColumn priceColumn = new GridSummaryColumn();
priceColumn.Name = "PriceAmount";
priceColumn.Format = "{Sum:c}";
priceColumn.MappingName = "UnitPrice";
priceColumn.SummaryType = SummaryType.DoubleAggregate;

GridSummaryColumn countColumn = new GridSummaryColumn();
countColumn.Name = "ProductCount";
countColumn.Format = "{Count:d}";
countColumn.MappingName = "Quantity";
countColumn.SummaryType = SummaryType.CountAggregate;

captionSummaryRow.SummaryColumns.Add(priceColumn);
captionSummaryRow.SummaryColumns.Add(countColumn);

sfDataGrid.CaptionSummaryRow = captionSummaryRow;
```

**VB.NET Example:**
```vb
' Create caption summary with title and columns
Dim captionSummaryRow As New GridSummaryRow()
captionSummaryRow.Name = "CaptionSummary"
captionSummaryRow.ShowSummaryInRow = False
captionSummaryRow.TitleColumnCount = 3
captionSummaryRow.Title = "Total Price: {PriceAmount} for {ProductCount} Products"

' Add multiple summary columns
Dim priceColumn As New GridSummaryColumn()
priceColumn.Name = "PriceAmount"
priceColumn.Format = "{Sum:c}"
priceColumn.MappingName = "UnitPrice"
priceColumn.SummaryType = SummaryType.DoubleAggregate

Dim countColumn As New GridSummaryColumn()
countColumn.Name = "ProductCount"
countColumn.Format = "{Count:d}"
countColumn.MappingName = "Quantity"
countColumn.SummaryType = SummaryType.CountAggregate

captionSummaryRow.SummaryColumns.Add(priceColumn)
captionSummaryRow.SummaryColumns.Add(countColumn)

sfDataGrid.CaptionSummaryRow = captionSummaryRow
```

---

## Custom Summaries

### Implementing Custom Aggregate

**C# Example:**
```csharp
// 1. Create custom aggregate class
public class CustomAggregate : ISummaryAggregate
{
    public CustomAggregate()
    {
    }

    public double StdDev { get; set; }

    public Action<IEnumerable, string, PropertyDescriptor> CalculateAggregateFunc()
    {
        return (items, property, pd) =>
        {
            var enumerableItems = items as IEnumerable<SalesByYear>;
            if (pd.Name == "StdDev")
            {
                this.StdDev = enumerableItems.StdDev<SalesByYear>(q => q.Total);
            }
        };
    }
}

// 2. Extension method for standard deviation
public static class LinqExtensions
{
    public static double StdDev<T>(this IEnumerable<T> values, Func<T, double?> selector)
    {
        double result = 0;
        var count = values.Count();
        if (count > 0)
        {
            double? avg = values.Average(selector);
            double sum = values.Select(selector).Sum(d =>
            {
                if (d.HasValue)
                {
                    return Math.Pow(d.Value - avg.Value, 2);
                }
                return 0.0;
            });
            result = Math.Sqrt((sum) / (count - 1));
        }
        return result;
    }
}

// 3. Use custom aggregate in summary
GridTableSummaryRow tableSummaryRow = new GridTableSummaryRow();
tableSummaryRow.Name = "TableSummary";
tableSummaryRow.ShowSummaryInRow = true;
tableSummaryRow.Title = "Standard Deviation for Total Sales: {TotalSales}";
tableSummaryRow.Position = TableSummaryRowPosition.Top;

GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "TotalSales";
summaryColumn.SummaryType = SummaryType.Custom;
summaryColumn.CustomAggregate = new CustomAggregate();
summaryColumn.Format = "{StdDev}";

tableSummaryRow.SummaryColumns.Add(summaryColumn);
sfDataGrid.TableSummaryRows.Add(tableSummaryRow);
```

**VB.NET Example:**
```vb
' 1. Create custom aggregate class
Public Class CustomAggregate
    Implements ISummaryAggregate

    Public Sub New()
    End Sub

    Public Property StdDev() As Double

    Public Function CalculateAggregateFunc() As Action(Of IEnumerable, String, PropertyDescriptor) Implements ISummaryAggregate.CalculateAggregateFunc
        Return Sub(items, [property], pd)
            Dim enumerableItems = TryCast(items, IEnumerable(Of SalesByYear))
            If pd.Name = "StdDev" Then
                Me.StdDev = enumerableItems.StdDev(Of SalesByYear)(Function(q) q.Total)
            End If
        End Sub
    End Function
End Class

' 2. Extension method for standard deviation
Public Module LinqExtensions
    <System.Runtime.CompilerServices.Extension>
    Public Function StdDev(Of T)(values As IEnumerable(Of T), selector As Func(Of T, Double?)) As Double
        Dim result As Double = 0
        Dim count = values.Count()
        If count > 0 Then
            Dim avg? As Double = values.Average(selector)
            Dim sum As Double = values.Select(selector).Sum(Function(d)
                If d.HasValue Then
                    Return Math.Pow(d.Value - avg.Value, 2)
                End If
                Return 0.0
            End Function)
            result = Math.Sqrt((sum) / (count - 1))
        End If
        Return result
    End Function
End Module

' 3. Use custom aggregate in summary
Dim tableSummaryRow As New GridTableSummaryRow()
tableSummaryRow.Name = "TableSummary"
tableSummaryRow.ShowSummaryInRow = True
tableSummaryRow.Title = "Standard Deviation for Total Sales: {TotalSales}"
tableSummaryRow.Position = TableSummaryRowPosition.Top

Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "TotalSales"
summaryColumn.SummaryType = SummaryType.Custom
summaryColumn.CustomAggregate = New CustomAggregate()
summaryColumn.Format = "{StdDev}"

tableSummaryRow.SummaryColumns.Add(summaryColumn)
sfDataGrid.TableSummaryRows.Add(tableSummaryRow)
```

---

## Summary Formatting

### Using Summary Functions

**C# Example:**
```csharp
// Basic function usage
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "UnitPrice";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
summaryColumn.Format = "{Sum}"; // Shows sum value
summaryColumn.MappingName = "UnitPrice";
```

**VB.NET Example:**
```vb
' Basic function usage
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "UnitPrice"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
summaryColumn.Format = "{Sum}" ' Shows sum value
summaryColumn.MappingName = "UnitPrice"
```

### Formatting Summary Values

**C# Example:**
```csharp
// Apply currency format
summaryColumn.Format = "{Sum:c}"; // Currency format

// Apply numeric format with decimals
summaryColumn.Format = "{Sum:n2}"; // Two decimal places

// Apply percentage format
summaryColumn.Format = "{Average:p}"; // Percentage format

// Apply date format
summaryColumn.Format = "{Count:d}"; // Date format
```

**VB.NET Example:**
```vb
' Apply currency format
summaryColumn.Format = "{Sum:c}" ' Currency format

' Apply numeric format with decimals
summaryColumn.Format = "{Sum:n2}" ' Two decimal places

' Apply percentage format
summaryColumn.Format = "{Average:p}" ' Percentage format

' Apply date format
summaryColumn.Format = "{Count:d}" ' Date format
```

### Adding Text to Summary

**C# Example:**
```csharp
// Add descriptive text
GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Format = "Total Price: {Sum:c}";
summaryColumn.MappingName = "UnitPrice";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;
```

**VB.NET Example:**
```vb
' Add descriptive text
Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Format = "Total Price: {Sum:c}"
summaryColumn.MappingName = "UnitPrice"
summaryColumn.SummaryType = SummaryType.DoubleAggregate
```

### Formatting Row Summary

**C# Example:**
```csharp
// Format entire summary row
GridTableSummaryRow tableSummaryRow = new GridTableSummaryRow();
tableSummaryRow.ShowSummaryInRow = true;
tableSummaryRow.Title = "Total Price: {PriceSum} for {ProductCount} Products";

// Define summary columns for placeholders
GridSummaryColumn priceColumn = new GridSummaryColumn();
priceColumn.Name = "PriceSum";
priceColumn.Format = "{Sum:c}";
priceColumn.MappingName = "UnitPrice";
priceColumn.SummaryType = SummaryType.DoubleAggregate;

GridSummaryColumn countColumn = new GridSummaryColumn();
countColumn.Name = "ProductCount";
countColumn.Format = "{Count}";
countColumn.MappingName = "ProductName";
countColumn.SummaryType = SummaryType.CountAggregate;

tableSummaryRow.SummaryColumns.Add(priceColumn);
tableSummaryRow.SummaryColumns.Add(countColumn);
```

**VB.NET Example:**
```vb
' Format entire summary row
Dim tableSummaryRow As New GridTableSummaryRow()
tableSummaryRow.ShowSummaryInRow = True
tableSummaryRow.Title = "Total Price: {PriceSum} for {ProductCount} Products"

' Define summary columns for placeholders
Dim priceColumn As New GridSummaryColumn()
priceColumn.Name = "PriceSum"
priceColumn.Format = "{Sum:c}"
priceColumn.MappingName = "UnitPrice"
priceColumn.SummaryType = SummaryType.DoubleAggregate

Dim countColumn As New GridSummaryColumn()
countColumn.Name = "ProductCount"
countColumn.Format = "{Count}"
countColumn.MappingName = "ProductName"
countColumn.SummaryType = SummaryType.CountAggregate

tableSummaryRow.SummaryColumns.Add(priceColumn)
tableSummaryRow.SummaryColumns.Add(countColumn)
```

---

## Summary Calculation Options

### On-Demand Calculation

**C# Example:**
```csharp
// Calculate summaries on-demand for performance
sfDataGrid.SummaryCalculationMode = 
    CalculationMode.OnDemandCaptionSummary | 
    CalculationMode.OnDemandGroupSummary;
```

**VB.NET Example:**
```vb
' Calculate summaries on-demand for performance
sfDataGrid.SummaryCalculationMode = 
    CalculationMode.OnDemandCaptionSummary Or 
    CalculationMode.OnDemandGroupSummary
```

### Calculate for Selected Rows

**C# Example:**
```csharp
// Global setting for all summaries
sfDataGrid.SummaryCalculationUnit = SummaryCalculationUnit.SelectedRows;

// Or per summary row
GridTableSummaryRow summaryRow = new GridTableSummaryRow();
summaryRow.CalculationUnit = SummaryCalculationUnit.SelectedRows;
summaryRow.ShowSummaryInRow = true;
summaryRow.Title = "Total for selected: {UnitPrice}";
summaryRow.Position = VerticalPosition.Top;

GridSummaryColumn summaryColumn = new GridSummaryColumn();
summaryColumn.Name = "UnitPrice";
summaryColumn.Format = "{Sum:c}";
summaryColumn.MappingName = "UnitPrice";
summaryColumn.SummaryType = SummaryType.DoubleAggregate;

summaryRow.SummaryColumns.Add(summaryColumn);
sfDataGrid.TableSummaryRows.Add(summaryRow);
```

**VB.NET Example:**
```vb
' Global setting for all summaries
sfDataGrid.SummaryCalculationUnit = SummaryCalculationUnit.SelectedRows

' Or per summary row
Dim summaryRow As New GridTableSummaryRow()
summaryRow.CalculationUnit = SummaryCalculationUnit.SelectedRows
summaryRow.ShowSummaryInRow = True
summaryRow.Title = "Total for selected: {UnitPrice}"
summaryRow.Position = VerticalPosition.Top

Dim summaryColumn As New GridSummaryColumn()
summaryColumn.Name = "UnitPrice"
summaryColumn.Format = "{Sum:c}"
summaryColumn.MappingName = "UnitPrice"
summaryColumn.SummaryType = SummaryType.DoubleAggregate

summaryRow.SummaryColumns.Add(summaryColumn)
sfDataGrid.TableSummaryRows.Add(summaryRow)
```

---

## Summary Appearance

### Table Summary Style

**C# Example:**
```csharp
// Customize table summary appearance
sfDataGrid.Style.TableSummaryRowStyle.BackColor = Color.LightSkyBlue;
sfDataGrid.Style.TableSummaryRowStyle.Borders.All = 
    new GridBorder(Color.Black, GridBorderWeight.Medium);
sfDataGrid.Style.TableSummaryRowStyle.Font = 
    new GridFontInfo(new Font("Arial", 10f, FontStyle.Bold));
```

**VB.NET Example:**
```vb
' Customize table summary appearance
sfDataGrid.Style.TableSummaryRowStyle.BackColor = Color.LightSkyBlue
sfDataGrid.Style.TableSummaryRowStyle.Borders.All = 
    New GridBorder(Color.Black, GridBorderWeight.Medium)
sfDataGrid.Style.TableSummaryRowStyle.Font = 
    New GridFontInfo(New Font("Arial", 10f, FontStyle.Bold))
```

### Group Summary Style

**C# Example:**
```csharp
// Customize group summary appearance
sfDataGrid.Style.GroupSummaryRowStyle.BackColor = Color.LightSkyBlue;
sfDataGrid.Style.GroupSummaryRowStyle.Font = 
    new GridFontInfo(new Font("Arial", 10f, FontStyle.Bold));
```

**VB.NET Example:**
```vb
' Customize group summary appearance
sfDataGrid.Style.GroupSummaryRowStyle.BackColor = Color.LightSkyBlue
sfDataGrid.Style.GroupSummaryRowStyle.Font = 
    New GridFontInfo(New Font("Arial", 10f, FontStyle.Bold))
```

### Caption Summary Style

**C# Example:**
```csharp
// Customize caption summary appearance
sfDataGrid.Style.CaptionSummaryRowStyle.BackColor = Color.LightSkyBlue;
sfDataGrid.Style.CaptionSummaryRowStyle.Font = 
    new GridFontInfo(new Font("Arial", 10f, FontStyle.Bold));
```

**VB.NET Example:**
```vb
' Customize caption summary appearance
sfDataGrid.Style.CaptionSummaryRowStyle.BackColor = Color.LightSkyBlue
sfDataGrid.Style.CaptionSummaryRowStyle.Font = 
    New GridFontInfo(New Font("Arial", 10f, FontStyle.Bold))
```

### Custom Summary Renderer

**C# Example:**
```csharp
// Create custom table summary renderer
public class CustomGridTableSummaryRenderer : GridTableSummaryCellRenderer
{
    protected override void OnRender(Graphics paint, Rectangle cellRect, 
        string cellValue, CellStyleInfo style, DataColumnBase column, 
        RowColumnIndex rowColumnIndex)
    {
        if (string.IsNullOrEmpty(cellValue))
            return;

        // Apply custom number format
        NumberFormatInfo format = new NumberFormatInfo();
        format.NumberDecimalDigits = 3;
        format.NumberDecimalSeparator = " * ";
        format.NumberGroupSeparator = ",";

        cellValue = Convert.ToDouble(
            double.Parse(cellValue, NumberStyles.Currency)
        ).ToString("N", format);

        StringFormat stringFormat = new StringFormat();
        stringFormat.LineAlignment = StringAlignment.Center;
        stringFormat.Alignment = StringAlignment.Center;

        paint.DrawString(cellValue, style.Font.GetFont(), 
            Brushes.Black, cellRect, stringFormat);
    }
}

// Replace default renderer
sfDataGrid.CellRenderers.Remove("TableSummary");
sfDataGrid.CellRenderers.Add("TableSummary", new CustomGridTableSummaryRenderer());
```

**VB.NET Example:**
```vb
' Create custom table summary renderer
Public Class CustomGridTableSummaryRenderer
    Inherits GridTableSummaryCellRenderer

    Protected Overrides Sub OnRender(paint As Graphics, cellRect As Rectangle, 
        cellValue As String, style As CellStyleInfo, column As DataColumnBase, 
        rowColumnIndex As RowColumnIndex)
        
        If String.IsNullOrEmpty(cellValue) Then
            Return
        End If

        ' Apply custom number format
        Dim format As New NumberFormatInfo()
        format.NumberDecimalDigits = 3
        format.NumberDecimalSeparator = " * "
        format.NumberGroupSeparator = ","

        cellValue = Convert.ToDouble(
            Double.Parse(cellValue, NumberStyles.Currency)
        ).ToString("N", format)

        Dim stringFormat As New StringFormat()
        stringFormat.LineAlignment = StringAlignment.Center
        stringFormat.Alignment = StringAlignment.Center

        paint.DrawString(cellValue, style.Font.GetFont(), 
            Brushes.Black, cellRect, stringFormat)
    End Sub
End Class

' Replace default renderer
sfDataGrid.CellRenderers.Remove("TableSummary")
sfDataGrid.CellRenderers.Add("TableSummary", New CustomGridTableSummaryRenderer())
```

---

## Edge Cases and Troubleshooting

### Common Issues

1. **Summary Not Displaying**
   - Verify `SummaryColumns` are added to `GridSummaryRow`
   - Check `MappingName` matches column property name
   - Ensure `SummaryType` is appropriate for data type

2. **Summary Title Not Showing**
   - Set `ShowSummaryInRow = true` to display title
   - Define `Title` property with placeholders matching `GridSummaryColumn.Name`
   - Verify placeholder names match summary column names

3. **Formatting Issues**
   - Use correct format specifiers (c, n, p, d, etc.)
   - Check format string syntax: `{FunctionName:FormatSpecifier}`
   - Ensure aggregate type supports the function used

4. **TitleColumnCount Not Working**
   - Only works when `ShowSummaryInRow = false`
   - If `FrozenColumnCount` is less than `TitleColumnCount`, title spans to frozen range
   - Summary columns in title range will not be displayed

5. **Custom Aggregate Not Calculating**
   - Verify `ISummaryAggregate` interface is implemented correctly
   - Check `CalculateAggregateFunc` return action
   - Ensure property name in `Format` matches aggregate property

6. **Performance Issues with Many Summaries**
   - Use `SummaryCalculationMode` with `OnDemandCaptionSummary` or `OnDemandGroupSummary`
   - Calculate only necessary summaries
   - Consider using custom aggregates for complex calculations

7. **Selected Rows Summary Not Working**
   - Cell selection is not supported for `SummaryCalculationUnit.SelectedRows`
   - Only works with row selection
   - `GridSummaryRow.CalculationUnit` takes priority over `SfDataGrid.SummaryCalculationUnit`

### Best Practices

1. **Summary Configuration**
   - Use descriptive names for `GridSummaryColumn.Name`
   - Keep format strings simple and readable
   - Document custom aggregate logic

2. **Performance Optimization**
   - Enable on-demand calculation for large datasets
   - Limit number of summary rows and columns
   - Use appropriate aggregate types

3. **User Experience**
   - Provide clear summary labels
   - Use consistent formatting across summaries
   - Consider summary positioning (top vs bottom)

4. **Custom Aggregates**
   - Handle null values appropriately
   - Consider edge cases (empty collections, invalid data)
   - Optimize calculation logic

5. **Styling**
   - Use consistent styling for all summary types
   - Ensure summaries are visually distinct from data rows
   - Consider readability with background colors and fonts

### Title and Column Summary Limitations

When displaying column summaries with title (`TitleColumnCount` and `ShowSummaryInRow = false`):

- Title spans the first N columns based on `TitleColumnCount`
- Frozen columns take precedence: if `FrozenColumnCount` < `TitleColumnCount`, title only spans frozen columns
- Summary columns defined in title range are not displayed
- Applies to all summary types: Table, Group, and Caption
