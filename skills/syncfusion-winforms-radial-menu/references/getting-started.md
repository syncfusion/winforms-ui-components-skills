# Getting Started with RadialMenu

This guide walks you through setting up the Syncfusion WinForms RadialMenu control in your Windows Forms application, from installation to creating your first functional radial menu.

## Assembly Dependencies

Before using the RadialMenu control, you need to add references to the following Syncfusion assemblies:

**Required DLL References:**
- **Syncfusion.Grid.Base.dll** - Base grid functionality
- **Syncfusion.Grid.Windows.dll** - Windows Forms grid components
- **Syncfusion.Shared.Base.dll** - Shared base utilities
- **Syncfusion.Shared.Windows.dll** - Windows Forms shared components
- **Syncfusion.Tools.Base.dll** - Tools base functionality
- **Syncfusion.Tools.Windows.dll** - Windows Forms tools (contains RadialMenu)

These assemblies can be referenced either by adding the DLLs directly or by installing the NuGet package.

## NuGet Package Installation

The easiest way to add the RadialMenu control is through NuGet Package Manager:

**Steps to Install:**
1. Right-click your project in Solution Explorer
2. Select "Manage NuGet Packages"
3. Search for "Syncfusion.Windows.Forms.Tools"
4. Click "Install" on the package

```xml
<!-- Package Manager Console Command -->
Install-Package Syncfusion.Windows.Forms.Tools
```

**Important Licensing Note:**
Starting with v16.2.0.x, if you reference Syncfusion assemblies from trial setup or NuGet feed, you must include a license key in your projects. Register your license key in your application to use Syncfusion components. Learn more about [Syncfusion licensing](https://help.syncfusion.com/common/essential-studio/licensing/overview).

## Required Namespace

After adding the assembly references, include the following namespace in your form code:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

## Creating RadialMenu via Designer

The Designer approach is the quickest way to add a RadialMenu to your form.

**Step 1: Add Control from Toolbox**

1. Open your form in Design View
2. Locate "RadialMenu" in the Toolbox under Syncfusion Controls
3. Drag and drop the RadialMenu onto your form

When you add the control via designer, Visual Studio automatically adds all required assembly references.

![RadialMenu in Designer](../../../../../docs/Getting-Started_images/RadialMenu-img1.png)

**Step 2: Configure Basic Properties**

After adding the control, configure these essential properties in the Properties window:

```csharp
// Set these properties in the Properties window
this.radialMenu1.Visible = true;  // Make menu visible initially
this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;  // Apply modern theme
this.radialMenu1.Size = new Size(280, 280);  // Set menu size
this.radialMenu1.Location = new Point(100, 100);  // Position on form
```

**Step 3: Add Menu Items Using Smart Tags**

1. Click the small arrow (Smart Tag) in the top-right corner of the RadialMenu
2. Select "Edit Items" from the Smart Tag menu
3. The RadialMenuItem Collection Editor opens
4. Click "Add" to create new menu items
5. Configure each item's properties (Text, ImageIndex, etc.)
6. Click "OK" to save

![Adding Menu Items via Smart Tags](../../../../../docs/Getting-Started_images/RadialMenu-img3.png)

**Important Note for .NET Core:**
In .NET Core, when adding child items to a RadialMenuItem directly from the Visual Studio Properties window, the default Collection Editor may open instead of the expected editor. As a workaround, use the main RadialMenu Collection Editor to add items, then configure child items as needed. A permanent fix is in progress.

## Creating RadialMenu Manually in Code

For more control and dynamic scenarios, create the RadialMenu programmatically.

**Complete Example: Basic RadialMenu Setup**

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace RadialMenuDemo
{
    public partial class Form1 : Form
    {
        private RadialMenu radialMenu1;

        public Form1()
        {
            InitializeComponent();
            InitializeRadialMenu();
        }

        private void InitializeRadialMenu()
        {
            // Create RadialMenu instance
            this.radialMenu1 = new RadialMenu();

            // Configure basic properties
            this.radialMenu1.Visible = true;  // IMPORTANT: Must be true to display
            this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
            this.radialMenu1.Size = new Size(280, 280);
            this.radialMenu1.Location = new Point(100, 100);
            this.radialMenu1.MenuVisibility = true;  // Show menu on load

            // Add to form controls
            this.Controls.Add(this.radialMenu1);
        }
    }
}
```

**Result:**
This creates an empty radial menu with modern Office 2016 Colorful styling, visible at application startup.

## Adding Menu Items via Code

Once the RadialMenu is created, populate it with menu items using the Items collection.

**Example 1: Adding Simple Text Items**

```csharp
private void AddMenuItems()
{
    // Create menu item instances
    RadialMenuItem editItem = new RadialMenuItem();
    RadialMenuItem cutItem = new RadialMenuItem();
    RadialMenuItem copyItem = new RadialMenuItem();
    RadialMenuItem pasteItem = new RadialMenuItem();

    // Configure item properties
    editItem.Text = "Edit";
    cutItem.Text = "Cut";
    copyItem.Text = "Copy";
    pasteItem.Text = "Paste";

    // Add items to RadialMenu
    this.radialMenu1.Items.Add(editItem);
    this.radialMenu1.Items.Add(cutItem);
    this.radialMenu1.Items.Add(copyItem);
    this.radialMenu1.Items.Add(pasteItem);
}
```

**Result:**
Four menu items appear in the radial menu with labels "Edit", "Cut", "Copy", and "Paste".

**Example 2: Adding Items with Images**

```csharp
private void AddMenuItemsWithIcons()
{
    // First, create and configure an ImageListAdv
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Image.FromFile("icons/edit.png"));
    imageList.Images.Add(Image.FromFile("icons/cut.png"));
    imageList.Images.Add(Image.FromFile("icons/copy.png"));
    imageList.Images.Add(Image.FromFile("icons/paste.png"));

    // Attach ImageList to RadialMenu
    this.radialMenu1.ImageList = imageList;

    // Create items with image indices
    RadialMenuItem editItem = new RadialMenuItem();
    editItem.Text = "Edit";
    editItem.ImageIndex = 0;  // First image

    RadialMenuItem cutItem = new RadialMenuItem();
    cutItem.Text = "Cut";
    cutItem.ImageIndex = 1;  // Second image

    RadialMenuItem copyItem = new RadialMenuItem();
    copyItem.Text = "Copy";
    copyItem.ImageIndex = 2;  // Third image

    RadialMenuItem pasteItem = new RadialMenuItem();
    pasteItem.Text = "Paste";
    pasteItem.ImageIndex = 3;  // Fourth image

    // Add items to menu
    this.radialMenu1.Items.Add(editItem);
    this.radialMenu1.Items.Add(cutItem);
    this.radialMenu1.Items.Add(copyItem);
    this.radialMenu1.Items.Add(pasteItem);

    // Optional: Set display style to show both text and images
    this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
}
```

**Result:**
Four menu items with both icons and text labels, displayed with images above text.

## Complete Minimal Working Example

Here's a complete, ready-to-run example that demonstrates a functional RadialMenu with event handling:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace RadialMenuGetStarted
{
    public partial class MainForm : Form
    {
        private RadialMenu radialMenu;
        private TextBox outputTextBox;

        public MainForm()
        {
            InitializeComponent();
            SetupUI();
            CreateRadialMenu();
        }

        private void SetupUI()
        {
            // Create output textbox to show results
            this.outputTextBox = new TextBox();
            this.outputTextBox.Multiline = true;
            this.outputTextBox.ScrollBars = ScrollBars.Vertical;
            this.outputTextBox.Dock = DockStyle.Bottom;
            this.outputTextBox.Height = 100;
            this.outputTextBox.ReadOnly = true;
            this.Controls.Add(this.outputTextBox);
        }

        private void CreateRadialMenu()
        {
            // Initialize RadialMenu
            this.radialMenu = new RadialMenu();
            this.radialMenu.Visible = true;
            this.radialMenu.MenuVisibility = true;
            this.radialMenu.Style = RadialMenuStyle.Office2016Colorful;
            this.radialMenu.Size = new Size(300, 300);
            this.radialMenu.Location = new Point(
                (this.ClientSize.Width - 300) / 2,
                (this.ClientSize.Height - 300) / 2
            );

            // Create menu items
            RadialMenuItem newItem = new RadialMenuItem();
            newItem.Text = "New";
            newItem.Click += MenuItem_Click;

            RadialMenuItem openItem = new RadialMenuItem();
            openItem.Text = "Open";
            openItem.Click += MenuItem_Click;

            RadialMenuItem saveItem = new RadialMenuItem();
            saveItem.Text = "Save";
            saveItem.Click += MenuItem_Click;

            RadialMenuItem closeItem = new RadialMenuItem();
            closeItem.Text = "Close";
            closeItem.Click += MenuItem_Click;

            // Add items to menu
            this.radialMenu.Items.Add(newItem);
            this.radialMenu.Items.Add(openItem);
            this.radialMenu.Items.Add(saveItem);
            this.radialMenu.Items.Add(closeItem);

            // Add menu to form
            this.Controls.Add(this.radialMenu);
        }

        private void MenuItem_Click(object sender, EventArgs e)
        {
            RadialMenuItem clickedItem = sender as RadialMenuItem;
            if (clickedItem != null)
            {
                string message = $"[{DateTime.Now:HH:mm:ss}] Clicked: {clickedItem.Text}";
                this.outputTextBox.AppendText(message + Environment.NewLine);
            }
        }
    }
}
```

**Result:**
- A centered radial menu with four items (New, Open, Save, Close)
- When you click any menu item, the action is logged in the textbox
- The menu uses modern Office 2016 Colorful styling

## Basic Configuration Properties

After creating your RadialMenu, configure these essential properties:

**Visibility Properties:**
```csharp
// Make menu visible immediately
this.radialMenu1.Visible = true;

// Show menu content on form load (otherwise just the center icon shows)
this.radialMenu1.MenuVisibility = true;
```

**Size and Position:**
```csharp
// Set menu dimensions (typically square for circular appearance)
this.radialMenu1.Size = new Size(280, 280);

// Position the menu on the form
this.radialMenu1.Location = new Point(100, 100);
```

**Styling:**
```csharp
// Apply one of the built-in themes
this.radialMenu1.Style = RadialMenuStyle.Office2016Colorful;
// Other options: Default, Office2016White, Office2016DarkGray, Office2016Black
```

**Item Layout:**
```csharp
// Control how many items are visible per level (slice count)
this.radialMenu1.WedgeCount = 6;

// Control how text and images are displayed
this.radialMenu1.DisplayStyle = DisplayStyle.ImageAboveText;
// Other options: Text, Image, TextAboveImage
```

## Common Scenarios

**Scenario 1: Context Menu for Text Editor**

```csharp
private void CreateEditorContextMenu()
{
    this.radialMenu1.Items.Clear();
    
    string[] commands = { "Cut", "Copy", "Paste", "Delete", "Select All" };
    foreach (string cmd in commands)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = cmd;
        item.Click += EditorCommand_Click;
        this.radialMenu1.Items.Add(item);
    }

    this.radialMenu1.WedgeCount = 5;  // Show all 5 items
}

private void EditorCommand_Click(object sender, EventArgs e)
{
    RadialMenuItem item = sender as RadialMenuItem;
    // Execute the corresponding editor command
    switch (item.Text)
    {
        case "Cut":
            textBox1.Cut();
            break;
        case "Copy":
            textBox1.Copy();
            break;
        case "Paste":
            textBox1.Paste();
            break;
        // Add other cases...
    }
}
```

**Scenario 2: Touch-Friendly Navigation Menu**

```csharp
private void CreateTouchFriendlyMenu()
{
    // Larger size for touch targets
    this.radialMenu1.Size = new Size(400, 400);
    
    // Larger outer rim for easier access
    this.radialMenu1.OuterRimThickness = 40;
    
    // Show only text for clarity
    this.radialMenu1.DisplayStyle = DisplayStyle.Text;
    
    // Always visible for easy access
    this.radialMenu1.MenuVisibility = true;
    this.radialMenu1.Visible = true;
}
```

## Troubleshooting

**Problem: RadialMenu is not visible**
```csharp
// Solution: Ensure Visible property is set to true
this.radialMenu1.Visible = true;

// If only center icon shows, set MenuVisibility too
this.radialMenu1.MenuVisibility = true;
```

**Problem: Menu items don't appear**
```csharp
// Solution: Verify items are added to Items collection
Console.WriteLine($"Item count: {this.radialMenu1.Items.Count}");

// Make sure WedgeCount is sufficient
this.radialMenu1.WedgeCount = this.radialMenu1.Items.Count;
```

**Problem: Images don't display**
```csharp
// Solution: Verify ImageList is attached and ImageIndex is valid
this.radialMenu1.ImageList = imageListAdv1;

// Check index is within bounds
if (imageListAdv1.Images.Count > 0)
{
    item.ImageIndex = 0;  // Valid index
}
```

## Next Steps

Now that you have a basic RadialMenu working, explore these advanced features:

- **Hierarchical Menus** - Create nested submenus for complex navigation
- **Special Elements** - Add RadialColorPalette, RadialFontListBox, and RadialMenuSlider
- **Custom Styling** - Customize colors, sizes, and appearance
- **Themes** - Apply Office 2016 themes for professional looks
- **Keyboard Support** - Enable keyboard shortcuts with SuperAccelerator
- **Advanced Features** - Configure wedge count, persistence, and tooltips

## Key Takeaways

1. **Always set Visible = true** - The RadialMenu won't show otherwise
2. **Use MenuVisibility** - Controls whether menu items are shown initially
3. **Configure WedgeCount** - Determines maximum visible items per level
4. **Attach ImageList first** - Before setting ImageIndex on items
5. **Choose appropriate DisplayStyle** - Based on your UI requirements
6. **Handle Click events** - To respond to user interactions
