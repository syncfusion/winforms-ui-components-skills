# Rows, Columns, and Summaries in Windows Forms Pivot Grid

## Table of Contents

- [Overview](#overview)
- [Pivot Rows](#pivot-rows)
  - [Defining Pivot Rows](#defining-pivot-rows)
  - [Synchronizing Pivot Rows](#synchronizing-pivot-rows)
  - [Sorting Pivot Rows](#sorting-pivot-rows)
- [Pivot Columns](#pivot-columns)
  - [Defining Pivot Columns](#defining-pivot-columns)
  - [Synchronizing Pivot Columns](#synchronizing-pivot-columns)
  - [Sorting Pivot Columns](#sorting-pivot-columns)
- [Summaries](#summaries)
  - [Summary Types](#summary-types)
  - [Customizing Summary Type](#customizing-summary-type)
  - [Custom Summaries](#custom-summaries)
- [Use Cases](#use-cases)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion Windows Forms Pivot Grid control uses PivotItem objects to define rows and columns, along with various summary types for aggregating data. This guide covers how to configure rows, columns, and summaries to create powerful data analysis reports.

## Pivot Rows

Pivot rows are defined by using the `PivotItem` object which holds the information needed for rows that appear in the pivot grid control.

### Defining Pivot Rows

To define a pivot row item, the following properties of `PivotItem` object are used:

| Property Name | Description | Type |
|--------------|-------------|------|
| Comparer | Gets or sets the IComparer object used for sorting. If this value is null, then sorting will be performed under the assumption that this field is IComparable. | IComparer |
| FieldHeader | Gets or sets the title you want to see in the header for this pivot item. | string |
| FieldMappingName | Gets or sets the property's mapping name. | string |
| Format | Gets or sets the format item for the specified field. | string |
| TotalHeader | Gets or sets the string you want to append to the pivot item's summary cells. | string |

#### C# Example

```csharp
// Defining pivot item
PivotItem pivotItem = new PivotItem() 
{ 
    FieldHeader = "Product", 
    FieldMappingName = "Product", 
    TotalHeader = "Total" 
};

// Adding pivot row item to pivot grid
this.pivotGridControl1.PivotRows.Add(pivotItem);
```

#### VB.NET Example

```vb
' Defining pivot item
Dim pivotItem As New PivotItem() With 
{
    .FieldHeader = "Product", 
    .FieldMappingName = "Product", 
    .TotalHeader = "Total"
}

' Adding pivot row item to pivot grid
Me.pivotGridControl1.PivotRows.Add(pivotItem)
```

### Synchronizing Pivot Rows

To synchronize the newly added or modified pivot row items with the pivot grid control, the `SynchronizePivotItems` method will be used. This method will be invoked whenever the collection of pivot row items gets changed.

### Sorting Pivot Rows

By default, the pivot grid control sorts the row data in ascending order. The sorting order can be changed by defining custom comparer and it needs to be assigned using the `Comparer` property of corresponding pivot item.

#### C# Example - Custom Comparer

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        this.pivotGridControl1.PivotRows.Add(new PivotItem 
        { 
            FieldMappingName = "Product", 
            Comparer = new ReverseOrderComparer() 
        });
    }
}

// Reverse order comparer for sorting the data in descending order
public class ReverseOrderComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (x == null && y == null)
            return 0;
        else if (y == null)
            return 1;
        else if (x == null)
            return -1;
        else
            return -x.ToString().CompareTo(y.ToString());
    }
}
```

#### VB.NET Example - Custom Comparer

```vb
Partial Public Class Form1
    Inherits Form
    Public Sub New()
        Me.pivotGridControl1.PivotRows.Add(New PivotItem With 
        {
            .FieldMappingName = "Product", 
            .Comparer = New ReverseOrderComparer()
        })
    End Sub
End Class

' Reverse order comparer for sorting the data in descending order
Public Class ReverseOrderComparer
    Implements IComparer
    
    Public Function Compare(ByVal x As Object, ByVal y As Object) As Integer Implements IComparer.Compare
        If x Is Nothing AndAlso y Is Nothing Then
            Return 0
        ElseIf y Is Nothing Then
            Return 1
        ElseIf x Is Nothing Then
            Return -1
        Else
            Return -x.ToString().CompareTo(y.ToString())
        End If
    End Function
End Class
```

## Pivot Columns

Pivot columns are defined by using the `PivotItem` object which holds the information needed for columns that appear in the pivot grid control.

### Defining Pivot Columns

To define a pivot column item, the same properties of `PivotItem` object are used as with pivot rows:

- **Comparer**: IComparer object for sorting
- **FieldHeader**: Title for the header
- **FieldMappingName**: Property's mapping name
- **Format**: Format item for the field
- **TotalHeader**: String for summary cells

#### C# Example

```csharp
// Defining pivot item
PivotItem pivotItem = new PivotItem() 
{ 
    FieldHeader = "Country", 
    FieldMappingName = "Country", 
    TotalHeader = "Total" 
};

// Adding pivot column item to pivot grid
this.pivotGridControl1.PivotColumns.Add(pivotItem);
```

#### VB.NET Example

```vb
' Defining pivot item
Dim pivotItem As New PivotItem() With 
{
    .FieldHeader = "Country", 
    .FieldMappingName = "Country", 
    .TotalHeader = "Total"
}

' Adding pivot column item to pivot grid
Me.pivotGridControl1.PivotColumns.Add(pivotItem)
```

### Synchronizing Pivot Columns

To synchronize the newly added or modified pivot column items with the pivot grid control, the `SynchronizePivotItems` method will be used. This method will be invoked whenever the collection of pivot column items gets changed.

### Sorting Pivot Columns

By default, the pivot grid control sorts the column data in ascending order. The sorting order can be changed by defining custom comparer and it needs to be assigned using the `Comparer` property of corresponding pivot item.

#### C# Example

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        this.pivotGridControl1.PivotColumns.Add(new PivotItem 
        { 
            FieldMappingName = "Country", 
            Comparer = new ReverseOrderComparer() 
        });
    }
}

// Reverse order comparer for sorting the data in descending order
public class ReverseOrderComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (x == null && y == null)
            return 0;
        else if (y == null)
            return 1;
        else if (x == null)
            return -1;
        else
            return -x.ToString().CompareTo(y.ToString());
    }
}
```

## Summaries

Summaries can be defined for the pivot calculation values in the pivot grid. Pivot grid control supports 19 built-in summary types to customize the summaries.

### Summary Types

Pivot grid summarizes the data for various data types by using the `SummaryType` enumerator. The SummaryType value should be defined while defining the PivotCalculation items using `PivotComputationInfo` class to specify the type of the summary.

| Summary Type | Description |
|-------------|-------------|
| DoubleTotalSum | Computes the sum of double or integer values. |
| DoubleAverage | Computes the simple average of double or integer values. |
| DoubleMaximum | Computes the maximum of double or integer values. |
| DoubleMinimum | Computes the minimum of double or integer values. |
| DoubleStandardDeviation | Computes the standard deviation of double or integer values. |
| DoubleVariance | Computes the variance of double or integer values. |
| Count | Computes the count of double or integer values. |
| DecimalTotalSum | Computes the sum of decimal values. |
| IntTotalSum | Computes the sum of integer values. |
| Custom | Specifies that you are using a custom SummaryBase object to define the calculation. |

### Customizing Summary Type

Summary type of pivot calculation values can be customized by using the pivot computation information dialog. While double clicking on a calculation item in the value layout section of pivot schema designer, the pivot computation information dialog will be displayed. The summary type of pivot calculation item can be changed by using the "Summarize Value By" combo box.

### Custom Summaries

PivotGrid allows to set custom summaries for pivot calculation values by creating a custom `SummaryBase` class. For creating a custom summary, a new class need to be added by inheriting the abstract class `SummaryBase`. Summary logics can be overridden by overriding the following methods: `Combine()`, `CombineSummary()`, `GetResult()`, `GetInstance()` and `Reset()`.

#### C# Example - Custom Summary

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
        { 
            FieldName = "Amount", 
            Format = "C", 
            SummaryType = SummaryType.Custom, 
            Summary = new CustomSummaryBase() 
        });
    }
}

public class CustomSummaryBase : SummaryBase
{
    private double mTotalValue;

    public override void Combine(object other)
    {
        mTotalValue += (double)other;
    }

    public override void CombineSummary(SummaryBase other)
    {
        CustomSummaryBase customSummaryBase = other as CustomSummaryBase;
        if (customSummaryBase != null)
        {
            mTotalValue += customSummaryBase.mTotalValue;
        }
    }

    public override SummaryBase GetInstance()
    {
        return new CustomSummaryBase();
    }

    public override object GetResult()
    {
        return mTotalValue / 3.33333;
    }

    public override void Reset()
    {
        mTotalValue = 0;
    }
}
```

#### VB.NET Example - Custom Summary

```vb
Partial Public Class Form1
    Inherits Form
    Public Sub New()
        pivotGridControl1.PivotCalculations.Add(New PivotComputationInfo With 
        {
            .FieldName = "Amount", 
            .Format = "C", 
            .SummaryType = SummaryType.Custom, 
            .Summary = New CustomSummaryBase()
        })
    End Sub
End Class

Public Class CustomSummaryBase
    Inherits SummaryBase
    Private mTotalValue As Double

    Public Overrides Sub Combine(ByVal other As Object)
        mTotalValue += CDbl(other)
    End Sub

    Public Overrides Sub CombineSummary(ByVal other As SummaryBase)
        Dim customSummaryBase As CustomSummaryBase = TryCast(other, CustomSummaryBase)
        If customSummaryBase IsNot Nothing Then
            mTotalValue += customSummaryBase.mTotalValue
        End If
    End Sub

    Public Overrides Function GetInstance() As SummaryBase
        Return New CustomSummaryBase()
    End Function

    Public Overrides Function GetResult() As Object
        Return mTotalValue / 3.33333
    End Function

    Public Overrides Sub Reset()
        mTotalValue = 0
    End Sub
End Class
```

## Use Cases

### Business Reporting
Configure rows, columns, and summaries to create comprehensive business reports with product categories as rows, time periods as columns, and sales summaries.

### Data Analysis
Use different summary types to analyze data from multiple perspectives - totals, averages, counts, standard deviations, etc.

### Custom Aggregations
Implement custom summary logic for specialized calculations that go beyond built-in summary types.

## Common Patterns

### Multiple Row Items
```csharp
pivotGridControl1.PivotRows.Add(new PivotItem { FieldMappingName = "Product", TotalHeader = "Product Total" });
pivotGridControl1.PivotRows.Add(new PivotItem { FieldMappingName = "Category", TotalHeader = "Category Total" });
```

### Multiple Column Items
```csharp
pivotGridControl1.PivotColumns.Add(new PivotItem { FieldMappingName = "Year", TotalHeader = "Year Total" });
pivotGridControl1.PivotColumns.Add(new PivotItem { FieldMappingName = "Quarter", TotalHeader = "Quarter Total" });
```

### Multiple Summaries
```csharp
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Amount", 
    Format = "C", 
    SummaryType = SummaryType.DoubleTotalSum 
});
pivotGridControl1.PivotCalculations.Add(new PivotComputationInfo 
{ 
    FieldName = "Amount", 
    FieldHeader = "Average Amount",
    Format = "C", 
    SummaryType = SummaryType.DoubleAverage 
});
```

## Troubleshooting

### Issue: Rows/Columns Not Displaying
**Solution**: Ensure that `FieldMappingName` matches the actual property name in your data source. Field names are case-sensitive.

### Issue: Incorrect Sorting
**Solution**: Verify that your custom comparer is properly implementing the IComparer interface and handling null values correctly.

### Issue: Summary Values Not Updating
**Solution**: Make sure to call the appropriate refresh methods after modifying pivot items. The `SynchronizePivotItems` method should be invoked when collections change.

### Issue: Custom Summary Not Working
**Solution**: Ensure all required methods (`Combine`, `CombineSummary`, `GetResult`, `GetInstance`, `Reset`) are properly overridden in your custom summary class.

### Issue: Formatting Not Applied
**Solution**: Check that the Format property is set with a valid format string (e.g., "C" for currency, "#,##0" for thousands separator).

### Issue: Total Headers Not Showing
**Solution**: Verify that the `TotalHeader` property is set on your PivotItem objects. If subtotals are hidden via `ShowSubTotals = false`, total headers won't be visible.
