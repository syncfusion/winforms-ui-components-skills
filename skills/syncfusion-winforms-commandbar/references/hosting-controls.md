# Hosting Controls in Windows Forms CommandBar

## Table of Contents
- [PopupMenu Integration](#popupmenu-integration)
- [XPToolBar Integration](#xptoolbar-integration)

## PopupMenu Integration

PopupMenu can be integrated with CommandBar to provide drop-down menu functionality.

### Through designer

1. Drag `PopupMenu` from toolbox to the form
2. Select the `CommandBar` instance in the form
3. Set the `PopupMenu` property to the PopupMenu control created

### Through code

Create and configure PopupMenu with menu items:

```csharp
PopupMenu popupMenu = new PopupMenu();
popupMenu.ParentBarItem = new ParentBarItem();

// Add menu items
popupMenu.ParentBarItem.Items.AddRange(new BarItem[]
{
    new BarItem() { BarName = "file", Text = "File" },
    new BarItem() { BarName = "edit", Text = "Edit" },
    new BarItem() { BarName = "view", Text = "View" }
});

// Attach to command bar
this.commandBar1.PopupMenu = popupMenu;
```

### Complete PopupMenu example

```csharp
private void SetupPopupMenu()
{
    PopupMenu popupMenu = new PopupMenu();
    popupMenu.ParentBarItem = new ParentBarItem();
    
    // Create menu structure
    BarItem fileMenu = new BarItem() { BarName = "file", Text = "File" };
    BarItem newItem = new BarItem() { BarName = "new", Text = "New" };
    BarItem openItem = new BarItem() { BarName = "open", Text = "Open" };
    BarItem exitItem = new BarItem() { BarName = "exit", Text = "Exit" };
    
    fileMenu.Items.Add(newItem);
    fileMenu.Items.Add(openItem);
    fileMenu.Items.Add(new BarItem() { Text = "-" });  // Separator
    fileMenu.Items.Add(exitItem);
    
    popupMenu.ParentBarItem.Items.Add(fileMenu);
    
    // Wire up click handlers
    newItem.Click += (s, e) => { /* Handle New */ };
    openItem.Click += (s, e) => { /* Handle Open */ };
    exitItem.Click += (s, e) => { Application.Exit(); };
    
    // Attach to command bar
    this.commandBar1.PopupMenu = popupMenu;
}
```

### PopupMenu with icons

```csharp
PopupMenu popupMenu = new PopupMenu();
popupMenu.ParentBarItem = new ParentBarItem();

BarItem fileMenu = new BarItem() 
{ 
    BarName = "file", 
    Text = "File",
    Image = Image.FromFile(@"file.png")
};

BarItem saveItem = new BarItem() 
{ 
    BarName = "save", 
    Text = "Save",
    Image = Image.FromFile(@"save.png")
};

fileMenu.Items.Add(saveItem);
popupMenu.ParentBarItem.Items.Add(fileMenu);
this.commandBar1.PopupMenu = popupMenu;
```

## XPToolBar Integration

XPToolBar acts as a container for menu items within CommandBar. Menu items can ONLY be added via XPToolBar.

### Through designer

1. Drag `XPToolBar` from toolbox
2. Drop it directly on the `CommandBar` instance
3. XPToolBar appears as a strip within CommandBar
4. Add menu items to XPToolBar using its smart tags

### Through code

Create and configure XPToolBar:

```csharp
XPToolBar xpToolbar = new XPToolBar();
xpToolbar.Name = "xpToolbar1";

// Add to command bar
this.commandBar1.Controls.Add(xpToolbar);
```

### Adding items to XPToolBar

```csharp
XPToolBar xpToolbar = new XPToolBar();
xpToolbar.Name = "mainToolbar";

// Create bar items
BarItem fileItem = new BarItem();
fileItem.BarName = "fileItem";
fileItem.Text = "New";

BarItem editItem = new BarItem();
editItem.BarName = "editItem";
editItem.Text = "Open";

// Add items to toolbar
xpToolbar.Items.Add(fileItem);
xpToolbar.Items.Add(editItem);

// Add toolbar to command bar
this.commandBar1.Controls.Add(xpToolbar);
```

### Complete XPToolBar example

```csharp
private void SetupXPToolBar()
{
    XPToolBar xpToolbar = new XPToolBar();
    xpToolbar.Name = "mainToolbar";
    xpToolbar.ThemesEnabled = true;
    
    // File menu
    BarItem newItem = new BarItem() { BarName = "new", Text = "New" };
    BarItem openItem = new BarItem() { BarName = "open", Text = "Open" };
    BarItem saveItem = new BarItem() { BarName = "save", Text = "Save" };
    
    // Edit menu
    BarItem cutItem = new BarItem() { BarName = "cut", Text = "Cut" };
    BarItem copyItem = new BarItem() { BarName = "copy", Text = "Copy" };
    BarItem pasteItem = new BarItem() { BarName = "paste", Text = "Paste" };
    
    // Add items
    xpToolbar.Items.Add(newItem);
    xpToolbar.Items.Add(openItem);
    xpToolbar.Items.Add(saveItem);
    xpToolbar.Items.Add(new BarItem() { Text = "-" });  // Separator
    xpToolbar.Items.Add(cutItem);
    xpToolbar.Items.Add(copyItem);
    xpToolbar.Items.Add(pasteItem);
    
    // Wire up handlers
    newItem.Click += (s, e) => { MessageBox.Show("New clicked"); };
    openItem.Click += (s, e) => { MessageBox.Show("Open clicked"); };
    saveItem.Click += (s, e) => { MessageBox.Show("Save clicked"); };
    
    // Add to command bar
    this.commandBar1.Controls.Add(xpToolbar);
}
```

### XPToolBar with images

```csharp
// Create toolbar with images
XPToolBar xpToolbar = new XPToolBar();
xpToolbar.Name = "toolbar";

// Create items with images
ImageList imageList = new ImageList();
imageList.Images.Add(Image.FromFile(@"new.png"));
imageList.Images.Add(Image.FromFile(@"open.png"));
imageList.Images.Add(Image.FromFile(@"save.png"));

BarItem newItem = new BarItem() 
{ 
    BarName = "new", 
    Text = "New",
    Image = imageList.Images[0]
};

BarItem openItem = new BarItem() 
{ 
    BarName = "open", 
    Text = "Open",
    Image = imageList.Images[1]
};

xpToolbar.Items.Add(newItem);
xpToolbar.Items.Add(openItem);

this.commandBar1.Controls.Add(xpToolbar);
```

## Mixed control hosting

Combine multiple control types in one CommandBar:

```csharp
// Create command bar
CommandBar commandBar = new CommandBar();
commandBar.Text = "Advanced Toolbar";

// Add XPToolBar for menu items
XPToolBar xpToolbar = new XPToolBar();
xpToolbar.Items.Add(new BarItem() { Text = "File" });
xpToolbar.Items.Add(new BarItem() { Text = "Edit" });

// Add regular controls in panel
Panel controlPanel = new Panel();
controlPanel.Dock = DockStyle.Fill;

TextBox searchBox = new TextBox();
searchBox.Width = 150;

Button searchButton = new Button();
searchButton.Text = "Search";
searchButton.Width = 70;

controlPanel.Controls.Add(searchBox);
controlPanel.Controls.Add(searchButton);

// Add both to command bar
commandBar.Controls.Add(xpToolbar);
commandBar.Controls.Add(controlPanel);

commandBarController1.CommandBars.Add(commandBar);
```

## Troubleshooting

**Issue: Menu items not appearing in XPToolBar**

Solution: XPToolBar must be added to CommandBar before items are visible:

```csharp
// Correct: Add toolbar first
commandBar1.Controls.Add(xpToolbar);

// Then add items or wire events
xpToolbar.Items.Add(newItem);
```

**Issue: PopupMenu not showing when dropdown clicked**

Solution: Ensure PopupMenu is properly initialized and attached:

```csharp
// Verify PopupMenu is set
if (commandBar1.PopupMenu != null)
{
    Console.WriteLine("PopupMenu is attached");
}
else
{
    Console.WriteLine("PopupMenu not attached");
}
```

**Issue: Controls overlapping in mixed hosting**

Solution: Use proper container layout:

```csharp
// Use panels with correct sizing
Panel mainPanel = new Panel();
mainPanel.Dock = DockStyle.Fill;

// Add with calculated positions
control1.Location = new Point(0, 0);
control2.Location = new Point(control1.Width + 5, 0);

mainPanel.Controls.Add(control1);
mainPanel.Controls.Add(control2);

commandBar1.Controls.Add(mainPanel);
```
