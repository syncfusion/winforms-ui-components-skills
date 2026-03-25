# Getting Started with Windows Forms ListView

This guide covers the basics of adding and configuring the Syncfusion Windows Forms ListView (SfListView) control in your application.

## Assembly Deployment

### Required Assemblies

Add the following assembly references to your project:

- `Syncfusion.Core.WinForms`
- `Syncfusion.DataSource.WinForms`
- `Syncfusion.GridCommon.WinForms`
- `Syncfusion.SfListView.WinForms`

### NuGet Package

Install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.SfListView.WinForms
```

Or search for "Syncfusion.SfListView.WinForms" in the NuGet Package Manager UI.

## Creating Your First ListView

### Method 1: Adding Control via Designer

1. Open your Windows Forms project in Visual Studio
2. Open the Toolbox (View → Toolbox)
3. Locate `SfListView` in the Syncfusion Controls section
4. Drag and drop it onto your form
5. Required assembly references will be added automatically

### Method 2: Adding Control in Code

**C# Example:**

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.ListView;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Create ListView instance
            SfListView sfListView1 = new SfListView();
            sfListView1.Location = new Point(100, 100);
            sfListView1.Size = new Size(300, 320);
            
            // Add to form
            this.Controls.Add(sfListView1);
        }
    }
}
```

**VB.NET Example:**

```vb
Imports Syncfusion.WinForms.ListView

Namespace WindowsFormsApplication1
    Partial Public Class Form1
        Inherits Form
        
        Public Sub New()
            InitializeComponent()
            
            ' Create ListView instance
            Dim listView As New SfListView()
            listView.Location = New Point(100, 100)
            listView.Size = New Size(300, 320)
            
            ' Add to form
            Me.Controls.Add(listView)
        End Sub
    End Class
End Namespace
```

## Creating Data Objects

### Step 1: Define Your Data Class

Create a class with public properties for your data:

**C#:**

```csharp
public class CountryInfo
{
    public string CountryName { get; set; }
    public string Continent { get; set; }
}
```

**VB.NET:**

```vb
Public Class CountryInfo
    Public Property CountryName() As String
    Public Property Continent() As String
End Class
```

### Step 2: Create Data Source

Create a method to generate sample data:

**C#:**

```csharp
public List<CountryInfo> GetDataSource()
{
    List<CountryInfo> countryInfoCollection = new List<CountryInfo>();
    
    // Asia
    countryInfoCollection.Add(new CountryInfo() { CountryName = "China", Continent = "Asia" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "India", Continent = "Asia" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Japan", Continent = "Asia" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Malaysia", Continent = "Asia" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Singapore", Continent = "Asia" });
    
    // Africa
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Kenya", Continent = "Africa" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Nigeria", Continent = "Africa" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "South Africa", Continent = "Africa" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Uganda", Continent = "Africa" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Zimbabwe", Continent = "Africa" });
    
    // Europe
    countryInfoCollection.Add(new CountryInfo() { CountryName = "France", Continent = "Europe" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Germany", Continent = "Europe" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Italy", Continent = "Europe" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Spain", Continent = "Europe" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "United Kingdom", Continent = "Europe" });
    
    // North America
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Canada", Continent = "North America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Cuba", Continent = "North America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Jamaica", Continent = "North America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Mexico", Continent = "North America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "United States of America", Continent = "North America" });
    
    // Oceania
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Australia", Continent = "Oceania" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "New Zealand", Continent = "Oceania" });
    
    // South America
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Argentina", Continent = "South America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Brazil", Continent = "South America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Chile", Continent = "South America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Colombia", Continent = "South America" });
    countryInfoCollection.Add(new CountryInfo() { CountryName = "Uruguay", Continent = "South America" });
    
    return countryInfoCollection;
}
```

**VB.NET:**

```vb
Public Function GetDataSource() As List(Of CountryInfo)
    Dim countryInfoCollection As New List(Of CountryInfo)()
    
    ' Asia
    countryInfoCollection.Add(New CountryInfo() With {.CountryName = "China", .Continent = "Asia"})
    countryInfoCollection.Add(New CountryInfo() With {.CountryName = "India", .Continent = "Asia"})
    countryInfoCollection.Add(New CountryInfo() With {.CountryName = "Japan", .Continent = "Asia"})
    
    ' Add more countries as shown in C# example...
    
    Return countryInfoCollection
End Function
```

## Binding Data to ListView

### Basic Data Binding

Use the `DataSource` and `DisplayMember` properties:

**C#:**

```csharp
// Set data source
sfListView1.DataSource = GetDataSource();

// Set which property to display
sfListView1.DisplayMember = "CountryName";
```

**VB.NET:**

```vb
' Set data source
sfListView1.DataSource = GetDataSource()

' Set which property to display
sfListView1.DisplayMember = "CountryName"
```

### Complete Example

Here's a complete working example:

```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.ListView;

namespace ListViewGettingStarted
{
    public partial class Form1 : Form
    {
        private SfListView sfListView1;
        
        public Form1()
        {
            InitializeComponent();
            InitializeListView();
        }
        
        private void InitializeListView()
        {
            // Create and configure ListView
            sfListView1 = new SfListView();
            sfListView1.Location = new Point(20, 20);
            sfListView1.Size = new Size(400, 500);
            sfListView1.Dock = DockStyle.Fill;
            
            // Bind data
            sfListView1.DataSource = GetDataSource();
            sfListView1.DisplayMember = "CountryName";
            
            // Add to form
            this.Controls.Add(sfListView1);
        }
        
        private List<CountryInfo> GetDataSource()
        {
            // Data creation code from above
            return new List<CountryInfo>
            {
                new CountryInfo { CountryName = "China", Continent = "Asia" },
                new CountryInfo { CountryName = "Germany", Continent = "Europe" },
                new CountryInfo { CountryName = "United States", Continent = "North America" }
                // Add more items...
            };
        }
    }
    
    public class CountryInfo
    {
        public string CountryName { get; set; }
        public string Continent { get; set; }
    }
}
```

## Quick Start Features

### Enable Selection

```csharp
sfListView1.SelectionMode = Syncfusion.WinForms.ListView.Enums.SelectionMode.One;
```

### Add Grouping

```csharp
using Syncfusion.DataSource;

sfListView1.View.GroupDescriptors.Add(new GroupDescriptor()
{
    PropertyName = "Continent"
});
```

### Add Sorting

```csharp
sfListView1.View.SortDescriptors.Add(new SortDescriptor()
{
    PropertyName = "CountryName",
    Direction = ListSortDirection.Ascending
});
```

### Add Filtering

```csharp
sfListView1.View.Filter = FilterCountries;
sfListView1.View.RefreshFilter();

private bool FilterCountries(object obj)
{
    var country = obj as CountryInfo;
    return country.Continent == "Asia" || country.Continent == "Europe";
}
```

## Common Initial Configurations

### Size and Position

```csharp
// Fixed size and position
sfListView1.Location = new Point(10, 10);
sfListView1.Size = new Size(400, 500);

// Or fill entire form
sfListView1.Dock = DockStyle.Fill;

// Or use anchoring
sfListView1.Anchor = AnchorStyles.Top | AnchorStyles.Bottom | 
                     AnchorStyles.Left | AnchorStyles.Right;
```

### Basic Appearance

```csharp
// Set item height
sfListView1.ItemHeight = 40;

// Set colors
sfListView1.Style.ItemStyle.BackColor = Color.White;
sfListView1.Style.ItemStyle.HoverBackColor = Color.LightBlue;
```

## Best Practices

1. **Always set DisplayMember** - This tells the ListView which property to display
2. **Use proper sizing** - Set appropriate Size or Dock to make ListView visible
3. **Create strongly-typed classes** - Use proper data classes instead of anonymous types
4. **Initialize in constructor** - Set up ListView in Form constructor or Load event
5. **Handle events early** - Subscribe to events before setting DataSource

## Next Steps

Now that you have a basic ListView running, explore:

- [Data Binding](data-binding.md) - Advanced binding scenarios
- [Selection](selection.md) - Selection modes and events
- [Data Operations](data-operations.md) - Grouping, sorting, and filtering
- [Appearance](appearance.md) - Styling and themes

## Related Topics

- [Overview](../SKILL.md#component-overview) - Component features and architecture
- [Data Binding](data-binding.md) - Comprehensive data binding guide
- [Common Patterns](../SKILL.md#common-patterns) - Real-world usage patterns
