# Filtering and Sorting in Windows Forms Pivot Grid

## Table of Contents

- [Overview](#overview)
- [Filtering](#filtering)
  - [Using Filter Expression Programmatically](#using-filter-expression-programmatically)
  - [Defining Filter Expressions](#defining-filter-expressions)
  - [Using Grouping Bar's Built-in Popup](#using-grouping-bars-built-in-popup)
  - [Filter Operators](#filter-operators)
- [Sorting](#sorting)
  - [Using Custom Comparer Programmatically](#using-custom-comparer-programmatically)
  - [Using Grouping Bar](#using-grouping-bar)
- [Use Cases](#use-cases)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion Windows Forms Pivot Grid control provides powerful filtering and sorting capabilities that allow users to analyze and organize data effectively. Filtering displays only a subset of data that meets specific criteria, while sorting organizes data in ascending or descending order.

## Filtering

The filtered data displays only a subset of data that meets a specific criteria and hides the data that you do not want to display. The filters are automatically re-applied every time when the pivot grid is refreshed or updated until you remove those filters. In the pivot grid, filters are additive, which means that each additional filter is based on the current filter and further reduces the subset of data. At a time, 'n' number of filtering conditions can be applied to the pivot grid.

### Using Filter Expression Programmatically

The `FilterExpression` class is used to encapsulate the information required to define a filter and it contains the following properties:

- **Expression**: Specifies the well-formed logical expression that defines the filter.
- **Name**: Specifies the name of the filter expression.
- **DimensionName**: Specifies the dimension name for filter expression.
- **DimensionHeader**: Specifies the dimension header for filter expression.
- **Format**: Specifies the format of filter expression.
- **Evaluator**: Evaluates the specified value.

Set the FieldMappingName property value into the DimensionName and DimensionHeader properties. Because it must be in the ItemProperties collection. If the DimensionHeader value is not defined in the FilterExpression, the DimensionName value is assigned to the DimensionHeader property automatically.

### Defining Filter Expressions

To define a filter, create an instance of `FilterExpression` class and add that instance to `Filters` collection of pivot grid control.

#### C# Example - Basic Filter

```csharp
FilterExpression filterExpression1 = new FilterExpression()
{
    DimensionHeader = "Product",
    Name = "Product",
    DimensionName = "Product",
    Expression = "Product = Bike"
};
this.pivotGridControl1.Filters.Add(filterExpression1);
```

#### VB.NET Example - Basic Filter

```vb
Dim filterExpression1 As New FilterExpression() With
{
    .DimensionHeader = "Product",
    .Name = "Product",
    .DimensionName = "Product",
    .Expression = "Product = Bike"
}
pivotGridControl1.Filters.Add(filterExpression1)
```

### Filter Operators

#### OR Operator

The OR operator allows filtering data that matches any of the specified conditions.

##### C# Example

```csharp
FilterExpression filterExpression1 = new FilterExpression() 
{ 
    DimensionHeader = "Date", 
    Name = "Date", 
    DimensionName = "Date", 
    Expression = "Date = FY 2007 OR Date = FY 2008" 
}; 
this.pivotGridControl1.Filters.Add(filterExpression1);
```

##### VB.NET Example

```vb
Dim filterExpression1 As New FilterExpression() With
{
    .DimensionHeader = "Date",
    .Name = "Date",
    .DimensionName = "Date",
    .Expression = "Date = FY 2007 OR Date = FY 2008"
}
pivotGridControl1.Filters.Add(filterExpression1)
```

#### IN Operator

The IN operator allows filtering data that matches any value in a specified list.

##### C# Example

```csharp
FilterExpression filterExpression1 = new FilterExpression() 
{ 
    DimensionHeader = "Date", 
    Name = "Date", 
    DimensionName = "Date", 
    Expression = "Date IN FY 2005,FY 2006,FY 2007" 
}; 
this.pivotGridControl1.Filters.Add(filterExpression1);
```

##### VB.NET Example

```vb
Dim filterExpression1 As New FilterExpression() With
{
    .DimensionHeader = "Date",
    .Name = "Date",
    .DimensionName = "Date",
    .Expression = "Date IN FY 2005,FY 2006,FY 2007"
}
pivotGridControl1.Filters.Add(filterExpression1)
```

### Using Grouping Bar's Built-in Popup

The filtering operation can also be performed by using the built-in popup integrated with the filter, row and column header areas of grouping bar.

While clicking on the filter icon present in the header item of row or column or filter header area, a filter popup will be opened by displaying its corresponding list of values. By selecting and unselecting the required values in the list, filtering will be applied to particular header item.

#### Steps to Use Filter Popup:
1. Enable the grouping bar: `pivotGridControl1.ShowGroupBar = true;`
2. Click on the filter icon in any header item
3. Select or deselect values from the popup list
4. Click OK to apply the filter

## Sorting

Pivot grid provides support for sorting which enables users to quickly visualize, organize and understand the data in a better way. Sorting feature also helps to find the data that you want and make more effective decisions ultimately.

### Using Custom Comparer Programmatically

By default, the pivot grid control populates the data in the ascending order. This sorting order can be changed by using the `Comparer` property of pivot item.

#### C# Example

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        this.pivotGridControl1.PivotRows.Add(new PivotItem
        {
            FieldMappingName = "Product",
            TotalHeader = "Total",
            Comparer = new ReverseOrderComparer()
        });
    }
}

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

#### VB.NET Example

```vb
Partial Public Class Form1
    Inherits Form
    Public Sub New()
        Me.pivotGridControl1.PivotRows.Add(New PivotItem With
        {
            .FieldMappingName = "Product",
            .TotalHeader = "Total",
            .Comparer = New ReverseOrderComparer()
        })
    End Sub
End Class

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

### Using Grouping Bar

The sorting operation can also be performed by clicking on the header item of required pivot field present in row and column header areas of grouping bar.

By default, the pivot field values are sorted in the ascending order. On clicking the same header item once again will reverse the sorting direction. The sort indicator present at the right corner of the header item denotes the type of sorting applied to the pivot field such as ascending order or descending order.

#### Steps to Sort via Grouping Bar:
1. Enable the grouping bar: `pivotGridControl1.ShowGroupBar = true;`
2. Click on any header item in row or column area
3. First click sorts in ascending order
4. Second click reverses to descending order
5. Sort indicator shows current sort direction

#### Disabling Sorting in Grouping Bar

```csharp
this.pivotGridControl1.AllowSorting = false;
```

```vb
Me.pivotGridControl1.AllowSorting = False
```

## Use Cases

### Data Analysis
Filter data to focus on specific products, time periods, or regions while sorting to identify top performers or trends.

### Report Generation
Apply filters to create targeted reports for specific departments or categories, then sort to present data in a meaningful order.

### Performance Monitoring
Filter to show only items above or below certain thresholds, then sort to prioritize attention on critical items.

### Comparative Analysis
Use multiple filters with OR/IN operators to compare specific data sets side by side in sorted order.

## Common Patterns

### Multiple Filters

```csharp
// Filter by product
FilterExpression productFilter = new FilterExpression()
{
    DimensionHeader = "Product",
    Name = "Product",
    DimensionName = "Product",
    Expression = "Product = Bike OR Product = Car"
};
pivotGridControl1.Filters.Add(productFilter);

// Filter by date range
FilterExpression dateFilter = new FilterExpression()
{
    DimensionHeader = "Date",
    Name = "Date",
    DimensionName = "Date",
    Expression = "Date IN FY 2005,FY 2006,FY 2007"
};
pivotGridControl1.Filters.Add(dateFilter);
```

### Custom Sorting with Multiple Criteria

```csharp
public class CustomMultiCriteriaComparer : IComparer
{
    public int Compare(object x, object y)
    {
        if (x == null && y == null)
            return 0;
        else if (y == null)
            return 1;
        else if (x == null)
            return -1;
        
        string strX = x.ToString();
        string strY = y.ToString();
        
        // Custom logic for multi-criteria sorting
        // Example: Sort by length first, then alphabetically
        if (strX.Length != strY.Length)
            return strX.Length.CompareTo(strY.Length);
        else
            return strX.CompareTo(strY);
    }
}
```

### Disabling Filtering

```csharp
this.pivotGridControl1.AllowFiltering = false;
```

## Troubleshooting

### Issue: Filter Not Applied
**Solution**: Verify that DimensionName matches the FieldMappingName of your pivot item. Ensure the Expression syntax is correct and uses proper operators (=, OR, IN).

### Issue: Filter Expression Syntax Error
**Solution**: Check the Expression string format:
- Use `=` for equality: `"Product = Bike"`
- Use `OR` for multiple values: `"Date = FY 2007 OR Date = FY 2008"`
- Use `IN` for lists: `"Date IN FY 2005,FY 2006,FY 2007"`
- Ensure no extra spaces or special characters

### Issue: Sorting Not Working
**Solution**: Make sure AllowSorting is set to true. Verify that your custom comparer properly implements IComparer interface and handles null values.

### Issue: Filter Popup Not Showing
**Solution**: Ensure that:
- ShowGroupBar is set to true
- AllowFiltering is set to true
- The field is added to PivotRows, PivotColumns, or Filters collection

### Issue: Sort Indicator Not Visible
**Solution**: Check that the grouping bar is enabled and the field is added to row or column headers, not just the filter area.

### Issue: Multiple Filters Conflict
**Solution**: Remember that filters are additive. Each additional filter further restricts the data. Review all active filters in the Filters collection to ensure they work together logically.

### Issue: Custom Comparer Throws Exception
**Solution**: Ensure your Compare method properly handles:
- Null values for both parameters
- Type conversions
- Edge cases with empty strings or special characters

### Issue: Filter Expression with Special Characters
**Solution**: If your data contains special characters or spaces, ensure they are properly formatted in the Expression string. Consider using escape characters if needed.
