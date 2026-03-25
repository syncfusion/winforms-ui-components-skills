# Getting Started with NavigationView

This guide covers the initial setup and basic implementation of the NavigationView control in Windows Forms applications.

## Assembly Deployment

Before using NavigationView, you need the required Syncfusion assemblies or NuGet package.

### Required Assemblies

The NavigationView control requires these six assemblies:

1. `Syncfusion.Grid.Base.dll`
2. `Syncfusion.Grid.Windows.dll`
3. `Syncfusion.Shared.Base.dll`
4. `Syncfusion.Shared.Windows.dll`
5. `Syncfusion.Tools.Base.dll`
6. `Syncfusion.Tools.Windows.dll`

### NuGet Installation

Install via NuGet Package Manager:

```
Install-Package Syncfusion.Tools.Windows
```

Or search for "Syncfusion.Tools.WinForms" in the NuGet Package Manager UI.

**Benefits of NuGet:**
- Automatic dependency resolution
- Version management
- Easy updates across projects

## Adding NavigationView to a Form

You can add the NavigationView control using Visual Studio designer or programmatically through code.

### Method 1: Adding via Designer

**Steps:**

1. **Open your Windows Forms project** in Visual Studio
2. **Open the form designer** for the form where you want to add NavigationView
3. **Open the Toolbox** (View → Toolbox or Ctrl+Alt+X)
4. **Locate NavigationView** in the Syncfusion Tools section
5. **Drag and drop** the NavigationView control onto your form
6. **Position and size** the control using the Properties window or designer handles

**Advantages:**
- Visual placement and sizing
- Properties window for easy configuration
- Automatic code generation in `InitializeComponent()`
- Required assemblies are automatically referenced

**Auto-Generated Code:**

When you add via designer, Visual Studio generates initialization code:

```csharp
private void InitializeComponent()
{
    this.navigationView1 = new Syncfusion.Windows.Forms.Tools.NavigationView();
    this.SuspendLayout();
    
    // navigationView1
    this.navigationView1.Location = new System.Drawing.Point(12, 12);
    this.navigationView1.Name = "navigationView1";
    this.navigationView1.Size = new System.Drawing.Size(400, 21);
    this.navigationView1.TabIndex = 0;
    
    // Form1
    this.Controls.Add(this.navigationView1);
    this.ResumeLayout(false);
}
```

### Method 2: Adding via Code

**Steps:**

1. **Add assembly references** to your project (if not using NuGet)
2. **Add using directive** at the top of your code file
3. **Create NavigationView instance** in your form's constructor or initialization method
4. **Configure properties** as needed
5. **Add to form's Controls collection**

**Complete Code Example:**

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms.Tools.Navigation;

namespace NavigationViewDemo
{
    public partial class Form1 : Form
    {
        private NavigationView navigationView1;
        
        public Form1()
        {
            InitializeComponent();
            CreateNavigationView();
        }
        
        private void CreateNavigationView()
        {
            // Create NavigationView instance
            navigationView1 = new NavigationView();
            
            // Set size and position
            navigationView1.Width = 400;
            navigationView1.Height = 25;
            navigationView1.Location = new System.Drawing.Point(20, 20);
            navigationView1.Name = "navigationView1";
            
            // Add to form
            this.Controls.Add(navigationView1);
        }
    }
}
```

**Required Namespace:**

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms.Tools.Navigation; // For Bar class
```

## Creating Your First NavigationView

Here's a complete minimal example that creates a functional breadcrumb navigation.

### Basic Example with Bars

```csharp
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms.Tools.Navigation;

private void CreateBasicNavigationView()
{
    // Create the NavigationView control
    NavigationView navigationView1 = new NavigationView();
    navigationView1.Width = 400;
    navigationView1.Height = 25;
    navigationView1.Location = new System.Drawing.Point(20, 20);
    
    // Create Bar instances
    Bar bar1 = new Bar();
    Bar bar2 = new Bar();
    Bar bar3 = new Bar();
    Bar bar4 = new Bar();
    
    // Set Bar text
    bar1.Text = "MyComputer";
    bar2.Text = "Local Disk (C:)";
    bar3.Text = "Local Disk (D:)";
    bar4.Text = "Local Disk (E:)";
    
    // Add root Bar to NavigationView
    this.navigationView1.Bars.AddRange(new Bar[] { bar1 });
    
    // Add child Bars to root Bar
    bar1.Bars.AddRange(new Bar[] { bar2, bar3, bar4 });
    
    // Set selected Bar (what's displayed)
    this.navigationView1.SelectedBar = bar1;
    
    // Add NavigationView to form
    this.Controls.Add(navigationView1);
}
```

**What this creates:**
- A breadcrumb navigation showing "MyComputer"
- Clicking the arrow next to "MyComputer" reveals a dropdown with three drive options
- Selecting a drive updates the displayed path

### Understanding the Bar Structure

**Bar Basics:**
- A `Bar` represents a node in the navigation hierarchy
- Bars can contain child Bars (nested structure)
- The `SelectedBar` determines what path is displayed
- Root Bars go in `NavigationView.Bars` collection
- Child Bars go in parent `Bar.Bars` collection

**Hierarchy Example:**

```
NavigationView
└── Bars (root collection)
    └── MyComputer (Bar)
        └── Bars (children)
            ├── Local Disk (C:)
            ├── Local Disk (D:)
            └── Local Disk (E:)
```

## Edit Mode

NavigationView supports edit mode, allowing users to type a path directly instead of clicking through the hierarchy.

**Enabling Edit Mode:**

Users can click on the text area of the NavigationView to switch to edit mode. They can then type a valid navigation path to quickly reach a location.

**Usage:**
1. Click on the breadcrumb text area
2. Type the desired path
3. Press Enter to navigate

**Example Typed Path:**
```
MyComputer > Local Disk (C:) > Users > Documents
```

## Complete Working Example

Here's a complete, copy-paste-ready example:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Windows.Forms.Tools.Navigation;

namespace NavigationViewGettingStarted
{
    public partial class Form1 : Form
    {
        private NavigationView navigationView1;
        
        public Form1()
        {
            InitializeComponent();
            SetupNavigationView();
        }
        
        private void SetupNavigationView()
        {
            // Create NavigationView
            navigationView1 = new NavigationView();
            navigationView1.Width = 500;
            navigationView1.Height = 25;
            navigationView1.Location = new Point(20, 20);
            navigationView1.Name = "navigationView1";
            
            // Create root Bar
            Bar rootBar = new Bar { Text = "MyComputer" };
            
            // Create child Bars
            Bar driveC = new Bar { Text = "Local Disk (C:)" };
            Bar driveD = new Bar { Text = "Local Disk (D:)" };
            Bar driveE = new Bar { Text = "Local Disk (E:)" };
            
            // Create nested child
            Bar users = new Bar { Text = "Users" };
            driveC.Bars.Add(users);
            
            // Build hierarchy
            rootBar.Bars.AddRange(new Bar[] { driveC, driveD, driveE });
            navigationView1.Bars.Add(rootBar);
            
            // Set initial selection
            navigationView1.SelectedBar = rootBar;
            
            // Handle selection changes
            navigationView1.BarSelected += NavigationView1_BarSelected;
            
            // Add to form
            this.Controls.Add(navigationView1);
        }
        
        private void NavigationView1_BarSelected(object sender, EventArgs e)
        {
            // Get the selected Bar
            Bar selectedBar = navigationView1.SelectedBar;
            
            // Show selection in title or status bar
            this.Text = $"Current Location: {selectedBar.Text}";
        }
    }
}
```

## Next Steps

Now that you have a basic NavigationView set up, you can:

1. **Add more complex hierarchies** - Read [bar-hierarchy.md](bar-hierarchy.md) for advanced Bar management
2. **Enable history tracking** - Read [dropdown-and-history.md](dropdown-and-history.md) for history features
3. **Add icons to Bars** - Read [images-and-styling.md](images-and-styling.md) for ImageList setup
4. **Customize appearance** - Read [images-and-styling.md](images-and-styling.md) for visual styles
5. **Implement advanced features** - Read [advanced-features.md](advanced-features.md) for edit mode and popup control

## Common Setup Issues

**Issue:** NavigationView namespace not found
- **Solution:** Add `using Syncfusion.Windows.Forms.Tools;` directive
- **Solution:** Add `using Syncfusion.Windows.Forms.Tools.Navigation;` for Bar class

**Issue:** Bar class not found
- **Solution:** Add `using Syncfusion.Windows.Forms.Tools.Navigation;` directive

**Issue:** Assemblies not found
- **Solution:** Install Syncfusion.Tools.WinForms NuGet package
- **Solution:** Manually reference the 6 required DLLs listed at the top of this document

**Issue:** Control not visible on form
- **Solution:** Set appropriate `Width` and `Height` (minimum 20 height recommended)
- **Solution:** Check `Location` property places control within form bounds
- **Solution:** Ensure `SelectedBar` is set to display a Bar
