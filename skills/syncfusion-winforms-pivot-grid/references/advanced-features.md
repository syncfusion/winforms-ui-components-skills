# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Asynchronous Data Processing](#asynchronous-data-processing)
- [Serialization and Deserialization](#serialization-and-deserialization)
- [Freezing Headers](#freezing-headers)
- [Touch Support](#touch-support)

## Overview

Advanced features enhance Pivot Grid capabilities for large datasets, persistence, touch-enabled interfaces, and improved user experience through asynchronous operations and header freezing.

## Asynchronous Data Processing

Handle large datasets without blocking the UI thread using asynchronous loading capabilities.

### Enable Asynchronous Loading

Enable asynchronous loading to perform long-running operations on a background thread.  

```csharp
// Enable asynchronous loading support
this.pivotGridControl1.EnableAsyncLoading = true;

// Check if pivot grid is in async mode
bool isAsync = this.pivotGridControl1.InAsyncMode;
```

**When to use:** Large datasets that cause UI responsiveness issues during filtering, sorting, or pivot operations.

### Customize Loading Icon

Change the default loading animation displayed during asynchronous operations.

```csharp
// Set custom loading icon
this.pivotGridControl1.BusyAnimationIcon = Image.FromFile(@"Loading.gif");
```

### Disable Loading Icon

Hide the loading icon during asynchronous operations.

```csharp
// Disable loading icon
this.pivotGridControl1.BusyAnimationIcon = null;
```

### Handle Asynchronous Events

Monitor async operation lifecycle with events.

```csharp
public Form1()
{
    InitializeComponent();
    
    // Subscribe to async events
    this.pivotGridControl1.AsyncLoadStarted += PivotGridControl1_AsyncLoadStarted;
    this.pivotGridControl1.AsyncLoadCompleted += PivotGridControl1_AsyncLoadCompleted;
}

private void PivotGridControl1_AsyncLoadStarted(object sender, CancelEventArgs e)
{
    // Required code can be added.
    MessageBox.Show("Asynchronous mode has been started.");
}

private void PivotGridControl1_AsyncLoadCompleted(object sender, AsyncCompletedEventArgs e)
{
    // Required code can be added.
    MessageBox.Show("Asynchronous mode has been completed.");
}
```

**Use cases:**
- Filtering large datasets
- Sorting operations on thousands of records
- Drag-and-drop operations in pivot table field list
- Dynamic pivot reorganization

---

## Serialization and Deserialization

Save and restore pivot grid configuration to XML format.

### Serialize Using Save File Dialog

Open a file dialog to save pivot grid settings.

```csharp
// Serialize with save dialog
this.pivotGridControl1.Serialize();
```

### Serialize to Stream

Save pivot grid configuration to a stream.

```csharp
using (FileStream fileStream = File.Create("PivotGrid.xml"))
{
    this.pivotGridControl1.Serialize(fileStream);
}
```

### Serialize to Specific File

Save directly to a file path.

```csharp
// Serialize to specific path
this.pivotGridControl1.Serialize(@"D:\PivotGrid.xml");
```

### Serialize to XML String

Export configuration as XML string.

```csharp
// Get XML string representation
string xmlString = this.pivotGridControl1.SerializeToXml();
```

### Customize Serialization Options

Control which elements are serialized using `SerializationOptions`.

```csharp
using (var file = File.Create("PivotGrid.xml"))
{
    SerializationOptions options = new SerializationOptions();
    
    // Disable specific serialization components
    options.SerializeGrouping = false;          // Grouping bar items
    options.SerializeSorting = false;           // Sorted items
    options.SerializeFiltering = false;         // Filtered items
    options.SerializePivotRows = false;         // Pivot row items
    options.SerializePivotColumns = false;      // Pivot column items
    options.SerializePivotCalculations = false; // Calculation items
    options.SerializeConditionalFormats = false;// Conditional formats
    options.SerializeExpandCollapseState = false; // Expander states
    
    this.pivotGridControl1.Serialize(file, options);
}
```

### Deserialize Using Open File Dialog

Load pivot grid settings via file dialog.

```csharp
// Deserialize with open dialog
this.pivotGridControl1.Deserialize();
```

### Deserialize from Stream

Load configuration from a stream.

```csharp
using (FileStream fileStream = File.OpenRead("PivotGrid.xml"))
{
    this.pivotGridControl1.Deserialize(fileStream);
}
```

### Deserialize from Specific File

Load directly from a file path.

```csharp
// Deserialize from specific path
this.pivotGridControl1.Deserialize(@"D:\PivotGrid.xml");
```

### Deserialize from XML String

Load configuration from XML string.

```csharp
// Restore from XML string
this.pivotGridControl1.DeserializeFromXml(xmlString);
```

### Customize Deserialization Options

Control which elements are deserialized using `DeserializationOptions`.

```csharp
using (FileStream fileStream = File.OpenRead("PivotGrid.xml"))
{
    DeserializationOptions options = new DeserializationOptions();
    
    // Disable specific deserialization components
    options.DeserializeGrouping = false;          // Grouping bar items
    options.DeserializeSorting = false;           // Sorting operations
    options.DeserializeFiltering = false;         // Filtering operations
    options.DeserailizePivotRows = false;         // Pivot row items
    options.DeserializePivotColumns = false;      // Pivot column items
    options.DeserializePivotCalculations = false; // Calculation items
    options.DeserializeConditionalFormats = false;// Conditional formats
    options.DeserializeExpandCollapseState = false; // Expander states
    
    this.pivotGridControl1.Deserialize(fileStream, options);
}
```

**Common scenarios:**
- Save user's preferred pivot layout
- Export/import analysis configurations
- Restore pivot state after application restart
- Share pivot configurations between users

---

## Freezing Headers

Keep row and column headers visible during scrolling.

### Enable Frozen Headers

Freeze headers to keep them visible when scrolling through large datasets.

```csharp
// Freeze row and column headers
this.pivotGridControl1.TableControl.FreezeHeaders = true;
```

**Benefits:**
- Headers remain visible during scrolling
- Easier navigation through large pivot tables
- Better user experience with extensive data
- Maintains context while exploring value cells

**When to use:** Large pivot tables with many rows and columns where users need to scroll frequently.

**Note:** By default, headers are not frozen. Enable this feature for improved usability with large datasets.

---

## Touch Support

Enable touch-friendly interactions for tablets and touch-screen devices.

### Enable Touch Mode

Activate touch support for the pivot grid.

```csharp
// Enable touch mode
this.pivotGridControl1.EnableTouchMode = true;
```

### Enable Excel-Like Touch Selection

Configure touch selection with indicators for multi-cell selection.

```csharp
// Enable touch mode and Excel-like selection
this.pivotGridControl1.EnableTouchMode = true;
this.pivotGridControl1.TableModel.Options.AllowSelection = Syncfusion.Windows.Forms.Grid.GridSelectionFlags.Any;
this.pivotGridControl1.TableModel.Options.ExcelLikeSelectionFrame = true;
this.pivotGridControl1.TableModel.Options.ExcelLikeCurrentCell = true;
```

**Touch selection features:**
- Touch indicator (bubble) for cell selection
- Drag to extend selection range
- Multi-cell selection support
- Excel-like selection frame

### Disable Touch Indicator

Hide the touch indicator bubble during selection.

```csharp
// Hide touch indicator
this.pivotGridControl1.TableControl.ShowTouchIndicator = false;
```

**Touch gestures supported:**
- **Swipe** - Scroll horizontally and vertically
- **Pan** - Navigate through large pivot tables
- **Tap** - Expand/collapse header cells
- **Drag-and-drop** - Move pivot items between grouping bar and field list

**Use cases:**
- Tablet applications
- Touch-screen kiosks
- Windows Surface devices
- Touch-enabled laptops

**Note:** Excel 2003 selection frame is not compatible with touch selection. Use Excel-like selection frame instead.

---

## Complete Example

Comprehensive example implementing all advanced features.

```csharp
public class AdvancedPivotForm : Form
{
    private PivotGridControl pivotGridControl1;
    private ToolStrip toolStrip1;
    private StatusStrip statusStrip1;
    private ToolStripStatusLabel statusLabel;
    
    public AdvancedPivotForm()
    {
        InitializeComponent();
        SetupAdvancedFeatures();
        AttachEventHandlers();
    }
    
    private void SetupAdvancedFeatures()
    {
        // Enable async loading
        pivotGridControl1.EnableAsyncLoading = true;
        
        // Freeze headers for better navigation
        pivotGridControl1.TableControl.FreezeHeaders = true;
        
        // Enable touch support for tablets
        pivotGridControl1.EnableTouchMode = true;
        
        // Configure touch selection
        pivotGridControl1.TableModel.Options.AllowSelection = 
            Syncfusion.Windows.Forms.Grid.GridSelectionFlags.Any;
        pivotGridControl1.TableModel.Options.ExcelLikeSelectionFrame = true;
        pivotGridControl1.TableModel.Options.ExcelLikeCurrentCell = true;
        
        // Add toolbar buttons
        AddToolbarButtons();
    }
    
    private void AttachEventHandlers()
    {
        // Async event handlers
        pivotGridControl1.AsyncLoadStarted += (s, e) =>
        {
            statusLabel.Text = "Loading data asynchronously...";
        };
        
        pivotGridControl1.AsyncLoadCompleted += (s, e) =>
        {
            if (e.Error == null)
                statusLabel.Text = "Data loaded successfully";
            else
                statusLabel.Text = $"Error: {e.Error.Message}";
        };
    }
    
    private void AddToolbarButtons()
    {
        // Save configuration button
        ToolStripButton btnSave = new ToolStripButton("Save Config");
        btnSave.Click += (s, e) =>
        {
            SaveFileDialog dlg = new SaveFileDialog();
            dlg.Filter = "XML Files (*.xml)|*.xml";
            if (dlg.ShowDialog() == DialogResult.OK)
            {
                pivotGridControl1.Serialize(dlg.FileName);
                MessageBox.Show("Configuration saved successfully!");
            }
        };
        
        // Load configuration button
        ToolStripButton btnLoad = new ToolStripButton("Load Config");
        btnLoad.Click += (s, e) =>
        {
            OpenFileDialog dlg = new OpenFileDialog();
            dlg.Filter = "XML Files (*.xml)|*.xml";
            if (dlg.ShowDialog() == DialogResult.OK)
            {
                pivotGridControl1.Deserialize(dlg.FileName);
                MessageBox.Show("Configuration loaded successfully!");
            }
        };
        
        toolStrip1.Items.AddRange(new ToolStripItem[] { btnSave, btnLoad });
    }
}
```

## Best Practices

1. **Asynchronous Loading** - Enable for datasets with >10,000 records to maintain UI responsiveness
2. **Serialization** - Save user configurations to improve workflow efficiency
3. **Frozen Headers** - Always enable for pivot tables with extensive rows/columns
4. **Touch Support** - Enable for applications targeting tablets and touch devices
5. **Custom Loading Icons** - Use branded animations for professional appearance
6. **Event Monitoring** - Implement async event handlers for user feedback
7. **Selective Serialization** - Use SerializationOptions to save only necessary components
