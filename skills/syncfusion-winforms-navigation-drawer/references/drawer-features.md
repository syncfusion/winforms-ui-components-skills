# Drawer Features and Configuration

This guide covers the core features of the Navigation Drawer, including ContentView, DrawerView, transitions, positioning, and animation control.

## Table of Contents
- [ContentView](#contentview)
- [DrawerView](#drawerview)
- [Transition Types](#transition-types)
  - [SlideOnTop](#slideontoP)
  - [Push](#push)
  - [Reveal](#reveal)
- [Position Configuration](#position-configuration)
- [Drawer Dimensions](#drawer-dimensions)
- [Toggle Drawer](#toggle-drawer)
- [Animation Duration](#animation-duration)
- [Best Practices](#best-practices)

## ContentView

The ContentView is the main view of the NavigationDrawer where you place your primary application content. When the drawer opens, it appears alongside or over this content area depending on the transition type.

### Adding Content to ContentView

Use the `ContentViewContainer` property to add controls to the main content area:

```csharp
// Create a RichTextBox for content
RichTextBox richTextBox = new RichTextBox();
richTextBox.Dock = DockStyle.Fill;
richTextBox.Text = "Content View\n\nThis is the main content area of the Navigation Drawer.";

// Add to ContentViewContainer
this.navigationDrawer1.ContentViewContainer.Controls.Add(richTextBox);
```

**VB.NET:**
```vb
' Create a RichTextBox for content
Dim richTextBox As New RichTextBox()
richTextBox.Dock = DockStyle.Fill
richTextBox.Text = "Content View" & vbLf & vbLf & "This is the main content area of the Navigation Drawer."

' Add to ContentViewContainer
Me.navigationDrawer1.ContentViewContainer.Controls.Add(richTextBox)
```

![ContentView with RichTextBox](../assets/Concepts-And-Features_img1.jpg)

### Adding Multiple Controls

```csharp
// Create a panel for complex layouts
Panel contentPanel = new Panel();
contentPanel.Dock = DockStyle.Fill;

// Add various controls
Label titleLabel = new Label();
titleLabel.Text = "Welcome to the Application";
titleLabel.Font = new Font("Segoe UI", 16, FontStyle.Bold);
titleLabel.Location = new Point(20, 20);
titleLabel.AutoSize = true;

TextBox searchBox = new TextBox();
searchBox.Location = new Point(20, 60);
searchBox.Width = 300;
searchBox.PlaceholderText = "Search...";

DataGridView dataGrid = new DataGridView();
dataGrid.Location = new Point(20, 100);
dataGrid.Size = new Size(600, 400);

// Add controls to panel
contentPanel.Controls.Add(titleLabel);
contentPanel.Controls.Add(searchBox);
contentPanel.Controls.Add(dataGrid);

// Add panel to ContentViewContainer
this.navigationDrawer1.ContentViewContainer.Controls.Add(contentPanel);
```

**Important Notes:**
- The ContentViewContainer is a standard Control container
- You can add any Windows Forms control to it
- Use `Dock = DockStyle.Fill` for controls that should occupy the entire content area
- The ContentView remains visible when the drawer is closed

## DrawerView

The DrawerView is the sliding panel that contains the drawer header and menu items. It appears when the drawer is opened and can be configured with various properties.

### DrawerView Structure

The DrawerView contains:
1. **DrawerHeader** - Optional header section at the top
2. **DrawerMenuItem** collection - Menu items displayed below the header

![DrawerView structure](../assets/navigationdrawer_img2.png)

### Item Collection

Access the Items collection to manage drawer content:

```csharp
// Add header
DrawerHeader header = new DrawerHeader { Text = "Menu" };
navigationDrawer1.Items.Add(header);

// Add menu items
DrawerMenuItem item1 = new DrawerMenuItem { Text = "Home" };
DrawerMenuItem item2 = new DrawerMenuItem { Text = "Settings" };
navigationDrawer1.Items.Add(item1);
navigationDrawer1.Items.Add(item2);

// Remove an item
navigationDrawer1.Items.Remove(item1);

// Clear all items
navigationDrawer1.Items.Clear();
```

## Transition Types

The Transition property specifies how the drawer panel animates when opening and closing. Three transition types are available.

### SlideOnTop

The drawer content slides on top of the ContentView, overlaying it without affecting the content's position.

```csharp
// Set SlideOnTop transition
this.navigationDrawer1.Transition = Transition.SlideOnTop;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Transition = Transition.SlideOnTop
```

![SlideOnTop transition](../assets/navigationdrawer_img2.png)

**Characteristics:**
- Drawer overlays the content
- Content remains stationary
- Most common transition type
- Best for temporary overlays that don't affect the main content

**Use Cases:**
- Mobile-style hamburger menus
- Quick access panels
- Temporary navigation overlays

### Push

The drawer and content view move simultaneously. As the drawer opens, it pushes the content view aside.

```csharp
// Set Push transition
this.navigationDrawer1.Transition = Transition.Push;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Transition = Transition.Push
```

![Push transition](../assets/navigationdrawer_img3.png)

**Characteristics:**
- Drawer and content move together
- Content is pushed to the side
- Creates a sense of spatial relationship
- Content remains fully visible but repositioned

**Use Cases:**
- Desktop applications with persistent navigation
- Applications where content should remain visible
- Side-by-side layouts

### Reveal

The drawer content remains in place while the ContentView slides to reveal the drawer beneath it.

```csharp
// Set Reveal transition
this.navigationDrawer1.Transition = Transition.Reveal;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Transition = Transition.Reveal
```

![Reveal transition](../assets/navigationdrawer_img4.png)

**Characteristics:**
- Drawer appears to be behind the content
- Content slides to reveal drawer
- Creates a layered effect
- Drawer appears stationary

**Use Cases:**
- Applications with layered UI metaphors
- Hidden settings panels
- Secondary navigation that should feel "underneath" the main content

### Transition Comparison

| Transition | Drawer Movement | Content Movement | Use Case |
|------------|----------------|------------------|----------|
| SlideOnTop | Slides in | Stationary | Mobile-style menus |
| Push | Slides in | Slides away | Desktop side-by-side |
| Reveal | Stationary | Slides away | Layered interfaces |

## Position Configuration

The Position property determines which edge of the form the drawer slides from.

### Left Position

```csharp
// Drawer slides from left edge
this.navigationDrawer1.Position = SlidePosition.Left;
```

![Left position](../assets/navigationdrawer_img5.png)

**Best for:**
- Primary navigation menus (LTR languages)
- Standard hamburger menu pattern
- Most common position in modern applications

### Right Position

```csharp
// Drawer slides from right edge
this.navigationDrawer1.Position = SlidePosition.Right;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Position = SlidePosition.Right
```

![Right position](../assets/navigationdrawer_img6.png)

**Best for:**
- Secondary navigation or options
- Settings panels
- Right-to-left (RTL) language applications
- Contextual actions

### Top Position

```csharp
// Drawer slides from top edge
this.navigationDrawer1.Position = SlidePosition.Top;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Position = SlidePosition.Top
```

![Top position](../assets/navigationdrawer_img7.png)

**Best for:**
- Notification panels
- Drop-down toolbars
- Search interfaces
- Horizontal menu layouts

### Bottom Position

```csharp
// Drawer slides from bottom edge
this.navigationDrawer1.Position = SlidePosition.Bottom;
```

**VB.NET:**
```vb
Me.navigationDrawer1.Position = SlidePosition.Bottom
```

![Bottom position](../assets/navigationdrawer_img8.png)

**Best for:**
- Player controls (media applications)
- Quick action panels
- Bottom sheets (mobile-style interfaces)
- Contextual menus

### Dynamic Position Changing

```csharp
// Change position based on user preference
private void ChangeDrawerPosition(SlidePosition position)
{
    navigationDrawer1.Position = position;
    
    // Adjust dimensions based on position
    if (position == SlidePosition.Left || position == SlidePosition.Right)
    {
        navigationDrawer1.DrawerWidth = 250;
        navigationDrawer1.DrawerHeight = this.Height;
    }
    else // Top or Bottom
    {
        navigationDrawer1.DrawerWidth = this.Width;
        navigationDrawer1.DrawerHeight = 200;
    }
}
```

## Drawer Dimensions

Configure the size of the drawer panel using DrawerWidth and DrawerHeight properties.

### Setting Dimensions

```csharp
// For left/right positioned drawers
this.navigationDrawer1.DrawerWidth = 250;
this.navigationDrawer1.DrawerHeight = this.Height;

// For top/bottom positioned drawers
this.navigationDrawer1.DrawerWidth = this.Width;
this.navigationDrawer1.DrawerHeight = 200;
```

### Responsive Dimensions

```csharp
// Make drawer width relative to form size
this.navigationDrawer1.DrawerWidth = this.Width / 4; // 25% of form width

// Handle form resize
this.Resize += (sender, e) =>
{
    if (navigationDrawer1.Position == SlidePosition.Left || 
        navigationDrawer1.Position == SlidePosition.Right)
    {
        navigationDrawer1.DrawerHeight = this.Height;
    }
    else
    {
        navigationDrawer1.DrawerWidth = this.Width;
    }
};
```

### Recommended Dimensions

**Left/Right Drawers:**
- Minimum: 200px width
- Typical: 250-300px width
- Maximum: 40% of form width
- Height: Match form height

**Top/Bottom Drawers:**
- Width: Match form width
- Minimum: 150px height
- Typical: 200-250px height
- Maximum: 40% of form height

## Toggle Drawer

The `ToggleDrawer()` method programmatically opens or closes the drawer.

### Basic Toggle

```csharp
// Toggle drawer visibility
this.navigationDrawer1.ToggleDrawer();
```

**VB.NET:**
```vb
Me.navigationDrawer1.ToggleDrawer()
```

### Toggle with Button

```csharp
// Add a hamburger button to toggle drawer
Button hamburgerButton = new Button();
hamburgerButton.Text = "☰";
hamburgerButton.Font = new Font("Segoe UI", 20);
hamburgerButton.Size = new Size(50, 50);
hamburgerButton.Click += (sender, e) => navigationDrawer1.ToggleDrawer();

this.Controls.Add(hamburgerButton);
```

### Toggle with Keyboard Shortcut

```csharp
// Handle keyboard shortcut (Ctrl+M)
protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
    if (keyData == (Keys.Control | Keys.M))
    {
        navigationDrawer1.ToggleDrawer();
        return true;
    }
    return base.ProcessCmdKey(ref msg, keyData);
}
```

### Conditional Toggle

```csharp
// Toggle only if certain conditions are met
private void ConditionalToggle()
{
    if (IsUserAuthenticated() && HasNavigationPermissions())
    {
        navigationDrawer1.ToggleDrawer();
    }
    else
    {
        MessageBox.Show("Navigation access denied.");
    }
}
```

## Animation Duration

The `AnimationDuration` property controls how quickly the drawer opens and closes, specified in milliseconds.

### Setting Animation Duration

```csharp
// Set animation duration to 150 milliseconds
this.navigationDrawer1.AnimationDuration = 150;
```

**VB.NET:**
```vb
Me.navigationDrawer1.AnimationDuration = 150
```

### Recommended Duration Values

- **Fast** (100-150ms): Snappy, responsive feel, good for frequently toggled drawers
- **Normal** (200-300ms): Balanced, smooth animation, most common
- **Slow** (400-500ms): Emphasizes the transition, good for important UI changes
- **Very Slow** (600ms+): Avoid unless specifically needed for accessibility

### Animation Duration Examples

```csharp
// Quick animation for mobile-style interactions
navigationDrawer1.AnimationDuration = 100;

// Standard smooth animation
navigationDrawer1.AnimationDuration = 250;

// Slower animation for emphasis
navigationDrawer1.AnimationDuration = 400;

// Instant (no animation)
navigationDrawer1.AnimationDuration = 0;
```

### User Preference Support

```csharp
// Allow users to adjust animation speed
public enum AnimationSpeed { Instant, Fast, Normal, Slow }

public void SetAnimationSpeed(AnimationSpeed speed)
{
    switch (speed)
    {
        case AnimationSpeed.Instant:
            navigationDrawer1.AnimationDuration = 0;
            break;
        case AnimationSpeed.Fast:
            navigationDrawer1.AnimationDuration = 100;
            break;
        case AnimationSpeed.Normal:
            navigationDrawer1.AnimationDuration = 250;
            break;
        case AnimationSpeed.Slow:
            navigationDrawer1.AnimationDuration = 500;
            break;
    }
}
```

## Best Practices

### Performance Optimization

```csharp
// Avoid heavy operations during drawer animation
navigationDrawer1.Opening += (sender, e) =>
{
    // Good: Lightweight UI updates
    statusLabel.Text = "Opening menu...";
};

navigationDrawer1.Opened += (sender, e) =>
{
    // Good: Heavy operations after animation completes
    LoadMenuData();
    UpdateMenuItems();
};
```

### Responsive Design

```csharp
// Adjust drawer for different form sizes
private void UpdateDrawerLayout()
{
    if (this.Width < 800)
    {
        // Smaller screens: full overlay
        navigationDrawer1.Transition = Transition.SlideOnTop;
        navigationDrawer1.DrawerWidth = this.Width * 3 / 4;
    }
    else
    {
        // Larger screens: push content
        navigationDrawer1.Transition = Transition.Push;
        navigationDrawer1.DrawerWidth = 300;
    }
}
```

### Accessibility Considerations

```csharp
// Ensure drawer is keyboard accessible
navigationDrawer1.TabStop = true;

// Provide clear visual feedback
navigationDrawer1.Opening += (sender, e) =>
{
    hamburgerButton.BackColor = Color.LightBlue;
};

navigationDrawer1.Closed += (sender, e) =>
{
    hamburgerButton.BackColor = SystemColors.Control;
};
```

### State Management

```csharp
// Track drawer state for app logic
private bool isDrawerOpen = false;

navigationDrawer1.Opened += (sender, e) => isDrawerOpen = true;
navigationDrawer1.Closed += (sender, e) => isDrawerOpen = false;

// Use state in application logic
private void PerformAction()
{
    if (isDrawerOpen)
    {
        // Close drawer before performing action
        navigationDrawer1.ToggleDrawer();
    }
    
    // Perform the action
    ExecuteMainAction();
}
```

## Complete Configuration Example

```csharp
private void ConfigureNavigationDrawer()
{
    // Basic setup
    navigationDrawer1.DrawerWidth = 280;
    navigationDrawer1.DrawerHeight = this.Height;
    
    // Set transition and position
    navigationDrawer1.Transition = Transition.SlideOnTop;
    navigationDrawer1.Position = SlidePosition.Left;
    
    // Configure animation
    navigationDrawer1.AnimationDuration = 200;
    
    // Add content to ContentView
    Panel mainContent = new Panel { Dock = DockStyle.Fill };
    mainContent.BackColor = Color.White;
    navigationDrawer1.ContentViewContainer.Controls.Add(mainContent);
    
    // Add header and items to DrawerView
    DrawerHeader header = new DrawerHeader { Text = "Main Menu" };
    navigationDrawer1.Items.Add(header);
    
    string[] menuItems = { "Dashboard", "Reports", "Analytics", "Settings" };
    foreach (string itemText in menuItems)
    {
        DrawerMenuItem item = new DrawerMenuItem { Text = itemText };
        navigationDrawer1.Items.Add(item);
    }
}
```

## Next Steps

- **Apply themes:** See [customization.md](customization.md) for styling options
- **Handle events:** See [events.md](events.md) for transition event handling
- **Advanced scenarios:** See [advanced-usage.md](advanced-usage.md) for complex use cases
