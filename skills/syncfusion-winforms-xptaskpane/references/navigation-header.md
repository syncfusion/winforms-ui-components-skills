# Navigation and Header Controls

## Table of Contents
- [Header Structure Overview](#header-structure-overview)
- [Header Left Toolbar](#header-left-toolbar)
- [Header Right Toolbar](#header-right-toolbar)
- [Controlling Toolbar Item Visibility](#controlling-toolbar-item-visibility)
- [Customizing Toolbar Images](#customizing-toolbar-images)
- [Header Label Styling](#header-label-styling)

## Header Structure Overview

The XPTaskPane header contains two toolbars with navigation and control buttons:

```
┌─────────────────────────────────────────┐
│ [◀] [▶]         Header Title      [☰] [×] │  Header Bar
└─────────────────────────────────────────┘
│                                           │
│          Task Page Content                │
│                                           │
```

**Header Components:**
- **HeaderLeftToolbar** - Contains previous/next navigation buttons (left side)
- **HeaderRightToolbar** - Contains dropdown menu and close button (right side)
- **HeaderLabel** - Displays the title of the current page

## Header Left Toolbar

The left toolbar provides navigation between pages:

**Items in HeaderLeftToolbar:**
- `Items[0]` - Left Arrow button (previous page)
- `Items[1]` - Right Arrow button (next page)

```csharp
// Access header left toolbar
ToolBar leftToolbar = xpTaskPane1.HeaderLeftToolbar;

// Items[0] = Previous button
// Items[1] = Next button
```

**Functionality:**
- Click previous button to navigate to previous page
- Click next button to navigate to next page
- Buttons automatically enable/disable based on page position
- First page disables previous button
- Last page disables next button

## Header Right Toolbar

The right toolbar provides page selection and control options:

**Items in HeaderRightToolbar:**
- `Items[0]` - Header Menu (dropdown showing all pages)
- `Items[1]` - Close Button

```csharp
// Access header right toolbar
ToolBar rightToolbar = xpTaskPane1.HeaderRightToolbar;

// Items[0] = Menu/Dropdown
// Items[1] = Close button
```

**Functionality:**
- Menu shows list of all available pages
- User clicks menu to see dropdown of pages
- Click page name in dropdown to navigate to it
- Close button can close/hide the XPTaskPane

## Controlling Toolbar Item Visibility

Show or hide toolbar items individually:

**Hide Previous Button:**

```csharp
xpTaskPane1.HeaderLeftToolbar.Items[0].Visible = false;
```

**Hide Next Button:**

```csharp
xpTaskPane1.HeaderLeftToolbar.Items[1].Visible = false;
```

**Hide Dropdown Menu:**

```csharp
xpTaskPane1.HeaderRightToolbar.Items[0].Visible = false;
```

**Hide Close Button:**

```csharp
xpTaskPane1.HeaderRightToolbar.Items[1].Visible = false;
```

**VB.NET Examples:**

```vb
' Hide all navigation buttons
Me.xpTaskPane1.HeaderLeftToolbar.Items(0).Visible = False
Me.xpTaskPane1.HeaderLeftToolbar.Items(1).Visible = False

' Hide menu and close button
Me.xpTaskPane1.HeaderRightToolbar.Items(0).Visible = False
Me.xpTaskPane1.HeaderRightToolbar.Items(1).Visible = False
```

**Visibility Configuration Example:**

```csharp
public void ConfigureHeaderToolbars(bool showNavigation, bool showMenu, bool showClose)
{
    // Configure left toolbar (navigation)
    xpTaskPane1.HeaderLeftToolbar.Items[0].Visible = showNavigation; // Previous
    xpTaskPane1.HeaderLeftToolbar.Items[1].Visible = showNavigation; // Next

    // Configure right toolbar (menu and close)
    xpTaskPane1.HeaderRightToolbar.Items[0].Visible = showMenu;      // Menu
    xpTaskPane1.HeaderRightToolbar.Items[1].Visible = showClose;     // Close
}

// Call with desired configuration
ConfigureHeaderToolbars(true, true, true);  // Show all
ConfigureHeaderToolbars(false, true, false); // Show only menu
```

## Customizing Toolbar Images

Replace default toolbar button images with custom ones:

**Setup ImageList:**

```csharp
// Create ImageList with custom images
ImageList headerImages = new ImageList();
headerImages.Images.Add(Image.FromFile("prev.png"));      // Index 0
headerImages.Images.Add(Image.FromFile("next.png"));      // Index 1
headerImages.Images.Add(Image.FromFile("menu.png"));      // Index 2
```

**Apply Images to Previous Button:**

```csharp
xpTaskPane1.HeaderLeftToolbar.Items[0].ImageList = headerImages;
xpTaskPane1.HeaderLeftToolbar.Items[0].ImageIndex = 0; // prev.png
```

**Apply Images to Next Button:**

```csharp
xpTaskPane1.HeaderLeftToolbar.Items[1].ImageList = headerImages;
xpTaskPane1.HeaderLeftToolbar.Items[1].ImageIndex = 1; // next.png
```

**Apply Images to Menu Item:**

```csharp
// HeaderMenuItem provides direct access to menu item
xpTaskPane1.HeaderMenuItem.ImageList = headerImages;
xpTaskPane1.HeaderMenuItem.ImageIndex = 2; // menu.png
```

**Complete Custom Image Example:**

```csharp
private void SetCustomHeaderImages()
{
    // Create and populate ImageList
    ImageList navImages = new ImageList();
    navImages.Images.Add(Image.FromFile("arrow_left.png"));
    navImages.Images.Add(Image.FromFile("arrow_right.png"));
    navImages.Images.Add(Image.FromFile("dropdown.png"));

    // Apply to previous button
    xpTaskPane1.HeaderLeftToolbar.Items[0].ImageList = navImages;
    xpTaskPane1.HeaderLeftToolbar.Items[0].ImageIndex = 0;

    // Apply to next button
    xpTaskPane1.HeaderLeftToolbar.Items[1].ImageList = navImages;
    xpTaskPane1.HeaderLeftToolbar.Items[1].ImageIndex = 1;

    // Apply to menu
    xpTaskPane1.HeaderMenuItem.ImageList = navImages;
    xpTaskPane1.HeaderMenuItem.ImageIndex = 2;
}
```

## Header Label Styling

Customize the appearance of the header title text:

**Access HeaderLabel:**

```csharp
Label headerLabel = xpTaskPane1.HeaderLabel;
```

**Set Font:**

```csharp
xpTaskPane1.HeaderLabel.Font = new Font("Verdana", 9.75F, FontStyle.Bold);
```

```vb
Me.xpTaskPane1.HeaderLabel.Font = New Font("Verdana", 9.75F, FontStyle.Bold)
```

**Set Foreground Color:**

```csharp
xpTaskPane1.HeaderLabel.ForeColor = System.Drawing.Color.Navy;
```

```vb
Me.xpTaskPane1.HeaderLabel.ForeColor = System.Drawing.Color.Navy
```

**Complete Header Label Styling:**

```csharp
public void StyleHeaderLabel()
{
    // Font styling
    xpTaskPane1.HeaderLabel.Font = new Font("Segoe UI", 10F, FontStyle.Bold);
    
    // Color styling
    xpTaskPane1.HeaderLabel.ForeColor = Color.FromArgb(0, 102, 204); // Blue
    
    // Text alignment (if supported)
    xpTaskPane1.HeaderLabel.TextAlign = ContentAlignment.MiddleLeft;
}
```

## Navigation Button Behavior

**Automatic Button State:**

- Previous Button automatically disables on first page
- Next Button automatically disables on last page
- Buttons re-enable as user navigates

```csharp
// Example: First page on load
// Previous button: Enabled = false
// Next button: Enabled = true

// After clicking next to reach page 2 of 3
// Previous button: Enabled = true
// Next button: Enabled = true

// On last page
// Previous button: Enabled = true
// Next button: Enabled = false
```

## Menu Behavior

**Dropdown Menu Features:**

- Shows all page titles
- User selects page from menu
- Selection navigates to chosen page
- Current page shown in menu

```csharp
// Menu population is automatic
// All XPTaskPage.Title values appear in dropdown
// Selection updates SelectedPage property
```

## Navigation Patterns

**Pattern 1: Hide Navigation Buttons**

```csharp
// Only allow menu-based navigation
xpTaskPane1.HeaderLeftToolbar.Items[0].Visible = false;
xpTaskPane1.HeaderLeftToolbar.Items[1].Visible = false;
// Users must use dropdown menu
```

**Pattern 2: Read-Only Navigation**

```csharp
// Show all navigation but prevent manual page changes
xpTaskPane1.HeaderRightToolbar.Items[0].Visible = false;
// Programmatic navigation only via SelectedPage property
```

**Pattern 3: Minimal Header**

```csharp
// Hide everything except close button
xpTaskPane1.HeaderLeftToolbar.Items[0].Visible = false;
xpTaskPane1.HeaderLeftToolbar.Items[1].Visible = false;
xpTaskPane1.HeaderRightToolbar.Items[0].Visible = false;
// Only close functionality available
```

**Next:** Learn page ordering and navigation flow in page-reordering.md
