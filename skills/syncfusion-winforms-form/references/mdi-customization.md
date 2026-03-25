# MDI Customization

This guide covers Multiple Document Interface (MDI) implementation with SfForm, including creating parent and child forms, customization, and management.

## Table of Contents
- [MDI Overview](#mdi-overview)
- [Creating MDI Parent Form](#creating-mdi-parent-form)
- [Adding MDI Child Forms](#adding-mdi-child-forms)
- [Child Form Appearance](#child-form-appearance)
- [Getting Active MDI Child](#getting-active-mdi-child)
- [Key Event Handling](#key-event-handling)
- [MDI Management](#mdi-management)

## MDI Overview

Multiple Document Interface (MDI) applications allow multiple documents or forms to be displayed within a single parent window. This is commonly used in applications like:

- Text editors (multiple documents open simultaneously)
- Image editors (multiple images in one workspace)
- Development environments (multiple files/views)
- Data entry applications (multiple records open at once)

### MDI Terminology

- **MDI Parent:** The container form that hosts child windows
- **MDI Child:** Individual windows within the parent form
- **Client Area:** The workspace inside the parent where children are displayed
- **Active MDI Child:** The currently selected/focused child window

### Key Concepts

- Only one form can be the MDI parent
- Multiple child forms can exist within the parent
- Children are confined to the parent's client area
- Each child can be minimized, maximized, or restored independently
- Parent form manages child windows (cascading, tiling, etc.)

## Creating MDI Parent Form

To create an MDI parent form, set the `IsMdiContainer` property to `true`.

### Basic MDI Parent Setup

**C#:**
```csharp
public class MainForm : SfForm
{
    public MainForm()
    {
        InitializeComponent();
        
        // Enable MDI container
        this.IsMdiContainer = true;
        
        // Configure parent form
        this.Text = "MDI Parent Application";
        this.Size = new Size(1200, 800);
        this.WindowState = FormWindowState.Maximized;
    }
}
```

**VB.NET:**
```vb
Public Class MainForm
    Inherits SfForm
    
    Public Sub New()
        InitializeComponent()
        
        ' Enable MDI container
        Me.IsMdiContainer = True
        
        ' Configure parent form
        Me.Text = "MDI Parent Application"
        Me.Size = New Size(1200, 800)
        Me.WindowState = FormWindowState.Maximized
    End Sub
End Class
```

### MDI Parent with Custom Styling

**C#:**
```csharp
public MainForm()
{
    InitializeComponent();
    
    // Enable MDI
    this.IsMdiContainer = true;
    
    // Customize parent form
    this.Text = "Document Manager";
    this.Size = new Size(1400, 900);
    
    // Title bar styling
    this.Style.TitleBar.BackColor = Color.FromArgb(0, 122, 204);
    this.Style.TitleBar.ForeColor = Color.White;
    this.Style.TitleBar.Height = 35;
    
    // Border and shadow
    this.Style.Border = new Pen(Color.FromArgb(0, 122, 204), 2);
    this.Style.ShadowOpacity = 150;
    
    // MDI client area background
    // Note: Set after IsMdiContainer = true
    foreach (Control control in this.Controls)
    {
        if (control is MdiClient mdiClient)
        {
            mdiClient.BackColor = Color.FromArgb(240, 240, 240);
        }
    }
}
```

### Important Notes

- Set `IsMdiContainer = true` before adding child forms
- Setting this property changes the form's client area to an MdiClient control
- The MDI client area has a default gray background
- You can customize the client area background by accessing the MdiClient control

## Adding MDI Child Forms

MDI child forms are added by setting their `MdiParent` property to point to the parent form.

### Creating a Simple Child Form

**C#:**
```csharp
// In parent form
private void CreateNewChild()
{
    // Create new child form
    SfForm child = new SfForm();
    child.Text = "Document 1";
    child.Size = new Size(600, 400);
    
    // Set this form as the parent
    child.MdiParent = this;
    
    // Show the child
    child.Show();
}
```

**VB.NET:**
```vb
' In parent form
Private Sub CreateNewChild()
    ' Create new child form
    Dim child As New SfForm()
    child.Text = "Document 1"
    child.Size = New Size(600, 400)
    
    ' Set this form as the parent
    child.MdiParent = Me
    
    ' Show the child
    child.Show()
End Sub
```

### Creating Multiple Children

**C#:**
```csharp
public MainForm()
{
    InitializeComponent();
    this.IsMdiContainer = true;
    
    // Create first child
    SfForm child1 = new SfForm();
    child1.Text = "Child 1";
    child1.MdiParent = this;
    child1.Show();
    
    // Create second child
    SfForm child2 = new SfForm();
    child2.Text = "Child 2";
    child2.MdiParent = this;
    child2.Show();
    
    // Create third child
    SfForm child3 = new SfForm();
    child3.Text = "Child 3";
    child3.MdiParent = this;
    child3.Show();
}
```

**VB.NET:**
```vb
Public Sub New()
    InitializeComponent()
    Me.IsMdiContainer = True
    
    ' Create first child
    Dim child1 As New SfForm()
    child1.Text = "Child 1"
    child1.MdiParent = Me
    child1.Show()
    
    ' Create second child
    Dim child2 As New SfForm()
    child2.Text = "Child 2"
    child2.MdiParent = Me
    child2.Show()
    
    ' Create third child
    Dim child3 As New SfForm()
    child3.Text = "Child 3"
    child3.MdiParent = Me
    child3.Show()
End Sub
```

### Custom Child Form Class

**C#:**
```csharp
// Create a custom child form class
public class DocumentForm : SfForm
{
    public DocumentForm(string documentName)
    {
        InitializeComponent();
        
        this.Text = documentName;
        this.Size = new Size(700, 500);
        
        // Add controls (textbox, etc.)
        TextBox editor = new TextBox();
        editor.Multiline = true;
        editor.Dock = DockStyle.Fill;
        editor.Font = new Font("Consolas", 10);
        this.Controls.Add(editor);
    }
}

// In parent form
private void CreateDocument()
{
    DocumentForm doc = new DocumentForm("Untitled.txt");
    doc.MdiParent = this;
    doc.Show();
}
```

### Positioning Child Forms

**C#:**
```csharp
private void CreatePositionedChild()
{
    SfForm child = new SfForm();
    child.Text = "Positioned Child";
    child.Size = new Size(500, 400);
    child.MdiParent = this;
    
    // Position the child form
    child.StartPosition = FormStartPosition.Manual;
    child.Location = new Point(50, 50);
    
    child.Show();
}
```

## Child Form Appearance

Each MDI child form can be individually customized using the same styling options as standalone forms.

### Basic Child Styling

**C#:**
```csharp
private void CreateStyledChild()
{
    SfForm child = new SfForm();
    child.Text = "Styled Child";
    child.MdiParent = this;
    
    // Customize title bar
    child.Style.TitleBar.BackColor = Color.MidnightBlue;
    child.Style.TitleBar.ForeColor = Color.White;
    
    // Customize buttons
    child.Style.TitleBar.CloseButtonForeColor = Color.White;
    child.Style.TitleBar.MinimizeButtonForeColor = Color.White;
    child.Style.TitleBar.MaximizeButtonForeColor = Color.White;
    
    // Background
    child.BackColor = ColorTranslator.FromHtml("#EDF3F3");
    
    child.Show();
}
```

**VB.NET:**
```vb
Private Sub CreateStyledChild()
    Dim child As New SfForm()
    child.Text = "Styled Child"
    child.MdiParent = Me
    
    ' Customize title bar
    child.Style.TitleBar.BackColor = Color.MidnightBlue
    child.Style.TitleBar.ForeColor = Color.White
    
    ' Customize buttons
    child.Style.TitleBar.CloseButtonForeColor = Color.White
    child.Style.TitleBar.MinimizeButtonForeColor = Color.White
    child.Style.TitleBar.MaximizeButtonForeColor = Color.White
    
    ' Background
    child.BackColor = ColorTranslator.FromHtml("#EDF3F3")
    
    child.Show()
End Sub
```

### Consistent Child Styling

**C#:**
```csharp
// Create a helper method for consistent child styling
private SfForm CreateStandardChild(string title)
{
    SfForm child = new SfForm();
    child.Text = title;
    child.Size = new Size(650, 450);
    child.MdiParent = this;
    
    // Apply standard styling
    child.Style.TitleBar.BackColor = Color.White;
    child.Style.TitleBar.ForeColor = Color.Black;
    child.Style.TitleBar.Height = 32;
    
    child.Style.TitleBar.CloseButtonForeColor = Color.Black;
    child.Style.TitleBar.CloseButtonHoverBackColor = Color.FromArgb(232, 17, 35);
    child.Style.TitleBar.CloseButtonHoverForeColor = Color.White;
    
    child.Style.Border = new Pen(Color.LightGray, 1);
    child.BackColor = Color.White;
    
    return child;
}

// Usage
private void CreateNewDocument()
{
    SfForm doc = CreateStandardChild($"Document {documentCount++}");
    doc.Show();
}
```

### Color-Coded Children

**C#:**
```csharp
private void CreateColorCodedChildren()
{
    // Red child
    SfForm redChild = new SfForm();
    redChild.Text = "Important Document";
    redChild.MdiParent = this;
    redChild.Style.TitleBar.BackColor = Color.FromArgb(200, 50, 50);
    redChild.Style.TitleBar.ForeColor = Color.White;
    redChild.Show();
    
    // Green child
    SfForm greenChild = new SfForm();
    greenChild.Text = "Completed Document";
    greenChild.MdiParent = this;
    greenChild.Style.TitleBar.BackColor = Color.FromArgb(50, 150, 50);
    greenChild.Style.TitleBar.ForeColor = Color.White;
    greenChild.Show();
    
    // Blue child
    SfForm blueChild = new SfForm();
    blueChild.Text = "Draft Document";
    blueChild.MdiParent = this;
    blueChild.Style.TitleBar.BackColor = Color.FromArgb(50, 100, 200);
    blueChild.Style.TitleBar.ForeColor = Color.White;
    blueChild.Show();
}
```

## Getting Active MDI Child

The `ActiveMdiChild` property returns the currently focused child form.

### Basic Usage

**C#:**
```csharp
private void GetCurrentChild()
{
    // Get the active MDI child
    SfForm activeChild = this.ActiveMdiChild as SfForm;
    
    if (activeChild != null)
    {
        MessageBox.Show($"Active child: {activeChild.Text}");
    }
    else
    {
        MessageBox.Show("No active child");
    }
}
```

**VB.NET:**
```vb
Private Sub GetCurrentChild()
    ' Get the active MDI child
    Dim activeChild As SfForm = TryCast(Me.ActiveMdiChild, SfForm)
    
    If activeChild IsNot Nothing Then
        MessageBox.Show($"Active child: {activeChild.Text}")
    Else
        MessageBox.Show("No active child")
    End If
End Sub
```

### Manipulating Active Child

**C#:**
```csharp
// Close active child
private void CloseActiveChild()
{
    SfForm activeChild = this.ActiveMdiChild as SfForm;
    activeChild?.Close();
}

// Save active child
private void SaveActiveChild()
{
    SfForm activeChild = this.ActiveMdiChild as SfForm;
    
    if (activeChild != null)
    {
        // Access child's content and save
        // Implementation depends on child form structure
        MessageBox.Show($"Saving {activeChild.Text}");
    }
}

// Maximize active child
private void MaximizeActiveChild()
{
    SfForm activeChild = this.ActiveMdiChild as SfForm;
    
    if (activeChild != null)
    {
        activeChild.WindowState = FormWindowState.Maximized;
    }
}
```

### Menu Integration

**C#:**
```csharp
// Create menu with child operations
private void InitializeMenu()
{
    MenuStrip menuStrip = new MenuStrip();
    
    // File menu
    ToolStripMenuItem fileMenu = new ToolStripMenuItem("File");
    ToolStripMenuItem newItem = new ToolStripMenuItem("New", null, (s, e) => CreateNewChild());
    ToolStripMenuItem closeItem = new ToolStripMenuItem("Close", null, (s, e) => CloseActiveChild());
    
    fileMenu.DropDownItems.Add(newItem);
    fileMenu.DropDownItems.Add(closeItem);
    
    // Window menu
    ToolStripMenuItem windowMenu = new ToolStripMenuItem("Window");
    ToolStripMenuItem cascadeItem = new ToolStripMenuItem("Cascade", null, (s, e) => this.LayoutMdi(MdiLayout.Cascade));
    ToolStripMenuItem tileHItem = new ToolStripMenuItem("Tile Horizontal", null, (s, e) => this.LayoutMdi(MdiLayout.TileHorizontal));
    ToolStripMenuItem tileVItem = new ToolStripMenuItem("Tile Vertical", null, (s, e) => this.LayoutMdi(MdiLayout.TileVertical));
    
    windowMenu.DropDownItems.Add(cascadeItem);
    windowMenu.DropDownItems.Add(tileHItem);
    windowMenu.DropDownItems.Add(tileVItem);
    
    menuStrip.Items.Add(fileMenu);
    menuStrip.Items.Add(windowMenu);
    
    this.MainMenuStrip = menuStrip;
    this.Controls.Add(menuStrip);
}
```

## Key Event Handling

By default, MDI child forms don't receive key events when a control inside them has focus. Use `KeyPreview` to change this behavior.

### Enabling KeyPreview

**C#:**
```csharp
private void CreateChildWithKeyHandling()
{
    SfForm child = new SfForm();
    child.Text = "Child with Key Handling";
    child.MdiParent = this;
    
    // Enable key preview
    child.KeyPreview = true;
    
    // Add key event handlers
    child.KeyDown += Child_KeyDown;
    child.KeyPress += Child_KeyPress;
    child.KeyUp += Child_KeyUp;
    
    child.Show();
}

private void Child_KeyDown(object sender, KeyEventArgs e)
{
    // Handle key down
    if (e.Control && e.KeyCode == Keys.S)
    {
        // Ctrl+S pressed
        SaveActiveChild();
        e.Handled = true;
    }
}

private void Child_KeyPress(object sender, KeyPressEventArgs e)
{
    // Handle key press
}

private void Child_KeyUp(object sender, KeyEventArgs e)
{
    // Handle key up
}
```

**VB.NET:**
```vb
Private Sub CreateChildWithKeyHandling()
    Dim child As New SfForm()
    child.Text = "Child with Key Handling"
    child.MdiParent = Me
    
    ' Enable key preview
    child.KeyPreview = True
    
    ' Add key event handlers
    AddHandler child.KeyDown, AddressOf Child_KeyDown
    AddHandler child.KeyPress, AddressOf Child_KeyPress
    AddHandler child.KeyUp, AddressOf Child_KeyUp
    
    child.Show()
End Sub

Private Sub Child_KeyDown(sender As Object, e As KeyEventArgs)
    ' Handle key down
    If e.Control AndAlso e.KeyCode = Keys.S Then
        ' Ctrl+S pressed
        SaveActiveChild()
        e.Handled = True
    End If
End Sub
```

### Common Keyboard Shortcuts

**C#:**
```csharp
private void InitializeChildShortcuts(SfForm child)
{
    child.KeyPreview = true;
    child.KeyDown += (s, e) =>
    {
        switch (e.KeyCode)
        {
            case Keys.S when e.Control:
                // Ctrl+S - Save
                SaveDocument(child);
                e.Handled = true;
                break;
                
            case Keys.W when e.Control:
                // Ctrl+W - Close
                child.Close();
                e.Handled = true;
                break;
                
            case Keys.N when e.Control:
                // Ctrl+N - New
                CreateNewChild();
                e.Handled = true;
                break;
                
            case Keys.F when e.Control:
                // Ctrl+F - Find
                ShowFindDialog(child);
                e.Handled = true;
                break;
        }
    };
}
```

## MDI Management

### Layout Management

**C#:**
```csharp
// Cascade windows
private void CascadeWindows()
{
    this.LayoutMdi(MdiLayout.Cascade);
}

// Tile horizontally
private void TileHorizontal()
{
    this.LayoutMdi(MdiLayout.TileHorizontal);
}

// Tile vertically
private void TileVertical()
{
    this.LayoutMdi(MdiLayout.TileVertical);
}

// Arrange icons
private void ArrangeIcons()
{
    this.LayoutMdi(MdiLayout.ArrangeIcons);
}
```

### Getting All Child Forms

**C#:**
```csharp
private void ProcessAllChildren()
{
    // Get all MDI children
    Form[] children = this.MdiChildren;
    
    foreach (Form child in children)
    {
        if (child is SfForm sfChild)
        {
            // Process each child
            Console.WriteLine($"Child: {sfChild.Text}");
        }
    }
}

// Count children
private int GetChildCount()
{
    return this.MdiChildren.Length;
}

// Close all children
private void CloseAllChildren()
{
    foreach (Form child in this.MdiChildren)
    {
        child.Close();
    }
}
```

### Child Form Events

**C#:**
```csharp
private void CreateChildWithEvents()
{
    SfForm child = new SfForm();
    child.Text = "New Document";
    child.MdiParent = this;
    
    // Handle child events
    child.Activated += Child_Activated;
    child.Deactivate += Child_Deactivated;
    child.FormClosing += Child_FormClosing;
    child.FormClosed += Child_FormClosed;
    
    child.Show();
}

private void Child_Activated(object sender, EventArgs e)
{
    // Child becomes active
    SfForm child = sender as SfForm;
    UpdateStatusBar($"Active: {child?.Text}");
}

private void Child_Deactivated(object sender, EventArgs e)
{
    // Child loses focus
}

private void Child_FormClosing(object sender, FormClosingEventArgs e)
{
    // Child is about to close - can cancel here
    if (HasUnsavedChanges(sender as SfForm))
    {
        DialogResult result = MessageBox.Show(
            "Save changes?", 
            "Unsaved Changes", 
            MessageBoxButtons.YesNoCancel);
            
        if (result == DialogResult.Cancel)
        {
            e.Cancel = true;  // Cancel closing
        }
        else if (result == DialogResult.Yes)
        {
            SaveDocument(sender as SfForm);
        }
    }
}

private void Child_FormClosed(object sender, FormClosedEventArgs e)
{
    // Child is closed
    UpdateChildCount();
}
```

## Complete MDI Example

**C#:**
```csharp
public class MDIMainForm : SfForm
{
    private int documentCounter = 1;
    private MenuStrip menuStrip;
    private StatusStrip statusStrip;
    private ToolStripStatusLabel statusLabel;
    
    public MDIMainForm()
    {
        InitializeComponent();
        
        // Configure as MDI parent
        this.IsMdiContainer = true;
        this.Text = "MDI Application";
        this.Size = new Size(1400, 900);
        this.WindowState = FormWindowState.Maximized;
        
        // Style parent
        this.Style.TitleBar.BackColor = Color.FromArgb(0, 120, 215);
        this.Style.TitleBar.ForeColor = Color.White;
        
        // Setup UI
        InitializeMenu();
        InitializeStatusBar();
        
        // Customize MDI client area
        CustomizeMdiClientArea();
    }
    
    private void InitializeMenu()
    {
        menuStrip = new MenuStrip();
        
        // File menu
        ToolStripMenuItem fileMenu = new ToolStripMenuItem("&File");
        fileMenu.DropDownItems.Add("&New", null, (s, e) => CreateNewChild());
        fileMenu.DropDownItems.Add("&Close", null, (s, e) => CloseActiveChild());
        fileMenu.DropDownItems.Add(new ToolStripSeparator());
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
        
        // Window menu
        ToolStripMenuItem windowMenu = new ToolStripMenuItem("&Window");
        windowMenu.DropDownItems.Add("&Cascade", null, (s, e) => this.LayoutMdi(MdiLayout.Cascade));
        windowMenu.DropDownItems.Add("Tile &Horizontal", null, (s, e) => this.LayoutMdi(MdiLayout.TileHorizontal));
        windowMenu.DropDownItems.Add("Tile &Vertical", null, (s, e) => this.LayoutMdi(MdiLayout.TileVertical));
        windowMenu.DropDownItems.Add(new ToolStripSeparator());
        windowMenu.DropDownItems.Add("Close &All", null, (s, e) => CloseAllChildren());
        
        menuStrip.Items.Add(fileMenu);
        menuStrip.Items.Add(windowMenu);
        menuStrip.MdiWindowListItem = windowMenu;  // Auto-populate with child list
        
        this.MainMenuStrip = menuStrip;
        this.Controls.Add(menuStrip);
    }
    
    private void InitializeStatusBar()
    {
        statusStrip = new StatusStrip();
        statusLabel = new ToolStripStatusLabel("Ready");
        statusStrip.Items.Add(statusLabel);
        this.Controls.Add(statusStrip);
    }
    
    private void CustomizeMdiClientArea()
    {
        foreach (Control control in this.Controls)
        {
            if (control is MdiClient mdiClient)
            {
                mdiClient.BackColor = Color.FromArgb(245, 245, 245);
            }
        }
    }
    
    private void CreateNewChild()
    {
        SfForm child = new SfForm();
        child.Text = $"Document {documentCounter++}";
        child.Size = new Size(650, 500);
        child.MdiParent = this;
        
        // Style child
        child.Style.TitleBar.BackColor = Color.White;
        child.Style.TitleBar.ForeColor = Color.Black;
        child.Style.Border = new Pen(Color.LightGray, 1);
        
        child.Show();
        UpdateStatus($"Created: {child.Text}");
    }
    
    private void CloseActiveChild()
    {
        this.ActiveMdiChild?.Close();
    }
    
    private void CloseAllChildren()
    {
        foreach (Form child in this.MdiChildren)
        {
            child.Close();
        }
    }
    
    private void UpdateStatus(string message)
    {
        statusLabel.Text = message;
    }
}
```
