# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Asynchronous Data Processing](#asynchronous-data-processing)
- [Serialization and Deserialization](#serialization-and-deserialization)
- [Freezing Headers](#freezing-headers)
- [Touch Support](#touch-support)
- [Localization](#localization)
- [Performance Optimization](#performance-optimization)

## Overview

Advanced features enhance Pivot Grid capabilities for large datasets, persistence, accessibility, and multi-language support.

## Asynchronous Data Processing

Handle large datasets without blocking the UI thread.

### Async Data Loading

```csharp
using System.Threading.Tasks;

private async Task LoadDataAsync()
{
    // Show loading indicator
    statusLabel.Text = "Loading data...";
    progressBar.Visible = true;
    
    try
    {
        // Load data on background thread
        var data = await Task.Run(() => ProductSales.GetLargeDataSet());
        
        // Update UI on main thread
        pivotGridControl1.ItemSource = data;
        pivotGridControl1.TableControl.Refresh(true);
        
        statusLabel.Text = $"Loaded {data.Count} records";
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error loading data: {ex.Message}", "Error");
    }
    finally
    {
        progressBar.Visible = false;
    }
}
```

### Background Processing

```csharp
private void ProcessLargeDataset()
{
    BackgroundWorker worker = new BackgroundWorker();
    worker.WorkerReportsProgress = true;
    
    worker.DoWork += (s, e) =>
    {
        var data = ProductSales.GetLargeDataSet();
        e.Result = data;
        
        // Report progress
        for (int i = 0; i < 100; i += 10)
        {
            Thread.Sleep(100);  // Simulate work
            worker.ReportProgress(i);
        }
    };
    
    worker.ProgressChanged += (s, e) =>
    {
        progressBar.Value = e.ProgressPercentage;
    };
    
    worker.RunWorkerCompleted += (s, e) =>
    {
        if (e.Error == null)
        {
            pivotGridControl1.ItemSource = e.Result;
            MessageBox.Show("Data loaded successfully!");
        }
    };
    
    worker.RunWorkerAsync();
}
```

## Serialization and Deserialization

Save and restore pivot grid configuration.

### Serialize Pivot State

```csharp
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

private void SavePivotConfiguration(string fileName)
{
    try
    {
        PivotConfiguration config = new PivotConfiguration
        {
            Rows = pivotGridControl1.PivotRows.ToList(),
            Columns = pivotGridControl1.PivotColumns.ToList(),
            Calculations = pivotGridControl1.PivotCalculations.ToList(),
            Filters = pivotGridControl1.Filters.ToList()
        };
        
        using (FileStream fs = new FileStream(fileName, FileMode.Create))
        {
            BinaryFormatter formatter = new BinaryFormatter();
            formatter.Serialize(fs, config);
        }
        
        MessageBox.Show("Configuration saved successfully!");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Save failed: {ex.Message}", "Error");
    }
}

[Serializable]
public class PivotConfiguration
{
    public List<PivotItem> Rows { get; set; }
    public List<PivotItem> Columns { get; set; }
    public List<PivotComputationInfo> Calculations { get; set; }
    public List<FilterExpression> Filters { get; set; }
}
```

### Deserialize Pivot State

```csharp
private void LoadPivotConfiguration(string fileName)
{
    try
    {
        PivotConfiguration config;
        
        using (FileStream fs = new FileStream(fileName, FileMode.Open))
        {
            BinaryFormatter formatter = new BinaryFormatter();
            config = (PivotConfiguration)formatter.Deserialize(fs);
        }
        
        // Clear existing configuration
        pivotGridControl1.PivotRows.Clear();
        pivotGridControl1.PivotColumns.Clear();
        pivotGridControl1.PivotCalculations.Clear();
        pivotGridControl1.Filters.Clear();
        
        // Apply loaded configuration
        foreach (var row in config.Rows)
            pivotGridControl1.PivotRows.Add(row);
        foreach (var col in config.Columns)
            pivotGridControl1.PivotColumns.Add(col);
        foreach (var calc in config.Calculations)
            pivotGridControl1.PivotCalculations.Add(calc);
        foreach (var filter in config.Filters)
            pivotGridControl1.Filters.Add(filter);
        
        // Refresh grid
        pivotGridControl1.TableControl.Refresh(true);
        
        MessageBox.Show("Configuration loaded successfully!");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Load failed: {ex.Message}", "Error");
    }
}
```

### JSON Serialization (Alternative)

```csharp
using System.Text.Json;

private void SaveAsJson(string fileName)
{
    var config = new
    {
        Rows = pivotGridControl1.PivotRows.Select(r => new { r.FieldMappingName, r.TotalHeader }),
        Columns = pivotGridControl1.PivotColumns.Select(c => new { c.FieldMappingName, c.TotalHeader }),
        Calculations = pivotGridControl1.PivotCalculations.Select(calc => new 
        { 
            calc.FieldName, 
            calc.Format, 
            SummaryType = calc.SummaryType.ToString() 
        })
    };
    
    string json = JsonSerializer.Serialize(config, new JsonSerializerOptions { WriteIndented = true });
    File.WriteAllText(fileName, json);
}
```

## Freezing Headers

Keep headers visible while scrolling.

### Freeze Row Headers

```csharp
// Freeze first N columns (row headers)
pivotGridControl1.TableControl.FrozenColumns = 2;

// Allow scrolling through data while headers remain visible
```

### Freeze Column Headers

```csharp
// Freeze first N rows (column headers)
pivotGridControl1.TableControl.FrozenRows = 2;
```

### Freeze Both

```csharp
// Freeze row and column headers
pivotGridControl1.TableControl.FrozenRows = 2;
pivotGridControl1.TableControl.FrozenColumns = 1;
```

## Touch Support

Enable touch-friendly interactions for tablets.

### Enable Touch Mode

```csharp
// Enable touch support
pivotGridControl1.TableControl.EnableTouch = true;

// Adjust touch sensitivity
pivotGridControl1.TableControl.TouchMode = TouchMode.Enabled;
```

### Touch Gestures

```csharp
// Configure touch gestures
pivotGridControl1.TableControl.TouchGestureManager.EnableGestures = true;

// Swipe gestures
pivotGridControl1.TableControl.GestureRecognized += (s, e) =>
{
    switch (e.GestureType)
    {
        case GestureType.Swipe:
            Console.WriteLine("Swipe detected");
            break;
        case GestureType.Pinch:
            Console.WriteLine("Pinch/zoom detected");
            break;
    }
};
```

## Localization

Support multiple languages and cultures.

### Set Culture

```csharp
using System.Globalization;
using System.Threading;

// Set application culture
CultureInfo culture = new CultureInfo("de-DE");  // German
Thread.CurrentThread.CurrentCulture = culture;
Thread.CurrentThread.CurrentUICulture = culture;

// Apply to pivot grid
pivotGridControl1.Culture = culture;
```

### Localized Resource Files

Create resource files for different languages:
- `PivotGridResources.resx` (default/English)
- `PivotGridResources.de.resx` (German)
- `PivotGridResources.fr.resx` (French)

### Custom Localization

```csharp
// Customize UI strings
pivotGridControl1.TableControl.QueryCellInfo += (s, e) =>
{
    // Translate header text
    if (e.RowIndex == 0 && e.ColIndex > 0)
    {
        string headerText = e.Style.CellValue?.ToString();
        e.Style.CellValue = GetLocalizedString(headerText);
    }
};

private string GetLocalizedString(string key)
{
    // Implement translation logic
    var translations = new Dictionary<string, Dictionary<string, string>>
    {
        ["Product"] = new Dictionary<string, string>
        {
            ["en"] = "Product",
            ["de"] = "Produkt",
            ["fr"] = "Produit"
        },
        ["Total"] = new Dictionary<string, string>
        {
            ["en"] = "Total",
            ["de"] = "Gesamt",
            ["fr"] = "Total"
        }
    };
    
    string currentLanguage = Thread.CurrentThread.CurrentCulture.TwoLetterISOLanguageName;
    return translations.ContainsKey(key) && translations[key].ContainsKey(currentLanguage)
        ? translations[key][currentLanguage]
        : key;
}
```

## Performance Optimization

### Virtual Mode

```csharp
// Enable virtualization for large datasets
pivotGridControl1.TableControl.VirtualMode = true;
```

### Lazy Loading

```csharp
// Load data incrementally
private void LoadDataIncrementally()
{
    const int batchSize = 1000;
    var allData = new List<ProductSales>();
    
    for (int i = 0; i < 10000; i += batchSize)
    {
        var batch = LoadDataBatch(i, batchSize);
        allData.AddRange(batch);
        
        // Update grid periodically
        if ((i / batchSize) % 5 == 0)
        {
            pivotGridControl1.ItemSource = allData;
            pivotGridControl1.TableControl.Refresh(false);
            Application.DoEvents();  // Allow UI to update
        }
    }
    
    pivotGridControl1.TableControl.Refresh(true);
}
```

### Caching

```csharp
// Cache pivot engine results
private Dictionary<string, object> pivotCache = new Dictionary<string, object>();

private void UseCaching()
{
    string cacheKey = GetCurrentPivotConfiguration();
    
    if (pivotCache.ContainsKey(cacheKey))
    {
        // Use cached result
        pivotGridControl1.InternalEngine = (PivotEngine)pivotCache[cacheKey];
    }
    else
    {
        // Calculate and cache
        pivotGridControl1.TableControl.Refresh(true);
        pivotCache[cacheKey] = pivotGridControl1.InternalEngine.Clone();
    }
}
```

## Complete Example

```csharp
public class AdvancedPivotForm : Form
{
    private PivotGridControl pivotGridControl1;
    
    public AdvancedPivotForm()
    {
        InitializeComponent();
        SetupAdvancedFeatures();
    }
    
    private void SetupAdvancedFeatures()
    {
        // Freeze headers
        pivotGridControl1.TableControl.FrozenRows = 1;
        pivotGridControl1.TableControl.FrozenColumns = 1;
        
        // Enable touch
        pivotGridControl1.TableControl.EnableTouch = true;
        
        // Set localization
        pivotGridControl1.Culture = CultureInfo.CurrentCulture;
        
        // Optimize performance
        pivotGridControl1.TableControl.VirtualMode = true;
        
        // Add save/load buttons
        AddConfigurationButtons();
    }
    
    private void AddConfigurationButtons()
    {
        Button btnSave = new Button { Text = "Save Config" };
        btnSave.Click += (s, e) =>
        {
            SaveFileDialog dlg = new SaveFileDialog();
            dlg.Filter = "Config Files (*.cfg)|*.cfg";
            if (dlg.ShowDialog() == DialogResult.OK)
                SavePivotConfiguration(dlg.FileName);
        };
        
        Button btnLoad = new Button { Text = "Load Config" };
        btnLoad.Click += (s, e) =>
        {
            OpenFileDialog dlg = new OpenFileDialog();
            dlg.Filter = "Config Files (*.cfg)|*.cfg";
            if (dlg.ShowDialog() == DialogResult.OK)
                LoadPivotConfiguration(dlg.FileName);
        };
        
        this.Controls.AddRange(new Control[] { btnSave, btnLoad });
    }
}
```

## Best Practices

1. **Async Operations** - Use async/await for data loading to keep UI responsive
2. **Persist Configuration** - Save user's pivot layouts for quick access
3. **Optimize Large Datasets** - Use virtualization and lazy loading
4. **Support Touch** - Enable for tablet/touch-screen devices
5. **Localize** - Support multiple languages for global applications
6. **Cache Results** - Cache frequently-used pivot configurations
