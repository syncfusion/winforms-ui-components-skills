# Getting Started with TreeNavigator

This guide covers installation, setup, and basic usage of the Syncfusion WinForms TreeNavigator control.

## Assembly Deployment

The TreeNavigator control requires the following assemblies:

**Primary Assembly:**
- **Syncfusion.Tools.Windows** - Contains TreeNavigator control and UI operations

**Dependent Assembly:**
- **Syncfusion.Shared.Base** - Contains style-related properties and base classes

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms;
```

```vbnet
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Windows.Forms
```

**Assembly Details:**

| Assembly | Description |
|----------|-------------|
| **Syncfusion.Tools.Windows** | Contains TreeNavigator control classes, UI operations, and fundamentals |
| **Syncfusion.Shared.Base** | Provides style-related properties and supporting controls |

---

## NuGet Installation

The TreeNavigator control is available through NuGet packages.

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**Package Dependencies:**
- Syncfusion.Tools.Windows (includes Syncfusion.Shared.Base)

---

## Adding TreeNavigator via Designer

The easiest way to add TreeNavigator to your form is through the Visual Studio designer.

**Steps:**

1. **Create New Project**: Create a Windows Forms Application project in Visual Studio

2. **Open Toolbox**: View → Toolbox (or Ctrl+Alt+X)

3. **Locate Control**: Find "TreeNavigator" in the Syncfusion Controls section

4. **Drag and Drop**: Drag the TreeNavigator control onto your form designer

5. **Verify References**: After dropping the control, Visual Studio automatically adds the required assemblies

**Result:** The TreeNavigator appears on your form with default settings.

---

## Adding TreeNavigator Programmatically

You can add TreeNavigator through code for dynamic scenarios or non-designer workflows.

**C# Example:**

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms;

public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Create TreeNavigator instance
        TreeNavigator treeNavigator = new TreeNavigator();
        treeNavigator.Location = new Point(20, 20);
        treeNavigator.Size = new Size(250, 400);
        
        // Add to form
        this.Controls.Add(treeNavigator);
    }
}
```

**VB.NET Example:**

```vbnet
Imports Syncfusion.Windows.Forms.Tools
Imports Syncfusion.Windows.Forms

Public Class Form1
    Public Sub New()
        InitializeComponent()
        
        ' Create TreeNavigator instance
        Dim treeNavigator As New TreeNavigator()
        treeNavigator.Location = New Point(20, 20)
        treeNavigator.Size = New Size(250, 400)
        
        ' Add to form
        Me.Controls.Add(treeNavigator)
    End Sub
End Class
```

---

## Configuring the Header

The TreeNavigator header displays the title and provides context for the current navigation level.

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();

// Set header text
treeNavigator.Header.HeaderText = "This PC";

// Optional: Customize header appearance
treeNavigator.Header.Height = 50;
treeNavigator.Header.HeaderBackColor = Color.FromArgb(0, 120, 215);
treeNavigator.Header.HeaderForeColor = Color.White;
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()

' Set header text
treeNavigator.Header.HeaderText = "This PC"

' Optional: Customize header appearance
treeNavigator.Header.Height = 50
treeNavigator.Header.HeaderBackColor = Color.FromArgb(0, 120, 215)
treeNavigator.Header.HeaderForeColor = Color.White
```

---

## Adding TreeMenuItem Items (Programmatically)

TreeNavigator uses TreeMenuItem objects to represent navigation items.

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();
treeNavigator.Header.HeaderText = "This PC";

// Create root-level items
TreeMenuItem desktop = new TreeMenuItem();
desktop.Text = "Desktop";

TreeMenuItem documents = new TreeMenuItem();
documents.Text = "Documents";

TreeMenuItem downloads = new TreeMenuItem();
downloads.Text = "Downloads";

TreeMenuItem pictures = new TreeMenuItem();
pictures.Text = "Pictures";

// Add items to TreeNavigator
treeNavigator.Items.Add(desktop);
treeNavigator.Items.Add(documents);
treeNavigator.Items.Add(downloads);
treeNavigator.Items.Add(pictures);
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()
treeNavigator.Header.HeaderText = "This PC"

' Create root-level items
Dim desktop As New TreeMenuItem()
desktop.Text = "Desktop"

Dim documents As New TreeMenuItem()
documents.Text = "Documents"

Dim downloads As New TreeMenuItem()
downloads.Text = "Downloads"

Dim pictures As New TreeMenuItem()
pictures.Text = "Pictures"

' Add items to TreeNavigator
treeNavigator.Items.Add(desktop)
treeNavigator.Items.Add(documents)
treeNavigator.Items.Add(downloads)
treeNavigator.Items.Add(pictures)
```

---

## Adding Nested (Child) Items

TreeMenuItem objects can contain child items, creating hierarchical navigation.

**C# Example:**

```csharp
TreeNavigator treeNavigator = new TreeNavigator();
treeNavigator.Header.HeaderText = "Files";

// Create parent item
TreeMenuItem documents = new TreeMenuItem { Text = "Documents" };

// Create child items
TreeMenuItem workFiles = new TreeMenuItem { Text = "Work Files" };
TreeMenuItem personalFiles = new TreeMenuItem { Text = "Personal Files" };
TreeMenuItem projects = new TreeMenuItem { Text = "Projects" };

// Add parent to TreeNavigator
treeNavigator.Items.Add(documents);

// Add child items to parent
documents.Items.Add(workFiles);
documents.Items.Add(personalFiles);
documents.Items.Add(projects);

// Add sub-items to Work Files
workFiles.Items.Add(new TreeMenuItem { Text = "Reports" });
workFiles.Items.Add(new TreeMenuItem { Text = "Spreadsheets" });
```

**VB.NET Example:**

```vbnet
Dim treeNavigator As New TreeNavigator()
treeNavigator.Header.HeaderText = "Files"

' Create parent item
Dim documents As New TreeMenuItem With {.Text = "Documents"}

' Create child items
Dim workFiles As New TreeMenuItem With {.Text = "Work Files"}
Dim personalFiles As New TreeMenuItem With {.Text = "Personal Files"}
Dim projects As New TreeMenuItem With {.Text = "Projects"}

' Add parent to TreeNavigator
treeNavigator.Items.Add(documents)

' Add child items to parent
documents.Items.Add(workFiles)
documents.Items.Add(personalFiles)
documents.Items.Add(projects)

' Add sub-items to Work Files
workFiles.Items.Add(New TreeMenuItem With {.Text = "Reports"})
workFiles.Items.Add(New TreeMenuItem With {.Text = "Spreadsheets"})
```

---

## Adding Items Through Designer

You can configure TreeNavigator items visually using the Visual Studio designer.

**Steps:**

1. **Select Control**: Click the TreeNavigator control on the form designer

2. **Open Smart Tag**: Click the small arrow icon in the upper-right corner of the control

3. **Edit Items**: In the Smart Tag menu, click on the "Items" property

4. **Collection Editor**: The TreeMenuItem Collection Editor opens

5. **Add Items**: 
   - Click "Add" button to create new TreeMenuItem
   - Set the "Text" property in the right panel
   - Repeat for each item

6. **Add Child Items** (for nested navigation):
   - Select a parent TreeMenuItem in the left panel
   - Find the "Items" property in the right panel
   - Click the ellipsis (...) button to open the child items editor
   - Add child items using the same process

7. **Click OK**: Close the editor to apply changes

**Note:** The designer provides a visual way to build your navigation structure without writing code.

---

## .NET Core Designer Workaround

**Known Issue:** In .NET Core projects, when adding child items to a TreeMenuItem directly from the Visual Studio Properties window, the default Collection Editor opens instead of the expected TreeMenuItem editor.

**Workaround:**
1. Use the main TreeNavigator Collection Editor (accessed via Smart Tag or Items property)
2. Add all items at the TreeNavigator level first
3. Then configure child items for each TreeMenuItem as needed

**Permanent Fix:** This issue is being addressed in a future release.

---

## Complete Getting Started Example

Here's a complete example combining all the basics:

**C# Complete Example:**

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace TreeNavigatorDemo
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            SetupTreeNavigator();
        }

        private void SetupTreeNavigator()
        {
            // Create TreeNavigator
            TreeNavigator treeNavigator = new TreeNavigator();
            treeNavigator.Location = new Point(20, 20);
            treeNavigator.Size = new Size(280, 450);
            
            // Configure header
            treeNavigator.Header.HeaderText = "My Computer";
            treeNavigator.Header.Height = 45;
            
            // Set visual style
            treeNavigator.Style = TreeNavigatorStyle.Office2016Colorful;
            
            // Create root items
            TreeMenuItem desktop = new TreeMenuItem { Text = "Desktop" };
            TreeMenuItem documents = new TreeMenuItem { Text = "Documents" };
            TreeMenuItem downloads = new TreeMenuItem { Text = "Downloads" };
            TreeMenuItem pictures = new TreeMenuItem { Text = "Pictures" };
            TreeMenuItem music = new TreeMenuItem { Text = "Music" };
            
            // Add all root items to TreeNavigator
            treeNavigator.Items.Add(desktop);
            treeNavigator.Items.Add(documents);
            treeNavigator.Items.Add(downloads);
            treeNavigator.Items.Add(pictures);
            treeNavigator.Items.Add(music);

            // Add child items to Documents
            documents.Items.Add(new TreeMenuItem { Text = "Work" });
            documents.Items.Add(new TreeMenuItem { Text = "Personal" });
            documents.Items.Add(new TreeMenuItem { Text = "Projects" });
            
            // Add child items to Pictures
            pictures.Items.Add(new TreeMenuItem { Text = "Vacation 2024" });
            pictures.Items.Add(new TreeMenuItem { Text = "Family" });
            pictures.Items.Add(new TreeMenuItem { Text = "Screenshots" });
            
            // Add to form
            this.Controls.Add(treeNavigator);
        }
    }
}
```

**VB.NET Complete Example:**

```vbnet
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Namespace TreeNavigatorDemo
    Public Partial Class Form1
        Inherits Form
        
        Public Sub New()
            InitializeComponent()
            SetupTreeNavigator()
        End Sub

        Private Sub SetupTreeNavigator()
            ' Create TreeNavigator
            Dim treeNavigator As New TreeNavigator()
            treeNavigator.Location = New Point(20, 20)
            treeNavigator.Size = New Size(280, 450)
            
            ' Configure header
            treeNavigator.Header.HeaderText = "My Computer"
            treeNavigator.Header.Height = 45
            
            ' Set visual style
            treeNavigator.Style = TreeNavigatorStyle.Office2016Colorful
            
            ' Create root items
            Dim desktop As New TreeMenuItem With {.Text = "Desktop"}
            Dim documents As New TreeMenuItem With {.Text = "Documents"}
            Dim downloads As New TreeMenuItem With {.Text = "Downloads"}
            Dim pictures As New TreeMenuItem With {.Text = "Pictures"}
            Dim music As New TreeMenuItem With {.Text = "Music"}
            
            ' Add all root items to TreeNavigator
            treeNavigator.Items.Add(desktop)
            treeNavigator.Items.Add(documents)
            treeNavigator.Items.Add(downloads)
            treeNavigator.Items.Add(pictures)
            treeNavigator.Items.Add(music)
            
            ' Add child items to Documents
            documents.Items.Add(New TreeMenuItem With {.Text = "Work"})
            documents.Items.Add(New TreeMenuItem With {.Text = "Personal"})
            documents.Items.Add(New TreeMenuItem With {.Text = "Projects"})
            
            ' Add child items to Pictures
            pictures.Items.Add(New TreeMenuItem With {.Text = "Vacation 2024"})
            pictures.Items.Add(New TreeMenuItem With {.Text = "Family"})
            pictures.Items.Add(New TreeMenuItem With {.Text = "Screenshots"})
            
            ' Add to form
            Me.Controls.Add(treeNavigator)
        End Sub
    End Class
End Namespace
```

---

## Troubleshooting

### TreeNavigator Not in Toolbox

**Problem:** TreeNavigator doesn't appear in the Visual Studio toolbox.

**Solution:**
1. Right-click the toolbox → Choose Items
2. Browse to Syncfusion.Tools.Windows.dll
3. Check the TreeNavigator checkbox
4. Click OK

### Assembly Not Found Error

**Problem:** Build error: "Could not load file or assembly 'Syncfusion.Tools.Windows'"

**Solution:**
1. Verify NuGet package is installed: `Install-Package Syncfusion.Tools.Windows`
2. Check project references include both Syncfusion.Tools.Windows and Syncfusion.Shared.Base
3. Ensure package version matches other Syncfusion packages in your project

### Items Not Displaying

**Problem:** Added items don't appear in the TreeNavigator.

**Solution:**
1. Verify items are added to the `Items` collection: `treeNavigator.Items.Add(item)`
2. Ensure TreeNavigator has sufficient size: Set `Size` property with adequate width/height
3. Check if header is consuming too much space: Adjust `Header.Height` if needed
4. Verify control is added to form: `this.Controls.Add(treeNavigator)`

---

## Next Steps

- **Navigation Modes**: Learn about Default vs Extended navigation modes
- **Appearance**: Customize visual styles, colors, and themes
- **Selection Events**: Handle user navigation with selection events
- **TreeMenuItem Management**: Advanced item manipulation and hierarchies
