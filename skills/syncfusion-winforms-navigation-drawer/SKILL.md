---
name: syncfusion-winforms-navigation-drawer
description: Implement Syncfusion NavigationDrawer control in Windows Forms - a sliding panel menu that appears from screen edges. Use when creating drawer navigation, side menus, or hamburger menus with panel transitions. Covers DrawerMenuItem configuration, DrawerHeader setup, slide-out menus, and panel transitions (SlideOnTop, Push, Reveal) for modern sliding panel navigation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Windows Forms Navigation Drawer

The Syncfusion Windows Forms Navigation Drawer is a sliding panel menu that comes out from the edge of the window, allowing content to be displayed in a hidden panel. It can be shown by swiping from any of the four screen edges or opened programmatically on demand.

## When to Use This Skill

Use this skill when you need to:

- **Implement sliding panel menus** from screen edges (left, right, top, or bottom)
- **Create hamburger menu navigation** with collapsible sidebar panels
- **Build hidden panels** that slide in with smooth animations
- **Add drawer-based navigation** to Windows Forms applications
- **Configure transition effects** (SlideOnTop, Push, Reveal)
- **Customize drawer appearance** with built-in Office 2016 themes
- **Handle drawer events** (Opening, Closing, Opened, Closed)
- **Toggle drawer visibility** programmatically or through user interaction
- **Position navigation panels** at different screen edges with configurable animations

## Component Overview

**Key Features:**
- **Four position options:** Left, Right, Top, Bottom
- **Three transition types:** SlideOnTop, Push, Reveal
- **Built-in themes:** Default, Office2016Colorful, Office2016White, Office2016DarkGray, Office2016Black
- **Drawer events:** Opening, Closing, Opened, Closed
- **Configurable animation duration**
- **Toggle drawer method** for programmatic control
- **Header and menu items** with image support

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when you need to:
- Install the NavigationDrawer control via NuGet or designer
- Add the control to a Windows Forms project
- Create a NavigationDrawer instance programmatically
- Add header (DrawerHeader) and menu items (DrawerMenuItem)
- Set up basic drawer dimensions (DrawerWidth, DrawerHeight)
- Add images to menu items
- Configure text and image positioning (TextAlign, TextImageRelation)
- Understand basic sidebar placement

### Drawer Features and Configuration
📄 **Read:** [references/drawer-features.md](references/drawer-features.md)

Read this reference when you need to:
- Configure ContentView (main view content)
- Understand DrawerView structure
- Implement transition types (SlideOnTop, Push, Reveal)
- Set drawer position (Left, Right, Top, Bottom)
- Configure drawer dimensions and animation duration
- Use ToggleDrawer method for programmatic control
- Understand the differences between transition effects

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)

Read this reference when you need to:
- Apply built-in themes (Default, Office2016Colorful, Office2016White, Office2016DarkGray, Office2016Black)
- Customize drawer item colors (DefaultColor, BackColor, HoverColor)
- Style menu items with custom colors
- Apply consistent visual themes across the application

### Events
📄 **Read:** [references/events.md](references/events.md)

Read this reference when you need to:
- Handle Opening event (when expand transition begins)
- Handle Closing event (when collapse transition begins)
- Handle Opened event (when expand transition ends)
- Handle Closed event (when collapse transition ends)
- Respond to drawer state changes
- Cancel drawer operations
- Implement custom logic during transitions

### Advanced Usage
📄 **Read:** [references/advanced-usage.md](references/advanced-usage.md)

Read this reference when you need to:
- Configure ContentViewContainer with complex controls
- Integrate RichTextBox or other controls in ContentView
- Manage dynamic item addition/removal
- Implement complex item hierarchies
- Optimize performance for large menus
- Follow best practices for drawer implementation

## Quick Start

### Basic Navigation Drawer

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create NavigationDrawer instance
NavigationDrawer navigationDrawer1 = new NavigationDrawer();
this.Controls.Add(navigationDrawer1);

// Set drawer dimensions
navigationDrawer1.DrawerWidth = this.Width / 4;
navigationDrawer1.DrawerHeight = this.Height;

// Set position and transition
navigationDrawer1.Position = SlidePosition.Left;
navigationDrawer1.Transition = Transition.SlideOnTop;

// Add header
DrawerHeader drawerHeader1 = new DrawerHeader();
drawerHeader1.Text = "Menu";
navigationDrawer1.Items.Add(drawerHeader1);

// Add menu items
DrawerMenuItem menuItem1 = new DrawerMenuItem();
menuItem1.Text = "Home";
menuItem1.Image = Image.FromFile(@"home.png");

DrawerMenuItem menuItem2 = new DrawerMenuItem();
menuItem2.Text = "Settings";
menuItem2.Image = Image.FromFile(@"settings.png");

navigationDrawer1.Items.Add(menuItem1);
navigationDrawer1.Items.Add(menuItem2);

// Add content to ContentView
RichTextBox richTextBox = new RichTextBox();
richTextBox.Text = "Main content area";
navigationDrawer1.ContentViewContainer.Controls.Add(richTextBox);
```

## Common Patterns

### Pattern 1: Left Slide Panel with Push Transition

```csharp
// Create drawer that pushes content when opened
NavigationDrawer drawer = new NavigationDrawer();
drawer.Position = SlidePosition.Left;
drawer.Transition = Transition.Push;
drawer.DrawerWidth = 250;
drawer.DrawerHeight = this.Height;
drawer.AnimationDuration = 150; // milliseconds

this.Controls.Add(drawer);
```

### Pattern 2: Toggle Drawer with Button Click

```csharp
// Toggle drawer visibility on button click
private void hamburgerButton_Click(object sender, EventArgs e)
{
    navigationDrawer1.ToggleDrawer();
}
```

### Pattern 3: Themed Drawer with Custom Colors

```csharp
// Apply Office 2016 theme with custom item colors
navigationDrawer1.Style = NavigationDrawerStyle.Office2016Colorful;

// Customize menu item colors
drawerMenuItem1.DefaultColor = System.Drawing.Color.LightBlue;
drawerMenuItem1.HoverColor = System.Drawing.Color.SkyBlue;
drawerMenuItem1.BackColor = System.Drawing.Color.LightBlue;
```

### Pattern 4: Handle Drawer Events

```csharp
// Subscribe to drawer events
navigationDrawer1.Opening += NavigationDrawer1_Opening;
navigationDrawer1.Opened += NavigationDrawer1_Opened;
navigationDrawer1.Closing += NavigationDrawer1_Closing;
navigationDrawer1.Closed += NavigationDrawer1_Closed;

private void NavigationDrawer1_Opening(object sender, OpeningEventArgs e)
{
    // Logic when drawer starts opening
    Console.WriteLine("Drawer is opening...");
}

private void NavigationDrawer1_Opened(object sender, EventArgs e)
{
    // Logic when drawer is fully open
    Console.WriteLine("Drawer opened");
}

private void NavigationDrawer1_Closing(object sender, CancelEventArgs e)
{
    // Can cancel closing if needed
    // e.Cancel = true;
    Console.WriteLine("Drawer is closing...");
}

private void NavigationDrawer1_Closed(object sender, EventArgs e)
{
    // Logic when drawer is fully closed
    Console.WriteLine("Drawer closed");
}
```

### Pattern 5: Right-Side Drawer with Reveal Transition

```csharp
// Create drawer that reveals from behind content
NavigationDrawer drawer = new NavigationDrawer();
drawer.Position = SlidePosition.Right;
drawer.Transition = Transition.Reveal;
drawer.DrawerWidth = 300;
drawer.AnimationDuration = 200;

// Add items
DrawerHeader header = new DrawerHeader { Text = "Options" };
drawer.Items.Add(header);

string[] menuItems = { "Profile", "Settings", "Notifications", "Help" };
foreach (string item in menuItems)
{
    DrawerMenuItem menuItem = new DrawerMenuItem { Text = item };
    drawer.Items.Add(menuItem);
}

this.Controls.Add(drawer);
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Position` | `SlidePosition` | Drawer sliding position: Left, Right, Top, Bottom |
| `Transition` | `Transition` | Animation type: SlideOnTop, Push, Reveal |
| `DrawerWidth` | `int` | Width of the drawer panel |
| `DrawerHeight` | `int` | Height of the drawer panel |
| `AnimationDuration` | `int` | Duration in milliseconds for drawer animation |
| `Style` | `NavigationDrawerStyle` | Visual theme: Default, Office2016Colorful, etc. |
| `Items` | `Collection` | Collection of DrawerHeader and DrawerMenuItem objects |
| `ContentViewContainer` | `Control` | Container for main content area controls |

## Key Methods

| Method | Description |
|--------|-------------|
| `ToggleDrawer()` | Toggles drawer visibility (open ↔ closed) |

## Key Events

| Event | Description |
|-------|-------------|
| `Opening` | Occurs when expand transition begins |
| `Opened` | Occurs when expand transition ends |
| `Closing` | Occurs when collapse transition begins (cancelable) |
| `Closed` | Occurs when collapse transition ends |

## Related Components

- **TreeNavigator** - For hierarchical tree-based navigation
- **Wizard** - For multi-step workflows and wizards
- **TabControlAdv** - For tabbed navigation interfaces
