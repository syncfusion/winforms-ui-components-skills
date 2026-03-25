# Covered Ranges

This guide covers covered ranges (merged cells) in GridControl, which allows spanning multiple cells into a single cell, similar to Excel's merge functionality.

## Overview

Covered ranges allow multiple cells to be displayed as a single cell. Unlike merged cells that combine data, covered ranges maintain the original cell structure but display them as one.

**Key points:**
- Range of cells displayed as single cell
- Value stored in top-left cell of range
- Other cells in range are hidden
- Similar to Excel merge cells

## Adding Covered Ranges

### Basic Syntax:

```csharp
// Add covered range
gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(topRow, leftCol, bottomRow, rightCol));
```

### Simple Example:

```csharp
// Cover cells from (2,2) to (5,5)
gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(2, 2, 5, 5));

// Set value in the covered range
gridControl1[2, 2].CellValue = "Merged Cell";
```

### Multiple Covered Ranges:

```csharp
// Add multiple covered ranges
gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(2, 2, 4, 4));
gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(6, 2, 8, 5));
gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(10, 3, 12, 7));

// Set values
gridControl1[2, 2].CellValue = "First Merged Area";
gridControl1[6, 2].CellValue = "Second Merged Area";
gridControl1[10, 3].CellValue = "Third Merged Area";
```

## Dynamic Covered Ranges with QueryCoveredRange

The `QueryCoveredRange` event allows dynamic covered range determination based on cell content or conditions.

### Basic Event Usage:

```csharp
gridControl1.QueryCoveredRange += GridControl1_QueryCoveredRange;

private void GridControl1_QueryCoveredRange(object sender, GridQueryCoveredRangeEventArgs e)
{
    // Checking the cell to start covered range
    if (e.RowIndex == 2 && e.ColIndex == 2)
    {
        // Setting the range to be covered
        e.Range = GridRangeInfo.Cells(e.RowIndex, e.ColIndex, e.RowIndex + 3, e.ColIndex + 5);
        
        // Must set Handled to true
        e.Handled = true;
    }
}
```

### Conditional Covered Ranges:

```csharp
private void GridControl1_QueryCoveredRange(object sender, GridQueryCoveredRangeEventArgs e)
{
    // Cover cells based on content
    string cellValue = gridControl1[e.RowIndex, e.ColIndex].Text;
    
    if (cellValue == "MERGE")
    {
        // Merge 2x3 area
        e.Range = GridRangeInfo.Cells(e.RowIndex, e.ColIndex, e.RowIndex + 1, e.ColIndex + 2);
        e.Handled = true;
    }
    else if (cellValue.StartsWith("TITLE"))
    {
        // Merge entire row
        e.Range = GridRangeInfo.Cells(e.RowIndex, 1, e.RowIndex, gridControl1.ColCount);
        e.Handled = true;
    }
}
```

## Styling Covered Cells

Apply styles to covered ranges for visual distinction.

### Basic Styling:

```csharp
// Add covered range
gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(2, 2, 4, 5));

// Style the covered cell
GridStyleInfo style = new GridStyleInfo();
style.BackColor = Color.LightBlue;
style.TextColor = Color.DarkBlue;
style.Font.Bold = true;
style.Font.Size = 12f;
style.HorizontalAlignment = GridHorizontalAlignment.Center;
style.VerticalAlignment = GridVerticalAlignment.Middle;

gridControl1.ChangeCells(GridRangeInfo.Cells(2, 2, 4, 5), style);

// Set value
gridControl1[2, 2].CellValue = "Styled Merged Cell";
```

### Create Title Row:

```csharp
// Cover entire first row for title
gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(1, 1, 1, 10));

// Style title
GridStyleInfo titleStyle = new GridStyleInfo
{
    BackColor = Color.Navy,
    TextColor = Color.White,
    Font = { Bold = true, Size = 14f },
    HorizontalAlignment = GridHorizontalAlignment.Center,
    VerticalAlignment = GridVerticalAlignment.Middle
};

gridControl1.ChangeCells(GridRangeInfo.Row(1), titleStyle);
gridControl1[1, 1].CellValue = "Report Title";
```

## Removing Covered Ranges

### Remove Specific Range:

```csharp
// Remove a specific covered range
gridControl1.CoveredRanges.Remove(GridRangeInfo.Cells(2, 2, 5, 7));
```

### Clear All Covered Ranges:

```csharp
// Remove all covered ranges
gridControl1.CoveredRanges.Clear();
```

### Remove Covered Range at Cell:

```csharp
// Remove covered range that contains specific cell
private void RemoveCoveredRangeAtCell(int row, int col)
{
    GridRangeInfo rangeToRemove = null;
    
    foreach (GridRangeInfo range in gridControl1.CoveredRanges)
    {
        if (range.Contains(GridRangeInfo.Cell(row, col)))
        {
            rangeToRemove = range;
            break;
        }
    }
    
    if (rangeToRemove != null)
    {
        gridControl1.CoveredRanges.Remove(rangeToRemove);
    }
}
```

## Practical Examples

### Report Header:

```csharp
private void CreateReportHeader()
{
    // Title row
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(1, 1, 1, 10));
    gridControl1[1, 1].CellValue = "Sales Report - Q4 2023";
    gridControl1[1, 1].BackColor = Color.DarkBlue;
    gridControl1[1, 1].TextColor = Color.White;
    gridControl1[1, 1].Font.Bold = true;
    gridControl1[1, 1].Font.Size = 16f;
    gridControl1[1, 1].HorizontalAlignment = GridHorizontalAlignment.Center;
    
    // Subtitle row
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(2, 1, 2, 10));
    gridControl1[2, 1].CellValue = "Regional Performance Analysis";
    gridControl1[2, 1].BackColor = Color.LightBlue;
    gridControl1[2, 1].Font.Italic = true;
    gridControl1[2, 1].HorizontalAlignment = GridHorizontalAlignment.Center;
}
```

### Section Headers:

```csharp
private void CreateSectionHeaders()
{
    // Section 1
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(5, 1, 5, 5));
    gridControl1[5, 1].CellValue = "Product Details";
    gridControl1[5, 1].BackColor = Color.LightGray;
    gridControl1[5, 1].Font.Bold = true;
    
    // Section 2
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(15, 1, 15, 5));
    gridControl1[15, 1].CellValue = "Financial Summary";
    gridControl1[15, 1].BackColor = Color.LightGray;
    gridControl1[15, 1].Font.Bold = true;
}
```

### Multi-Column Header:

```csharp
private void CreateMultiColumnHeader()
{
    // Main header
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(1, 2, 1, 4));
    gridControl1[1, 2].CellValue = "Q1 Sales";
    gridControl1[1, 2].HorizontalAlignment = GridHorizontalAlignment.Center;
    gridControl1[1, 2].BackColor = Color.SteelBlue;
    gridControl1[1, 2].TextColor = Color.White;
    
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(1, 5, 1, 7));
    gridControl1[1, 5].CellValue = "Q2 Sales";
    gridControl1[1, 5].HorizontalAlignment = GridHorizontalAlignment.Center;
    gridControl1[1, 5].BackColor = Color.SteelBlue;
    gridControl1[1, 5].TextColor = Color.White;
    
    // Sub-headers
    gridControl1[2, 2].Text = "Jan";
    gridControl1[2, 3].Text = "Feb";
    gridControl1[2, 4].Text = "Mar";
    gridControl1[2, 5].Text = "Apr";
    gridControl1[2, 6].Text = "May";
    gridControl1[2, 7].Text = "Jun";
}
```

## Covered Ranges vs Merged Cells

### Covered Ranges (GridControl):
- Original cell structure maintained
- Value stored in top-left cell
- Other cells accessible but hidden
- Better for dynamic layouts

### Merged Cells (Excel):
- Cells physically merged
- Data combined
- Other cells lose identity

## Best Practices

1. **Always set value in top-left cell** of covered range
2. **Apply styling to entire covered range** for consistency
3. **Use centered alignment** for merged cell content
4. **Clear covered ranges** before recreating them
5. **Test with different grid sizes** to ensure covered ranges fit
6. **Document covered range locations** for maintenance
7. **Use QueryCoveredRange** for dynamic scenarios

## Common Patterns

### Create Summary Section:

```csharp
private void CreateSummarySection(int startRow)
{
    // Summary header
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(startRow, 1, startRow, 10));
    gridControl1[startRow, 1].CellValue = "Summary";
    gridControl1[startRow, 1].BackColor = Color.LightGreen;
    gridControl1[startRow, 1].Font.Bold = true;
    gridControl1[startRow, 1].HorizontalAlignment = GridHorizontalAlignment.Center;
}
```

### Create Note Section:

```csharp
private void AddNoteSection(int row, int col, string note)
{
    gridControl1.CoveredRanges.Add(GridRangeInfo.Cells(row, col, row + 2, col + 5));
    gridControl1[row, col].CellValue = note;
    gridControl1[row, col].BackColor = Color.LightYellow;
    gridControl1[row, col].VerticalAlignment = GridVerticalAlignment.Top;
    gridControl1[row, col].WrapText = true;
}
```

## Troubleshooting

### Covered range not showing
- Verify range is added to CoveredRanges collection
- Check if range coordinates are valid
- Ensure grid has enough rows/columns

### Value not displaying
- Verify value is set in top-left cell of range
- Check if styling is hiding text (color, font)
- Ensure cell type is appropriate

### Overlapping covered ranges
- Check for range conflicts
- Use Clear() before adding new ranges
- Validate range boundaries

### Performance issues
- Limit number of covered ranges
- Use QueryCoveredRange for large, dynamic scenarios
- Consider alternative layouts if too many merges

## Next Steps

- Implement dynamic covered ranges based on data
- Create custom report layouts
- Add covered range validation
- Export covered ranges to Excel
- Implement interactive merge/unmerge functionality
