# Button Configuration & Settings

## Table of Contents
- [Close Button](#close-button)
- [Dropdown Button](#dropdown-button)
- [Button Customization](#button-customization)
- [Complete Examples](#complete-examples)

## Close Button

### Enable/Disable Close Button

The close button removes documents from the tab strip:

```csharp
// Show/hide the main close button (at far right)
tabbedMDIManager.CloseButtonVisible = true;

// Show individual close buttons on each tab
tabbedMDIManager.ShowCloseButton = true;

// Show close button only on the active tab
tabbedMDIManager.ShowCloseButtonForActiveTabOnly = true;
```

### Visual Examples

```
CloseButtonVisible = true
┌──────────────────────────────────┐
│ Doc1 │ Doc2 │ Doc3        [✕]    │  ← Single close at right end
└──────────────────────────────────┘

ShowCloseButton = true
┌──────────────────────────────────┐
│ Doc1[✕] │ Doc2[✕] │ Doc3[✕]      │  ← Close button per tab
└──────────────────────────────────┘

ShowCloseButtonForActiveTabOnly = true
┌──────────────────────────────────┐
│ Doc1 │ Doc2[✕] │ Doc3            │  ← Only active tab shows close
└──────────────────────────────────┘
```

### Close Button Colors

Customize the close button appearance:

```csharp
// Change close button color
tabbedMDIManager.CloseButtonColor = Color.Red;

// Example: Color coding based on document state
tabbedMDIManager.CloseButtonColor = Color.DarkRed;
```

### Middle Mouse Button Close

Allow users to close tabs by middle-clicking:

```csharp
// Enable middle mouse button to close tabs
tabbedMDIManager.CloseOnMiddleButtonClick = true;

// Now users can click middle mouse button on a tab to close it
```

### Show/Hide Close Button Per Form

Control close button visibility for individual documents:

```csharp
private void CreateDocumentWithCloseControl()
{
    Form doc = new Form();
    doc.Text = "Important Document";
    doc.MdiParent = this;
    doc.Show();

    // Show close button for this form
    tabbedMDIManager.ShowCloseButtonForForm(doc, true);

    // Or hide it to prevent accidental closure
    // tabbedMDIManager.ShowCloseButtonForForm(doc, false);
}

// Example: Restrict closing for unsaved documents
private void RestrictCloseForUnsavedDocs()
{
    foreach (Form child in this.MdiChildren)
    {
        if (HasUnsavedChanges(child))
        {
            tabbedMDIManager.ShowCloseButtonForForm(child, false);
        }
    }
}

private bool HasUnsavedChanges(Form form)
{
    // Your logic to check for unsaved changes
    return form.Text.Contains("*");  // Example: asterisk means unsaved
}
```

## Dropdown Button

### Enable Dropdown Button

The dropdown shows a list of all open documents:

```csharp
// Show the dropdown button
tabbedMDIManager.DropDownButtonVisible = true;

// Dropdown allows quick navigation to any open document
```

### Visual Appearance

```
┌──────────────────────────────────┐
│ Doc1 │ Doc2 │ Doc3        [▼]    │  ← Dropdown button
└──────────────────────────────────┘

Click [▼] shows:
┌──────────────────────────────────┐
│ ✓ Doc1                           │  ← Current document
│   Doc2                           │
│   Doc3                           │
│   Doc4                           │
│   Doc5                           │
└──────────────────────────────────┘
```

### Customize Dropdown Style

The `BeforeDropDownPopup` event lets you customize the dropdown appearance:

```csharp
private void SetupDropdownStyle()
{
    tabbedMDIManager.BeforeDropDownPopup += new DropDownPopupEventHandler(
        tabbedMDIManager_BeforeDropDownPopup);
}

private void tabbedMDIManager_BeforeDropDownPopup(object sender, DropDownPopupEventArgs e)
{
    // Set dropdown style
    e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2007;

    // Or prevent popup (cancel)
    // e.Cancel = true;
}
```

### Dropdown Styles Available

```csharp
// Different visual styles for dropdown
e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2003;
e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2007;
e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2010;
e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;
e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Metro;
```

## Button Customization

### Combined Button Configuration

```csharp
private void ConfigureAllButtons()
{
    // Setup all button options together
    tabbedMDIManager.CloseButtonVisible = true;      // Main close button
    tabbedMDIManager.ShowCloseButton = true;         // Per-tab close buttons
    tabbedMDIManager.CloseButtonColor = Color.Gray;  // Close button color
    tabbedMDIManager.DropDownButtonVisible = true;   // Dropdown button
    tabbedMDIManager.CloseOnMiddleButtonClick = true; // Middle mouse click

    // Customize dropdown
    tabbedMDIManager.BeforeDropDownPopup += (sender, e) =>
    {
        e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;
    };
}
```

### Full Button Control Panel

```csharp
public partial class ButtonControlForm : Form
{
    private TabbedMDIManager tabbedMDI;

    public ButtonControlForm()
    {
        InitializeComponent();
        SetupUI();
    }

    private void SetupUI()
    {
        this.IsMdiContainer = true;
        this.Text = "Button Configuration Demo";
        this.Size = new Size(900, 600);

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        // Create control panel
        CreateControlPanel();

        // Create sample documents
        CreateSampleDocuments();
    }

    private void CreateControlPanel()
    {
        Panel controlPanel = new Panel();
        controlPanel.Dock = DockStyle.Top;
        controlPanel.Height = 100;
        controlPanel.BackColor = Color.LightGray;
        controlPanel.BorderStyle = BorderStyle.FixedSingle;
        this.Controls.Add(controlPanel);

        // Checkboxes for button options
        CheckBox cbShowCloseButton = new CheckBox();
        cbShowCloseButton.Text = "Show Close Button";
        cbShowCloseButton.Location = new Point(10, 10);
        cbShowCloseButton.Checked = false;
        cbShowCloseButton.CheckedChanged += (s, e) =>
            tabbedMDI.ShowCloseButton = cbShowCloseButton.Checked;
        controlPanel.Controls.Add(cbShowCloseButton);

        CheckBox cbCloseOnActive = new CheckBox();
        cbCloseOnActive.Text = "Close Button on Active Tab Only";
        cbCloseOnActive.Location = new Point(200, 10);
        cbCloseOnActive.CheckedChanged += (s, e) =>
            tabbedMDI.ShowCloseButtonForActiveTabOnly = cbCloseOnActive.Checked;
        controlPanel.Controls.Add(cbCloseOnActive);

        CheckBox cbDropdown = new CheckBox();
        cbDropdown.Text = "Show Dropdown Button";
        cbDropdown.Location = new Point(470, 10);
        cbDropdown.Checked = true;
        cbDropdown.CheckedChanged += (s, e) =>
            tabbedMDI.DropDownButtonVisible = cbDropdown.Checked;
        controlPanel.Controls.Add(cbDropdown);

        CheckBox cbMiddleClick = new CheckBox();
        cbMiddleClick.Text = "Middle Click to Close";
        cbMiddleClick.Location = new Point(650, 10);
        cbMiddleClick.Checked = true;
        cbMiddleClick.CheckedChanged += (s, e) =>
            tabbedMDI.CloseOnMiddleButtonClick = cbMiddleClick.Checked;
        controlPanel.Controls.Add(cbMiddleClick);

        // Color picker for close button
        Label lblColor = new Label();
        lblColor.Text = "Close Button Color:";
        lblColor.Location = new Point(10, 40);
        controlPanel.Controls.Add(lblColor);

        Button btnColorPicker = new Button();
        btnColorPicker.Text = "Change Color";
        btnColorPicker.Location = new Point(120, 38);
        btnColorPicker.Click += (s, e) =>
        {
            ColorDialog cd = new ColorDialog();
            if (cd.ShowDialog() == DialogResult.OK)
            {
                tabbedMDI.CloseButtonColor = cd.Color;
                btnColorPicker.BackColor = cd.Color;
            }
        };
        controlPanel.Controls.Add(btnColorPicker);
    }

    private void CreateSampleDocuments()
    {
        for (int i = 1; i <= 5; i++)
        {
            Form doc = new Form();
            doc.Text = $"Document {i}";
            doc.MdiParent = this;
            doc.Show();
        }
    }
}
```

## Complete Examples

### Example 1: Conservative Settings (Minimal Buttons)

```csharp
private void SetupConservativeUI()
{
    // Only main close button at right, no individual close buttons
    tabbedMDIManager.CloseButtonVisible = true;
    tabbedMDIManager.ShowCloseButton = false;
    tabbedMDIManager.DropDownButtonVisible = false;

    // Result: Clean, minimal UI
    // ┌──────────────────────────────────┐
    // │ Doc1 │ Doc2 │ Doc3        [✕]    │
    // └──────────────────────────────────┘
}
```

### Example 2: Full-Featured Buttons

```csharp
private void SetupFullFeaturedUI()
{
    // All button features enabled
    tabbedMDIManager.ShowCloseButton = true;
    tabbedMDIManager.DropDownButtonVisible = true;
    tabbedMDIManager.CloseOnMiddleButtonClick = true;
    tabbedMDIManager.CloseButtonColor = Color.Red;

    // Customize dropdown
    tabbedMDIManager.BeforeDropDownPopup += (sender, e) =>
    {
        e.ParentBarItem.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016;
    };

    // Result: Rich interaction options
    // ┌──────────────────────────────────┐
    // │ Doc1[✕] │ Doc2[✕] │ Doc3[✕] [▼]  │
    // └──────────────────────────────────┘
}
```

### Example 3: Conditional Button Display

```csharp
private void ConditionalButtonDisplay()
{
    // Show close button only for temporary documents
    tabbedMDIManager.ShowCloseButton = false;

    tabbedMDIManager.BeforeMDIChildAdded += (sender, e) =>
    {
        bool isTemporary = e.NewControl.Text.Contains("Temp");
        tabbedMDIManager.ShowCloseButtonForForm(e.NewControl as Form, isTemporary);
    };
}
```

## Best Practices

1. **Consistency** - Keep button configuration consistent throughout your application
2. **User clarity** - Provide visual feedback about what buttons do
3. **Save vs. Close** - Consider warning users before closing unsaved documents
4. **Middle-click** - Only enable if users expect it (common in browsers, not all apps)
5. **Colors** - Use meaningful colors (e.g., red for "close", not just for aesthetics)

## Troubleshooting

### Issue: Close Button Not Appearing
**Solution:** Ensure `CloseButtonVisible` or `ShowCloseButton` is set to `true`

### Issue: Dropdown Shows No Items
**Solution:** Verify documents are properly added to the tab group before clicking dropdown

### Issue: ShowCloseButtonForForm Not Working
**Solution:** Ensure it's called AFTER the form is added to MdiChildren:

```csharp
Form doc = new Form() { MdiParent = this };
doc.Show();  // Must show first
tabbedMDIManager.ShowCloseButtonForForm(doc, true);  // Then configure
```
