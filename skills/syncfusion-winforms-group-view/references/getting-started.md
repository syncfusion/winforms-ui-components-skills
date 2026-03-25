# Getting Started with GroupView

This guide covers installation, basic setup, and initial configuration of the Syncfusion GroupView control in Windows Forms applications.

## Assembly Deployment

The GroupView control requires the following assembly reference:
- **Syncfusion.Shared.Base.dll**

### Adding Assembly Reference via NuGet

Install the NuGet package in your Windows Forms project:

```powershell
Install-Package Syncfusion.Shared.Base
```

Or via .NET CLI:

```bash
dotnet add package Syncfusion.Shared.Base
```

### Adding Assembly Reference Manually

1. Right-click on your project in Solution Explorer
2. Select **Add Reference**
3. Browse to Syncfusion installation directory (typically `C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<framework-version>`)
4. Select **Syncfusion.Shared.Base.dll**
5. Click **OK**

### Required Namespace

Include the namespace in your form or code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

## Adding GroupView via Designer

### Step 1: Add Control to Toolbox

If GroupView is not in your Visual Studio toolbox:

1. Open Visual Studio
2. Create or open a Windows Forms project
3. Right-click on the **Toolbox** panel
4. Select **Choose Items**
5. In the **.NET Framework Components** tab, click **Browse**
6. Navigate to Syncfusion assemblies folder
7. Select **Syncfusion.Shared.Base.dll**
8. Click **OK** - GroupView will appear in the toolbox

### Step 2: Drag-and-Drop to Form

1. Open your form in the Designer view
2. Locate **GroupView** in the Toolbox (usually under "Syncfusion Controls" category)
3. Drag **GroupView** onto your form
4. The control will be added with default settings

![GroupView added to form](images-placeholder)

### Step 3: Add Items via Collection Editor

1. Select the GroupView control on the form
2. In the **Properties** window, locate the **GroupViewItems** property
3. Click the ellipsis (…) button to open the **GroupViewItem Collection Editor**
4. Click **Add** to create new items
5. Configure each item's properties:
   - **Text**: Display text for the item
   - **ImageIndex**: Index of image in ImageList (-1 for no image)
   - **Visible**: Whether item is visible (true/false)
   - **ToolTipText**: Tooltip text when hovering over item
   - **Name**: Unique identifier for the item
6. Click **OK** to apply changes

**Example Configuration in Collection Editor:**
```
Item 0:
- Text: "My Computer"
- ImageIndex: 0
- Visible: True
- Name: "itemMyComputer"

Item 1:
- Text: "Network"
- ImageIndex: 1
- Visible: True
- Name: "itemNetwork"

Item 2:
- Text: "Recycle Bin"
- ImageIndex: 2
- Visible: True
- Name: "itemRecycleBin"
```

## Adding GroupView via Code

### Step 1: Create GroupView Instance

Create an instance of the GroupView control in your form class:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;
using System.Windows.Forms;

public partial class MyForm : Form
{
    private GroupView groupView1;
    
    public MyForm()
    {
        InitializeComponent();
        
        // Create GroupView instance
        this.groupView1 = new GroupView();
        
        // Set location and size
        this.groupView1.Location = new Point(10, 10);
        this.groupView1.Size = new Size(200, 400);
        this.groupView1.Name = "groupView1";
        
        // Add to form's control collection
        this.Controls.Add(this.groupView1);
    }
}
```

### Step 2: Add Items to GroupView

Add items to the GroupViewItems collection:

```csharp
// Method 1: Add items one by one
GroupViewItem item1 = new GroupViewItem(
    text: "My Computer",
    imageIndex: 0,
    visible: true,
    toolTipText: null,
    name: "itemMyComputer"
);
this.groupView1.GroupViewItems.Add(item1);

GroupViewItem item2 = new GroupViewItem(
    text: "Network",
    imageIndex: 1,
    visible: true,
    toolTipText: null,
    name: "itemNetwork"
);
this.groupView1.GroupViewItems.Add(item2);
```

Or add multiple items at once using AddRange:

```csharp
// Method 2: Add items using AddRange
this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
    new GroupViewItem("My Computer", 0, true, null, "itemMyComputer"),
    new GroupViewItem("Network", 1, true, null, "itemNetwork"),
    new GroupViewItem("Recycle Bin", 2, true, null, "itemRecycleBin"),
    new GroupViewItem("Control Panel", 3, true, null, "itemControlPanel"),
    new GroupViewItem("Documents", 4, true, null, "itemDocuments")
});
```

### GroupViewItem Constructor Parameters

The GroupViewItem constructor accepts the following parameters:

```csharp
public GroupViewItem(
    string text,          // Display text
    int imageIndex,       // Index in ImageList (-1 for no image)
    bool visible,         // Item visibility
    string toolTipText,   // Tooltip text (null for none)
    string name           // Unique identifier
)
```

## Adding Images to GroupView

GroupView supports both small (16x16) and large (32x32) images via ImageList controls.

### Step 1: Create and Configure ImageList

```csharp
using System.Resources;

public MyForm()
{
    InitializeComponent();
    
    // Create ImageList for small icons
    ImageList imageList1 = new ImageList();
    imageList1.ImageSize = new Size(16, 16); // Small images
    imageList1.ColorDepth = ColorDepth.Depth32Bit;
    
    // Add images from resources
    ResourceManager rm = new ResourceManager("MyApp.Properties.Resources", 
                                            typeof(MyForm).Assembly);
    imageList1.Images.Add((Image)rm.GetObject("computer_icon"));
    imageList1.Images.Add((Image)rm.GetObject("network_icon"));
    imageList1.Images.Add((Image)rm.GetObject("recycle_icon"));
    
    // Or add images from files
    imageList1.Images.Add(Image.FromFile(@"icons\computer.png"));
    imageList1.Images.Add(Image.FromFile(@"icons\network.png"));
    imageList1.Images.Add(Image.FromFile(@"icons\recycle.png"));
    
    // Set key names for images (optional but recommended)
    imageList1.Images.SetKeyName(0, "computer");
    imageList1.Images.SetKeyName(1, "network");
    imageList1.Images.SetKeyName(2, "recycle");
}
```

### Step 2: Assign ImageList to GroupView

```csharp
// Assign ImageList to GroupView
this.groupView1.SmallImageList = imageList1;

// Enable small image view
this.groupView1.SmallImageView = true;
```

### Using Large Images

For larger icons (32x32 or custom size):

```csharp
// Create ImageList for large icons
ImageList largeImageList = new ImageList();
largeImageList.ImageSize = new Size(32, 32);
largeImageList.ColorDepth = ColorDepth.Depth32Bit;

// Add large images
largeImageList.Images.Add(Image.FromFile(@"icons\computer_large.png"));
largeImageList.Images.Add(Image.FromFile(@"icons\network_large.png"));
largeImageList.Images.Add(Image.FromFile(@"icons\recycle_large.png"));

// Assign to GroupView
this.groupView1.LargeImageList = largeImageList;

// To use large images, set SmallImageView to false
this.groupView1.SmallImageView = false;
```

### Complete Example with Images

```csharp
public partial class MyForm : Form
{
    private GroupView groupView1;
    private ImageList imageList1;
    
    public MyForm()
    {
        InitializeComponent();
        
        // Create and setup ImageList
        this.imageList1 = new ImageList();
        this.imageList1.ImageSize = new Size(16, 16);
        this.imageList1.Images.Add(Image.FromFile(@"icons\computer.png"));
        this.imageList1.Images.Add(Image.FromFile(@"icons\network.png"));
        this.imageList1.Images.Add(Image.FromFile(@"icons\recycle.png"));
        
        // Create GroupView
        this.groupView1 = new GroupView();
        this.groupView1.Location = new Point(10, 10);
        this.groupView1.Size = new Size(200, 300);
        this.groupView1.FlatLook = true;
        
        // Assign ImageList
        this.groupView1.SmallImageList = this.imageList1;
        this.groupView1.SmallImageView = true;
        
        // Add items with image indices
        this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
            new GroupViewItem("My Computer", 0, true, null, "item0"),
            new GroupViewItem("Network", 1, true, null, "item1"),
            new GroupViewItem("Recycle Bin", 2, true, null, "item2")
        });
        
        // Add to form
        this.Controls.Add(this.groupView1);
    }
}
```

## Setting Selected Item

You can programmatically select an item at runtime using the SelectedItem property:

```csharp
// Select item by index (0-based)
this.groupView1.SelectedItem = 1; // Selects the second item

// Get currently selected item
int currentSelection = this.groupView1.SelectedItem;
if (currentSelection != -1)
{
    string selectedText = this.groupView1.GroupViewItems[currentSelection].Text;
    MessageBox.Show($"Selected item: {selectedText}");
}
```

### Selection at Design-Time

You can also set the initial selected item at design time:

1. Select the GroupView control
2. In Properties window, find **SelectedItem** property
3. Enter the index of the item to select (0 for first item, 1 for second, etc.)
4. Enter -1 for no selection

## Basic Configuration

### Enable Flat Look

Remove 3D borders for a modern appearance:

```csharp
this.groupView1.FlatLook = true;
```

### Set Border Style

Configure the control's border:

```csharp
this.groupView1.BorderStyle = BorderStyle.FixedSingle;
// Options: None, FixedSingle, Fixed3D
```

### Enable Scrolling

For lists with many items:

```csharp
this.groupView1.IntegratedScrolling = true;
```

## Complete Working Example

Here's a complete, runnable example that combines all concepts:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace GroupViewDemo
{
    public partial class MainForm : Form
    {
        private GroupView groupView1;
        private ImageList imageList1;
        
        public MainForm()
        {
            InitializeComponent();
            SetupGroupView();
        }
        
        private void SetupGroupView()
        {
            // Create ImageList
            this.imageList1 = new ImageList();
            this.imageList1.ImageSize = new Size(16, 16);
            
            // Add images (ensure these files exist in your project)
            try
            {
                this.imageList1.Images.Add(Image.FromFile(@"icons\folder.png"));
                this.imageList1.Images.Add(Image.FromFile(@"icons\document.png"));
                this.imageList1.Images.Add(Image.FromFile(@"icons\settings.png"));
            }
            catch
            {
                // If images not found, continue without images
                MessageBox.Show("Image files not found. Continuing without icons.");
            }
            
            // Create GroupView
            this.groupView1 = new GroupView();
            this.groupView1.Location = new Point(20, 20);
            this.groupView1.Size = new Size(250, 400);
            this.groupView1.FlatLook = true;
            this.groupView1.BorderStyle = BorderStyle.FixedSingle;
            this.groupView1.IntegratedScrolling = true;
            
            // Assign ImageList if images were loaded
            if (this.imageList1.Images.Count > 0)
            {
                this.groupView1.SmallImageList = this.imageList1;
                this.groupView1.SmallImageView = true;
            }
            
            // Add items
            this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
                new GroupViewItem("My Documents", 0, true, "Access your documents", "item1"),
                new GroupViewItem("Recent Files", 1, true, "View recently opened files", "item2"),
                new GroupViewItem("Settings", 2, true, "Application settings", "item3")
            });
            
            // Set initial selection
            this.groupView1.SelectedItem = 0;
            
            // Subscribe to selection event
            this.groupView1.GroupViewItemSelected += GroupView1_ItemSelected;
            
            // Add to form
            this.Controls.Add(this.groupView1);
        }
        
        private void GroupView1_ItemSelected(object sender, EventArgs e)
        {
            int index = this.groupView1.SelectedItem;
            if (index >= 0 && index < this.groupView1.GroupViewItems.Count)
            {
                string itemText = this.groupView1.GroupViewItems[index].Text;
                this.Text = $"GroupView Demo - Selected: {itemText}";
            }
        }
    }
}
```

## Next Steps

After setting up your GroupView control, explore:
- **Control Settings**: Configure appearance, behavior, spacing, and orientation
- **Item Customization**: Customize text, colors, and image properties
- **Interactive Features**: Add tooltips, context menus, and button views
- **Events**: Handle selection, highlighting, and renaming events
- **GroupBar Integration**: Add GroupView as a client control to GroupBar
