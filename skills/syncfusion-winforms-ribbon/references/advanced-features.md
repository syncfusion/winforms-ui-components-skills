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
// Setup contextual tab group
ContextualTabGroup tableTools = new ContextualTabGroup {
    Caption = "Table Tools",
    Color = Color.Orange,
    Visible = false
};
ribbonControlAdv1.Header.AddContextualTabGroup(tableTools);

ToolStripTabItem tableDesignTab = new ToolStripTabItem {
    Text = "Design",
    Tag = tableTools
};
ribbonControlAdv1.Header.MainItems.Add(tableDesignTab);

// Show/hide on selection change
dataGridView1.SelectionChanged += (s, e) => {
    tableTools.Visible = dataGridView1.SelectedCells.Count > 0;
    if (tableTools.Visible) ribbonControlAdv1.SelectedTab = tableDesignTab;
};
```

## StatusStripEx Integration

```csharp
// Add status bar to RibbonForm
StatusStripEx statusStrip = new StatusStripEx { Dock = DockStyle.Bottom };
ToolStripStatusLabel statusLabel = new ToolStripStatusLabel {
    Text = "Ready",
    Spring = true,
    TextAlign = ContentAlignment.MiddleLeft
};
statusStrip.Items.Add(statusLabel);
this.Controls.Add(statusStrip);
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
        ChildRibbon = new RibbonControlAdv { Dock = DockStyle.Top };
        
        ToolStripTabItem childTab = new ToolStripTabItem {
            Text = "Child Tab",
            MergeAction = MergeAction.Append
        };
        ChildRibbon.Header.MainItems.Add(childTab);
        this.Controls.Add(ChildRibbon);
    }
    
    protected override void OnFormClosed(FormClosedEventArgs e)
    {
        if (this.MdiParent != null)
            ((ParentForm)this.MdiParent).ribbonControlAdv1.UnmergeRibbon(ChildRibbon);
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

```csharp
// Use resource files (.resx) for localization
homeTab.Text = Resources.Home;
cutButton.Text = Resources.Cut;
cutButton.ToolTipText = Resources.CutTooltip;

// Runtime language switching
private void SetLanguage(string culture)
{
    Thread.CurrentThread.CurrentUICulture = new CultureInfo(culture);
    homeTab.Text = Resources.Home;  // Update all items
}

// RTL support
ribbonControlAdv1.RightToLeft = RightToLeft.Yes;
```

## ToolTip and SuperTooltip

```csharp
// Standard tooltip with shortcut
saveButton.ToolTipText = "Save (Ctrl+S)";
saveButton.ShortcutKeys = Keys.Control | Keys.S;
saveButton.ShowShortcutKeys = true;

// SuperToolTip (rich tooltips)
SuperToolTip superToolTip = new SuperToolTip();
ToolTipInfo info = new ToolTipInfo {
    Header = { Text = "Save", Image = Properties.Resources.Save32 },
    Body = { Text = "Save the current document to disk." },
    Footer = { Text = "Press F1 for more help" }
};
superToolTip.SetToolTip(saveButton, info);
```

## Complete Advanced Example

```csharp
public partial class AdvancedRibbonForm : RibbonForm
{
    public AdvancedRibbonForm()
    {
        InitializeComponent();
        
        // Touch support
        if (SystemInformation.NativeTouchSupported)
            ribbonControlAdv1.RibbonStyle = RibbonStyle.Touch;
        
        // KeyTips
        ribbonControlAdv1.ShowKeyTips = true;
        homeTab.KeyTip = "H";
        
        // Contextual tabs
        ContextualTabGroup pictureTools = new ContextualTabGroup {
            Caption = "Picture Tools",
            Color = Color.Blue,
            Visible = false
        };
        ribbonControlAdv1.Header.AddContextualTabGroup(pictureTools);
        
        ToolStripTabItem pictureTab = new ToolStripTabItem {
            Text = "Format",
            Tag = pictureTools
        };
        ribbonControlAdv1.Header.MainItems.Add(pictureTab);
        
        // StatusStrip
        StatusStripEx statusStrip = new StatusStripEx { Dock = DockStyle.Bottom };
        statusStrip.Items.Add(new ToolStripStatusLabel { Text = "Ready", Spring = true });
        this.Controls.Add(statusStrip);
        
        // SuperTooltips
        SuperToolTip superToolTip = new SuperToolTip();
        superToolTip.SetToolTip(saveButton, new ToolTipInfo {
            Header = { Text = "Save", Image = Properties.Resources.Save32 },
            Body = { Text = "Save the current document.\n\nPress Ctrl+S or F12" },
            Footer = { Text = "Press F1 for help" }
        });
        
        // Keyboard shortcuts
        saveButton.ShortcutKeys = Keys.Control | Keys.S;
        saveButton.KeyTip = "S";
        
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
