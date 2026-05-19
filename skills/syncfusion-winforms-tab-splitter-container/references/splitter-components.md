# TabSplitterContainer Components and Page Properties

## Table of Contents
- [Splitter Component Overview](#splitter-component-overview)
- [Primary Page Collection](#primary-page-collection)
- [Secondary Page Collection](#secondary-page-collection)
- [Swap Button Functionality](#swap-button-functionality)
- [Expand and Collapse Button](#expand-and-collapse-button)
- [Orientation Toggle Buttons](#orientation-toggle-buttons)
- [TabSplitterPage Properties](#tabsplitterpage-properties)
  - [Text Property](#text-property)
  - [Image Property](#image-property)
  - [ToolTip Property](#tooltip-property)
  - [BorderStyle Property](#borderstyle-property)
  - [BackgroundImage Properties](#backgroundimage-properties)
  - [Visible Property](#visible-property)
- [Runtime Page Manipulation](#runtime-page-manipulation)
- [Best Practices](#best-practices)

## Splitter Component Overview

The TabSplitterContainer is composed of several interactive components that work together to provide a flexible split-view interface:

**Visual Structure:**
```
┌─────────────────────────────────────────────────────────┐
│  [Tab1] [Tab2]              │  [Tab3] [Tab4]  [⇄][─][×] │
├─────────────────────────────┼─────────────────────────────┤
│                             │                             │
│   PRIMARY PAGES             │   SECONDARY PAGES           │
│   (Left or Top Pane)        │   (Right or Bottom Pane)    │
│                             │                             │
│                             │                             │
└─────────────────────────────┴─────────────────────────────┘
```

**Built-in Components:**
- **Primary Pages Tab Group**: Left (or top) collection of tabbed pages
- **Secondary Pages Tab Group**: Right (or bottom) collection of tabbed pages
- **Splitter Bar**: Draggable divider between the two pane groups
- **Swap Button** (⇄): Exchanges the positions of primary and secondary pages
- **Orientation Toggle Buttons**: Switch between horizontal and vertical layouts
- **Collapse/Expand Button** (─): Collapses the secondary pane to maximize primary pane

## Primary Page Collection

### PrimaryPages Property

The `PrimaryPages` property holds a collection of `TabSplitterPage` objects that appear in the primary (left or top) pane.

**Property Type:** `TabSplitterPagesCollection`

**Access Pattern:**
```csharp
// Access the collection
TabSplitterPagesCollection primaryPages = tabSplitterContainer1.PrimaryPages;

// Get page count
int pageCount = tabSplitterContainer1.PrimaryPages.Count;

// Access individual pages by index
TabSplitterPage firstPage = tabSplitterContainer1.PrimaryPages[0];
```

### Using the TabSplitterPage Collection Editor

At design time, use the Collection Editor for visual page management:

1. Select the TabSplitterContainer control
2. In the Properties window, find **PrimaryPages**
3. Click the ellipsis (...) button
4. The TabSplitterPage Collection Editor opens:
   - Click **Add** to create a new page
   - Select a page to edit its properties
   - Use **Remove** to delete selected pages
   - Use up/down arrows to reorder pages

### Adding Pages Programmatically

```csharp
private void AddPrimaryPages()
{
    // Method 1: Add individual pages
    TabSplitterPage page1 = new TabSplitterPage();
    page1.Text = "Editor";
    this.tabSplitterContainer1.PrimaryPages.Add(page1);
    
    // Method 2: AddRange for multiple pages
    TabSplitterPage page2 = new TabSplitterPage() { Text = "Preview" };
    TabSplitterPage page3 = new TabSplitterPage() { Text = "Outline" };
    
    this.tabSplitterContainer1.PrimaryPages.AddRange(new TabSplitterPage[] 
    { 
        page2, 
        page3 
    });
}
```

## Secondary Page Collection

### SecondaryPages Property

The `SecondaryPages` property manages the collection of pages in the secondary (right or bottom) pane.

**Property Type:** `TabSplitterPagesCollection`

**Usage Pattern:**
```csharp
private void ConfigureSecondaryPages()
{
    // Clear existing pages
    this.tabSplitterContainer1.SecondaryPages.Clear();
    
    // Create and add new pages
    TabSplitterPage designPage = new TabSplitterPage();
    designPage.Text = "Design";
    
    TabSplitterPage propertiesPage = new TabSplitterPage();
    propertiesPage.Text = "Properties";
    
    TabSplitterPage outputPage = new TabSplitterPage();
    outputPage.Text = "Output";
    
    // Add to secondary collection
    this.tabSplitterContainer1.SecondaryPages.AddRange(new TabSplitterPage[] 
    { 
        designPage, 
        propertiesPage, 
        outputPage 
    });
}
```

### Collection Operations

```csharp
// Insert at specific position
TabSplitterPage newPage = new TabSplitterPage() { Text = "Inserted Page" };
this.tabSplitterContainer1.SecondaryPages.Insert(1, newPage);

// Remove specific page
this.tabSplitterContainer1.SecondaryPages.Remove(newPage);

// Remove by index
this.tabSplitterContainer1.SecondaryPages.RemoveAt(0);

// Check if contains
if (this.tabSplitterContainer1.SecondaryPages.Contains(designPage))
{
    // Page exists in collection
}
```

## Swap Button Functionality

### Swapped Property

The `Swapped` property controls whether the primary and secondary page collections exchange positions.

**Property Type:** `bool`  
**Default Value:** `false`

**When to Use:**
- Allow users to customize their workspace layout
- Swap views based on user preference or workflow
- Provide keyboard shortcuts for quick swapping

### Code Examples

```csharp
// Swap the page collections
this.tabSplitterContainer1.Swapped = true;

// Toggle swap state
this.tabSplitterContainer1.Swapped = !this.tabSplitterContainer1.Swapped;

// Respond to user button click
private void btnSwapPanes_Click(object sender, EventArgs e)
{
    this.tabSplitterContainer1.Swapped = !this.tabSplitterContainer1.Swapped;
}

// Conditional swap based on document type
private void LoadDocument(Document doc)
{
    if (doc.Type == DocumentType.Design)
    {
        // Show design pane on left (primary)
        this.tabSplitterContainer1.Swapped = true;
    }
    else
    {
        // Show code pane on left (primary)
        this.tabSplitterContainer1.Swapped = false;
    }
}
```

### Visual Effect

When `Swapped = false`:
```
[Primary Pages]  │  [Secondary Pages]
```

When `Swapped = true`:
```
[Secondary Pages]  │  [Primary Pages]
```

## Expand and Collapse Button

### Collapsed Property

The `Collapsed` property controls the visibility of the secondary pane. When collapsed, the secondary pane is hidden, and the primary pane uses the full container area.

**Property Type:** `bool`  
**Default Value:** `false`

**When to Use:**
- Maximize workspace for primary content
- Implement focus mode for editing
- Hide secondary views when not needed
- Respond to screen size constraints

### Implementation Examples

```csharp
// Collapse the secondary pane
this.tabSplitterContainer1.Collapsed = true;

// Expand the secondary pane
this.tabSplitterContainer1.Collapsed = false;

// Toggle collapse state
this.tabSplitterContainer1.Collapsed = !this.tabSplitterContainer1.Collapsed;

// Implement focus mode toggle
private void btnFocusMode_Click(object sender, EventArgs e)
{
    this.tabSplitterContainer1.Collapsed = !this.tabSplitterContainer1.Collapsed;
    btnFocusMode.Text = this.tabSplitterContainer1.Collapsed 
        ? "Exit Focus Mode" 
        : "Enter Focus Mode";
}

// Auto-collapse based on form size
private void Form_Resize(object sender, EventArgs e)
{
    if (this.Width < 800)
    {
        this.tabSplitterContainer1.Collapsed = true;
    }
    else
    {
        this.tabSplitterContainer1.Collapsed = false;
    }
}
```

## Orientation Toggle Buttons

The TabSplitterContainer provides built-in buttons that allow users to toggle between horizontal and vertical orientations. These buttons are automatically displayed in the secondary page header area.

**Visual Buttons:**
- **Horizontal Button**: Arranges panes side-by-side
- **Vertical Button**: Arranges panes top-to-bottom

**Note:** While the buttons are provided automatically, you can also control orientation programmatically (see orientation-and-position.md for details).

## TabSplitterPage Properties

Each `TabSplitterPage` supports extensive customization through its properties.

### Text Property

The `Text` property sets the label displayed on the tab.

**Property Type:** `string`  
**Default Value:** Empty string

```csharp
TabSplitterPage page = new TabSplitterPage();
page.Text = "Code Editor";

// Dynamic text updates
private void UpdatePageTitle(TabSplitterPage page, string documentName)
{
    page.Text = documentName;
    
    // Indicate modified state
    if (IsDocumentModified(documentName))
    {
        page.Text += " *";
    }
}
```

### Image Property

The `Image` property adds an icon to the tab, providing visual identification.

**Property Type:** `System.Drawing.Image`  
**Default Value:** `null`

```csharp
// Set image from resources
TabSplitterPage codePage = new TabSplitterPage();
codePage.Text = "Code";
codePage.Image = Properties.Resources.CodeIcon;

// Set image from file
TabSplitterPage designPage = new TabSplitterPage();
designPage.Text = "Design";
designPage.Image = Image.FromFile(@"C:\Icons\design.png");

// Set image with ImageTransparentColor
TabSplitterPage page = new TabSplitterPage();
page.Text = "Document";
page.Image = Image.FromFile(@"C:\Icons\document.bmp");
page.ImageTransparentColor = Color.Magenta; // Make magenta transparent
```

### ImageTransparentColor Property

When using bitmap images with a specific background color, use `ImageTransparentColor` to make that color transparent.

**Property Type:** `System.Drawing.Color`  
**Default Value:** `Color.Empty`

```csharp
TabSplitterPage page = new TabSplitterPage();
page.Image = LoadBitmapIcon();
page.ImageTransparentColor = Color.White; // Make white background transparent
```

### ToolTip Property

The `ToolTip` property displays helpful text when users hover over a tab.

**Property Type:** `string`  
**Default Value:** Empty string

```csharp
TabSplitterPage xmlPage = new TabSplitterPage();
xmlPage.Text = "XML";
xmlPage.ToolTip = "XML Source View - Shows the raw XML markup";

// Dynamic tooltips with context
private void SetupPageWithTooltip(TabSplitterPage page, string title, string description)
{
    page.Text = title;
    page.ToolTip = $"{title} - {description}";
}

// Example usage
TabSplitterPage propertiesPage = new TabSplitterPage();
SetupPageWithTooltip(propertiesPage, "Properties", 
    "View and edit document properties and metadata");
```

### BorderStyle Property

The `BorderStyle` property controls the appearance of the page border.

**Property Type:** `System.Windows.Forms.BorderStyle`  
**Default Value:** `BorderStyle.Fixed3D`

**Options:**
- `BorderStyle.None`: No border
- `BorderStyle.FixedSingle`: Single-line border
- `BorderStyle.Fixed3D`: 3D border (default)

```csharp
// Remove border for clean appearance
TabSplitterPage page1 = new TabSplitterPage();
page1.BorderStyle = BorderStyle.None;

// Single line border
TabSplitterPage page2 = new TabSplitterPage();
page2.BorderStyle = BorderStyle.FixedSingle;

// 3D border (default)
TabSplitterPage page3 = new TabSplitterPage();
page3.BorderStyle = BorderStyle.Fixed3D;
```

**When to Use Each Style:**
- **None**: Modern flat design, embedded controls with their own borders
- **FixedSingle**: Minimalist appearance, clear page boundaries
- **Fixed3D**: Traditional Windows look, emphasizes depth

### BackgroundImage Properties

Customize the page background with images using `BackgroundImage` and `BackgroundImageLayout`.

**Property Type:** `System.Drawing.Image`  
**Layout Type:** `System.Windows.Forms.ImageLayout`

```csharp
TabSplitterPage page = new TabSplitterPage();
page.Text = "Welcome";

// Set background image
page.BackgroundImage = Image.FromFile(@"C:\Images\background.png");

// Configure layout
page.BackgroundImageLayout = ImageLayout.Stretch;
```

**ImageLayout Options:**
- `ImageLayout.None`: Image at default size in upper-left corner
- `ImageLayout.Tile`: Repeats the image to fill the page
- `ImageLayout.Center`: Centers the image at original size
- `ImageLayout.Stretch`: Stretches to fill the entire page
- `ImageLayout.Zoom`: Scales while maintaining aspect ratio

```csharp
// Tile a pattern
TabSplitterPage patternPage = new TabSplitterPage();
patternPage.BackgroundImage = Properties.Resources.Pattern;
patternPage.BackgroundImageLayout = ImageLayout.Tile;

// Centered logo
TabSplitterPage logoPage = new TabSplitterPage();
logoPage.BackgroundImage = Properties.Resources.CompanyLogo;
logoPage.BackgroundImageLayout = ImageLayout.Center;

// Full-screen wallpaper
TabSplitterPage wallpaperPage = new TabSplitterPage();
wallpaperPage.BackgroundImage = Properties.Resources.Wallpaper;
wallpaperPage.BackgroundImageLayout = ImageLayout.Stretch;
```

### Visible Property

The `Visible` property controls whether a page is displayed in the tab collection.

**Property Type:** `bool`  
**Default Value:** `true`

**When to Use:**
- Conditionally show/hide pages based on user permissions
- Hide pages during certain workflow states
- Implement feature toggles
- Show pages only when relevant content is available

```csharp
// Hide a page
TabSplitterPage adminPage = new TabSplitterPage();
adminPage.Text = "Admin";
adminPage.Visible = false;

// Show page conditionally
private void CheckUserPermissions()
{
    if (CurrentUser.IsAdmin)
    {
        adminPage.Visible = true;
    }
    else
    {
        adminPage.Visible = false;
    }
}

// Toggle page visibility
private void btnTogglePage_Click(object sender, EventArgs e)
{
    debugPage.Visible = !debugPage.Visible;
}

// Show pages based on document state
private void OnDocumentOpened(Document doc)
{
    // Show design page only for visual documents
    designPage.Visible = doc.SupportsVisualDesign;
    
    // Show properties page only if document has metadata
    propertiesPage.Visible = doc.HasMetadata;
}
```

## Runtime Page Manipulation

### Adding Pages Dynamically

```csharp
private void AddNewPage(string title, Control content, bool isPrimary = true)
{
    TabSplitterPage page = new TabSplitterPage();
    page.Text = title;
    page.Controls.Add(content);
    
    if (isPrimary)
    {
        this.tabSplitterContainer1.PrimaryPages.Add(page);
    }
    else
    {
        this.tabSplitterContainer1.SecondaryPages.Add(page);
    }
}

// Example usage
private void btnAddCodeView_Click(object sender, EventArgs e)
{
    TextBox codeEditor = new TextBox();
    codeEditor.Multiline = true;
    codeEditor.Dock = DockStyle.Fill;
    
    AddNewPage("Code View", codeEditor, isPrimary: true);
}
```

### Removing Pages Dynamically

```csharp
private void RemovePageByTitle(string title)
{
    // Search in primary pages
    TabSplitterPage pageToRemove = null;
    foreach (TabSplitterPage page in this.tabSplitterContainer1.PrimaryPages)
    {
        if (page.Text == title)
        {
            pageToRemove = page;
            break;
        }
    }
    
    if (pageToRemove != null)
    {
        this.tabSplitterContainer1.PrimaryPages.Remove(pageToRemove);
        pageToRemove.Dispose();
        return;
    }
    
    // Search in secondary pages
    foreach (TabSplitterPage page in this.tabSplitterContainer1.SecondaryPages)
    {
        if (page.Text == title)
        {
            pageToRemove = page;
            break;
        }
    }
    
    if (pageToRemove != null)
    {
        this.tabSplitterContainer1.SecondaryPages.Remove(pageToRemove);
        pageToRemove.Dispose();
    }
}
```

### Reordering Pages

```csharp
private void MovePageToFront(TabSplitterPage page)
{
    if (this.tabSplitterContainer1.PrimaryPages.Contains(page))
    {
        this.tabSplitterContainer1.PrimaryPages.Remove(page);
        this.tabSplitterContainer1.PrimaryPages.Insert(0, page);
    }
}

private void SwapPages(int index1, int index2)
{
    var pages = this.tabSplitterContainer1.PrimaryPages;
    
    if (index1 >= 0 && index1 < pages.Count && 
        index2 >= 0 && index2 < pages.Count)
    {
        TabSplitterPage temp = pages[index1];
        pages[index1] = pages[index2];
        pages[index2] = temp;
    }
}
```

## Best Practices

### Page Management
1. **Dispose pages properly** when removing them to prevent memory leaks
2. **Use meaningful Text values** for accessibility and user experience
3. **Limit the number of pages** per collection (typically 3-7 for optimal UX)
4. **Set ToolTips** for pages with abbreviated Text values

### Performance Optimization
1. **Use Visible property** instead of adding/removing pages frequently
2. **Lazy-load page content** - only populate controls when the page is first accessed
3. **Dispose of complex controls** when pages are hidden or removed

### Visual Consistency
1. **Use consistent image sizes** for page icons (16x16 or 24x24 recommended)
2. **Apply ImageTransparentColor** consistently across bitmap icons
3. **Choose BorderStyle** based on application theme
4. **Use BackgroundImage sparingly** - it can affect readability

### User Experience
1. **Preserve user's Swapped and Collapsed preferences** across sessions
2. **Provide keyboard shortcuts** for common actions (swap, collapse, page switching)
3. **Update page Text dynamically** to reflect content state (e.g., modified indicator)
4. **Show/hide pages contextually** using the Visible property rather than removing them
