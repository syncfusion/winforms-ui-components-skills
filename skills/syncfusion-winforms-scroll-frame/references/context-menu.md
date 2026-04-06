# Context Menu Customization

This guide covers customizing the scrollbar context menu in SfScrollFrame, including disabling, replacing, and extending the default menu.

## Default Context Menu

SfScrollFrame provides a built-in context menu for scrollbar interaction, similar to Microsoft's standard scrollbars. The menu appears when right-clicking on the scrollbar track, thumb, or arrow buttons.

### Default Menu Items

**Vertical Scrollbar:**
- Scroll Here (jump to clicked position)
- Top (scroll to minimum)
- Bottom (scroll to maximum)
- Page Up
- Page Down
- Scroll Up
- Scroll Down

**Horizontal Scrollbar:**
- Scroll Here
- Left Edge (scroll to minimum)
- Right Edge (scroll to maximum)
- Page Left
- Page Right
- Scroll Left
- Scroll Right

### Default Behavior

The context menu executes scroll actions immediately when items are clicked:

```csharp
// No code needed - context menu works automatically
SfScrollFrame sfScrollFrame1 = new SfScrollFrame();
sfScrollFrame1.Control = listView1;

// Right-click on scrollbar shows context menu automatically
```

## ContextMenuShowing Event

The `ContextMenuShowing` event fires before the context menu displays, allowing you to customize or cancel the menu.

### Event Signature

```csharp
public event EventHandler<ContextMenuShowingEventArgs> ContextMenuShowing;
```

### ContextMenuShowingEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `ContextMenu` | ContextMenu | The context menu to display (can be modified or replaced) |
| `Location` | Point | Screen coordinates where menu will appear |
| `Cancel` | bool | Set to `true` to prevent menu from showing |

### Subscribe to Event

```csharp
// Subscribe to vertical scrollbar event
this.sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += VerticalScrollBar_ContextMenuShowing;

// Subscribe to horizontal scrollbar event
this.sfScrollFrame1.HorizontalScrollBar.ContextMenuShowing += HorizontalScrollBar_ContextMenuShowing;
```

## Disabling the Default Context Menu

Prevent the context menu from appearing by setting `Cancel = true` in the event handler.

### Disable for Vertical Scrollbar

```csharp
this.sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += VerticalScrollBar_ContextMenuShowing;

private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Disable context menu
    e.Cancel = true;
}
```

### Disable for Horizontal Scrollbar

```csharp
this.sfScrollFrame1.HorizontalScrollBar.ContextMenuShowing += HorizontalScrollBar_ContextMenuShowing;

private void HorizontalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Disable context menu
    e.Cancel = true;
}
```

### Disable for Both Scrollbars

```csharp
private void SetupSfScrollFrame()
{
    sfScrollFrame1.Control = listView1;
    
    // Subscribe both scrollbars to same handler
    sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += DisableContextMenu;
    sfScrollFrame1.HorizontalScrollBar.ContextMenuShowing += DisableContextMenu;
}

private void DisableContextMenu(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Cancel menu for any scrollbar
    e.Cancel = true;
}
```

### Conditional Disabling

```csharp
// Disable menu only under certain conditions
private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Example: Disable menu if user doesn't have edit permissions
    if (!HasEditPermission())
    {
        e.Cancel = true;
    }
}
```

## Showing Custom Context Menu

Replace the default menu with a completely custom menu by setting the `ContextMenu` property.

### Basic Custom Menu

```csharp
this.sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += VerticalScrollBar_ContextMenuShowing;

private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Create custom context menu
    ContextMenu contextMenu = new System.Windows.Forms.ContextMenu();
    contextMenu.MenuItems.Add("Custom Item");
    
    // Replace default menu
    e.ContextMenu = contextMenu;
}
```

### Custom Menu with Actions

```csharp
private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Create custom context menu
    ContextMenu contextMenu = new ContextMenu();
    
    // Add menu items with click handlers
    MenuItem item1 = new MenuItem("Scroll to Top", (s, ev) =>
    {
        sfScrollFrame1.VerticalScrollBar.Value = sfScrollFrame1.VerticalScrollBar.Minimum;
    });
    
    MenuItem item2 = new MenuItem("Scroll to Middle", (s, ev) =>
    {
        int middle = (sfScrollFrame1.VerticalScrollBar.Maximum + 
                     sfScrollFrame1.VerticalScrollBar.Minimum) / 2;
        sfScrollFrame1.VerticalScrollBar.Value = middle;
    });
    
    MenuItem item3 = new MenuItem("Scroll to Bottom", (s, ev) =>
    {
        sfScrollFrame1.VerticalScrollBar.Value = sfScrollFrame1.VerticalScrollBar.Maximum;
    });
    
    contextMenu.MenuItems.Add(item1);
    contextMenu.MenuItems.Add(item2);
    contextMenu.MenuItems.Add(item3);
    
    // Set custom menu
    e.ContextMenu = contextMenu;
}
```

### Custom Menu for Both Scrollbars

```csharp
private void SetupCustomMenus()
{
    sfScrollFrame1.Control = listView1;
    
    // Vertical scrollbar custom menu
    sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += (sender, e) =>
    {
        ContextMenu menu = new ContextMenu();
        menu.MenuItems.Add("Jump to Top", (s, ev) => 
            sfScrollFrame1.VerticalScrollBar.Value = 0);
        menu.MenuItems.Add("Jump to Bottom", (s, ev) => 
            sfScrollFrame1.VerticalScrollBar.Value = sfScrollFrame1.VerticalScrollBar.Maximum);
        e.ContextMenu = menu;
    };
    
    // Horizontal scrollbar custom menu
    sfScrollFrame1.HorizontalScrollBar.ContextMenuShowing += (sender, e) =>
    {
        ContextMenu menu = new ContextMenu();
        menu.MenuItems.Add("Jump to Left", (s, ev) => 
            sfScrollFrame1.HorizontalScrollBar.Value = 0);
        menu.MenuItems.Add("Jump to Right", (s, ev) => 
            sfScrollFrame1.HorizontalScrollBar.Value = sfScrollFrame1.HorizontalScrollBar.Maximum);
        e.ContextMenu = menu;
    };
}
```

## Adding Items to Default Menu

Extend the default menu by adding custom items to the existing `ContextMenu`.

### Add Single Item

```csharp
this.sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += VerticalScrollBar_ContextMenuShowing;

private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Add separator
    e.ContextMenu.MenuItems.Add("-");
    
    // Add custom item to default menu
    MenuItem item = new MenuItem("Show Scroll Position", DisplayScrollValue);
    e.ContextMenu.MenuItems.Add(item);
}

private void DisplayScrollValue(object sender, EventArgs e)
{
    int position = sfScrollFrame1.VerticalScrollBar.Value;
    int maximum = sfScrollFrame1.VerticalScrollBar.Maximum;
    MessageBox.Show($"Current Position: {position}\nMaximum: {maximum}", 
        "Scroll Position");
}
```

### Add Multiple Items

```csharp
private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Add separator before custom items
    e.ContextMenu.MenuItems.Add("-");
    
    // Add custom menu items
    e.ContextMenu.MenuItems.Add(new MenuItem("Reset View", (s, ev) =>
    {
        sfScrollFrame1.VerticalScrollBar.Value = 0;
        MessageBox.Show("View reset to top");
    }));
    
    e.ContextMenu.MenuItems.Add(new MenuItem("Show Statistics", (s, ev) =>
    {
        ShowScrollStatistics();
    }));
    
    e.ContextMenu.MenuItems.Add(new MenuItem("Configure Scrolling...", (s, ev) =>
    {
        OpenScrollSettings();
    }));
}

private void ShowScrollStatistics()
{
    var stats = $"Vertical Scroll:\n" +
                $"  Current: {sfScrollFrame1.VerticalScrollBar.Value}\n" +
                $"  Minimum: {sfScrollFrame1.VerticalScrollBar.Minimum}\n" +
                $"  Maximum: {sfScrollFrame1.VerticalScrollBar.Maximum}\n" +
                $"  SmallChange: {sfScrollFrame1.VerticalScrollBar.SmallChange}";
    MessageBox.Show(stats, "Scroll Statistics");
}

private void OpenScrollSettings()
{
    // Open settings dialog or panel
    MessageBox.Show("Settings dialog would open here", "Scroll Settings");
}
```

### Conditional Menu Items

```csharp
private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Add separator
    e.ContextMenu.MenuItems.Add("-");
    
    // Add "Scroll to Top" only if not already at top
    if (sfScrollFrame1.VerticalScrollBar.Value > sfScrollFrame1.VerticalScrollBar.Minimum)
    {
        e.ContextMenu.MenuItems.Add(new MenuItem("Quick Scroll to Top", (s, ev) =>
        {
            sfScrollFrame1.VerticalScrollBar.Value = sfScrollFrame1.VerticalScrollBar.Minimum;
        }));
    }
    
    // Add "Scroll to Bottom" only if not already at bottom
    if (sfScrollFrame1.VerticalScrollBar.Value < sfScrollFrame1.VerticalScrollBar.Maximum)
    {
        e.ContextMenu.MenuItems.Add(new MenuItem("Quick Scroll to Bottom", (s, ev) =>
        {
            sfScrollFrame1.VerticalScrollBar.Value = sfScrollFrame1.VerticalScrollBar.Maximum;
        }));
    }
}
```

## Advanced Menu Customization

### Submenu Example

```csharp
private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Add separator
    e.ContextMenu.MenuItems.Add("-");
    
    // Create submenu
    MenuItem jumpMenu = new MenuItem("Jump to Position");
    jumpMenu.MenuItems.Add(new MenuItem("0%", (s, ev) => JumpToPercent(0)));
    jumpMenu.MenuItems.Add(new MenuItem("25%", (s, ev) => JumpToPercent(25)));
    jumpMenu.MenuItems.Add(new MenuItem("50%", (s, ev) => JumpToPercent(50)));
    jumpMenu.MenuItems.Add(new MenuItem("75%", (s, ev) => JumpToPercent(75)));
    jumpMenu.MenuItems.Add(new MenuItem("100%", (s, ev) => JumpToPercent(100)));
    
    // Add submenu to context menu
    e.ContextMenu.MenuItems.Add(jumpMenu);
}

private void JumpToPercent(int percent)
{
    int min = sfScrollFrame1.VerticalScrollBar.Minimum;
    int max = sfScrollFrame1.VerticalScrollBar.Maximum;
    int range = max - min;
    int position = min + (int)(range * (percent / 100.0));
    
    sfScrollFrame1.VerticalScrollBar.Value = position;
}
```

### Checkable Menu Items

```csharp
private bool fastScrollingEnabled = false;

private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Add separator
    e.ContextMenu.MenuItems.Add("-");
    
    // Add checkable menu item
    MenuItem fastScrollItem = new MenuItem("Enable Fast Scrolling");
    fastScrollItem.Checked = fastScrollingEnabled;
    fastScrollItem.Click += (s, ev) =>
    {
        fastScrollingEnabled = !fastScrollingEnabled;
        if (fastScrollingEnabled)
        {
            sfScrollFrame1.VerticalScrollBar.SmallChange = 30;
            sfScrollFrame1.HorizontalScrollBar.SmallChange = 30;
        }
        else
        {
            sfScrollFrame1.VerticalScrollBar.SmallChange = 10;
            sfScrollFrame1.HorizontalScrollBar.SmallChange = 10;
        }
    };
    
    e.ContextMenu.MenuItems.Add(fastScrollItem);
}
```

### Context-Aware Menu

```csharp
private void VerticalScrollBar_ContextMenuShowing(object sender, 
    Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
{
    // Add separator
    e.ContextMenu.MenuItems.Add("-");
    
    // Get current position
    int current = sfScrollFrame1.VerticalScrollBar.Value;
    int max = sfScrollFrame1.VerticalScrollBar.Maximum;
    int min = sfScrollFrame1.VerticalScrollBar.Minimum;
    
    // Show different options based on position
    if (current == min)
    {
        e.ContextMenu.MenuItems.Add(new MenuItem("Already at Top", (s, ev) => { }) 
            { Enabled = false });
    }
    else if (current == max)
    {
        e.ContextMenu.MenuItems.Add(new MenuItem("Already at Bottom", (s, ev) => { }) 
            { Enabled = false });
    }
    else
    {
        int percentScrolled = (int)((current - min) / (double)(max - min) * 100);
        e.ContextMenu.MenuItems.Add(new MenuItem($"Scrolled: {percentScrolled}%", (s, ev) => { }) 
            { Enabled = false });
    }
}
```

## Complete Example

Here's a comprehensive example combining various menu customization techniques:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace SfScrollFrameMenuDemo
{
    public partial class MainForm : Form
    {
        private SfScrollFrame sfScrollFrame1;
        private ListView listView1;
        private bool showAdvancedOptions = false;

        public MainForm()
        {
            InitializeComponent();
            SetupControls();
            SetupContextMenus();
        }

        private void SetupControls()
        {
            // Create ListView with many items
            listView1 = new ListView();
            listView1.View = View.Details;
            listView1.Size = new Size(400, 400);
            listView1.Location = new Point(20, 20);
            listView1.Columns.Add("Item", 200);
            listView1.Columns.Add("Value", 180);
            
            for (int i = 0; i < 100; i++)
            {
                listView1.Items.Add(new ListViewItem(new[] { $"Item {i}", $"Value {i}" }));
            }
            
            // Attach SfScrollFrame
            sfScrollFrame1 = new SfScrollFrame();
            sfScrollFrame1.Control = listView1;
            
            this.Controls.Add(listView1);
        }

        private void SetupContextMenus()
        {
            // Vertical scrollbar context menu
            sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += VerticalScrollBar_ContextMenuShowing;
            
            // Horizontal scrollbar context menu
            sfScrollFrame1.HorizontalScrollBar.ContextMenuShowing += HorizontalScrollBar_ContextMenuShowing;
        }

        private void VerticalScrollBar_ContextMenuShowing(object sender, 
            Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
        {
            // Add separator
            e.ContextMenu.MenuItems.Add("-");
            
            // Quick jump options
            MenuItem jumpMenu = new MenuItem("Quick Jump");
            jumpMenu.MenuItems.Add(new MenuItem("Top (0%)", (s, ev) => JumpToPercentVertical(0)));
            jumpMenu.MenuItems.Add(new MenuItem("Quarter (25%)", (s, ev) => JumpToPercentVertical(25)));
            jumpMenu.MenuItems.Add(new MenuItem("Middle (50%)", (s, ev) => JumpToPercentVertical(50)));
            jumpMenu.MenuItems.Add(new MenuItem("Three Quarters (75%)", (s, ev) => JumpToPercentVertical(75)));
            jumpMenu.MenuItems.Add(new MenuItem("Bottom (100%)", (s, ev) => JumpToPercentVertical(100)));
            e.ContextMenu.MenuItems.Add(jumpMenu);
            
            // Show current position
            int current = sfScrollFrame1.VerticalScrollBar.Value;
            int max = sfScrollFrame1.VerticalScrollBar.Maximum;
            int min = sfScrollFrame1.VerticalScrollBar.Minimum;
            int percent = max > min ? (int)((current - min) / (double)(max - min) * 100) : 0;
            
            MenuItem positionItem = new MenuItem($"Current Position: {percent}%", (s, ev) => { });
            positionItem.Enabled = false;
            e.ContextMenu.MenuItems.Add(positionItem);
            
            // Advanced options toggle
            e.ContextMenu.MenuItems.Add("-");
            MenuItem advancedToggle = new MenuItem("Show Advanced Options");
            advancedToggle.Checked = showAdvancedOptions;
            advancedToggle.Click += (s, ev) => { showAdvancedOptions = !showAdvancedOptions; };
            e.ContextMenu.MenuItems.Add(advancedToggle);
            
            // Advanced options (if enabled)
            if (showAdvancedOptions)
            {
                e.ContextMenu.MenuItems.Add(new MenuItem("Scroll Speed Settings...", (s, ev) =>
                {
                    ShowScrollSpeedDialog();
                }));
                
                e.ContextMenu.MenuItems.Add(new MenuItem("Reset Scrollbar", (s, ev) =>
                {
                    sfScrollFrame1.VerticalScrollBar.Value = min;
                    MessageBox.Show("Scrollbar reset to top", "Reset Complete");
                }));
            }
        }

        private void HorizontalScrollBar_ContextMenuShowing(object sender, 
            Syncfusion.WinForms.Controls.Events.ContextMenuShowingEventArgs e)
        {
            // Simple custom menu for horizontal scrollbar
            e.ContextMenu.MenuItems.Add("-");
            e.ContextMenu.MenuItems.Add(new MenuItem("Jump to Left", (s, ev) =>
            {
                sfScrollFrame1.HorizontalScrollBar.Value = sfScrollFrame1.HorizontalScrollBar.Minimum;
            }));
            e.ContextMenu.MenuItems.Add(new MenuItem("Jump to Right", (s, ev) =>
            {
                sfScrollFrame1.HorizontalScrollBar.Value = sfScrollFrame1.HorizontalScrollBar.Maximum;
            }));
        }

        private void JumpToPercentVertical(int percent)
        {
            int min = sfScrollFrame1.VerticalScrollBar.Minimum;
            int max = sfScrollFrame1.VerticalScrollBar.Maximum;
            int range = max - min;
            int position = min + (int)(range * (percent / 100.0));
            
            sfScrollFrame1.VerticalScrollBar.Value = Math.Max(min, Math.Min(max, position));
        }

        private void ShowScrollSpeedDialog()
        {
            // Simple dialog to set scroll speed
            Form dialog = new Form();
            dialog.Text = "Scroll Speed Settings";
            dialog.Size = new Size(300, 150);
            dialog.StartPosition = FormStartPosition.CenterParent;
            
            Label label = new Label();
            label.Text = "SmallChange Value (1-50):";
            label.Location = new Point(20, 20);
            label.AutoSize = true;
            
            NumericUpDown numericUpDown = new NumericUpDown();
            numericUpDown.Location = new Point(20, 45);
            numericUpDown.Minimum = 1;
            numericUpDown.Maximum = 50;
            numericUpDown.Value = sfScrollFrame1.VerticalScrollBar.SmallChange;
            
            Button okButton = new Button();
            okButton.Text = "OK";
            okButton.Location = new Point(100, 80);
            okButton.Click += (s, e) =>
            {
                sfScrollFrame1.VerticalScrollBar.SmallChange = (int)numericUpDown.Value;
                sfScrollFrame1.HorizontalScrollBar.SmallChange = (int)numericUpDown.Value;
                dialog.Close();
            };
            
            dialog.Controls.Add(label);
            dialog.Controls.Add(numericUpDown);
            dialog.Controls.Add(okButton);
            dialog.ShowDialog(this);
        }
    }
}
```

## Best Practices

1. **Consistency:** Use similar menu structures for both horizontal and vertical scrollbars
2. **User Feedback:** Provide visual or message feedback when menu actions complete
3. **Context-Aware:** Show only relevant options based on current scroll position
4. **Separators:** Use separators (`"-"`) to group related menu items
5. **Disabled Items:** Disable items that aren't applicable rather than hiding them
6. **Shortcuts:** Consider adding keyboard shortcuts for frequent actions
7. **Testing:** Test menus in all scrollbar states and positions
8. **Localization:** Ensure custom menu strings can be localized (see [localization.md](localization.md))
