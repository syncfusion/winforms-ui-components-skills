# Data Binding in MultiSelectionComboBox

The MultiSelectionComboBox supports binding to external data sources via the standard `DataSource`, `DisplayMember`, and `ValueMember` properties — the same pattern used by most WinForms data controls.

## Supported Data Source Types

| Type | Notes |
|---|---|
| `ArrayList` | Simple list of objects |
| `DataView` | View over a `DataTable`, supports filtering/sorting |
| `DataTable` | Full table with columns |

## Core Properties

| Property | Purpose |
|---|---|
| `DataSource` | The list or table object to bind to |
| `DisplayMember` | Column/property name shown to the user in the dropdown |
| `ValueMember` | Column/property name used as the underlying value |

> When `DataSource` is set, the `Items` collection is read-only — do not call `Items.Add()` directly.

## Binding to a DataTable

```csharp
DataTable dt = new DataTable();
dt.Columns.Add("ID");
dt.Columns.Add("Name");
dt.Rows.Add("1", "Alice");
dt.Rows.Add("2", "Bob");
dt.Rows.Add("3", "Carol");

this.multiSelectionComboBox1.DataSource = dt;
this.multiSelectionComboBox1.DisplayMember = "Name";
this.multiSelectionComboBox1.ValueMember = "ID";
```

```vb
Dim dt As New DataTable()
dt.Columns.Add("ID")
dt.Columns.Add("Name")
dt.Rows.Add("1", "Alice")
dt.Rows.Add("2", "Bob")
dt.Rows.Add("3", "Carol")

Me.multiSelectionComboBox1.DataSource = dt
Me.multiSelectionComboBox1.DisplayMember = "Name"
Me.multiSelectionComboBox1.ValueMember = "ID"
```

## Binding to a DataView (External Data Source Pattern)

This pattern is useful when you need to filter or sort the source before binding:

```csharp
// Build the DataTable
DataTable dt = new DataTable("Table1");
dt.Columns.Add("FirstName");
dt.Columns.Add("LastName");
dt.Columns.Add("Occupation");
dt.Columns.Add("Place");

DataSet ds = new DataSet();
ds.Tables.Add(dt);

dt.Rows.Add("John",   "Tina",   "Doctor",    "Italy");
dt.Rows.Add("Mary",   "anu",    "Teacher",   "America");
dt.Rows.Add("asha",   "roy",    "Staff",     "London");
dt.Rows.Add("George", "Gaskin", "Nurse",     "Germany");
dt.Rows.Add("sam",    "jens",   "Engineer",  "Russia");
dt.Rows.Add("Ben",    "Geo",    "Developer", "India");

// Wrap in a DataView and bind
DataView view = new DataView(dt);
this.multiSelectionComboBox1.DataSource = view;
this.multiSelectionComboBox1.DisplayMember = "Place";
```

```vb
Dim dt As DataTable = New DataTable("Table1")
dt.Columns.Add("FirstName")
dt.Columns.Add("LastName")
dt.Columns.Add("Occupation")
dt.Columns.Add("Place")

Dim ds As DataSet = New DataSet()
ds.Tables.Add(dt)
dt.Rows.Add("John",   "Tina",   "Doctor",    "Italy")
dt.Rows.Add("Mary",   "anu",    "Teacher",   "America")
dt.Rows.Add("asha",   "roy",    "Staff",     "London")

Dim view As DataView = New DataView(dt)
Me.multiSelectionComboBox1.DataSource = view
Me.multiSelectionComboBox1.DisplayMember = "Place"
```

## Binding to an ArrayList

For simple, non-tabular data:

```csharp
var list = new System.Collections.ArrayList { "Red", "Green", "Blue", "Yellow" };
this.multiSelectionComboBox1.DataSource = list;
// No DisplayMember needed for plain strings
```

## DataSourceChanged Event

Fires when the bound data source is replaced. Use it to react to source swaps at runtime:

```csharp
this.multiSelectionComboBox1.DataSourceChanged += (sender, e) =>
{
    // Re-apply DisplayMember/ValueMember or refresh UI as needed
    Console.WriteLine("DataSource changed.");
};
```

```vb
Private Sub MultiSelectionComboBox1_DataSourceChanged(
    sender As System.Object, e As System.EventArgs) _
    Handles MultiSelectionComboBox1.DataSourceChanged
    ' React to data source change
End Sub
```

> Setting `DataSource` to `null` (`Nothing` in VB) also resets `DisplayMember` to an empty string automatically.
