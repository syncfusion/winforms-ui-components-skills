# XPTaskBar Items and Content Management

## Creating and Managing XPTaskBarItems

XPTaskBarItem objects represent individual clickable commands within a box. Each item displays text and optionally an image, and can be assigned a Tag for event routing.

## Adding Items to a Box

### Using Items Collection

Add items directly to a box's Items collection:

```csharp
XPTaskBarBox box = new XPTaskBarBox();
box.Text = "File Operations";

// Create items one by one
XPTaskBarItem item1 = new XPTaskBarItem("New Document", System.Drawing.Color.Empty, -1, "NewDoc");
XPTaskBarItem item2 = new XPTaskBarItem("Open File", System.Drawing.Color.Empty, -1, "OpenFile");
XPTaskBarItem item3 = new XPTaskBarItem("Save", System.Drawing.Color.Empty, -1, "Save");

// Add to collection
box.Items.Add(item1);
box.Items.Add(item2);
box.Items.Add(item3);
```

### Using AddRange

Add multiple items at once:

```csharp
box.Items.AddRange(new XPTaskBarItem[] {
    new XPTaskBarItem("Cut", System.Drawing.Color.Empty, -1, "Cut"),
    new XPTaskBarItem("Copy", System.Drawing.Color.Empty, -1, "Copy"),
    new XPTaskBarItem("Paste", System.Drawing.Color.Empty, -1, "Paste")
});
```

**VB.NET:**

```vb
box.Items.AddRange(New XPTaskBarItem() {
    New XPTaskBarItem("Cut", System.Drawing.Color.Empty, -1, "Cut"),
    New XPTaskBarItem("Copy", System.Drawing.Color.Empty, -1, "Copy"),
    New XPTaskBarItem("Paste", System.Drawing.Color.Empty, -1, "Paste")})
```

## Item Properties

### Text and Display

```csharp
// Set item text
item.Text = "Open Document";

// Access item text
string itemName = item.Text;
```

### Tag for Event Routing

Assign a unique identifier (Tag) to each item for event handling:

```csharp
// Create items with tags
var newItem = new XPTaskBarItem("New", System.Drawing.Color.Empty, -1, "cmd_new");
var openItem = new XPTaskBarItem("Open", System.Drawing.Color.Empty, -1, "cmd_open");
var saveItem = new XPTaskBarItem("Save", System.Drawing.Color.Empty, -1, "cmd_save");

box.Items.AddRange(new[] { newItem, openItem, saveItem });

// Tags are used in ItemClick event
box.ItemClick += (sender, e) => {
    string command = e.XPTaskBarItem.Tag as string;
    // Handle command
};
```

### Image Index

Display images with items using an ImageList:

```csharp
// Set up ImageList (typically created in designer with images added)
ImageList imageList = new ImageList();
// Assume images are already added to imageList

// Assign to box
box.ImageList = imageList;

// Set image index for items
box.Items[0].ImageIndex = 0;  // First image
box.Items[1].ImageIndex = 1;  // Second image
box.Items[2].ImageIndex = 2;  // Third image
```

**VB.NET:**

```vb
box.ImageList = imageList
box.Items(0).ImageIndex = 0
box.Items(1).ImageIndex = 1
box.Items(2).ImageIndex = 2
```

### Tooltip Text

Add helpful tooltips to items:

```csharp
// Set tooltip for individual items
box.Items[0].ToolTipText = "Create a new document";
box.Items[1].ToolTipText = "Open an existing document";

// Enable tooltip display
box.ShowToolTip = true;
```

## Managing Item Collections

### Accessing Items

```csharp
// Get item count
int itemCount = box.Items.Count;

// Access by index
XPTaskBarItem firstItem = box.Items[0];

// Iterate through items
foreach (XPTaskBarItem item in box.Items) {
    Console.WriteLine(item.Text);
}

// Find item by tag
var targetItem = box.Items.Cast<XPTaskBarItem>()
    .FirstOrDefault(i => i.Tag.ToString() == "cmd_save");
```

### Removing Items

```csharp
// Remove by index
box.Items.RemoveAt(0);

// Remove specific item
box.Items.Remove(targetItem);

// Clear all items
box.Items.Clear();
```

## Hosting Child Controls

### Using PreferredChildPanelHeight

XPTaskBar boxes can host nested controls (like buttons, text boxes, or panels) for more complex interactions. Set the height reserved for child controls:

```csharp
// Reserve space for child controls
box.PreferredChildPanelHeight = 100;  // 100 pixels

// Create a panel for child controls
Panel childPanel = new Panel();
childPanel.Dock = DockStyle.Fill;
childPanel.Height = 100;

// Add controls to the panel
Button button = new Button { Text = "Search", Dock = DockStyle.Top };
TextBox textBox = new TextBox { Dock = DockStyle.Top };

childPanel.Controls.Add(button);
childPanel.Controls.Add(textBox);

// Add panel to the box
box.Controls.Add(childPanel);
```

### Complex Child Control Example

```csharp
// Create a search box with both items and a text field
XPTaskBarBox searchBox = new XPTaskBarBox();
searchBox.Text = "Search Tools";
searchBox.PreferredChildPanelHeight = 60;

// Add predefined items
searchBox.Items.AddRange(new XPTaskBarItem[] {
    new XPTaskBarItem("Find Text", System.Drawing.Color.Empty, -1, "find"),
    new XPTaskBarItem("Replace", System.Drawing.Color.Empty, -1, "replace")
});

// Add a search input area
Panel inputPanel = new Panel { Dock = DockStyle.Fill };
TextBox searchInput = new TextBox { Dock = DockStyle.Top, Height = 20 };
Button searchButton = new Button { Text = "Go", Dock = DockStyle.Bottom, Height = 25 };
inputPanel.Controls.Add(searchButton);
inputPanel.Controls.Add(searchInput);

searchBox.Controls.Add(inputPanel);
xpTaskBar1.Controls.Add(searchBox);
```

## Animation During Item Operations

When adding or removing items, animation can be applied:

```csharp
// Configure animation for item changes
box.UseAdditionalAnimation = true;      // Animate on add/remove
box.AnimationDelay = 50;                // 50ms between frames
box.AnimationPositionsCount = 15;       // 15 animation steps

// Add items - will animate
box.Items.Add(new XPTaskBarItem("New Item", System.Drawing.Color.Empty, -1, "new"));

// Remove items - will animate
box.Items.RemoveAt(0);
```

## Common Patterns

### Pattern 1: Create and Populate Box

```csharp
private XPTaskBarBox CreateFileBox() {
    var box = new XPTaskBarBox { Text = "File Operations" };
    
    var items = new[] {
        ("New", "file_new"),
        ("Open", "file_open"),
        ("Save", "file_save"),
        ("Save As", "file_saveas"),
        ("Exit", "file_exit")
    };
    
    foreach (var (text, tag) in items) {
        box.Items.Add(new XPTaskBarItem(text, Color.Empty, -1, tag));
    }
    
    return box;
}
```

### Pattern 2: Route Items by Tag

```csharp
box.ItemClick += (sender, e) => {
    string command = e.XPTaskBarItem.Tag as string ?? "";
    
    switch (command) {
        case "file_new":
            CreateNewDocument();
            break;
        case "file_open":
            OpenDocument();
            break;
        case "file_save":
            SaveDocument();
            break;
        default:
            MessageBox.Show($"Unknown command: {command}");
            break;
    }
};
```

### Pattern 3: Dynamically Add Items

```csharp
// Add items based on recent files or application state
foreach (string recentFile in GetRecentFiles()) {
    var item = new XPTaskBarItem(
        text: Path.GetFileName(recentFile),
        foreColor: Color.Empty,
        imageIndex: -1,
        tag: "open_recent"
    );
    box.Items.Add(item);
}
```

### Pattern 4: Disable Items Conditionally

```csharp
// Items can be disabled by removing and readding based on conditions
void UpdateItemAvailability(XPTaskBarBox box, bool canEdit) {
    // Find edit items
    var pasteItem = box.Items.Cast<XPTaskBarItem>()
        .FirstOrDefault(i => i.Tag.ToString() == "paste");
    
    if (pasteItem != null) {
        if (!canEdit) {
            box.Items.Remove(pasteItem);
        }
    }
}
```

## Working with Multiple Boxes

### Organize Items Across Boxes

```csharp
var taskBar = new XPTaskBar();

// File operations box
var fileBox = new XPTaskBarBox { Text = "File" };
fileBox.Items.AddRange(new XPTaskBarItem[] {
    new XPTaskBarItem("New", Color.Empty, -1, "file_new"),
    new XPTaskBarItem("Open", Color.Empty, -1, "file_open")
});

// Edit operations box
var editBox = new XPTaskBarBox { Text = "Edit" };
editBox.Items.AddRange(new XPTaskBarItem[] {
    new XPTaskBarItem("Cut", Color.Empty, -1, "edit_cut"),
    new XPTaskBarItem("Copy", Color.Empty, -1, "edit_copy"),
    new XPTaskBarItem("Paste", Color.Empty, -1, "edit_paste")
});

taskBar.Controls.Add(fileBox);
taskBar.Controls.Add(editBox);
```

### Handle Clicks from Any Box

```csharp
// Subscribe to ItemClick on all boxes
foreach (Control control in xpTaskBar1.Controls) {
    if (control is XPTaskBarBox box) {
        box.ItemClick += (sender, e) => HandleItemClick(e.XPTaskBarItem);
    }
}

private void HandleItemClick(XPTaskBarItem item) {
    Console.WriteLine($"Clicked: {item.Text} (Tag: {item.Tag})");
}
```

## Next Steps

- See [behavior-and-events.md](behavior-and-events.md) to handle item clicks and animation events
- See [appearance-customization.md](appearance-customization.md) to customize item appearance
