# Advanced Features and Configuration

```markdown
# Advanced features — condensed

Short recipes for the advanced APIs you will use most: wedge count, visibility, persistence, index ordering and keyboard support. Each snippet has a VB counterpart.

Wedge count and visibility (C#):

```csharp
radialMenu.WedgeCount = 6;       // items per level
radialMenu.MenuVisibility = false; // only center icon shown initially
```

VB.NET:

```vbnet
radialMenu.WedgeCount = 6
radialMenu.MenuVisibility = False
```

Persist previous state (remember submenu level):

```csharp
radialMenu.PersistPreviousState = true;
```

Keyboard accelerators (C#):

```csharp
var sa = new SuperAccelerator(this);
sa.SetAccelerator(myItem, "N"); // Alt+N
```

VB.NET:

```vbnet
Dim sa = New SuperAccelerator(Me)
sa.SetAccelerator(myItem, "N")
```

Performance tips:
- For very large menus, lazy-load submenu items on first drill (create items inside the parent `Opening`/`ItemOpening` handler).
- Persist only when it improves UX; context menus often should not persist.

This page now contains compact, copy-ready advanced patterns for C# and VB.
```
    // Touch-first tablet interface
    if (IsTouchEnabled())
    {
        this.radialMenu1.WedgeCount = 5;  // Larger targets for fingers
        this.radialMenu1.Size = new Size(400, 400);  // Larger overall size
    }
    // Desktop mouse interface
    else
    {
        this.radialMenu1.WedgeCount = 8;  // More items, smaller targets OK
        this.radialMenu1.Size = new Size(300, 300);  // Standard size
    }
}

private bool IsTouchEnabled()
{
    // Check if device supports touch input
    return (SystemInformation.TabletPC || 
            System.Windows.Input.TouchDevice.RegisterEvents());
}
```

**Result:**
Interface adapts to input method, providing appropriately sized targets.

### Handling Items Beyond WedgeCount

When you have more items than WedgeCount allows, organize them hierarchically:

```csharp
private void OrganizeItemsBeyondWedgeCount()
{
    // Configure for 6 visible items
    this.radialMenu1.WedgeCount = 6;
    
    // We have 10 total actions to accommodate
    // Organize into categories
    
    // Category 1: File operations (parent)
    RadialMenuItem fileItem = new RadialMenuItem();
    fileItem.Text = "File";
    
    // File submenu (4 items - fits within WedgeCount)
    fileItem.Items.Add(CreateMenuItem("New"));
    fileItem.Items.Add(CreateMenuItem("Open"));
    fileItem.Items.Add(CreateMenuItem("Save"));
    fileItem.Items.Add(CreateMenuItem("Close"));
    
    // Category 2: Edit operations (parent)
    RadialMenuItem editItem = new RadialMenuItem();
    editItem.Text = "Edit";
    
    // Edit submenu (4 items - fits within WedgeCount)
    editItem.Items.Add(CreateMenuItem("Cut"));
    editItem.Items.Add(CreateMenuItem("Copy"));
    editItem.Items.Add(CreateMenuItem("Paste"));
    editItem.Items.Add(CreateMenuItem("Delete"));
    
    // Add other top-level items
    RadialMenuItem formatItem = new RadialMenuItem();
    formatItem.Text = "Format";
    
    RadialMenuItem toolsItem = new RadialMenuItem();
    toolsItem.Text = "Tools";
    
    // Top level now has only 4 items (within WedgeCount=6)
    this.radialMenu1.Items.Add(fileItem);
    this.radialMenu1.Items.Add(editItem);
    this.radialMenu1.Items.Add(formatItem);
    this.radialMenu1.Items.Add(toolsItem);
}

private RadialMenuItem CreateMenuItem(string text)
{
    RadialMenuItem item = new RadialMenuItem();
    item.Text = text;
    return item;
}
```

**Result:**
A well-organized hierarchical menu where no level exceeds the WedgeCount, ensuring all items are easily accessible.

### Complete WedgeCount Example

```csharp
private void CreateOptimalWedgeLayout()
{
    // Determine optimal wedge count based on item count and platform
    int totalTopLevelItems = 7;
    bool isTouchDevice = IsTouchEnabled();
    
    // Calculate appropriate wedge count
    if (isTouchDevice)
    {
        this.radialMenu1.WedgeCount = Math.Min(totalTopLevelItems, 6);
        this.radialMenu1.Size = new Size(380, 380);
    }
    else
    {
        this.radialMenu1.WedgeCount = Math.Min(totalTopLevelItems, 8);
        this.radialMenu1.Size = new Size(300, 300);
    }
    
    // Add items (example: text editor actions)
    string[] actions = { "Bold", "Italic", "Underline", "Font", 
                        "Color", "Size", "Align" };
    
    foreach (string action in actions)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = action;
        this.radialMenu1.Items.Add(item);
    }
    
    // Log configuration for debugging
    Debug.WriteLine($"WedgeCount: {this.radialMenu1.WedgeCount}, " +
                   $"Items: {this.radialMenu1.Items.Count}");
}
```

**Result:**
Menu automatically configures optimal wedge count based on device type and item count.

## MenuVisibility Property

The `MenuVisibility` property controls whether the menu items are displayed when the RadialMenu first appears, or if only the center icon is shown initially.

### Configuring MenuVisibility

```csharp
// Show menu items immediately on display
this.radialMenu1.MenuVisibility = true;

// Show only center icon, hide items until clicked
this.radialMenu1.MenuVisibility = false;
```

**When to Use MenuVisibility = true:**
- Menu is primary navigation
- Always-accessible toolbars
- Touch-first interfaces
- Context menus triggered by specific actions

**When to Use MenuVisibility = false:**
- Minimalist interfaces
- Space-constrained layouts
- Progressive disclosure patterns
- Desktop applications where menu appears on demand

### MenuVisibility Usage Patterns

**Pattern 1: Always-Visible Toolbar**

```csharp
private void CreateAlwaysVisibleToolbar()
{
    this.radialMenu1.MenuVisibility = true;  // Always show items
    this.radialMenu1.Visible = true;         // Menu itself is visible
    this.radialMenu1.Location = new Point(20, 20);  // Fixed position
    
    // Add toolbar items
    AddToolbarItems();
}

private void AddToolbarItems()
{
    string[] tools = { "Select", "Draw", "Erase", "Fill", "Text" };
    foreach (string tool in tools)
    {
        RadialMenuItem item = new RadialMenuItem();
        item.Text = tool;
        item.Click += Tool_Click;
        this.radialMenu1.Items.Add(item);
    }
}
```

**Result:**
A persistent toolbar with always-visible options, perfect for graphics applications.

**Pattern 2: Context Menu with Progressive Disclosure**

```csharp
private void CreateContextMenu()
{
    // Initially show only center icon
    this.radialMenu1.MenuVisibility = false;
    this.radialMenu1.Visible = true;
    
    // Show menu on right-click
    this.textBox1.MouseUp += (s, e) =>
    {
        if (e.Button == MouseButtons.Right)
        {
            // Position at cursor
            this.radialMenu1.Location = this.textBox1.PointToScreen(e.Location);
            
            // Show the menu items
            this.radialMenu1.MenuVisibility = true;
        }
    };
    
    // Hide when clicking outside
    this.radialMenu1.Leave += (s, e) =>
    {
        this.radialMenu1.MenuVisibility = false;
    };
}
```

**Result:**
Menu appears compact initially, expands when needed, ideal for context menus.

**Pattern 3: Toggle Visibility**

```csharp
private void CreateTogglableMenu()
{
    // Start hidden
    this.radialMenu1.MenuVisibility = false;
    
    // Toggle button to show/hide menu items
    Button toggleButton = new Button();
    toggleButton.Text = "Toggle Menu";
    toggleButton.Click += (s, e) =>
    {
        this.radialMenu1.MenuVisibility = !this.radialMenu1.MenuVisibility;
        toggleButton.Text = this.radialMenu1.MenuVisibility 
            ? "Hide Menu" 
            : "Show Menu";
    };
    
    this.Controls.Add(toggleButton);
}
```

**Result:**
User can manually control when menu items are visible, useful for customizable interfaces.

## PersistPreviousState Property

The `PersistPreviousState` property determines whether the RadialMenu remembers and restores its previous navigation state when reopened.

### Enabling State Persistence

```csharp
// Remember navigation state between openings
this.radialMenu1.PersistPreviousState = true;

// Always reset to root level when opening
this.radialMenu1.PersistPreviousState = false;
```

**How State Persistence Works:**
- When true: Menu remembers which submenu level was displayed
- When false: Menu always starts at root level
- Affects user experience and navigation efficiency
- Particularly useful for frequently-accessed submenus

### State Persistence Scenarios

**Scenario 1: Frequently Accessed Submenu**

```csharp
private void CreateFormattingMenuWithPersistence()
{
    // Enable persistence - users often repeat formatting
    this.radialMenu1.PersistPreviousState = true;
    
    // Create menu structure
    RadialMenuItem formatItem = new RadialMenuItem();
    formatItem.Text = "Format";
    
    RadialMenuItem fontItem = new RadialMenuItem();
    fontItem.Text = "Font";
    
    // Font submenu with many options
    fontItem.Items.Add(CreateMenuItem("Bold"));
    fontItem.Items.Add(CreateMenuItem("Italic"));
    fontItem.Items.Add(CreateMenuItem("Underline"));
    fontItem.Items.Add(CreateMenuItem("Strikethrough"));
    
    formatItem.Items.Add(fontItem);
    this.radialMenu1.Items.Add(formatItem);
    
    // User scenario:
    // 1. User opens menu, navigates: Format > Font
    // 2. Selects "Bold"
    // 3. Menu closes
    // 4. User opens menu again
    // Result: Menu opens directly to Font submenu (not root level)
}
```

**Result:**
Users can quickly access previously-used submenus without repeated navigation.

**Scenario 2: Always-Reset Context Menu**

```csharp
private void CreateContextMenuWithoutPersistence()
{
    // Disable persistence - each invocation is new context
    this.radialMenu1.PersistPreviousState = false;
    
    // Attach to control's context menu
    this.richTextBox1.MouseUp += (s, e) =>
    {
        if (e.Button == MouseButtons.Right)
        {
            this.radialMenu1.Location = richTextBox1.PointToScreen(e.Location);
            this.radialMenu1.MenuVisibility = true;
            
            // Menu always starts at root, regardless of previous state
        }
    };
}
```

**Result:**
Predictable behavior - menu always starts at the same place, regardless of previous navigation.

**When to Use PersistPreviousState:**

```csharp
private void ConfigurePersistenceBasedOnUsage()
{
    // Persistent state for:
    // - Formatting toolbars (users repeat actions)
    // - Drawing tool palettes
    // - Frequently-accessed settings
    if (IsFormattingToolbar() || IsToolPalette())
    {
        this.radialMenu1.PersistPreviousState = true;
    }
    
    // Non-persistent for:
    // - Context menus (different contexts each time)
    // - Primary navigation (predictable starting point)
    // - Document-specific actions
    else
    {
        this.radialMenu1.PersistPreviousState = false;
    }
}
```

## ImageList Configuration

The `ImageList` property (specifically `ImageListAdv`) provides icon images for menu items. Proper configuration is essential for visual clarity.

### Setting Up ImageListAdv

```csharp
// Create and configure ImageListAdv
ImageListAdv imageList = new ImageListAdv(this.components);

// Set image size (recommended: 32x32 for RadialMenu)
imageList.ImageSize = new Size(32, 32);

// Add images from files
imageList.Images.Add("new", Image.FromFile("icons/new.png"));
imageList.Images.Add("open", Image.FromFile("icons/open.png"));
imageList.Images.Add("save", Image.FromFile("icons/save.png"));

// OR add from resources
imageList.Images.Add("new", Properties.Resources.NewIcon);
imageList.Images.Add("open", Properties.Resources.OpenIcon);
imageList.Images.Add("save", Properties.Resources.SaveIcon);

// Attach to RadialMenu
this.radialMenu1.ImageList = imageList;
```

### Complete ImageList Example

```csharp
private void ConfigureImageListWithIcons()
{
    // Create ImageListAdv
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.ImageSize = new Size(32, 32);
    
    // Load icons from embedded resources
    imageList.Images.Add(Properties.Resources.IconEdit);    // Index 0
    imageList.Images.Add(Properties.Resources.IconCut);     // Index 1
    imageList.Images.Add(Properties.Resources.IconCopy);    // Index 2
    imageList.Images.Add(Properties.Resources.IconPaste);   // Index 3
    imageList.Images.Add(Properties.Resources.IconDelete);  // Index 4
    
    // Attach to menu
    this.radialMenu1.ImageList = imageList;
    
    // Create items referencing images by index
    RadialMenuItem editItem = new RadialMenuItem();
    editItem.Text = "Edit";
    editItem.ImageIndex = 0;
    
    RadialMenuItem cutItem = new RadialMenuItem();
    cutItem.Text = "Cut";
    cutItem.ImageIndex = 1;
    
    RadialMenuItem copyItem = new RadialMenuItem();
    copyItem.Text = "Copy";
    copyItem.ImageIndex = 2;
    
    RadialMenuItem pasteItem = new RadialMenuItem();
    pasteItem.Text = "Paste";
    pasteItem.ImageIndex = 3;
    
    RadialMenuItem deleteItem = new RadialMenuItem();
    deleteItem.Text = "Delete";
    deleteItem.ImageIndex = 4;
    
    // Add to menu
    this.radialMenu1.Items.Add(editItem);
    this.radialMenu1.Items.Add(cutItem);
    this.radialMenu1.Items.Add(copyItem);
    this.radialMenu1.Items.Add(pasteItem);
    this.radialMenu1.Items.Add(deleteItem);
}
```

**Result:**
All menu items display with consistent, high-quality icons.

### Dynamic Image Loading

```csharp
private void LoadImagesFromDirectory(string iconDirectory)
{
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.ImageSize = new Size(32, 32);
    
    // Load all PNG files from directory
    string[] iconFiles = Directory.GetFiles(iconDirectory, "*.png");
    
    foreach (string iconPath in iconFiles)
    {
        try
        {
            string iconName = Path.GetFileNameWithoutExtension(iconPath);
            Image icon = Image.FromFile(iconPath);
            imageList.Images.Add(iconName, icon);
        }
        catch (Exception ex)
        {
            Debug.WriteLine($"Failed to load icon {iconPath}: {ex.Message}");
        }
    }
    
    this.radialMenu1.ImageList = imageList;
}
```

**Result:**
Flexible icon loading from file system, useful for customizable themes or plugins.

## UseIndexBasedOrder Property

The `UseIndexBasedOrder` property controls whether menu items are arranged sequentially or based on explicit index values.

### Index-Based Ordering

```csharp
// Enable index-based ordering
this.radialMenu1.UseIndexBasedOrder = true;

// Disable (sequential ordering)
this.radialMenu1.UseIndexBasedOrder = false;
```

**Sequential Order (UseIndexBasedOrder = false):**
Items appear in the order they're added to the collection.

```csharp
private void CreateSequentialMenu()
{
    this.radialMenu1.UseIndexBasedOrder = false;
    
    // Items will appear in this exact order: Item1, Item2, Item3
    this.radialMenu1.Items.Add(CreateMenuItem("Item 1"));
    this.radialMenu1.Items.Add(CreateMenuItem("Item 2"));
    this.radialMenu1.Items.Add(CreateMenuItem("Item 3"));
}
```

**Index-Based Order (UseIndexBasedOrder = true):**
Items are positioned according to their ImageIndex property, allowing non-sequential arrangement.

```csharp
private void CreateIndexBasedMenu()
{
    this.radialMenu1.UseIndexBasedOrder = true;
    this.radialMenu1.WedgeCount = 8;  // 8 positions available
    
    // Create items with specific positions
    RadialMenuItem item1 = new RadialMenuItem();
    item1.Text = "First";
    item1.ImageIndex = 0;  // Position 0
    
    RadialMenuItem item2 = new RadialMenuItem();
    item2.Text = "Fourth";
    item2.ImageIndex = 3;  // Position 3 (skip 1 and 2)
    
    RadialMenuItem item3 = new RadialMenuItem();
    item3.Text = "Seventh";
    item3.ImageIndex = 6;  // Position 6 (skip 4 and 5)
    
    this.radialMenu1.Items.Add(item1);
    this.radialMenu1.Items.Add(item2);
    this.radialMenu1.Items.Add(item3);
    
    // Result: Items appear at positions 0, 3, and 6 with gaps between
}
```

**When to Use Index-Based Ordering:**
- Fixed position requirements (e.g., "Exit" always at bottom)
- Circular dial patterns (specific positions matter)
- Muscle memory interfaces (consistent positions)
- Conditional menus (items appear/disappear but stay in same positions)

### Complete Index-Based Example

```csharp
private void CreateFixedPositionMenu()
{
    this.radialMenu1.UseIndexBasedOrder = true;
    this.radialMenu1.WedgeCount = 8;
    
    // Define fixed positions for common actions
    // Position 0 (top): New
    RadialMenuItem newItem = new RadialMenuItem();
    newItem.Text = "New";
    newItem.ImageIndex = 0;
    
    // Position 2 (right): Open
    RadialMenuItem openItem = new RadialMenuItem();
    openItem.Text = "Open";
    openItem.ImageIndex = 2;
    
    // Position 4 (bottom): Exit
    RadialMenuItem exitItem = new RadialMenuItem();
    exitItem.Text = "Exit";
    exitItem.ImageIndex = 4;
    
    // Position 6 (left): Help
    RadialMenuItem helpItem = new RadialMenuItem();
    helpItem.Text = "Help";
    helpItem.ImageIndex = 6;
    
    // Add items
    this.radialMenu1.Items.Add(newItem);
    this.radialMenu1.Items.Add(openItem);
    this.radialMenu1.Items.Add(exitItem);
    this.radialMenu1.Items.Add(helpItem);
    
    // Result: Compass-like menu with items at cardinal positions
}
```

**Result:**
Menu items appear at consistent, predictable positions, enhancing muscle memory and usability.

## Keyboard Support

RadialMenu supports keyboard shortcuts through the SuperAccelerator component, enabling quick access via Alt+key combinations.

### Basic Keyboard Support Overview

Keyboard support allows users to:
- Press Alt to display accelerator keys
- Press a letter key to activate the corresponding item
- Navigate without mouse/touch
- Increase accessibility
- Speed up workflows for power users

**Requirements:**
- SuperAccelerator component
- Accelerator strings assigned to menu items
- Alt key to trigger accelerator display

## SuperAccelerator Configuration

The `SuperAccelerator` component provides keyboard shortcut functionality for RadialMenu items.

### Adding SuperAccelerator

**Step 1: Add SuperAccelerator Component**

```csharp
// Create SuperAccelerator instance
SuperAccelerator superAccelerator1 = new SuperAccelerator(this);
```

**Step 2: Assign Accelerators to Menu Items**

```csharp
private void ConfigureKeyboardShortcuts()
{
    // Create SuperAccelerator
    SuperAccelerator superAccelerator = new SuperAccelerator(this);
    
    // Create menu items
    RadialMenuItem editItem = new RadialMenuItem();
    editItem.Text = "Edit";
    
    RadialMenuItem cutItem = new RadialMenuItem();
    cutItem.Text = "Cut";
    
    RadialMenuItem copyItem = new RadialMenuItem();
    copyItem.Text = "Copy";
    
    RadialMenuItem pasteItem = new RadialMenuItem();
    pasteItem.Text = "Paste";
    
    // Assign accelerator keys
    superAccelerator.SetAccelerator(editItem, "E");
    superAccelerator.SetAccelerator(cutItem, "C");
    superAccelerator.SetAccelerator(copyItem, "X");  // Alt+X for Copy
    superAccelerator.SetAccelerator(pasteItem, "P");
    
    // Add items to menu
    this.radialMenu1.Items.Add(editItem);
    this.radialMenu1.Items.Add(cutItem);
    this.radialMenu1.Items.Add(copyItem);
    this.radialMenu1.Items.Add(pasteItem);
}
```

**Usage:**
1. Press Alt key
2. Accelerator letters appear on menu items
3. Press the letter (e.g., "C" for Cut)
4. Item's Click event fires

**Important:** Do not assign the same accelerator to multiple items at the same menu level.

### SuperAccelerator Appearance Customization

```csharp
private void CustomizeSuperAcceleratorAppearance()
{
    SuperAccelerator superAccelerator = new SuperAccelerator(this);
    
    // Customize appearance
    superAccelerator.BackColor = Color.DodgerBlue;
    superAccelerator.ForeColor = Color.White;
    superAccelerator.Font = new Font("Arial", 10, FontStyle.Bold);
    
    // Set accelerators
    superAccelerator.SetAccelerator(menuItem1, "N");
    superAccelerator.SetAccelerator(menuItem2, "O");
    superAccelerator.SetAccelerator(menuItem3, "S");
}
```

**Result:**
Accelerator keys display with custom styling when Alt is pressed.

### SuperAccelerator Style Property

SuperAccelerator supports visual themes matching the RadialMenu style.

```csharp
private void ApplySuperAcceleratorTheme()
{
    SuperAccelerator superAccelerator = new SuperAccelerator(this);
    
    // Match RadialMenu theme
    superAccelerator.Appearance = Syncfusion.Windows.Forms.Tools.Appearance.Office2016Colorful;
    
    // Available styles:
    // - Default
    // - Advanced
    // - Office2016Colorful
    // - Office2016White
    // - Office2016DarkGray
    // - Office2016Black
}
```

### Complete Keyboard Support Example

```csharp
public class KeyboardEnabledForm : Form
{
    private RadialMenu radialMenu;
    private SuperAccelerator superAccelerator;
    private RichTextBox editor;

    private void InitializeKeyboardSupport()
    {
        // Create menu
        this.radialMenu = new RadialMenu();
        this.radialMenu.Style = RadialMenuStyle.Office2016Colorful;
        this.radialMenu.Size = new Size(300, 300);
        this.radialMenu.Location = new Point(100, 100);
        this.radialMenu.Visible = true;
        this.radialMenu.MenuVisibility = true;
        
        // Create SuperAccelerator
        this.superAccelerator = new SuperAccelerator(this);
        this.superAccelerator.Appearance = Syncfusion.Windows.Forms.Tools.Appearance.Office2016Colorful;
        this.superAccelerator.BackColor = Color.DodgerBlue;
        this.superAccelerator.ForeColor = Color.White;
        this.superAccelerator.Font = new Font("Segoe UI", 9, FontStyle.Bold);
        
        // Create menu items with shortcuts
        CreateMenuWithShortcuts();
        
        this.Controls.Add(this.radialMenu);
    }

    private void CreateMenuWithShortcuts()
    {
        // File operations
        RadialMenuItem newItem = new RadialMenuItem();
        newItem.Text = "New";
        newItem.Click += (s, e) => CreateNewDocument();
        this.superAccelerator.SetAccelerator(newItem, "N");
        
        RadialMenuItem openItem = new RadialMenuItem();
        openItem.Text = "Open";
        openItem.Click += (s, e) => OpenDocument();
        this.superAccelerator.SetAccelerator(openItem, "O");
        
        RadialMenuItem saveItem = new RadialMenuItem();
        saveItem.Text = "Save";
        saveItem.Click += (s, e) => SaveDocument();
        this.superAccelerator.SetAccelerator(saveItem, "S");
        
        // Edit operations
        RadialMenuItem cutItem = new RadialMenuItem();
        cutItem.Text = "Cut";
        cutItem.Click += (s, e) => editor.Cut();
        this.superAccelerator.SetAccelerator(cutItem, "X");
        
        RadialMenuItem copyItem = new RadialMenuItem();
        copyItem.Text = "Copy";
        copyItem.Click += (s, e) => editor.Copy();
        this.superAccelerator.SetAccelerator(copyItem, "C");
        
        RadialMenuItem pasteItem = new RadialMenuItem();
        pasteItem.Text = "Paste";
        pasteItem.Click += (s, e) => editor.Paste();
        this.superAccelerator.SetAccelerator(pasteItem, "V");
        
        // Add to menu
        this.radialMenu.Items.Add(newItem);
        this.radialMenu.Items.Add(openItem);
        this.radialMenu.Items.Add(saveItem);
        this.radialMenu.Items.Add(cutItem);
        this.radialMenu.Items.Add(copyItem);
        this.radialMenu.Items.Add(pasteItem);
    }

    private void CreateNewDocument()
    {
        editor.Clear();
        MessageBox.Show("New document created (Alt+N)");
    }

    private void OpenDocument()
    {
        // Open file dialog logic
        MessageBox.Show("Open document (Alt+O)");
    }

    private void SaveDocument()
    {
        // Save file logic
        MessageBox.Show("Document saved (Alt+S)");
    }
}
```

**Result:**
Full keyboard navigation with visual feedback and themed accelerator display.

## Tooltip Support

The `ShowTooltip` property enables tooltips that display menu item text when hovering.

### Enabling Tooltips

```csharp
// Enable tooltips
this.radialMenu1.ShowToolTip = true;

// Disable tooltips
this.radialMenu1.ShowToolTip = false;
```

**When to Enable Tooltips:**
- Image-only display mode (DisplayStyle.Image)
- Icons that might be ambiguous
- First-time users learning the interface
- Accessibility requirements

### Tooltip Usage Example

```csharp
private void CreateIconMenuWithTooltips()
{
    // Configure for icon-only display
    this.radialMenu1.DisplayStyle = DisplayStyle.Image;
    this.radialMenu1.ShowToolTip = true;  // Show tooltips since no text visible
    
    // Set up image list
    ImageListAdv imageList = new ImageListAdv(this.components);
    imageList.Images.Add(Properties.Resources.BoldIcon);
    imageList.Images.Add(Properties.Resources.ItalicIcon);
    imageList.Images.Add(Properties.Resources.UnderlineIcon);
    this.radialMenu1.ImageList = imageList;
    
    // Create items - Text property becomes tooltip content
    RadialMenuItem boldItem = new RadialMenuItem();
    boldItem.Text = "Bold (Ctrl+B)";  // Shown in tooltip
    boldItem.ImageIndex = 0;
    
    RadialMenuItem italicItem = new RadialMenuItem();
    italicItem.Text = "Italic (Ctrl+I)";  // Shown in tooltip
    italicItem.ImageIndex = 1;
    
    RadialMenuItem underlineItem = new RadialMenuItem();
    underlineItem.Text = "Underline (Ctrl+U)";  // Shown in tooltip
    underlineItem.ImageIndex = 2;
    
    this.radialMenu1.Items.Add(boldItem);
    this.radialMenu1.Items.Add(italicItem);
    this.radialMenu1.Items.Add(underlineItem);
}
```

**Result:**
Icons display without text, but hovering shows descriptive tooltips with keyboard shortcuts.

## Complete Advanced Examples

### Example 1: Professional Text Editor Menu

```csharp
public class AdvancedTextEditorMenu
{
    private RadialMenu formattingMenu;
    private SuperAccelerator accelerator;
    private RichTextBox editor;

    public void Initialize(Form parentForm, RichTextBox textEditor)
    {
        this.editor = textEditor;
        
        // Configure menu
        this.formattingMenu = new RadialMenu();
        this.formattingMenu.Style = RadialMenuStyle.Office2016Colorful;
        this.formattingMenu.Size = new Size(350, 350);
        this.formattingMenu.WedgeCount = 6;
        this.formattingMenu.MenuVisibility = false;
        this.formattingMenu.PersistPreviousState = true;  // Remember last position
        this.formattingMenu.ShowToolTip = true;
        this.formattingMenu.DisplayStyle = DisplayStyle.ImageAboveText;
        this.formattingMenu.MenuItemImageSize = new Size(28, 28);
        
        // Configure keyboard support
        this.accelerator = new SuperAccelerator(parentForm);
        this.accelerator.Appearance = Syncfusion.Windows.Forms.Tools.Appearance.Office2016Colorful;
        
        // Set up images
        ConfigureImageList();
        
        // Create menu structure
        CreateFormattingMenu();
        
        // Attach event handlers
        AttachEventHandlers(textEditor);
        
        parentForm.Controls.Add(this.formattingMenu);
    }

    private void ConfigureImageList()
    {
        ImageListAdv imageList = new ImageListAdv();
        imageList.ImageSize = new Size(32, 32);
        
        // Add formatting icons
        imageList.Images.Add(Properties.Resources.FontIcon);
        imageList.Images.Add(Properties.Resources.ColorIcon);
        imageList.Images.Add(Properties.Resources.SizeIcon);
        imageList.Images.Add(Properties.Resources.StyleIcon);
        imageList.Images.Add(Properties.Resources.AlignIcon);
        imageList.Images.Add(Properties.Resources.MoreIcon);
        
        this.formattingMenu.ImageList = imageList;
    }

    private void CreateFormattingMenu()
    {
        // Font family
        RadialFontListBox fontList = new RadialFontListBox();
        fontList.Text = "Font";
        fontList.ImageIndex = 0;
        fontList.SelectedFontChanged += Font_Changed;
        this.accelerator.SetAccelerator(fontList, "F");
        
        // Text color
        RadialColorPalette textColor = new RadialColorPalette();
        textColor.Text = "Color";
        textColor.ImageIndex = 1;
        textColor.ColorSelected += TextColor_Selected;
        this.accelerator.SetAccelerator(textColor, "C");
        
        // Font size
        RadialMenuSlider fontSize = new RadialMenuSlider();
        fontSize.Text = "Size";
        fontSize.ImageIndex = 2;
        fontSize.MinimumValue = 8;
        fontSize.MaximumValue = 72;
        fontSize.SliderValueChanged += FontSize_Changed;
        this.accelerator.SetAccelerator(fontSize, "Z");
        
        // Text style (submenu)
        RadialMenuItem styleItem = CreateStyleSubmenu();
        styleItem.ImageIndex = 3;
        this.accelerator.SetAccelerator(styleItem, "S");
        
        // Alignment (submenu)
        RadialMenuItem alignItem = CreateAlignmentSubmenu();
        alignItem.ImageIndex = 4;
        this.accelerator.SetAccelerator(alignItem, "A");
        
        // More options (submenu)
        RadialMenuItem moreItem = CreateMoreOptionsSubmenu();
        moreItem.ImageIndex = 5;
        this.accelerator.SetAccelerator(moreItem, "M");
        
        // Add all items
        this.formattingMenu.Items.Add(fontList);
        this.formattingMenu.Items.Add(textColor);
        this.formattingMenu.Items.Add(fontSize);
        this.formattingMenu.Items.Add(styleItem);
        this.formattingMenu.Items.Add(alignItem);
        this.formattingMenu.Items.Add(moreItem);
    }

    private RadialMenuItem CreateStyleSubmenu()
    {
        RadialMenuItem parent = new RadialMenuItem();
        parent.Text = "Style";
        
        string[] styles = { "Bold", "Italic", "Underline", "Strikeout" };
        foreach (string style in styles)
        {
            RadialMenuItem item = new RadialMenuItem();
            item.Text = style;
            item.CheckMode = CheckMode.Check;
            item.Click += Style_Click;
            parent.Items.Add(item);
        }
        
        return parent;
    }

    private RadialMenuItem CreateAlignmentSubmenu()
    {
        RadialMenuItem parent = new RadialMenuItem();
        parent.Text = "Align";
        
        string[] alignments = { "Left", "Center", "Right", "Justify" };
        foreach (string align in alignments)
        {
            RadialMenuItem item = new RadialMenuItem();
            item.Text = align;
            item.CheckMode = CheckMode.Option;
            item.GroupName = "alignment";
            item.Checked = (align == "Left");
            item.Click += Alignment_Click;
            parent.Items.Add(item);
        }
        
        return parent;
    }

    private RadialMenuItem CreateMoreOptionsSubmenu()
    {
        RadialMenuItem parent = new RadialMenuItem();
        parent.Text = "More";
        
        // Add additional formatting options
        RadialMenuItem bulletItem = new RadialMenuItem();
        bulletItem.Text = "Bullet List";
        bulletItem.Click += (s, e) => ToggleBullets();
        
        RadialMenuItem numberItem = new RadialMenuItem();
        numberItem.Text = "Numbered List";
        numberItem.Click += (s, e) => ToggleNumbering();
        
        parent.Items.Add(bulletItem);
        parent.Items.Add(numberItem);
        
        return parent;
    }

    private void AttachEventHandlers(RichTextBox editor)
    {
        // Show menu on right-click
        editor.MouseUp += (s, e) =>
        {
            if (e.Button == MouseButtons.Right && editor.SelectionLength > 0)
            {
                this.formattingMenu.Location = editor.PointToScreen(e.Location);
                this.formattingMenu.MenuVisibility = true;
            }
        };
    }

    // Event handler implementations
    private void Font_Changed(object sender, SelectedFontChangedEventArgs e)
    {
        if (editor.SelectionLength > 0)
        {
            Font current = editor.SelectionFont ?? editor.Font;
            editor.SelectionFont = new Font(e.FontName, current.Size, current.Style);
        }
    }

    private void TextColor_Selected(object sender, ColorSelectedEventArgs e)
    {
        if (editor.SelectionLength > 0)
        {
            editor.SelectionColor = e.Color;
        }
    }

    private void FontSize_Changed(object sender, SliderValueChangedEventArgs e)
    {
        if (editor.SelectionLength > 0)
        {
            Font current = editor.SelectionFont ?? editor.Font;
            editor.SelectionFont = new Font(current.FontFamily, e.Value, current.Style);
        }
    }

    private void Style_Click(object sender, EventArgs e)
    {
        RadialMenuItem item = sender as RadialMenuItem;
        if (editor.SelectionLength == 0) return;

        Font current = editor.SelectionFont ?? editor.Font;
        FontStyle style = current.Style;

        switch (item.Text)
        {
            case "Bold":
                style = item.Checked ? style | FontStyle.Bold : style & ~FontStyle.Bold;
                break;
            case "Italic":
                style = item.Checked ? style | FontStyle.Italic : style & ~FontStyle.Italic;
                break;
            case "Underline":
                style = item.Checked ? style | FontStyle.Underline : style & ~FontStyle.Underline;
                break;
            case "Strikeout":
                style = item.Checked ? style | FontStyle.Strikeout : style & ~FontStyle.Strikeout;
                break;
        }

        editor.SelectionFont = new Font(current.FontFamily, current.Size, style);
    }

    private void Alignment_Click(object sender, EventArgs e)
    {
        RadialMenuItem item = sender as RadialMenuItem;
        
        switch (item.Text)
        {
            case "Left":
                editor.SelectionAlignment = HorizontalAlignment.Left;
                break;
            case "Center":
                editor.SelectionAlignment = HorizontalAlignment.Center;
                break;
            case "Right":
                editor.SelectionAlignment = HorizontalAlignment.Right;
                break;
        }
    }

    private void ToggleBullets()
    {
        editor.SelectionBullet = !editor.SelectionBullet;
    }

    private void ToggleNumbering()
    {
        // Implement numbered list logic
    }
}
```

**Result:**
A comprehensive, professional-grade text formatting menu with all advanced features integrated.

## Best Practices for Complex Scenarios

**1. Optimize WedgeCount for Content**
```csharp
// Calculate optimal wedge count based on total items
int optimalWedgeCount = Math.Min(this.radialMenu1.Items.Count, 8);
this.radialMenu1.WedgeCount = optimalWedgeCount;
```

**2. Use Persistence Strategically**
```csharp
// Persist for repeated actions
if (IsRepetitiveTask())
    this.radialMenu1.PersistPreviousState = true;
else
    this.radialMenu1.PersistPreviousState = false;
```

**3. Provide Multiple Access Methods**
```csharp
// Keyboard + Mouse + Touch
this.radialMenu1.ShowToolTip = true;  // Visual hints
SetupKeyboardShortcuts();             // Keyboard access
OptimizeForTouch();                   // Touch-friendly sizing
```

**4. Test with Real Content**
```csharp
// Always test with actual use cases
PopulateMenuWithRealItems();
TestNavigationPatterns();
VerifyKeyboardAccess();
CheckTooltipClarity();
```

**5. Monitor Performance**
```csharp
// For large menus, consider lazy loading
private void LoadMenuItemsOnDemand()
{
    // Only create items when submenu is accessed
}
```
