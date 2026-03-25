# Navigation and Scrolling

Guide for implementing navigation and scrolling in GridGroupingControl to enable users to browse through records efficiently.

## Overview

GridGroupingControl provides comprehensive navigation and scrolling capabilities through:
- **RecordNavigationBar**: Built-in control with first/previous/next/last/add buttons and record counter
- **Programmatic navigation**: Methods to navigate records, scroll to specific cells
- **Scrolling modes**: Built-in scrollbars with multiple visual styles (Metro, Office2007, Office2010)
- **Keyboard shortcuts**: Arrow keys, Page Up/Down, Home/End for navigation

Use navigation features when users need to browse large datasets quickly, and scrolling when displaying tables that exceed viewport dimensions.

## Navigation Bar

### Enabling Navigation Bar

Show the built-in navigation control with record browsing buttons:

```csharp
// Show navigation bar with tooltips
gridGroupingControl1.ShowNavigationBar = true;
gridGroupingControl1.ShowNavigationBarToolTips = true;
```

Navigation bar components:
- **First/Last buttons**: Jump to first/last record
- **Previous/Next buttons**: Move one record at a time
- **Add New button**: Insert new record
- **Label**: Displays "Record X of Y"

### Setting Current Record

Navigate to specific record programmatically:

```csharp
// Navigate to record 5
gridGroupingControl1.RecordNavigationBar.SetCurrentRecord(5, true);
```

### Customizing Appearance

#### Colors and Style

```csharp
// Customize navigation bar colors
gridGroupingControl1.RecordNavigationBar.BackColor = Color.Green;
gridGroupingControl1.RecordNavigationBar.ForeColor = Color.White;
```

#### Arrow Button Mode

Control which navigation buttons display:

```csharp
using Syncfusion.Windows.Forms;

// Show only next/previous buttons
gridGroupingControl1.RecordNavigationBar.DisplayArrowButtons = DisplayArrowButtons.Single;

// Show all buttons (first/previous/next/last)
gridGroupingControl1.RecordNavigationBar.DisplayArrowButtons = DisplayArrowButtons.Both;
```

#### Custom Label Text

Change the default "Record" prefix:

```csharp
// Change label from "Record" to "Row"
gridGroupingControl1.RecordNavigationBar.Label = "Row";
// Result: "Row 1 of 100" instead of "Record 1 of 100"
```

#### Width and Layout

```csharp
// Set navigation bar width
gridGroupingControl1.RecordNavigationControl.NavigationBarWidth = 400;

// Set background color
gridGroupingControl1.RecordNavigationControl.NavigationBarBackColor = Color.LightCoral;
gridGroupingControl1.RecordNavigationControl.ForeColor = Color.White;
```

### RTL Support

Enable right-to-left layout for navigation bar:

```csharp
// Enable RTL mode
gridGroupingControl1.RecordNavigationBar.RightToLeft = RightToLeft.Yes;
```

### Visual Styles

Apply Office-style themes to navigation bar:

```csharp
// Apply Office 2010 Blue theme
gridGroupingControl1.GridVisualStyles = GridVisualStyles.Office2010Blue;
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
```

### Programmatic Navigation

Navigate records using methods:

```csharp
// Move to first record
gridGroupingControl1.RecordNavigationBar.MoveFirst();

// Move to last record
gridGroupingControl1.RecordNavigationBar.MoveLast();

// Move to next record
gridGroupingControl1.RecordNavigationBar.MoveNext();

// Move to previous record
gridGroupingControl1.RecordNavigationBar.MovePrevious();
```

### Arrow Button Clicked Event

Customize behavior when navigation buttons are clicked:

```csharp
gridGroupingControl1.RecordNavigationBar.ArrowButtonClicked += RecordNavigationBar_ArrowButtonClicked;

void RecordNavigationBar_ArrowButtonClicked(object sender, ArrowButtonEventArgs e)
{
    // Customize arrow type (First, Previous, Next, Last, AddNew)
    if (someCondition)
        e.Arrow = ArrowType.AddNew;
}
```

## Scrolling

### Enabling/Disabling Scrollbars

Control scrollbar visibility:

```csharp
// Enable horizontal scrollbar
gridGroupingControl1.TableControl.HScroll = true;
gridGroupingControl1.TableControl.HScrollBehavior = GridScrollbarMode.Enabled;

// Enable vertical scrollbar
gridGroupingControl1.TableControl.VScroll = true;
gridGroupingControl1.TableControl.VScrollBehavior = GridScrollbarMode.Enabled;

// Disable horizontal scrollbar
gridGroupingControl1.TableControl.HScroll = false;
gridGroupingControl1.TableControl.HScrollBehavior = GridScrollbarMode.Disabled;
```

### Programmatic Scrolling

Scroll to specific cell position:

```csharp
// Scroll to cell at row 7, column 7
gridGroupingControl1.TableControl.ScrollCellInView(7, 7);

// Scroll to range of cells
GridRangeInfo range = GridRangeInfo.Cells(5, 3, 10, 8);
gridGroupingControl1.TableControl.ScrollCellInView(range);
```

### Scrollbar Appearance

#### Office-Style Scrollbars

Apply modern scrollbar styles:

```csharp
// Metro theme
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;

// Office 2007 theme
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Office2007;

// Office 2010 theme
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
```

#### Color Schemes

Apply color schemes to Office-style scrollbars:

```csharp
// Office 2010 with Black scheme
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
gridGroupingControl1.Office2010ScrollBarsColorScheme = Office2010ColorScheme.Black;

// Office 2007 with Silver scheme
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Office2007;
gridGroupingControl1.Office2007ScrollBarsColorScheme = Office2007ColorScheme.Silver;
```

Available Office 2010 schemes: Black, Blue, Silver, Managed
Available Office 2007 schemes: Blue, Black, Silver, Managed

#### Metro Scrollbar Colors

Customize Metro scrollbar background:

```csharp
// Set Metro scrollbar background color
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Metro;
gridGroupingControl1.TableControl.MetroColorTable.ScrollerBackground = Color.CadetBlue;
```

### IntelliMouse Scrolling

Enable automatic scrolling with mouse wheel:

```csharp
// Enable IntelliMouse scrolling
gridGroupingControl1.EnableIntelliMouse = true;
```

### Shared Scrollbars

Synchronize scrolling across multiple grids:

```csharp
// Grid 1: Share scrollbars
gridGroupingControl1.TableControl.HScrollBehavior = GridScrollbarMode.Shared;
gridGroupingControl1.TableControl.VScrollBehavior = GridScrollbarMode.Shared;
gridGroupingControl1.TableControl.UseSharedScrollBars = true;

// Link to external scrollbar controls
gridGroupingControl1.TableControl.HScrollBar.InnerScrollBar = hScrollBar1;
gridGroupingControl1.TableControl.VScrollBar.InnerScrollBar = vScrollBar1;

// Grid 2 can share the same scrollbars
gridGroupingControl2.TableControl.HScrollBar.InnerScrollBar = hScrollBar1;
gridGroupingControl2.TableControl.VScrollBar.InnerScrollBar = vScrollBar1;
```

### Scrolling Events

Handle scrollbar value changes:

```csharp
gridGroupingControl1.TableControl.HorizontalScroll += TableControl_HorizontalScroll;
gridGroupingControl1.TableControl.VerticalScroll += TableControl_VerticalScroll;

void TableControl_HorizontalScroll(object sender, ScrollEventArgs e)
{
    Console.WriteLine($"Horizontal scroll: {e.NewValue}");
}

void TableControl_VerticalScroll(object sender, ScrollEventArgs e)
{
    Console.WriteLine($"Vertical scroll: {e.NewValue}");
}
```

Use cases:
- Synchronize scroll position across multiple grids
- Display scroll position indicator
- Load data on-demand based on scroll position
- Track user navigation patterns

## Common Scenarios

### Scenario 1: Full-Featured Navigation with Custom Styling

```csharp
// Enable navigation bar with custom appearance
gridGroupingControl1.ShowNavigationBar = true;
gridGroupingControl1.ShowNavigationBarToolTips = true;

// Custom colors
gridGroupingControl1.RecordNavigationBar.BackColor = Color.FromArgb(0, 114, 198);
gridGroupingControl1.RecordNavigationBar.ForeColor = Color.White;

// Custom label
gridGroupingControl1.RecordNavigationBar.Label = "Employee";

// Office 2010 style
gridGroupingControl1.GridVisualStyles = GridVisualStyles.Office2010Blue;
gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
gridGroupingControl1.Office2010ScrollBarsColorScheme = Office2010ColorScheme.Blue;

// Result: "Employee 1 of 500" with blue Office theme
```

### Scenario 2: Synchronized Scrolling for Master-Detail Grids

```csharp
// Master grid
HScrollBar sharedHScroll = new HScrollBar();
VScrollBar sharedVScroll = new VScrollBar();

masterGrid.TableControl.HScrollBehavior = GridScrollbarMode.Shared;
masterGrid.TableControl.VScrollBehavior = GridScrollbarMode.Shared;
masterGrid.TableControl.UseSharedScrollBars = true;
masterGrid.TableControl.HScrollBar.InnerScrollBar = sharedHScroll;
masterGrid.TableControl.VScrollBar.InnerScrollBar = sharedVScroll;

// Detail grid shares same scrollbars
detailGrid.TableControl.HScrollBehavior = GridScrollbarMode.Shared;
detailGrid.TableControl.VScrollBehavior = GridScrollbarMode.Shared;
detailGrid.TableControl.UseSharedScrollBars = true;
detailGrid.TableControl.HScrollBar.InnerScrollBar = sharedHScroll;
detailGrid.TableControl.VScrollBar.InnerScrollBar = sharedVScroll;

// Both grids scroll together
```

### Scenario 3: Keyboard-Activated Navigation

```csharp
// Navigate on Ctrl+G shortcut
gridGroupingControl1.TableControl.KeyDown += TableControl_KeyDown;

void TableControl_KeyDown(object sender, KeyEventArgs e)
{
    if (e.Control && e.KeyCode == Keys.G)
    {
        // Show "Go To" dialog
        using (var dialog = new Form())
        {
            var txtRecord = new TextBox { Location = new Point(10, 10) };
            var btnGo = new Button { Text = "Go", Location = new Point(10, 40) };
            
            btnGo.Click += (s, args) => {
                if (int.TryParse(txtRecord.Text, out int recordNum))
                {
                    gridGroupingControl1.RecordNavigationBar.SetCurrentRecord(recordNum, true);
                    dialog.Close();
                }
            };
            
            dialog.Controls.AddRange(new Control[] { txtRecord, btnGo });
            dialog.ShowDialog();
        }
        
        e.Handled = true;
    }
}
```

### Scenario 4: Scroll-Based Data Loading

```csharp
// Load more data when scrolling near bottom
gridGroupingControl1.TableControl.VerticalScroll += TableControl_VerticalScroll;

void TableControl_VerticalScroll(object sender, ScrollEventArgs e)
{
    var scrollBar = gridGroupingControl1.TableControl.VScrollBar;
    int threshold = scrollBar.Maximum - (scrollBar.LargeChange * 2);
    
    if (e.NewValue >= threshold && !isLoading)
    {
        // Near bottom, load more records
        isLoading = true;
        LoadMoreRecords();
        isLoading = false;
    }
}

void LoadMoreRecords()
{
    // Fetch additional records from database
    var newRecords = FetchNextPage();
    
    // Add to data source
    foreach (var record in newRecords)
        dataTable.Rows.Add(record);
}
```

## Best Practices

### Navigation Bar

1. **Always enable tooltips** for better UX:
   ```csharp
   gridGroupingControl1.ShowNavigationBarToolTips = true;
   ```

2. **Use meaningful labels** based on data type:
   - "Employee" for employee grid
   - "Order" for orders grid
   - "Product" for product catalog

3. **Match visual style** with application theme:
   ```csharp
   // Consistent with form theme
   gridGroupingControl1.GridVisualStyles = GridVisualStyles.Office2010Blue;
   gridGroupingControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
   ```

4. **Keyboard shortcuts**: Provide alternative navigation methods (Ctrl+Home, Ctrl+End, Page Up/Down)

### Scrolling

1. **Enable IntelliMouse** for large datasets:
   ```csharp
   gridGroupingControl1.EnableIntelliMouse = true;
   ```

2. **Match scrollbar style** with control theme (Metro with modern UI, Office 2010 with ribbon interfaces)

3. **Use programmatic scrolling** to guide users:
   - Scroll to validation errors
   - Scroll to search results
   - Scroll to newly added records

4. **Handle scroll events** for performance optimization:
   - Load data on-demand (virtualization)
   - Cancel expensive operations while scrolling
   - Update status bar with current position

5. **Shared scrollbars**: Use only when synchronization is essential (master-detail views, side-by-side comparison)

### Performance

1. **Large datasets**: Combine navigation bar with virtual mode for optimal memory usage
2. **Smooth scrolling**: Use `VerticalPixelScroll` for pixel-based scrolling instead of line-based
3. **Scroll events**: Avoid expensive operations in scroll event handlers (debounce if needed)

### Accessibility

1. **Keyboard navigation**: Ensure all navigation features work via keyboard (Tab, Arrow keys, Page Up/Down)
2. **Screen readers**: Navigation bar announces current position automatically
3. **High contrast**: Test navigation bar colors in high-contrast modes
