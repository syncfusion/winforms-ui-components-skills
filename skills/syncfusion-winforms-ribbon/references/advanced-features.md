# Advanced Features

## Table of Contents
- [Contextual Tab Groups](#contextual-tab-groups)
- [StatusStripEx Integration](#statusstripex-integration)
- [Ribbon Merge Support](#ribbon-merge-support)
- [Keyboard Support and KeyTips](#keyboard-support-and-keytips)
- [Touch Support](#touch-support)
- [Localization](#localization)
- [ToolTip and SuperTooltip](#tooltip-and-supertooltip)
- [Complete Advanced Example](#complete-advanced-example)

## Contextual Tab Groups

Contextual tabs appear only when specific objects are selected (e.g., Picture Tools when image selected).

### Creating Contextual Tab Group

```csharp
// Create contextual tab group
ContextualTabGroup pictureTools = new ContextualTabGroup();
pictureTools.Caption = "Picture Tools";
pictureTools.Color = Color.Blue;

// Add to ribbon
ribbonControlAdv1.Header.AddContextualTabGroup(pictureTools);

// Create tab for the group
ToolStripTabItem formatTab = new ToolStripTabItem();
formatTab.Text = "Format";
formatTab.Panel.BackColor = Color.White;

// Assign tab to contextual group
formatTab.Tag = pictureTools;

// Add tab to ribbon
ribbonControlAdv1.Header.MainItems.Add(formatTab);

// Add controls to tab
ToolStripEx pictureGroup = new ToolStripEx();
pictureGroup.Text = "Picture Styles";
formatTab.Panel.Controls.Add(pictureGroup);
```

### Showing/Hiding Contextual Tabs

```csharp
// Show contextual tab when image selected
private void imageControl_Click(object sender, EventArgs e)
{
    pictureTools.Visible = true;
    formatTab.Panel.Tag = pictureTools; // Associate tab
    ribbonControlAdv1.SelectedTab = formatTab; // Activate tab
}

// Hide when selection cleared
private void ClearSelection()
{
    pictureTools.Visible = false;
}
```

### Complete Contextual Tab Example

```csharp
public partial class Form1 : RibbonForm
{
    private ContextualTabGroup tableTools;
    private ToolStripTabItem tableDesignTab;
    
    public Form1()
    {
        InitializeComponent();
        SetupContextualTabs();
    }
    
    private void SetupContextualTabs()
    {
        // Create contextual tab group
        tableTools = new ContextualTabGroup();
        tableTools.Caption = "Table Tools";
        tableTools.Color = Color.Orange;
        tableTools.Visible = false; // Hidden by default
        
        ribbonControlAdv1.Header.AddContextualTabGroup(tableTools);
        
        // Create design tab
        tableDesignTab = new ToolStripTabItem();
        tableDesignTab.Text = "Design";
        tableDesignTab.Panel.BackColor = Color.White;
        tableDesignTab.Tag = tableTools;
        
        ribbonControlAdv1.Header.MainItems.Add(tableDesignTab);
        
        // Add table style group
        ToolStripEx stylesGroup = new ToolStripEx();
        stylesGroup.Text = "Table Styles";
        tableDesignTab.Panel.Controls.Add(stylesGroup);
        
        // Add style buttons
        ToolStripButton style1 = new ToolStripButton("Light");
        ToolStripButton style2 = new ToolStripButton("Medium");
        ToolStripButton style3 = new ToolStripButton("Dark");
        
        stylesGroup.Items.AddRange(new ToolStripItem[] { style1, style2, style3 });
    }
    
    private void dataGridView1_SelectionChanged(object sender, EventArgs e)
    {
        // Show table tools when grid has selection
        if (dataGridView1.SelectedCells.Count > 0)
        {
            tableTools.Visible = true;
            ribbonControlAdv1.SelectedTab = tableDesignTab;
        }
        else
        {
            tableTools.Visible = false;
        }
    }
}
```

## StatusStripEx Integration

Add status bar at bottom of RibbonForm.

```csharp
// Create StatusStripEx
StatusStripEx statusStrip = new StatusStripEx();
statusStrip.Dock = DockStyle.Bottom;

// Add status label
ToolStripStatusLabel statusLabel = new ToolStripStatusLabel();
statusLabel.Text = "Ready";
statusLabel.Spring = true; // Fill available space
statusLabel.TextAlign = ContentAlignment.MiddleLeft;

statusStrip.Items.Add(statusLabel);

// Add to form
this.Controls.Add(statusStrip);

// Update status from ribbon actions
private void saveButton_Click(object sender, EventArgs e)
{
    // Save logic...
    statusLabel.Text = "Document saved";
}
```

## Ribbon Merge Support

Merge ribbons from MDI child forms into parent ribbon.

### Parent Form Setup

```csharp
public partial class ParentForm : RibbonForm
{
    public ParentForm()
    {
        InitializeComponent();
        
        // Enable MDI
        this.IsMdiContainer = true;
        
        // Create parent ribbon
        RibbonControlAdv parentRibbon = new RibbonControlAdv();
        // Setup parent ribbon...
        
        this.Controls.Add(parentRibbon);
    }
    
    private void OpenChildForm()
    {
        ChildForm child = new ChildForm();
        child.MdiParent = this;
        child.Show();
        
        // Merge child ribbon into parent
        ribbonControlAdv1.MergeRibbon(child.ChildRibbon);
    }
}
```

### Child Form Setup

```csharp
public partial class ChildForm : Form
{
    public RibbonControlAdv ChildRibbon { get; private set; }
    
    public ChildForm()
    {
        InitializeComponent();
        
        // Create child ribbon
        ChildRibbon = new RibbonControlAdv();
        ChildRibbon.Dock = DockStyle.Top;
        
        // Add child-specific tabs
        ToolStripTabItem childTab = new ToolStripTabItem();
        childTab.Text = "Child Tab";
        childTab.MergeAction = MergeAction.Append;
        
        ChildRibbon.Header.MainItems.Add(childTab);
        
        this.Controls.Add(ChildRibbon);
    }
    
    protected override void OnFormClosed(FormClosedEventArgs e)
    {
        // Unmerge when closing
        if (this.MdiParent != null)
        {
            ParentForm parent = (ParentForm)this.MdiParent;
            parent.ribbonControlAdv1.UnmergeRibbon(ChildRibbon);
        }
        
        base.OnFormClosed(e);
    }
}
```

## Keyboard Support and KeyTips

### KeyTips (Alt + Letter Navigation)

KeyTips show when user presses Alt key.

```csharp
// Enable KeyTips
ribbonControlAdv1.ShowKeyTips = true;

// Set KeyTip for tab
homeTab.KeyTip = "H"; // Press Alt+H to activate

// Set KeyTip for button
saveButton.KeyTip = "S"; // Press Alt+H, S to execute
```

### Keyboard Shortcuts

```csharp
// Assign keyboard shortcut
ToolStripButton cutButton = new ToolStripButton();
cutButton.Text = "Cut";
cutButton.ShortcutKeys = Keys.Control | Keys.X;
cutButton.ShowShortcutKeys = true; // Show in tooltip

cutButton.Click += (s, e) =>
{
    // Cut operation
    if (richTextBox1.SelectionLength > 0)
        richTextBox1.Cut();
};
```

### Common Keyboard Shortcuts

```csharp
// Ctrl+F1 - Toggle ribbon minimize
ribbonControlAdv1.RegisterKeyboardShortcut(Keys.Control | Keys.F1, () =>
{
    ribbonControlAdv1.DisplayOption = 
        ribbonControlAdv1.DisplayOption == RibbonDisplayOption.ShowTabsAndCommands
            ? RibbonDisplayOption.ShowTabs
            : RibbonDisplayOption.ShowTabsAndCommands;
});
```

## Touch Support

Optimize ribbon for touch input.

```csharp
// Set RibbonStyle to Touch for larger touch targets
ribbonControlAdv1.RibbonStyle = RibbonStyle.Touch;

// Increase item sizes for touch
ribbonControlAdv1.TouchMode = true;

// Detect touch mode automatically
if (SystemInformation.NativeTouchSupported)
{
    ribbonControlAdv1.RibbonStyle = RibbonStyle.Touch;
}
```

## Localization

Localize ribbon text and tooltips.

```csharp
// Using resource files (.resx)
homeTab.Text = Resources.Home;
cutButton.Text = Resources.Cut;
cutButton.ToolTipText = Resources.CutTooltip;

// Runtime language switching
private void SetLanguage(string culture)
{
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo(culture);
    
    // Update all text
    homeTab.Text = Resources.Home;
    cutButton.Text = Resources.Cut;
    // ... update other items
}

// Example usage
private void englishMenuItem_Click(object sender, EventArgs e)
{
    SetLanguage("en-US");
}

private void spanishMenuItem_Click(object sender, EventArgs e)
{
    SetLanguage("es-ES");
}
```

### Right-to-Left (RTL) Support

```csharp
// Enable RTL layout
ribbonControlAdv1.RightToLeft = RightToLeft.Yes;

// Or inherit from form
this.RightToLeft = RightToLeft.Yes;
```

## ToolTip and SuperTooltip

### Standard ToolTips

```csharp
// Set tooltip
saveButton.ToolTipText = "Save the current document";

// Tooltip with keyboard shortcut
saveButton.ToolTipText = "Save (Ctrl+S)";
saveButton.ShortcutKeys = Keys.Control | Keys.S;
saveButton.ShowShortcutKeys = true;
```

### SuperToolTip (Rich Tooltips)

```csharp
// Create SuperToolTip
SuperToolTip superToolTip = new SuperToolTip();

// Create tooltip info
ToolTipInfo info = new ToolTipInfo();
info.Header.Text = "Save";
info.Header.Image = Properties.Resources.Save32;
info.Body.Text = "Save the current document to disk.";
info.Footer.Text = "Press F1 for more help";

superToolTip.SetToolTip(saveButton, info);

// With custom styling
info.Body.TextMargin = new Padding(5);
info.Body.Size = new Size(200, 100);
info.Separator.Visible = true;
```

## Complete Advanced Example

```csharp
public partial class AdvancedRibbonForm : RibbonForm
{
    private ContextualTabGroup pictureTools;
    private ToolStripTabItem pictureTab;
    private SuperToolTip superToolTip;
    
    public AdvancedRibbonForm()
    {
        InitializeComponent();
        SetupAdvancedFeatures();
    }
    
    private void SetupAdvancedFeatures()
    {
        // Touch support
        if (SystemInformation.NativeTouchSupported)
        {
            ribbonControlAdv1.RibbonStyle = RibbonStyle.Touch;
        }
        
        // KeyTips
        ribbonControlAdv1.ShowKeyTips = true;
        homeTab.KeyTip = "H";
        
        // Contextual tabs
        SetupContextualTabs();
        
        // StatusStrip
        SetupStatusStrip();
        
        // SuperTooltips
        SetupSuperTooltips();
        
        // Keyboard shortcuts
        SetupKeyboardShortcuts();
    }
    
    private void SetupContextualTabs()
    {
        pictureTools = new ContextualTabGroup();
        pictureTools.Caption = "Picture Tools";
        pictureTools.Color = Color.Blue;
        pictureTools.Visible = false;
        
        ribbonControlAdv1.Header.AddContextualTabGroup(pictureTools);
        
        pictureTab = new ToolStripTabItem();
        pictureTab.Text = "Format";
        pictureTab.Tag = pictureTools;
        
        ribbonControlAdv1.Header.MainItems.Add(pictureTab);
    }
    
    private void SetupStatusStrip()
    {
        StatusStripEx statusStrip = new StatusStripEx();
        statusStrip.Dock = DockStyle.Bottom;
        
        ToolStripStatusLabel statusLabel = new ToolStripStatusLabel();
        statusLabel.Text = "Ready";
        statusLabel.Spring = true;
        
        statusStrip.Items.Add(statusLabel);
        this.Controls.Add(statusStrip);
    }
    
    private void SetupSuperTooltips()
    {
        superToolTip = new SuperToolTip();
        
        // Save button tooltip
        ToolTipInfo saveInfo = new ToolTipInfo();
        saveInfo.Header.Text = "Save";
        saveInfo.Header.Image = Properties.Resources.Save32;
        saveInfo.Body.Text = "Save the current document.\n\nPress Ctrl+S or F12";
        saveInfo.Footer.Text = "Press F1 for help";
        
        superToolTip.SetToolTip(saveButton, saveInfo);
    }
    
    private void SetupKeyboardShortcuts()
    {
        // Ctrl+S - Save
        saveButton.ShortcutKeys = Keys.Control | Keys.S;
        saveButton.KeyTip = "S";
        
        // Ctrl+F1 - Toggle ribbon
        this.KeyDown += (s, e) =>
        {
            if (e.Control && e.KeyCode == Keys.F1)
            {
                ribbonControlAdv1.DisplayOption = 
                    ribbonControlAdv1.DisplayOption == RibbonDisplayOption.ShowTabsAndCommands
                        ? RibbonDisplayOption.ShowTabs
                        : RibbonDisplayOption.ShowTabsAndCommands;
                
                e.Handled = true;
            }
        };
    }
    
    private void pictureBox1_Click(object sender, EventArgs e)
    {
        // Show picture tools when image selected
        pictureTools.Visible = true;
        ribbonControlAdv1.SelectedTab = pictureTab;
    }
}
```

## Best Practices

1. **Use contextual tabs for context-specific tools:** Show/hide based on selection

2. **Provide keyboard shortcuts:** Improve accessibility and power user experience

3. **Use SuperTooltips for complex commands:** Provide helpful descriptions with images

4. **Support touch mode:** Optimize for touch devices when available

5. **Implement localization:** Support multiple languages with resource files

6. **Add status bar:** Provide feedback to users

7. **Test keyboard navigation:** Ensure all features accessible via keyboard

8. **Use ribbon merge for MDI:** Maintain consistent ribbon in MDI applications

## Related Topics

- **Getting Started** - Basic ribbon setup
- **Ribbon States** - Display options and state management
- **Customization** - User customization features
- **Resize Behavior** - Responsive layout handling
