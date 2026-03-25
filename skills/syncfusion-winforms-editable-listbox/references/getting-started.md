# Getting Started with EditableList

Complete guide for installing, setting up, and creating your first EditableList control in Windows Forms applications.

## Installation and Assembly Deployment

### Required Assemblies

The EditableList control requires the following assembly:

- **Syncfusion.Shared.Base.dll** - Core functionality for Syncfusion Windows Forms controls

### Installation via NuGet

**Using Package Manager Console:**

```powershell
Install-Package Syncfusion.Shared.Base
```

**Using NuGet Package Manager UI:**

1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Shared.Base"
4. Click "Install" button
5. Accept the license agreement

**Using .NET CLI:**

```bash
dotnet add package Syncfusion.Shared.Base
```

### Manual Assembly Reference

If not using NuGet:

1. Download Syncfusion Essential Studio for Windows Forms
2. Install the suite
3. Right-click "References" in your project
4. Select "Add Reference"
5. Browse to installation directory (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\{version}\Assemblies\`)
6. Add **Syncfusion.Shared.Base.dll**

## Required Namespaces

Add these namespace imports to your form code:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;
```

**VB.NET:**
```vbnet
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing
Imports System.Windows.Forms
```

## Adding EditableList via Designer

The designer approach is recommended for most scenarios as it provides visual feedback and automatic code generation.

### Step-by-Step Designer Setup

1. **Open Form in Designer:**
   - Double-click your form (e.g., Form1.cs) in Solution Explorer
   - Ensure you're in Design view

2. **Locate Control in Toolbox:**
   - Open the Toolbox panel (View → Toolbox or Ctrl+Alt+X)
   - Expand "Syncfusion Controls for Windows Forms" section
   - Find "EditableList" control

3. **Add to Form:**
   - Drag EditableList from toolbox
   - Drop it onto your form
   - Position and resize as needed

4. **Automatic Assembly Addition:**
   - Visual Studio automatically adds required assembly references
   - Check References in Solution Explorer to verify Syncfusion.Shared.Base is added

5. **Configure Properties:**
   - Select the EditableList control on the form
   - Open Properties window (F4)
   - Configure properties like Size, Location, Style, etc.

### Adding Items via Designer

**Using Property Editor:**

1. Select EditableList control on form
2. In Properties window, expand **ListBox** property
3. Locate **Items** property in the ListBox section
4. Click the ellipsis button (...)
5. String Collection Editor opens
6. Enter items (one per line)
7. Click OK

**Example:**
```
Product A
Product B
Product C
```

## Adding EditableList via Code

For dynamic control creation or when you need programmatic setup.

### Basic Code Setup

**C# Example:**

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

namespace EditableListDemo
{
    public partial class Form1 : Form
    {
        // Declare control as private field
        private EditableList editableList1;
        
        public Form1()
        {
            InitializeComponent();
            CreateEditableList();
        }
        
        private void CreateEditableList()
        {
            // Initialize the control
            this.editableList1 = new EditableList();
            
            // Set location (X, Y coordinates)
            this.editableList1.Location = new Point(20, 20);
            
            // Set size (Width, Height)
            this.editableList1.Size = new Size(300, 200);
            
            // Set name for identification
            this.editableList1.Name = "editableList1";
            
            // Add items to the list
            this.editableList1.ListBox.Items.AddRange(new object[] {
                "Item 1",
                "Item 2",
                "Item 3",
                "Item 4"
            });
            
            // Add control to form's controls collection
            this.Controls.Add(this.editableList1);
        }
    }
}
```

**VB.NET Example:**

```vbnet
Imports Syncfusion.Windows.Forms.Tools
Imports System.Drawing
Imports System.Windows.Forms

Public Class Form1
    ' Declare control as private field
    Private editableList1 As EditableList
    
    Public Sub New()
        InitializeComponent()
        CreateEditableList()
    End Sub
    
    Private Sub CreateEditableList()
        ' Initialize the control
        Me.editableList1 = New EditableList()
        
        ' Set location (X, Y coordinates)
        Me.editableList1.Location = New Point(20, 20)
        
        ' Set size (Width, Height)
        Me.editableList1.Size = New Size(300, 200)
        
        ' Set name for identification
        Me.editableList1.Name = "editableList1"
        
        ' Add items to the list
        Me.editableList1.ListBox.Items.AddRange(New Object() {
            "Item 1",
            "Item 2",
            "Item 3",
            "Item 4"
        })
        
        ' Add control to form's controls collection
        Me.Controls.Add(Me.editableList1)
    End Sub
End Class
```

## Understanding Embedded Controls

EditableList is a composite control containing three embedded controls that you can access and configure:

### 1. ListBox Property

The main list display component.

```csharp
// Access ListBox
var listBox = this.editableList1.ListBox;

// Add items
listBox.Items.Add("New Item");

// Get selected item
var selected = listBox.SelectedItem;

// Handle selection event
listBox.SelectedIndexChanged += (s, e) => {
    Console.WriteLine($"Selected: {listBox.SelectedItem}");
};
```

### 2. TextBox Property

Used for inline editing of list items.

```csharp
// Access TextBox
var textBox = this.editableList1.TextBox;

// Set properties
textBox.MaxLength = 50;
textBox.Font = new Font("Arial", 10F);

// Handle text changes
textBox.TextChanged += (s, e) => {
    Console.WriteLine($"Editing: {textBox.Text}");
};
```

### 3. Button Property

Optional button shown during editing (controlled by WantButton property).

```csharp
// Access Button
var button = this.editableList1.Button;

// Configure button
button.Text = "Clear";
button.Width = 60;

// Handle click event
button.Click += (s, e) => {
    this.editableList1.TextBox.Clear();
};
```

## Complete Minimal Example

Here's a complete, minimal example to get started quickly:

**C#:**

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace EditableListQuickStart
{
    public partial class Form1 : Form
    {
        private EditableList editableList1;
        
        public Form1()
        {
            InitializeComponent();
            
            // Create EditableList
            this.editableList1 = new EditableList();
            this.editableList1.Location = new System.Drawing.Point(30, 30);
            this.editableList1.Size = new System.Drawing.Size(250, 180);
            
            // Add sample items
            this.editableList1.ListBox.Items.Add("Apple");
            this.editableList1.ListBox.Items.Add("Banana");
            this.editableList1.ListBox.Items.Add("Cherry");
            
            // Add to form
            this.Controls.Add(this.editableList1);
        }
    }
}
```

**VB.NET:**

```vbnet
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class Form1
    Private editableList1 As EditableList
    
    Public Sub New()
        InitializeComponent()
        
        ' Create EditableList
        Me.editableList1 = New EditableList()
        Me.editableList1.Location = New System.Drawing.Point(30, 30)
        Me.editableList1.Size = New System.Drawing.Size(250, 180)
        
        ' Add sample items
        Me.editableList1.ListBox.Items.Add("Apple")
        Me.editableList1.ListBox.Items.Add("Banana")
        Me.editableList1.ListBox.Items.Add("Cherry")
        
        ' Add to form
        Me.Controls.Add(Me.editableList1)
    End Sub
End Class
```

## Verifying Installation

To verify EditableList is properly installed and working:

1. **Build your project** (F6 or Build → Build Solution)
2. **Run the application** (F5 or Debug → Start Debugging)
3. **Test basic functionality:**
   - You should see the list with items
   - Click on an item to select it
   - Click again to enter edit mode
   - Edit the text and press Tab or click elsewhere
   - The item should update with your changes

## Common Setup Issues

### Issue: "Type or namespace 'Syncfusion' could not be found"
**Solution:** Ensure Syncfusion.Shared.Base assembly is referenced in your project. Check References in Solution Explorer.

### Issue: EditableList not in Toolbox
**Solution:** 
1. Right-click Toolbox → "Choose Items"
2. Browse to Syncfusion.Shared.Base.dll
3. Check EditableList
4. Click OK

### Issue: Runtime error "Could not load file or assembly 'Syncfusion.Shared.Base'"
**Solution:** Ensure the DLL is in your output directory or install via NuGet which handles deployment automatically.

### Issue: Items not visible after adding
**Solution:** Ensure you've set an appropriate Size for the control. Minimum recommended height is 100 pixels.

## Next Steps

Now that you have EditableList set up, explore:

- **Data Binding**: Populate from collections and databases
- **Appearance Customization**: AutoScroll, dock padding, button visibility
- **AutoComplete Integration**: Add intelligent text suggestions
- **Styling**: Apply modern themes (Metro, Office2016)
- **Event Handling**: Respond to user interactions

Refer to other reference files for detailed guidance on these topics.
