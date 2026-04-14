# Advanced Features of TabSplitterContainer

## Table of Contents
- [Overview](#overview)
- [Events](#events)
- [Localization Support](#localization-support)
- [Runtime Manipulation](#runtime-manipulation)
- [Dynamic Page Management](#dynamic-page-management)
- [Best Practices](#best-practices)
- [Performance Considerations](#performance-considerations)
- [Troubleshooting](#troubleshooting)

## Overview

This guide covers advanced features of TabSplitterContainer including event handling, localization, runtime manipulation of pages and splitter state, dynamic page management, and performance optimization techniques.

## Events

TabSplitterContainer provides various events to respond to user interactions and state changes.

### Common Events

While specific event documentation varies, typical events include:

**State Change Events:**
- Events fired when splitter position changes
- Events fired when collapsed state changes
- Events fired when swapped state changes
- Page selection change events

**Usage Pattern:**

```csharp
// Handle state changes
tabSplitterContainer1.PropertyChanged += (s, e) => {
    // Respond to property changes
    Console.WriteLine($"Property changed: {e.PropertyName}");
};
```

### Event Best Practices

1. **Unsubscribe Events:** Always unsubscribe from events when disposing controls
2. **Avoid Heavy Processing:** Keep event handlers lightweight
3. **Use Async When Needed:** For long-running operations, use async/await
4. **Check Sender:** Verify the sender is the expected control

```csharp
// Proper event subscription and cleanup
public class MyForm : Form
{
    private TabSplitterContainer splitter;
    
    public MyForm()
    {
        splitter = new TabSplitterContainer();
        // Subscribe to events
        this.FormClosing += OnFormClosing;
    }
    
    private void OnFormClosing(object sender, FormClosingEventArgs e)
    {
        // Unsubscribe and cleanup
        if (splitter != null)
        {
            // Unsubscribe from custom events here
            splitter.Dispose();
        }
    }
}
```

## Localization Support

TabSplitterContainer supports localization for international applications.

### Setting Culture

Configure the control's culture to display UI elements in different languages:

```csharp
using System.Globalization;

// Set culture to French
tabSplitterContainer1.Culture = new CultureInfo("fr-FR");

// Set culture to German
tabSplitterContainer1.Culture = new CultureInfo("de-DE");

// Set culture to Spanish
tabSplitterContainer1.Culture = new CultureInfo("es-ES");
```

### Localizing Tab Page Text

For complete localization, also localize TabSplitterPage text:

```csharp
// Resource-based localization
public void LocalizePages()
{
    // Assuming you have resource files (Resources.resx, Resources.fr-FR.resx, etc.)
    xamlPage.Text = Resources.XamlTabText;  // "XAML" or "XAML (code)"
    designPage.Text = Resources.DesignTabText;  // "Design" or "Conception"
    previewPage.Text = Resources.PreviewTabText;  // "Preview" or "Aperçu"
}
```

### Culture-Aware Applications

```csharp
public class LocalizedEditor : Form
{
    private TabSplitterContainer splitter;
    
    public LocalizedEditor(CultureInfo culture)
    {
        splitter = new TabSplitterContainer();
        splitter.Dock = DockStyle.Fill;
        
        // Apply culture
        splitter.Culture = culture;
        
        // Create localized pages
        TabSplitterPage codePage = new TabSplitterPage();
        codePage.Text = GetLocalizedString("CodeTab", culture);
        
        TabSplitterPage previewPage = new TabSplitterPage();
        previewPage.Text = GetLocalizedString("PreviewTab", culture);
        
        splitter.PrimaryPages.Add(codePage);
        splitter.SecondaryPages.Add(previewPage);
        
        this.Controls.Add(splitter);
    }
    
    private string GetLocalizedString(string key, CultureInfo culture)
    {
        // Implement resource lookup based on culture
        // Return localized string from resource files
        return key;  // Placeholder
    }
}
```

## Runtime Manipulation

### Programmatic Swap

Swap primary and secondary tab groups at runtime:

```csharp
// Swap tab groups
tabSplitterContainer1.Swapped = true;

// Restore original positions
tabSplitterContainer1.Swapped = false;

// Toggle swap on button click
private void btnSwap_Click(object sender, EventArgs e)
{
    tabSplitterContainer1.Swapped = !tabSplitterContainer1.Swapped;
    btnSwap.Text = tabSplitterContainer1.Swapped ? "Restore" : "Swap";
}
```

**When to use:**
- User preference for layout (some users prefer preview on top/left)
- Different workflow modes
- Accessibility requirements

### Programmatic Collapse/Expand

Control the collapsed state of the secondary pane:

```csharp
// Collapse secondary pane
tabSplitterContainer1.Collapsed = true;

// Expand secondary pane
tabSplitterContainer1.Collapsed = false;

// Toggle collapse
private void btnTogglePane_Click(object sender, EventArgs e)
{
    tabSplitterContainer1.Collapsed = !tabSplitterContainer1.Collapsed;
    btnTogglePane.Text = tabSplitterContainer1.Collapsed ? "Show Preview" : "Hide Preview";
}
```

**Use cases:**
- Maximize editing space
- Focus mode (hide distractions)
- Optional preview pane
- Context-sensitive panels

### Combined Runtime Control

```csharp
public class FlexibleEditor : Form
{
    private TabSplitterContainer splitter;
    private Button btnSwap;
    private Button btnCollapse;
    private CheckBox chkVertical;
    
    private void SetupControls()
    {
        // Swap button
        btnSwap.Click += (s, e) => {
            splitter.Swapped = !splitter.Swapped;
        };
        
        // Collapse button
        btnCollapse.Click += (s, e) => {
            splitter.Collapsed = !splitter.Collapsed;
            btnCollapse.Text = splitter.Collapsed ? "Expand" : "Collapse";
        };
        
        // Orientation toggle
        chkVertical.CheckedChanged += (s, e) => {
            splitter.Orientation = chkVertical.Checked ? 
                Orientation.Vertical : Orientation.Horizontal;
        };
    }
}
```

## Dynamic Page Management

### Adding Pages at Runtime

Add new pages dynamically based on user actions or application state:

```csharp
// Add new primary page
private void AddNewDocument()
{
    TabSplitterPage newPage = new TabSplitterPage();
    newPage.Text = $"Document {tabSplitterContainer1.PrimaryPages.Count + 1}";
    newPage.BackColor = Color.White;
    
    // Add editor control
    TextBox editor = new TextBox 
    { 
        Dock = DockStyle.Fill, 
        Multiline = true,
        ScrollBars = ScrollBars.Both
    };
    newPage.Controls.Add(editor);
    
    // Add to collection
    tabSplitterContainer1.PrimaryPages.Add(newPage);
}

// Add new secondary page
private void AddToolWindow(string title, Control content)
{
    TabSplitterPage toolPage = new TabSplitterPage();
    toolPage.Text = title;
    toolPage.BackColor = SystemColors.Control;
    
    // Add content
    content.Dock = DockStyle.Fill;
    toolPage.Controls.Add(content);
    
    // Add to secondary collection
    tabSplitterContainer1.SecondaryPages.Add(toolPage);
}
```

### Removing Pages at Runtime

Remove pages when they're no longer needed:

```csharp
// Remove specific page
private void CloseDocument(TabSplitterPage page)
{
    if (tabSplitterContainer1.PrimaryPages.Contains(page))
    {
        // Dispose controls to free resources
        foreach (Control ctrl in page.Controls)
        {
            ctrl.Dispose();
        }
        
        // Remove page
        tabSplitterContainer1.PrimaryPages.Remove(page);
        page.Dispose();
    }
}

// Remove all pages from a collection
private void ClearAllDocuments()
{
    // Clear and dispose
    foreach (TabSplitterPage page in tabSplitterContainer1.PrimaryPages)
    {
        foreach (Control ctrl in page.Controls)
        {
            ctrl.Dispose();
        }
        page.Dispose();
    }
    
    tabSplitterContainer1.PrimaryPages.Clear();
}

// Remove currently selected page
private void CloseCurrentDocument()
{
    // Note: You'll need to track which page is currently selected
    // This is a simplified example
    if (tabSplitterContainer1.PrimaryPages.Count > 0)
    {
        TabSplitterPage currentPage = tabSplitterContainer1.PrimaryPages[0];
        CloseDocument(currentPage);
    }
}
```

### Page Visibility Control

Show or hide specific pages without removing them:

```csharp
// Hide a page
designPage.Visible = false;

// Show a page
designPage.Visible = true;

// Toggle page visibility
private void TogglePageVisibility(TabSplitterPage page)
{
    page.Visible = !page.Visible;
}

// Show page based on condition
private void ShowPropertiesIfNeeded()
{
    // Show properties page only if an item is selected
    bool itemSelected = CheckIfItemSelected();
    propertiesPage.Visible = itemSelected;
}
```

### Managing Page Collections

```csharp
public class DocumentManager
{
    private TabSplitterContainer splitter;
    private int documentCounter = 0;
    
    // Add new document
    public TabSplitterPage CreateNewDocument()
    {
        documentCounter++;
        
        TabSplitterPage page = new TabSplitterPage();
        page.Text = $"Untitled-{documentCounter}";
        page.Tag = documentCounter;  // Store ID in Tag
        
        TextBox editor = new TextBox 
        { 
            Dock = DockStyle.Fill,
            Multiline = true,
            Font = new Font("Consolas", 10f)
        };
        page.Controls.Add(editor);
        
        splitter.PrimaryPages.Add(page);
        return page;
    }
    
    // Find page by ID
    public TabSplitterPage FindDocument(int id)
    {
        foreach (TabSplitterPage page in splitter.PrimaryPages)
        {
            if (page.Tag is int pageId && pageId == id)
            {
                return page;
            }
        }
        return null;
    }
    
    // Get page count
    public int GetDocumentCount()
    {
        return splitter.PrimaryPages.Count;
    }
    
    // Check if any documents are open
    public bool HasOpenDocuments()
    {
        return splitter.PrimaryPages.Count > 0;
    }
}
```

## Best Practices

### 1. Lazy Loading Page Content

Load page content only when needed to improve performance:

```csharp
public class LazyLoadEditor : Form
{
    private TabSplitterPage previewPage;
    private bool previewLoaded = false;
    
    private void OnPreviewPageSelected()
    {
        if (!previewLoaded)
        {
            LoadPreviewContent();
            previewLoaded = true;
        }
    }
    
    private void LoadPreviewContent()
    {
        // Load heavy content only when needed
        WebBrowser browser = new WebBrowser { Dock = DockStyle.Fill };
        previewPage.Controls.Add(browser);
        browser.DocumentText = GeneratePreview();
    }
}
```

### 2. Memory Management

Properly dispose resources to prevent memory leaks:

```csharp
public class MemoryEfficientEditor : Form
{
    private TabSplitterContainer splitter;
    
    protected override void Dispose(bool disposing)
    {
        if (disposing)
        {
            // Dispose all pages and their controls
            DisposePageCollection(splitter.PrimaryPages);
            DisposePageCollection(splitter.SecondaryPages);
            
            splitter?.Dispose();
        }
        base.Dispose(disposing);
    }
    
    private void DisposePageCollection(TabSplitterPageCollection pages)
    {
        foreach (TabSplitterPage page in pages)
        {
            // Dispose all controls in the page
            foreach (Control ctrl in page.Controls)
            {
                ctrl.Dispose();
            }
            page.Dispose();
        }
        pages.Clear();
    }
}
```

### 3. State Persistence

Save and restore splitter state across sessions:

```csharp
using System.Configuration;

public class StatefulEditor : Form
{
    private void SaveState()
    {
        // Save splitter state to settings
        Properties.Settings.Default.SplitterPosition = 
            tabSplitterContainer1.SplitterPosition;
        Properties.Settings.Default.IsCollapsed = 
            tabSplitterContainer1.Collapsed;
        Properties.Settings.Default.IsSwapped = 
            tabSplitterContainer1.Swapped;
        Properties.Settings.Default.IsVertical = 
            (tabSplitterContainer1.Orientation == Orientation.Vertical);
        
        Properties.Settings.Default.Save();
    }
    
    private void RestoreState()
    {
        // Restore splitter state from settings
        tabSplitterContainer1.SplitterPosition = 
            Properties.Settings.Default.SplitterPosition;
        tabSplitterContainer1.Collapsed = 
            Properties.Settings.Default.IsCollapsed;
        tabSplitterContainer1.Swapped = 
            Properties.Settings.Default.IsSwapped;
        tabSplitterContainer1.Orientation = 
            Properties.Settings.Default.IsVertical ? 
            Orientation.Vertical : Orientation.Horizontal;
    }
}
```

### 4. Performance Optimization

```csharp
// Suspend layout during bulk operations
private void AddMultiplePages()
{
    tabSplitterContainer1.SuspendLayout();
    
    try
    {
        for (int i = 0; i < 10; i++)
        {
            TabSplitterPage page = new TabSplitterPage();
            page.Text = $"Page {i + 1}";
            tabSplitterContainer1.PrimaryPages.Add(page);
        }
    }
    finally
    {
        tabSplitterContainer1.ResumeLayout(true);
    }
}
```

## Performance Considerations

### Minimize Page Count

- Keep page count reasonable (typically < 20 pages per collection)
- Remove unused pages rather than hiding them
- Use lazy loading for heavy content

### Optimize Control Count

- Minimize controls per page
- Use UserControls to encapsulate complex layouts
- Dispose unused controls promptly

### Reduce Redraws

- Use SuspendLayout/ResumeLayout for bulk changes
- Batch property changes
- Avoid unnecessary updates in loops

## Troubleshooting

### Pages Don't Update After Runtime Changes

**Problem:** Adding/removing pages doesn't reflect in UI

**Solutions:**
```csharp
// Force refresh
tabSplitterContainer1.Refresh();

// Or use layout suspension properly
tabSplitterContainer1.SuspendLayout();
// Make changes...
tabSplitterContainer1.ResumeLayout(true);  // true = perform layout
```

### Memory Leaks with Dynamic Pages

**Problem:** Memory usage grows when adding/removing pages

**Solutions:**
- Always dispose pages and their controls
- Clear event subscriptions before disposing
- Use `using` statements for disposable resources

```csharp
private void RemovePageProperly(TabSplitterPage page)
{
    // Unsubscribe events
    page.Click -= Page_Click;
    
    // Dispose controls
    foreach (Control ctrl in page.Controls)
    {
        ctrl.Dispose();
    }
    
    // Remove and dispose page
    tabSplitterContainer1.PrimaryPages.Remove(page);
    page.Dispose();
}
```

### Splitter Position Not Saving

**Problem:** SplitterPosition resets on restart

**Solutions:**
- Save/restore in FormClosing/FormLoad events
- Use application settings or config files
- Validate position before restoring (ensure it's within bounds)

```csharp
private void Form_Load(object sender, EventArgs e)
{
    int savedPosition = Properties.Settings.Default.SplitterPosition;
    
    // Validate position is within bounds
    if (savedPosition > 0 && savedPosition < tabSplitterContainer1.Height - 50)
    {
        tabSplitterContainer1.SplitterPosition = savedPosition;
    }
}
```

### Pages Flicker During Updates

**Problem:** UI flickers when updating pages

**Solutions:**
- Enable double buffering
- Use SuspendLayout during updates
- Update page content off the UI thread if possible

```csharp
// Enable double buffering on the page
tabSplitterPage1.DoubleBuffered = true;

// Or set via reflection for all controls
typeof(Control).GetProperty("DoubleBuffered", 
    System.Reflection.BindingFlags.NonPublic | 
    System.Reflection.BindingFlags.Instance)
    .SetValue(tabSplitterPage1, true, null);
```

## Advanced Scenarios

### Multi-Monitor Support

```csharp
// Adjust splitter for different screen resolutions
private void AdjustForScreen()
{
    Screen currentScreen = Screen.FromControl(this);
    int screenHeight = currentScreen.WorkingArea.Height;
    
    // Set splitter position relative to screen size
    tabSplitterContainer1.SplitterPosition = screenHeight / 2;
}
```

### Keyboard Shortcuts

```csharp
protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
    // Ctrl+1: Toggle collapse
    if (keyData == (Keys.Control | Keys.D1))
    {
        tabSplitterContainer1.Collapsed = !tabSplitterContainer1.Collapsed;
        return true;
    }
    
    // Ctrl+2: Toggle swap
    if (keyData == (Keys.Control | Keys.D2))
    {
        tabSplitterContainer1.Swapped = !tabSplitterContainer1.Swapped;
        return true;
    }
    
    return base.ProcessCmdKey(ref msg, keyData);
}
```

## Summary

TabSplitterContainer's advanced features enable:
- Event-driven applications with responsive UI updates
- Internationalized applications with localization support
- Dynamic layouts with runtime page management
- Flexible user interfaces with swap and collapse functionality
- Performance-optimized applications with lazy loading and proper resource management

Combine these features to create professional, flexible, and user-friendly split-view applications.
