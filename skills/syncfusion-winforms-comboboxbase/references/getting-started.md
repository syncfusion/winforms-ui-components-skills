# Getting Started with ComboBoxBase

This guide covers basic setup and configuration of the ComboBoxBase control using both Designer and Code approaches.

## Installation

### Assembly References

Add the following assembly references to your WinForms project:

**Required Assemblies:**
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Tools.Windows.dll`

**Location:** Typically found in:
```
C:\Program Files (x86)\Syncfusion\Essential Studio\{version}\precompiledassemblies\{.NET version}\
```

### NuGet Installation

Install via NuGet Package Manager:

```bash
Install-Package Syncfusion.Tools.Windows
```

Or via .NET CLI:

```bash
dotnet add package Syncfusion.Tools.Windows
```

### Namespace

Add the required namespace:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Designer Setup

The easiest way to add ComboBoxBase is through Visual Studio Designer.

### Step 1: Add ComboBoxBase to Toolbox

If ComboBoxBase isn't in your Toolbox:

1. Right-click on Toolbox → Choose Items
2. Browse to `Syncfusion.Tools.Windows.dll`
3. Select ComboBoxBase
4. Click OK

### Step 2: Drag and Drop

1. **Drag ComboBoxBase** from Toolbox to Form

   ![ComboBoxBase in Toolbox](../images/comboboxbase-toolbox.png)

2. **Drag ListBox** from Toolbox to Form

   ![ListBox Added](../images/listbox-added.png)

### Step 3: Connect ListBox to ComboBoxBase

1. Select ComboBoxBase on the Form
2. In Properties window, find **ListControl** property
3. Select `listBox1` from the dropdown

   ![Set ListControl Property](../images/set-listcontrol.png)

### Step 4: Add Items to ListBox

1. Select listBox1
2. In Properties window, find **Items** property
3. Click the ellipsis (...) button
4. Add items line by line
5. Click OK

   ![Add Items to ListBox](../images/add-items.png)

### Step 5: Run Application

Press F5 to run. The ComboBoxBase should display with dropdown capability.

## Code Setup

Create ComboBoxBase programmatically for more control.

### Basic Setup

**C# Example:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ComboBoxBaseDemo
{
    public partial class Form1 : Form
    {
        private ComboBoxBase comboBoxBase1;
        private ListBox listBox1;
        
        public Form1()
        {
            InitializeComponent();
            CreateComboBoxBase();
        }
        
        private void CreateComboBoxBase()
        {
            // Create instances
            comboBoxBase1 = new ComboBoxBase();
            listBox1 = new ListBox();
            
            // Configure ComboBoxBase
            comboBoxBase1.Location = new Point(20, 20);
            comboBoxBase1.Size = new Size(200, 25);
            comboBoxBase1.ListControl = listBox1;
            
            // Add items to ListBox
            listBox1.Items.Add("Option 1");
            listBox1.Items.Add("Option 2");
            listBox1.Items.Add("Option 3");
            listBox1.Items.Add("Option 4");
            listBox1.Items.Add("Option 5");
            
            // Add controls to form
            this.Controls.Add(listBox1);
            this.Controls.Add(comboBoxBase1);
        }
    }
}
```

**VB.NET Example:**
```vb
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Namespace ComboBoxBaseDemo
    Public Partial Class Form1
        Inherits Form
        
        Private comboBoxBase1 As ComboBoxBase
        Private listBox1 As ListBox
        
        Public Sub New()
            InitializeComponent()
            CreateComboBoxBase()
        End Sub
        
        Private Sub CreateComboBoxBase()
            ' Create instances
            comboBoxBase1 = New ComboBoxBase()
            listBox1 = New ListBox()
            
            ' Configure ComboBoxBase
            comboBoxBase1.Location = New Point(20, 20)
            comboBoxBase1.Size = New Size(200, 25)
            comboBoxBase1.ListControl = listBox1
            
            ' Add items to ListBox
            listBox1.Items.Add("Option 1")
            listBox1.Items.Add("Option 2")
            listBox1.Items.Add("Option 3")
            listBox1.Items.Add("Option 4")
            listBox1.Items.Add("Option 5")
            
            ' Add controls to form
            Me.Controls.Add(listBox1)
            Me.Controls.Add(comboBoxBase1)
        End Sub
    End Class
End Namespace
```

### Using AddRange for Multiple Items

**C#:**
```csharp
listBox1.Items.AddRange(new object[] {
    "Washington",
    "Oregon",
    "California",
    "Nevada",
    "Arizona"
});
```

**VB.NET:**
```vb
listBox1.Items.AddRange(New Object() {
    "Washington",
    "Oregon",
    "California",
    "Nevada",
    "Arizona"
})
```

## DataSource Configuration

For dynamic data binding, use the DataSource property on the ListBox.

### Simple String List

**C#:**
```csharp
using System.Collections.Generic;

private void SetupDataSource()
{
    // Create data source
    List<string> countries = new List<string>
    {
        "United States",
        "Canada",
        "Mexico",
        "United Kingdom",
        "Germany",
        "France",
        "Japan",
        "Australia"
    };
    
    // Set DataSource on ListBox
    listBox1.DataSource = countries;
    
    // Connect to ComboBoxBase
    comboBoxBase1.ListControl = listBox1;
}
```

**VB.NET:**
```vb
Imports System.Collections.Generic

Private Sub SetupDataSource()
    ' Create data source
    Dim countries As New List(Of String) From {
        "United States",
        "Canada",
        "Mexico",
        "United Kingdom",
        "Germany",
        "France",
        "Japan",
        "Australia"
    }
    
    ' Set DataSource on ListBox
    listBox1.DataSource = countries
    
    ' Connect to ComboBoxBase
    comboBoxBase1.ListControl = listBox1
End Sub
```

### Custom Object DataSource

**C# with Custom Class:**
```csharp
// Define custom class
public class USState
{
    public string Name { get; set; }
    public string Abbreviation { get; set; }
    
    public USState(string name, string abbreviation)
    {
        Name = name;
        Abbreviation = abbreviation;
    }
    
    public override string ToString()
    {
        return Name; // Display name in list
    }
}

private void SetupCustomDataSource()
{
    // Create data
    List<USState> states = new List<USState>
    {
        new USState("Washington", "WA"),
        new USState("West Virginia", "WV"),
        new USState("Wisconsin", "WI"),
        new USState("Wyoming", "WY")
    };
    
    // Set DataSource
    listBox1.DataSource = states;
    listBox1.DisplayMember = "Name";        // Property to display
    listBox1.ValueMember = "Abbreviation";  // Property for value
    
    comboBoxBase1.ListControl = listBox1;
}

// Get selected value
private void GetSelectedValue()
{
    if (listBox1.SelectedItem != null)
    {
        USState selected = (USState)listBox1.SelectedItem;
        string stateName = selected.Name;
        string stateCode = selected.Abbreviation;
        
        MessageBox.Show($"Selected: {stateName} ({stateCode})");
    }
}
```

**VB.NET with Custom Class:**
```vb
' Define custom class
Public Class USState
    Public Property Name As String
    Public Property Abbreviation As String
    
    Public Sub New(name As String, abbreviation As String)
        Me.Name = name
        Me.Abbreviation = abbreviation
    End Sub
    
    Public Overrides Function ToString() As String
        Return Name ' Display name in list
    End Function
End Class

Private Sub SetupCustomDataSource()
    ' Create data
    Dim states As New List(Of USState) From {
        New USState("Washington", "WA"),
        New USState("West Virginia", "WV"),
        New USState("Wisconsin", "WI"),
        New USState("Wyoming", "WY")
    }
    
    ' Set DataSource
    listBox1.DataSource = states
    listBox1.DisplayMember = "Name"        ' Property to display
    listBox1.ValueMember = "Abbreviation"  ' Property for value
    
    comboBoxBase1.ListControl = listBox1
End Sub

' Get selected value
Private Sub GetSelectedValue()
    If listBox1.SelectedItem IsNot Nothing Then
        Dim selected As USState = CType(listBox1.SelectedItem, USState)
        Dim stateName As String = selected.Name
        Dim stateCode As String = selected.Abbreviation
        
        MessageBox.Show($"Selected: {stateName} ({stateCode})")
    End If
End Sub
```

## Complete Setup Example

Full working example with all setup code:

**C#:**
```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace ComboBoxBaseFullExample
{
    public partial class MainForm : Form
    {
        private ComboBoxBase comboBoxBase1;
        private ListBox listBox1;
        private Label label1;
        
        public MainForm()
        {
            InitializeComponent();
            SetupUI();
        }
        
        private void SetupUI()
        {
            // Form configuration
            this.Text = "ComboBoxBase Demo";
            this.Size = new Size(400, 300);
            
            // Label
            label1 = new Label
            {
                Text = "Select a State:",
                Location = new Point(20, 20),
                AutoSize = true
            };
            
            // ComboBoxBase
            comboBoxBase1 = new ComboBoxBase
            {
                Location = new Point(20, 45),
                Size = new Size(250, 25)
            };
            
            // ListBox
            listBox1 = new ListBox();
            
            // Connect ListBox to ComboBoxBase
            comboBoxBase1.ListControl = listBox1;
            
            // Populate data
            PopulateData();
            
            // Add to form
            this.Controls.Add(label1);
            this.Controls.Add(listBox1);
            this.Controls.Add(comboBoxBase1);
        }
        
        private void PopulateData()
        {
            List<string> states = new List<string>
            {
                "Alabama", "Alaska", "Arizona", "Arkansas",
                "California", "Colorado", "Connecticut",
                "Delaware", "Florida", "Georgia"
            };
            
            listBox1.DataSource = states;
        }
    }
}
```

## Sizing and Positioning

### Setting Size

**C#:**
```csharp
comboBoxBase1.Size = new Size(200, 25); // Width, Height
```

**VB.NET:**
```vb
comboBoxBase1.Size = New Size(200, 25) ' Width, Height
```

### Setting Location

**C#:**
```csharp
comboBoxBase1.Location = new Point(20, 50); // X, Y
```

**VB.NET:**
```vb
comboBoxBase1.Location = New Point(20, 50) ' X, Y
```

### Docking

**C#:**
```csharp
comboBoxBase1.Dock = DockStyle.Top;
```

**VB.NET:**
```vb
comboBoxBase1.Dock = DockStyle.Top
```

## Troubleshooting

### Issue: Dropdown doesn't show items

**Cause:** ListControl not properly connected

**Solution:**
```csharp
// Ensure ListControl is set AFTER creating ListBox
comboBoxBase1.ListControl = listBox1;

// Ensure ListBox has items
if (listBox1.Items.Count == 0)
{
    listBox1.Items.Add("Item 1");
}
```

### Issue: ListBox visible on form

**Cause:** ListBox should be hidden (ComboBoxBase manages visibility)

**Solution:** Don't set ListBox location manually. ComboBoxBase handles popup positioning automatically.

### Issue: DataSource not displaying

**Cause:** DisplayMember not set for custom objects

**Solution:**
```csharp
listBox1.DisplayMember = "PropertyName"; // Name of property to display
// OR override ToString() in your custom class
```

### Issue: Cannot add items after DataSource is set

**Cause:** When DataSource is set, Items collection becomes read-only

**Solution:**
```csharp
// Clear DataSource first
listBox1.DataSource = null;

// Now you can add items manually
listBox1.Items.Add("New Item");
```

## Next Steps

- **ListControl Architecture:** Read [listcontrol-architecture.md](listcontrol-architecture.md) to understand pluggable architecture
- **Event Handling:** Read [event-handling.md](event-handling.md) for selection events
- **Appearance:** Read [appearance-behavior.md](appearance-behavior.md) for styling options
