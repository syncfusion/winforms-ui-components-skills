# Performance Optimization

## Table of Contents
- [Overview](#overview)
- [Engine Optimizations](#engine-optimizations)
- [Counter Logic](#counter-logic)
- [Display Update Optimizations](#display-update-optimizations)
- [Drawing and Rendering](#drawing-and-rendering)
- [Common Scenarios](#common-scenarios)
- [Best Practices](#best-practices)

## Overview

GridGroupingControl provides extensive performance optimization features for handling large datasets, high-frequency updates, and virtual mode scenarios. Performance can be tuned through:

- **Engine optimizations**: DisableCounters, VirtualMode, PassThroughSort, RecordsAsDisplayElements
- **Counter logic**: Control which counters are maintained (FilteredRecords, YAmount, Hidden elements)
- **Display updates**: Control invalidation and refresh behavior
- **Drawing optimizations**: GDI text rendering, solid borders, efficient column width calculation

Use these optimizations when:
- Loading 10,000+ records
- Handling high-frequency data updates (real-time feeds)
- Flat tables without grouping/filtering
- Memory footprint is critical

## Engine Optimizations

### AllowedOptimizations Property

Enable optimizations that engine applies when criteria are met:

```csharp
using Syncfusion.Grouping;

// Enable all optimizations
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.All;

// Enable specific optimizations
gridGroupingControl1.AllowedOptimizations = 
    EngineOptimizations.DisableCounters | 
    EngineOptimizations.VirtualMode;
```

**EngineOptimizations enumeration:**

| Optimization | When Applied | Memory Savings | Use Case |
|-------------|--------------|----------------|----------|
| **DisableCounters** | No filters, groups, or nested relations | Medium (40-80 bytes/record) | Flat tables with sorting only |
| **VirtualMode** | No sorting, filters, groups, or relations | High (records created on-demand) | Read-only display of millions of records |
| **PassThroughSort** | IBindingList with SupportsSorting=true | Medium (uses DataView.Sort) | Sorted DataTable/DataView sources |
| **RecordsAsDisplayElements** | No nested tables, preview rows, or column sets | Small (10-20 bytes/record) | Simple record display |
| **All** | Combination of above when applicable | Varies | Let engine decide based on schema |

**Critical**: Optimizations apply automatically when schema criteria are met. Setting `AllowedOptimizations` doesn't force them—it allows engine to use them when possible.

### DisableCounters Optimization

When no filters, groups, or nested relations exist:

```csharp
// Table with sorting only (no grouping/filtering)
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.DisableCounters;

// Engine disables FilteredRecords, YAmount counters
// Records collection matches data source 1:1
// Sorting still works, but no filtering/grouping overhead
```

Criteria:
- No `RecordFilters`
- No `GroupedColumns`
- No nested `Relations`
- Sorting allowed

### VirtualMode Optimization

Creates records on-demand, discards when out of view:

```csharp
// Enable virtual mode for million-row dataset
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.VirtualMode;

// Only visible records exist in memory
// Garbage collected when scrolled out of view
// PrimaryKey collection initialized only if accessed
```

Criteria (stricter than DisableCounters):
- No `SortedColumns`
- No `RecordFilters`
- No `GroupedColumns`
- No nested `Relations`

**Memory impact**: Can display 1,000,000+ records with <50MB footprint.

**Trade-off**: `PrimaryKeySortedRecords` collection built on-demand if accessed (enumerates all records).

### PassThroughSort Optimization

Uses IBindingList.Sort() instead of engine sorting:

```csharp
// DataView supports native sorting
DataView dataView = new DataView(dataTable);
gridGroupingControl1.DataSource = dataView;
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.PassThroughSort;

// Sorting uses DataView.Sort() (faster for large DataTables)
// Records accessed in virtual mode
```

Criteria:
- Data source implements `IBindingList`
- `IBindingList.SupportsSort` returns `true`
- All VirtualMode criteria met

**Disadvantage**: Loses `CurrentRecord` and `SelectedRecords` on sort. Slower insert/remove with DataView.

**Recommendation**: Use engine's sort unless VirtualMode + large DataTable scenario.

### RecordsAsDisplayElements Optimization

Returns Record instead of RecordRow when querying DisplayElements:

```csharp
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.RecordsAsDisplayElements;

// DisplayElements returns Record, not RecordRow
// Saves RecordPart allocation
// QueryCellStyleInfo receives Record directly
```

Criteria:
- No nested child tables
- No preview rows
- Single-row records (no ColumnSets)

**Code impact**: Check element type carefully:
```csharp
gridGroupingControl1.QueryCellStyleInfo += (s, e) => {
    var element = e.TableCellIdentity.DisplayElement;
    
    // With RecordsAsDisplayElements
    if (element is Record record)
    {
        // Direct access to record
    }
    // Without optimization
    else if (element is RecordRow recordRow)
    {
        var record = recordRow.ParentRecord;
    }
};
```

## Counter Logic

Control which counters engine maintains:

```csharp
using Syncfusion.Grouping;

// Only count visible elements and filtered records (minimal memory)
gridGroupingControl1.CounterLogic = EngineCounters.FilteredRecords;

// Count visible elements, filtered records, and YAmount (medium memory)
gridGroupingControl1.CounterLogic = EngineCounters.YAmount;

// All counters (highest memory, full feature support)
gridGroupingControl1.CounterLogic = EngineCounters.All;
```

**EngineCounters enumeration:**

| Counter | Memory Footprint | Supported Features | Use Case |
|---------|------------------|-------------------|----------|
| **FilteredRecords** | Smallest | Visible elements, filtered record count | Flat tables, no grouping |
| **YAmount** | Medium | + YAmount for scrolling | Tables with scrolling |
| **All** | Largest | + Hidden elements, custom counters | Full-featured grids with groups/filters |

**Work with AllowedOptimizations**: Combine CounterLogic with engine optimizations:

```csharp
// Optimal for flat table with million rows
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.All;
gridGroupingControl1.CounterLogic = EngineCounters.FilteredRecords;
```

## Display Update Optimizations

### Reducing Startup Flicker

```csharp
// Render once offline before form shown
gridGroupingControl1.AllowOptimizeLoadTime = true; // Default: true
```

Ensures grid fully initialized before first display.

### Currency Manager

Bypass Windows Forms CurrencyManager for performance:

```csharp
// Detach from CurrencyManager (no form synchronization)
gridGroupingControl1.BindToCurrencyManager = false;

// Engine relies solely on ListChanged events
// Use when: No other form controls bound to same data source
```

**Trade-off**: Loses automatic synchronization with other controls (TextBox, ComboBox) bound to same DataSource.

### Cache Record Values

Cache old values for custom collections:

```csharp
// Enable value caching for custom collections
gridGroupingControl1.CacheRecordValues = true;

// Access old values
gridGroupingControl1.SourceListListChanged += (s, e) => {
    if (e.ListChangedType == ListChangedType.ItemChanged)
    {
        var record = gridGroupingControl1.Table.Records[e.NewIndex];
        object oldValue = record.GetOldValue(columnIndex);
    }
};
```

Use for:
- Auditing changes
- Conditional formatting based on value changes
- Rollback functionality

### Insert/Remove Behavior

Control screen updates when records inserted/removed:

```csharp
using Syncfusion.Windows.Forms.Grid.Grouping;

// Only repaint changed area (faster)
gridGroupingControl1.InsertRemoveBehavior = 
    GridListChangedInsertRemoveBehavior.ScrollWithImmediateUpdate;

// Repaint entire grid (simpler but slower)
gridGroupingControl1.InsertRemoveBehavior = 
    GridListChangedInsertRemoveBehavior.InvalidateAll; // Default

// Same for EndEdit
gridGroupingControl1.InsertRemoveBehaviorWithEndEdit = 
    GridListChangedInsertRemoveBehavior.ScrollWithImmediateUpdate;
```

**ScrollWithImmediateUpdate**: Uses Windows ScrollWindow API, only repaints affected record. Ideal for large grids.

### Sort Position Changed

Update only changed record when sort position changes:

```csharp
// Only repaint moved record
gridGroupingControl1.SortPositionChangedBehavior = 
    GridListChangedInsertRemoveBehavior.ScrollWithImmediateUpdate;

// Repaint entire grid
gridGroupingControl1.SortPositionChangedBehavior = 
    GridListChangedInsertRemoveBehavior.InvalidateAll; // Default
```

### Invalidation Strategy

Control invalidation when ListChanged events occur:

```csharp
// Determine exact affected area, call InvalidateRange()
gridGroupingControl1.InvalidateAllWhenListChanged = false;

// Simply call Invalidate() (faster in high-frequency scenarios)
gridGroupingControl1.InvalidateAllWhenListChanged = true; // Default
```

**When false**: Engine evaluates counters to find record position (slower for high-frequency updates).
**When true**: Invalidates entire display (faster when many records change rapidly).

### Event Bubbling

Prevent events bubbling to nested tables:

```csharp
// Raise ListChanged events only on Engine (not nested tables)
gridGroupingControl1.RaiseSourceListChangedEventsOnEngineOnly = true; // Recommended

// Bubble events to nested objects (performance overhead)
gridGroupingControl1.RaiseSourceListChangedEventsOnEngineOnly = false; // Default
```

Requires `UseOldListChangedHandler = false`.

### Update Frequency

Control display refresh timing:

```csharp
// Update every 500ms (batch updates together)
gridGroupingControl1.UpdateDisplayFrequency = 500;

// Manual updates only (call grid.Update() explicitly)
gridGroupingControl1.UpdateDisplayFrequency = 0;

// Update immediately after each change
gridGroupingControl1.UpdateDisplayFrequency = 1; // Default
```

Use with high-frequency data feeds (stock tickers, sensor data).

### Old ListChanged Handler

Revert to pre-4.4 behavior if compatibility issues:

```csharp
// Use old handler (not recommended)
gridGroupingControl1.UseOldListChangedHandler = true;

// Use new optimized handler
gridGroupingControl1.UseOldListChangedHandler = false; // Default (recommended)
```

Only set to `true` if encountering compatibility issues with new handler.

## Drawing and Rendering

### UseDefaultsForFasterDrawing

Apply all drawing optimizations at once:

```csharp
// Enable all fast drawing settings
gridGroupingControl1.UseDefaultsForFasterDrawing = true;
```

Affects:
- `TableOptions.DrawTextWithGdiInterop` = true (GDI text)
- `TableOptions.GridLineBorder` = solid border
- `TableOptions.ColumnsMaxLengthStrategy` = efficient width calculation
- `TableOptions.VerticalPixelScroll` = smooth scrolling
- `Appearance.AnyRecordFieldCell.WrapText` = false
- `Appearance.AnyRecordFieldCell.Trimming` = StringTrimming.EllipsisCharacter

### Individual Drawing Settings

```csharp
// GDI text rendering (faster than GDI+)
gridGroupingControl1.TableDescriptor.TableOptions.DrawTextWithGdiInterop = true;

// Solid borders (faster than fancy borders)
gridGroupingControl1.TableDescriptor.TableOptions.GridLineBorder = 
    new GridBorder(GridBorderStyle.Solid);

// Efficient column width calculation
gridGroupingControl1.TableDescriptor.TableOptions.ColumnsMaxLengthStrategy = 
    GridColumnsMaxLengthStrategy.FirstNRecords;

// Pixel-based vertical scrolling
gridGroupingControl1.TableDescriptor.TableOptions.VerticalPixelScroll = true;

// Disable text wrapping
gridGroupingControl1.TableDescriptor.Appearance.AnyRecordFieldCell.WrapText = false;
gridGroupingControl1.TableDescriptor.Appearance.AnyRecordFieldCell.Trimming = 
    StringTrimming.EllipsisCharacter;
```

### Blinking Cells

Control cell highlighting duration after value changes:

```csharp
// Blink cells for 500ms when value changes
gridGroupingControl1.BlinkTime = 500; // Milliseconds

// Disable blinking for specific column
gridGroupingControl1.TableDescriptor.Columns["Price"].AllowBlink = false;
```

## Common Scenarios

### Scenario 1: Million-Row Flat Table (Virtual Mode)

```csharp
// Create large dataset
DataTable dataTable = new DataTable();
dataTable.Columns.Add("ID", typeof(int));
dataTable.Columns.Add("Name", typeof(string));
dataTable.Columns.Add("Value", typeof(decimal));

for (int i = 0; i < 1000000; i++)
{
    dataTable.Rows.Add(i, $"Item {i}", i * 1.5m);
}

// Optimal settings for million rows
gridGroupingControl1.DataSource = dataTable;
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.All;
gridGroupingControl1.CounterLogic = EngineCounters.FilteredRecords;
gridGroupingControl1.UseDefaultsForFasterDrawing = true;
gridGroupingControl1.InvalidateAllWhenListChanged = true;

// Result: ~50MB memory, smooth scrolling, instant load
```

### Scenario 2: High-Frequency Real-Time Updates

```csharp
// Stock ticker with 1000 symbols updating every 100ms
gridGroupingControl1.DataSource = stockDataTable;

// Batch updates, optimize invalidation
gridGroupingControl1.UpdateDisplayFrequency = 250; // Update every 250ms
gridGroupingControl1.InvalidateAllWhenListChanged = true;
gridGroupingControl1.InsertRemoveBehavior = 
    GridListChangedInsertRemoveBehavior.InvalidateAll;
gridGroupingControl1.UseDefaultsForFasterDrawing = true;

// Blink changed cells
gridGroupingControl1.BlinkTime = 300;
gridGroupingControl1.TableDescriptor.Columns["Price"].AllowBlink = true;

// Manual refresh control
Timer timer = new Timer { Interval = 250 };
timer.Tick += (s, e) => gridGroupingControl1.Update();
timer.Start();
```

### Scenario 3: Sorted DataView (PassThroughSort)

```csharp
// Large DataTable with native sorting
DataTable dataTable =new DataTable(); // 500,000 rows
DataView dataView = new DataView(dataTable);

gridGroupingControl1.DataSource = dataView;
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.PassThroughSort;
gridGroupingControl1.CounterLogic = EngineCounters.FilteredRecords;

// User sorts by column -> uses DataView.Sort()
gridGroupingControl1.TableDescriptor.SortedColumns.Add("CustomerName");

// Note: CurrentRecord/SelectedRecords lost on sort
// Re-select after sorting if needed
gridGroupingControl1.SourceListListChanged += (s, e) => {
    if (e.ListChangedType == ListChangedType.Reset)
    {
        // Re-establish selection after sort
        gridGroupingControl1.Table.SelectedRecords.Add(
            gridGroupingControl1.Table.Records[0]);
    }
};
```

### Scenario 4: Memory-Constrained Environment

```csharp
// Minimize memory footprint
gridGroupingControl1.AllowedOptimizations = EngineOptimizations.All;
gridGroupingControl1.CounterLogic = EngineCounters.FilteredRecords;
gridGroupingControl1.BindToCurrencyManager = false;
gridGroupingControl1.CacheRecordValues = false;

// Detach event handlers when grid not visible
this.VisibleChanged += (s, e) => {
    if (!this.Visible)
    {
        // Detach expensive event handlers
        gridGroupingControl1.TableControl.CellDrawn -= ExpensiveDrawHandler;
    }
    else
    {
        // Reattach when visible
        gridGroupingControl1.TableControl.CellDrawn += ExpensiveDrawHandler;
    }
};
```

## Best Practices

### Schema Design

1. **Avoid nested relations** when possible (allows VirtualMode):
   ```csharp
   // Instead of nested tables, use ForeignKeyReference for lookup
   ```

2. **Minimize filters and groups** for flat reporting grids

3. **Use PrimaryKey sparingly**: Accessing `PrimaryKeySortedRecords` in VirtualMode enumerates all records

### Optimization Strategy

1. **Start with AllowedOptimizations.All**:
   ```csharp
   gridGroupingControl1.AllowedOptimizations = EngineOptimizations.All;
   ```
   Let engine decide based on schema.

2. **Set CounterLogic based on features needed**:
   - `FilteredRecords`: Minimal, no grouping
   - `YAmount`: Add scrolling support
   - `All`: Full features

3. **Enable UseDefaultsForFasterDrawing** unless custom styling required

4. **Benchmark with real data**: Test with actual record counts and update frequencies

### High-Frequency Updates

1. **Batch updates together**:
   ```csharp
   dataTable.BeginLoadData();
   // ... add/update multiple rows
   dataTable.EndLoadData();
   ```

2. **Use UpdateDisplayFrequency** for streaming data:
   ```csharp
   gridGroupingControl1.UpdateDisplayFrequency = 500;
   ```

3. **Set InvalidateAllWhenListChanged = true** for rapid updates

4. **Minimize event handlers**: Detach expensive handlers during high-frequency periods

### Memory Management

1. **Dispose grids properly**:
   ```csharp
   protected override void OnFormClosing(FormClosingEventArgs e)
   {
       gridGroupingControl1.Dispose();
       base.OnFormClosing(e);
   }
   ```

2. **Clear data source** when navigating away:
   ```csharp
   gridGroupingControl1.DataSource = null;
   ```

3. **Use virtual mode** for read-only scenarios with >100K records

4. **Profile memory**: Use memory profiler to identify leaks (event handlers, cached objects)

### Troubleshooting Performance Issues

1. **Measure baseline**: Time operations (Load, Sort, Filter, Scroll)
2. **Enable optimizations incrementally**: Test each optimization's impact
3. **Check schema criteria**: Verify optimizations actually applied using debugger
4. **Review event handlers**: Expensive operations in QueryCellStyleInfo, CellDrawn
5. **Monitor ListChanged frequency**: High-frequency updates need batching
