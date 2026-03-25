# Data Binding

## Overview

All three gauge types (RadialGauge, LinearGauge, DigitalGauge) support **data binding for real-time value updates**. Data binding provides automatic synchronization between data sources and gauge displays without manual Value property updates.

**When to use data binding:**
- Real-time sensor data display
- Database-driven dashboards
- High-frequency data updates
- Multi-gauge synchronization from single source
- Monitoring systems

**When NOT to use:**
- Single static value display
- Manual user input
- Low-frequency updates (use direct Value property)

## Data Binding Properties

All gauge types share common data binding properties:

```csharp
// Data source (DataTable, DataSet, List, BindingSource, etc.)
gauge.DataSource = dataSource;

// Column/property name for value
gauge.DisplayMember = "SensorValue";

// Row/item index to display (0-based)
gauge.DisplayRecordIndex = 0;
```

## RadialGauge Data Binding

### Basic Setup

```csharp
using System.Data;
using Syncfusion.Windows.Forms.Gauge;

// Create data source
DataTable sensorData = new DataTable();
sensorData.Columns.Add("Temperature", typeof(float));
sensorData.Rows.Add(72.5f);

// Create and configure gauge
RadialGauge tempGauge = new RadialGauge();
tempGauge.MinimumValue = 0;
tempGauge.MaximumValue = 150;
tempGauge.GaugeLabel = "Temperature (°F)";

// Bind to data source
tempGauge.DataSource = sensorData;
tempGauge.DisplayMember = "Temperature";
tempGauge.DisplayRecordIndex = 0;

this.Controls.Add(tempGauge);

// Update data source (gauge updates automatically)
sensorData.Rows[0]["Temperature"] = 75.2f;
```

### Multiple Gauges from Single Source

```csharp
// Create data source with multiple columns
DataTable dashboardData = new DataTable();
dashboardData.Columns.Add("Speed", typeof(float));
dashboardData.Columns.Add("RPM", typeof(float));
dashboardData.Columns.Add("FuelLevel", typeof(float));
dashboardData.Rows.Add(65.0f, 3500.0f, 75.0f);

// Speed gauge
RadialGauge speedGauge = new RadialGauge();
speedGauge.DataSource = dashboardData;
speedGauge.DisplayMember = "Speed";
speedGauge.DisplayRecordIndex = 0;
speedGauge.MinimumValue = 0;
speedGauge.MaximumValue = 200;

// RPM gauge
RadialGauge rpmGauge = new RadialGauge();
rpmGauge.DataSource = dashboardData;
rpmGauge.DisplayMember = "RPM";
rpmGauge.DisplayRecordIndex = 0;
rpmGauge.MinimumValue = 0;
rpmGauge.MaximumValue = 8000;

// Fuel gauge
RadialGauge fuelGauge = new RadialGauge();
fuelGauge.DataSource = dashboardData;
fuelGauge.DisplayMember = "FuelLevel";
fuelGauge.DisplayRecordIndex = 0;
fuelGauge.MinimumValue = 0;
fuelGauge.MaximumValue = 100;

// Update all gauges by updating data source
dashboardData.Rows[0]["Speed"] = 72.0f;
dashboardData.Rows[0]["RPM"] = 4200.0f;
dashboardData.Rows[0]["FuelLevel"] = 68.0f;
// All three gauges update automatically
```

### High-Frequency Updates

```csharp
// Setup for real-time monitoring
DataTable realtimeData = new DataTable();
realtimeData.Columns.Add("SensorValue", typeof(float));
realtimeData.Rows.Add(0.0f);

RadialGauge gauge = new RadialGauge();
gauge.DataSource = realtimeData;
gauge.DisplayMember = "SensorValue";
gauge.DisplayRecordIndex = 0;

// Timer for high-frequency updates
Timer updateTimer = new Timer();
updateTimer.Interval = 100;  // 10 updates per second
updateTimer.Tick += (s, e) => {
    float newValue = GetSensorReading();  // Your data acquisition
    realtimeData.Rows[0]["SensorValue"] = newValue;
};
updateTimer.Start();
```

## LinearGauge Data Binding

### Basic Setup

```csharp
// Create data source
DataTable progressData = new DataTable();
progressData.Columns.Add("Progress", typeof(float));
progressData.Rows.Add(45.0f);

// Create and configure gauge
LinearGauge progressBar = new LinearGauge();
progressBar.LinearFrameType = LinearFrameType.Horizontal;
progressBar.MinimumValue = 0;
progressBar.MaximumValue = 100;

// Bind to data source
progressBar.DataSource = progressData;
progressBar.DisplayMember = "Progress";
progressBar.DisplayRecordIndex = 0;

this.Controls.Add(progressBar);

// Update progress
progressData.Rows[0]["Progress"] = 75.0f;
```

### Battery Level Monitor

```csharp
// Battery monitoring with data binding
DataTable batteryData = new DataTable();
batteryData.Columns.Add("BatteryLevel", typeof(float));
batteryData.Rows.Add(85.0f);

LinearGauge batteryGauge = new LinearGauge();
batteryGauge.LinearFrameType = LinearFrameType.Horizontal;
batteryGauge.MinimumValue = 0;
batteryGauge.MaximumValue = 100;
batteryGauge.DataSource = batteryData;
batteryGauge.DisplayMember = "BatteryLevel";
batteryGauge.DisplayRecordIndex = 0;

// Add color-coded ranges
batteryGauge.Ranges.Add(new LinearRange { StartValue = 0, EndValue = 15, Color = Color.Red, Height = 10 });
batteryGauge.Ranges.Add(new LinearRange { StartValue = 15, EndValue = 30, Color = Color.Orange, Height = 10 });
batteryGauge.Ranges.Add(new LinearRange { StartValue = 30, EndValue = 100, Color = Color.Green, Height = 10 });

// Simulate battery drain
Timer batteryTimer = new Timer();
batteryTimer.Interval = 5000;  // Update every 5 seconds
batteryTimer.Tick += (s, e) => {
    float currentLevel = (float)batteryData.Rows[0]["BatteryLevel"];
    batteryData.Rows[0]["BatteryLevel"] = Math.Max(0, currentLevel - 1);
};
batteryTimer.Start();
```

### Multi-Sensor Display

```csharp
// Multiple sensors with individual LinearGauges
DataTable sensorsData = new DataTable();
sensorsData.Columns.Add("Sensor1", typeof(float));
sensorsData.Columns.Add("Sensor2", typeof(float));
sensorsData.Columns.Add("Sensor3", typeof(float));
sensorsData.Rows.Add(45.0f, 67.0f, 82.0f);

// Create gauges for each sensor
for (int i = 0; i < 3; i++)
{
    LinearGauge sensor = new LinearGauge();
    sensor.LinearFrameType = LinearFrameType.Horizontal;
    sensor.Size = new Size(300, 60);
    sensor.Location = new Point(20, 20 + (i * 70));
    sensor.MinimumValue = 0;
    sensor.MaximumValue = 100;
    sensor.DataSource = sensorsData;
    sensor.DisplayMember = $"Sensor{i + 1}";
    sensor.DisplayRecordIndex = 0;
    
    this.Controls.Add(sensor);
}

// Update all sensors
sensorsData.Rows[0]["Sensor1"] = 52.0f;
sensorsData.Rows[0]["Sensor2"] = 71.0f;
sensorsData.Rows[0]["Sensor3"] = 88.0f;
```

## DigitalGauge Data Binding

DigitalGauge binds **string values** instead of numeric values.

### Basic Setup

```csharp
// Create data source with string column
DataTable displayData = new DataTable();
displayData.Columns.Add("DisplayText", typeof(string));
displayData.Rows.Add("12:34:56");

// Create and configure gauge
DigitalGauge ledDisplay = new DigitalGauge();
ledDisplay.CharacterType = CharacterType.SevenSegment;
ledDisplay.CharacterCount = 8;

// Bind to data source
ledDisplay.DataSource = displayData;
ledDisplay.DisplayMember = "DisplayText";
ledDisplay.DisplayRecordIndex = 0;

this.Controls.Add(ledDisplay);

// Update display
displayData.Rows[0]["DisplayText"] = "12:35:01";
```

### Digital Clock with Data Binding

```csharp
// Clock with data-bound display
DataTable clockData = new DataTable();
clockData.Columns.Add("Time", typeof(string));
clockData.Rows.Add(DateTime.Now.ToString("HH:mm:ss"));

DigitalGauge clock = new DigitalGauge();
clock.CharacterType = CharacterType.SevenSegment;
clock.CharacterCount = 8;
clock.ForeColor = Color.Red;
clock.BackColor = Color.Black;
clock.ShowInvisibleSegments = true;
clock.DataSource = clockData;
clock.DisplayMember = "Time";
clock.DisplayRecordIndex = 0;

// Update clock every second
Timer clockTimer = new Timer();
clockTimer.Interval = 1000;
clockTimer.Tick += (s, e) => {
    clockData.Rows[0]["Time"] = DateTime.Now.ToString("HH:mm:ss");
};
clockTimer.Start();

this.Controls.Add(clock);
```

### Status Message Display

```csharp
// Status messages with data binding
DataTable statusData = new DataTable();
statusData.Columns.Add("Status", typeof(string));
statusData.Rows.Add("READY");

DigitalGauge statusDisplay = new DigitalGauge();
statusDisplay.CharacterType = CharacterType.FourteenSegment;
statusDisplay.CharacterCount = 20;
statusDisplay.ForeColor = Color.Lime;
statusDisplay.BackColor = Color.Black;
statusDisplay.DataSource = statusData;
statusDisplay.DisplayMember = "Status";
statusDisplay.DisplayRecordIndex = 0;

// Update status
void SetStatus(string message)
{
    statusData.Rows[0]["Status"] = message.PadRight(20).Substring(0, 20);
}

SetStatus("CONNECTING...");
// Later...
SetStatus("CONNECTED");
```

## Advanced Data Binding Scenarios

### BindingSource for Complex Data

```csharp
using System.ComponentModel;

// Create BindingSource for advanced features
BindingSource bindingSource = new BindingSource();

// Create data source
DataTable data = new DataTable();
data.Columns.Add("Value", typeof(float));
data.Rows.Add(50.0f);

// Bind BindingSource to data
bindingSource.DataSource = data;

// Bind gauge to BindingSource
gauge.DataSource = bindingSource;
gauge.DisplayMember = "Value";

// Update through BindingSource
bindingSource.EndEdit();
((DataRowView)bindingSource.Current)["Value"] = 75.0f;
```

### Database-Driven Gauges

```csharp
using System.Data.SqlClient;

private void LoadDatabaseData()
{
    string connectionString = "your_connection_string";
    string query = "SELECT SensorValue FROM Sensors WHERE SensorID = 1";

    using (SqlConnection connection = new SqlConnection(connectionString))
    {
        SqlDataAdapter adapter = new SqlDataAdapter(query, connection);
        DataTable sensorData = new DataTable();
        adapter.Fill(sensorData);

        // Bind gauge to database data
        gauge.DataSource = sensorData;
        gauge.DisplayMember = "SensorValue";
        gauge.DisplayRecordIndex = 0;
    }
}

// Refresh data periodically
Timer refreshTimer = new Timer();
refreshTimer.Interval = 5000;  // Refresh every 5 seconds
refreshTimer.Tick += (s, e) => LoadDatabaseData();
refreshTimer.Start();
```

### Multiple Row Display

```csharp
// Display different rows from same data source
DataTable multiData = new DataTable();
multiData.Columns.Add("Value", typeof(float));
multiData.Rows.Add(45.0f);
multiData.Rows.Add(67.0f);
multiData.Rows.Add(82.0f);

// Gauge 1 - Row 0
RadialGauge gauge1 = new RadialGauge();
gauge1.DataSource = multiData;
gauge1.DisplayMember = "Value";
gauge1.DisplayRecordIndex = 0;  // First row

// Gauge 2 - Row 1
RadialGauge gauge2 = new RadialGauge();
gauge2.DataSource = multiData;
gauge2.DisplayMember = "Value";
gauge2.DisplayRecordIndex = 1;  // Second row

// Gauge 3 - Row 2
RadialGauge gauge3 = new RadialGauge();
gauge3.DataSource = multiData;
gauge3.DisplayMember = "Value";
gauge3.DisplayRecordIndex = 2;  // Third row

// Update specific row
multiData.Rows[1]["Value"] = 75.0f;  // Only gauge2 updates
```

### Custom Objects as Data Source

```csharp
using System.ComponentModel;

// Custom class with INotifyPropertyChanged
public class SensorReading : INotifyPropertyChanged
{
    private float _value;
    public float Value
    {
        get => _value;
        set
        {
            _value = value;
            OnPropertyChanged(nameof(Value));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

// Use with BindingList
BindingList<SensorReading> readings = new BindingList<SensorReading>();
readings.Add(new SensorReading { Value = 50.0f });

// Bind gauge
gauge.DataSource = readings;
gauge.DisplayMember = "Value";
gauge.DisplayRecordIndex = 0;

// Update (triggers PropertyChanged, gauge updates automatically)
readings[0].Value = 75.0f;
```

## Performance Considerations

### Optimize Update Frequency

```csharp
// Throttle updates for better performance
private DateTime lastUpdate = DateTime.MinValue;
private const int MinUpdateInterval = 100;  // milliseconds

private void UpdateGaugeData(float newValue)
{
    TimeSpan elapsed = DateTime.Now - lastUpdate;
    
    if (elapsed.TotalMilliseconds >= MinUpdateInterval)
    {
        dataTable.Rows[0]["Value"] = newValue;
        lastUpdate = DateTime.Now;
    }
}
```

### Batch Updates

```csharp
// Batch multiple updates
dataTable.BeginLoadData();  // Suspend constraints and events

dataTable.Rows[0]["Sensor1"] = value1;
dataTable.Rows[0]["Sensor2"] = value2;
dataTable.Rows[0]["Sensor3"] = value3;

dataTable.EndLoadData();  // Resume, triggers single update
```

### Asynchronous Data Loading

```csharp
using System.Threading.Tasks;

private async Task LoadSensorDataAsync()
{
    // Load data on background thread
    DataTable data = await Task.Run(() => {
        DataTable dt = new DataTable();
        dt.Columns.Add("Value", typeof(float));
        
        // Simulate data loading
        float value = GetSensorReadingFromHardware();
        dt.Rows.Add(value);
        
        return dt;
    });

    // Update gauge on UI thread
    gauge.DataSource = data;
    gauge.DisplayMember = "Value";
    gauge.DisplayRecordIndex = 0;
}
```

## Troubleshooting

### Issue: Gauge not updating when data changes

**Cause:** Data source doesn't support change notification

**Solution:**
```csharp
// Use DataTable, BindingList, or implement INotifyPropertyChanged
DataTable data = new DataTable();  // Supports change notification
// OR
BindingList<MyClass> data = new BindingList<MyClass>();  // Supports change notification
```

### Issue: DisplayMember property not found

**Cause:** Column/property name mismatch

**Solution:**
```csharp
// Verify column name matches exactly
DataTable data = new DataTable();
data.Columns.Add("SensorValue", typeof(float));  // Column name

gauge.DisplayMember = "SensorValue";  // Must match exactly (case-sensitive)
```

### Issue: DisplayRecordIndex out of range

**Cause:** Index exceeds row count

**Solution:**
```csharp
// Verify index is valid
if (dataTable.Rows.Count > 0)
{
    gauge.DisplayRecordIndex = 0;  // Valid
}

// Or check dynamically
int recordIndex = 2;
if (recordIndex < dataTable.Rows.Count)
{
    gauge.DisplayRecordIndex = recordIndex;
}
```

### Issue: Performance degradation with frequent updates

**Cause:** Too many update events

**Solution:**
```csharp
// Throttle updates
private void ThrottledUpdate(float newValue)
{
    if (DateTime.Now - lastUpdate > TimeSpan.FromMilliseconds(100))
    {
        dataTable.Rows[0]["Value"] = newValue;
        lastUpdate = DateTime.Now;
    }
}

// Or use BindingSource with SuspendBinding/ResumeBinding
bindingSource.SuspendBinding();
// Make multiple changes
bindingSource.ResumeBinding();
```

### Issue: Gauge shows wrong value after binding

**Cause:** DisplayRecordIndex not set or incorrect

**Solution:**
```csharp
gauge.DataSource = dataTable;
gauge.DisplayMember = "Value";
gauge.DisplayRecordIndex = 0;  // Explicitly set to first row
```

## Best Practices

1. **Use appropriate data sources** - DataTable for simple scenarios, BindingList for collections, BindingSource for complex scenarios
2. **Implement INotifyPropertyChanged** - When using custom objects as data source
3. **Set DisplayRecordIndex** - Always explicitly set for clarity
4. **Throttle high-frequency updates** - Prevent performance issues
5. **Batch multiple updates** - Use BeginLoadData/EndLoadData for DataTable
6. **Verify column names** - Ensure DisplayMember matches exactly (case-sensitive)
7. **Handle data source changes** - Update bindings when switching data sources
8. **Test with realistic data rates** - Ensure performance meets requirements
9. **Use async loading** - For database or network data sources
10. **Dispose data sources** - Clean up when gauge is disposed

## Example: Complete Monitoring Dashboard

```csharp
public class MonitoringDashboard : Form
{
    private DataTable sensorData;
    private Timer updateTimer;

    private RadialGauge tempGauge;
    private RadialGauge pressureGauge;
    private LinearGauge batteryGauge;
    private DigitalGauge statusDisplay;

    public MonitoringDashboard()
    {
        InitializeData();
        InitializeGauges();
        StartMonitoring();
    }

    private void InitializeData()
    {
        sensorData = new DataTable();
        sensorData.Columns.Add("Temperature", typeof(float));
        sensorData.Columns.Add("Pressure", typeof(float));
        sensorData.Columns.Add("BatteryLevel", typeof(float));
        sensorData.Columns.Add("Status", typeof(string));
        sensorData.Rows.Add(72.0f, 85.0f, 95.0f, "NORMAL");
    }

    private void InitializeGauges()
    {
        // Temperature gauge
        tempGauge = new RadialGauge();
        tempGauge.MinimumValue = 0;
        tempGauge.MaximumValue = 150;
        tempGauge.DataSource = sensorData;
        tempGauge.DisplayMember = "Temperature";
        tempGauge.DisplayRecordIndex = 0;
        tempGauge.GaugeLabel = "Temp (°F)";

        // Pressure gauge
        pressureGauge = new RadialGauge();
        pressureGauge.MinimumValue = 0;
        pressureGauge.MaximumValue = 200;
        pressureGauge.DataSource = sensorData;
        pressureGauge.DisplayMember = "Pressure";
        pressureGauge.DisplayRecordIndex = 0;
        pressureGauge.GaugeLabel = "Pressure (PSI)";

        // Battery gauge
        batteryGauge = new LinearGauge();
        batteryGauge.LinearFrameType = LinearFrameType.Horizontal;
        batteryGauge.MinimumValue = 0;
        batteryGauge.MaximumValue = 100;
        batteryGauge.DataSource = sensorData;
        batteryGauge.DisplayMember = "BatteryLevel";
        batteryGauge.DisplayRecordIndex = 0;

        // Status display
        statusDisplay = new DigitalGauge();
        statusDisplay.CharacterType = CharacterType.FourteenSegment;
        statusDisplay.CharacterCount = 15;
        statusDisplay.DataSource = sensorData;
        statusDisplay.DisplayMember = "Status";
        statusDisplay.DisplayRecordIndex = 0;

        // Add to form
        this.Controls.AddRange(new Control[] { 
            tempGauge, pressureGauge, batteryGauge, statusDisplay 
        });
    }

    private void StartMonitoring()
    {
        updateTimer = new Timer();
        updateTimer.Interval = 1000;  // Update every second
        updateTimer.Tick += UpdateSensorData;
        updateTimer.Start();
    }

    private void UpdateSensorData(object sender, EventArgs e)
    {
        // Simulate sensor readings
        sensorData.Rows[0]["Temperature"] = GetTemperature();
        sensorData.Rows[0]["Pressure"] = GetPressure();
        sensorData.Rows[0]["BatteryLevel"] = GetBatteryLevel();
        sensorData.Rows[0]["Status"] = GetSystemStatus();
        
        // All gauges update automatically
    }

    private float GetTemperature() => 72 + (float)(new Random().NextDouble() * 5);
    private float GetPressure() => 85 + (float)(new Random().NextDouble() * 10);
    private float GetBatteryLevel() => Math.Max(0, (float)sensorData.Rows[0]["BatteryLevel"] - 0.1f);
    private string GetSystemStatus() => "NORMAL";
}
```
