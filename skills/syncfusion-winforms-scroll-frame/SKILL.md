---
name: syncfusion-winforms-scroll-frame
description: Guide for implementing Syncfusion SfScrollFrame control in Windows Forms applications to attach theme-able scrollbars to any scrollable control. Use this skill when implementing custom scrollbars, scrollbar theming, or replacing default scrollbars in Windows Forms. Covers scrollbar attachment, appearance customization, theme application, and scrollbar event handling.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Scroll Frames (SfScrollFrame)

The `SfScrollFrame` control provides theme-able, customizable scrollbars for Windows Forms applications. It attaches modern, styled scrollbars to any control derived from Microsoft's `ScrollableControl` or `Container`, replacing the default system scrollbars with fully customizable alternatives that support themes, context menus, and extensive appearance options.

## When to Use This Skill

Use this skill when you need to:
- **Attach custom scrollbars** to Panel, ListView, ListBox, or other scrollable controls
- **Apply themes** to scrollbars (Office2016, Office2019, HighContrast)
- **Customize scrollbar appearance** (colors, sizes, states)
- **Handle scrollbar events** and context menus
- **Implement localization** for scrollbar context menus
- **Enable UI automation** for scrollbar testing (Coded UI, QTP)
- **Programmatically control scrolling** with custom scroll speeds
- **Replace default Windows scrollbars** with modern alternatives

## Component Overview

### Key Features

- **Universal Attachment:** Works with any ScrollableControl-derived control (Panel, ListView, ListBox, etc.)
- **Theme Support:** Built-in Office2016, Office2019, and HighContrast themes
- **Full Customization:** Customize colors, sizes, and states for all scrollbar elements
- **Context Menu:** Customizable right-click menu with localization support
- **Programmatic Scrolling:** Control scroll position and speed via code
- **UI Automation:** Support for Coded UI tests and QTP/UFT testing
- **Overlay Display:** Scrollbars appear over default positions without layout changes

### Architecture

```
SfScrollFrame
├── Control Property (attach to target)
├── HorizontalScrollBar
│   ├── Style (ScrollBarStyleInfo)
│   ├── Value, SmallChange
│   ├── EnableMaximumArrow, EnableMinimumArrow, EnableThumb
│   └── ContextMenuShowing event
└── VerticalScrollBar
    ├── Style (ScrollBarStyleInfo)
    ├── Value, SmallChange
    ├── EnableMaximumArrow, EnableMinimumArrow, EnableThumb
    └── ContextMenuShowing event
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly deployment and required references
- Attaching SfScrollFrame through designer
- Attaching SfScrollFrame through code
- Programmatic scrolling with Value property
- Configuring SmallChange for scroll speed
- Basic implementation examples

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Customizing scrollbar appearance with ScrollBarStyleInfo
- Arrow button styling (colors, hover, pressed, disabled states)
- Thumb styling (colors, width, border, states)
- Disabling arrow buttons and thumbs
- Built-in theme support (Office2016, Office2019, HighContrast)
- Loading and applying theme assemblies
- ThemeName property usage

### Context Menu Customization
📄 **Read:** [references/context-menu.md](references/context-menu.md)
- Default context menu functionality
- ContextMenuShowing event handling
- Disabling default context menu
- Showing custom context menus
- Adding items to default context menu
- ContextMenuShowingEventArgs properties

### Localization Support
📄 **Read:** [references/localization.md](references/localization.md)
- Localizing scrollbar context menu strings
- Adding resource files (.resx) to application
- Creating culture-specific resources
- Setting CurrentUICulture
- Editing default culture resources
- Localizing from different assemblies/namespaces

### UI Automation and Testing
📄 **Read:** [references/ui-automation.md](references/ui-automation.md)
- Microsoft UI Automation overview
- Coded UI Test (CUIT) support (Level 1 & 2)
- CodedUITestBuilder usage
- Recording actions and adding assertions
- Running automated tests
- QTP/UFT testing support

### Limitations and Constraints
📄 **Read:** [references/limitations.md](references/limitations.md)
- Applicable controls (ScrollableControl derivatives)
- Incompatible controls (custom scrollbar implementations)
- LargeChange property constraints
- Best practices and workarounds

## Quick Start

### Basic Implementation

```csharp
using Syncfusion.WinForms.Controls;

// Create SfScrollFrame instance
SfScrollFrame sfScrollFrame1 = new SfScrollFrame();

// Create a ListView (or any ScrollableControl)
ListView listView1 = new ListView();
listView1.View = View.Details;
listView1.Columns.Add("Column 1", 150);
listView1.Columns.Add("Column 2", 150);

// Add items
for (int i = 0; i < 100; i++)
{
    listView1.Items.Add(new ListViewItem(new[] { $"Item {i}", $"Value {i}" }));
}

// Attach SfScrollFrame to ListView
sfScrollFrame1.Control = listView1;

// Add ListView to form
this.Controls.Add(listView1);
```

### With Theme Applied

```csharp
using Syncfusion.WinForms.Controls;
using Syncfusion.WinForms.Themes;

// In Program.cs
static void Main()
{
    // Load theme assembly
    SfSkinManager.LoadAssembly(typeof(Office2016Theme).Assembly);
    
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);
    Application.Run(new MainForm());
}

// In form
public MainForm()
{
    InitializeComponent();
    
    // Create and attach SfScrollFrame
    SfScrollFrame sfScrollFrame1 = new SfScrollFrame();
    sfScrollFrame1.Control = listView1;
    
    // Apply theme
    sfScrollFrame1.ThemeName = "Office2016Colorful";
}
```

## Common Patterns

### Pattern 1: Attach to Panel with Custom Styling

```csharp
// Create Panel with content
Panel panel1 = new Panel();
panel1.AutoScroll = true;
panel1.Size = new Size(400, 300);

// Add controls to panel
for (int i = 0; i < 50; i++)
{
    Button btn = new Button();
    btn.Text = $"Button {i}";
    btn.Location = new Point(10, i * 30);
    panel1.Controls.Add(btn);
}

// Attach SfScrollFrame
SfScrollFrame sfScrollFrame1 = new SfScrollFrame();
sfScrollFrame1.Control = panel1;

// Customize vertical scrollbar appearance
sfScrollFrame1.VerticalScrollBar.Style.ScrollBarBackColor = Color.LightGray;
sfScrollFrame1.VerticalScrollBar.Style.ThumbColor = Color.Gray;
sfScrollFrame1.VerticalScrollBar.Style.ThumbHoverColor = Color.Black;
sfScrollFrame1.VerticalScrollBar.Style.ThumbPressedColor = Color.Blue;
sfScrollFrame1.VerticalScrollBar.Style.ThumbWidth = 10;

this.Controls.Add(panel1);
```

### Pattern 2: Programmatic Scrolling with Speed Control

```csharp
// Attach SfScrollFrame
sfScrollFrame1.Control = listView1;

// Configure scroll speed
sfScrollFrame1.HorizontalScrollBar.SmallChange = 10;
sfScrollFrame1.VerticalScrollBar.SmallChange = 20;

// Scroll to specific position
Button scrollButton = new Button();
scrollButton.Text = "Scroll to Middle";
scrollButton.Click += (s, e) =>
{
    // Get maximum scroll value
    int maxValue = sfScrollFrame1.VerticalScrollBar.Maximum;
    
    // Scroll to middle
    sfScrollFrame1.VerticalScrollBar.Value = maxValue / 2;
};
```

### Pattern 3: Custom Context Menu

```csharp
sfScrollFrame1.Control = listView1;

// Handle ContextMenuShowing event for vertical scrollbar
sfScrollFrame1.VerticalScrollBar.ContextMenuShowing += (sender, e) =>
{
    // Add custom menu item to default menu
    ToolStripMenuItem customItem = new ToolStripMenuItem("Show Current Position");

    customItem.Click += (s, ev) =>
    {
        int position = sfScrollFrame1.VerticalScrollBar.Value;
        MessageBox.Show($"Current scroll position: {position}");
    });

    e.ContextMenu.Items.Add(customItem);
};
```

### Pattern 4: Disabled Arrow Buttons with Custom Colors

```csharp
sfScrollFrame1.Control = panel1;

// Disable minimum arrows (top/left)
sfScrollFrame1.VerticalScrollBar.EnableMinimumArrow = false;
sfScrollFrame1.HorizontalScrollBar.EnableMinimumArrow = false;

// Set disabled button colors
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledBackColor = Color.Silver;
sfScrollFrame1.VerticalScrollBar.Style.ArrowButtonDisabledForeColor = Color.Gray;

sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonDisabledBackColor = Color.Silver;
sfScrollFrame1.HorizontalScrollBar.Style.ArrowButtonDisabledForeColor = Color.Gray;
```

## Key Properties

### SfScrollFrame Properties

| Property | Type | Description |
|----------|------|-------------|
| `Control` | Control | The control to which scrollbars are attached (must be ScrollableControl) |
| `HorizontalScrollBar` | ScrollBarBase | Access to horizontal scrollbar configuration |
| `VerticalScrollBar` | ScrollBarBase | Access to vertical scrollbar configuration |
| `ThemeName` | string | Theme to apply (Office2016Colorful, Office2016White, etc.) |

### ScrollBar Properties

| Property | Type | Description |
|----------|------|-------------|
| `Value` | int | Current scroll position |
| `SmallChange` | int | Scroll amount for arrow button clicks |
| `EnableMaximumArrow` | bool | Enable/disable bottom/right arrow button |
| `EnableMinimumArrow` | bool | Enable/disable top/left arrow button |
| `EnableThumb` | bool | Enable/disable scrollbar thumb |
| `Style` | ScrollBarStyleInfo | Appearance customization settings |

### ScrollBarStyleInfo Properties

| Property | Type | Description |
|----------|------|-------------|
| `ScrollBarBackColor` | Color | Background color of scrollbar track |
| `ThumbColor` | Color | Normal state thumb color |
| `ThumbHoverColor` | Color | Thumb color when mouse hovers |
| `ThumbPressedColor` | Color | Thumb color when clicked |
| `ThumbWidth` | int | Width/height of thumb (max: scrollbar size) |
| `ThumbBorderColor` | Color | Border color of thumb |
| `ArrowButtonBackColor` | Color | Normal state arrow button background |
| `ArrowButtonForeColor` | Color | Normal state arrow icon color |
| `ArrowButtonHoverBackColor` | Color | Arrow button background on hover |
| `ArrowButtonPressedBackColor` | Color | Arrow button background when clicked |
| `ArrowButtonDisabledBackColor` | Color | Disabled arrow button background |
| `ArrowButtonBorderColor` | Color | Arrow button border color |

## Common Use Cases

### Use Case 1: Enhanced ListView with Custom Scrollbars

**Scenario:** Replace default ListView scrollbars with themed, customized scrollbars.

**Implementation:**
```csharp
// Setup ListView
ListView listView = new ListView { View = View.Details, Size = new Size(400, 300) };
listView.Columns.Add("Name", 150);
listView.Columns.Add("Value", 150);

// Attach themed scrollbar
SfScrollFrame scrollFrame = new SfScrollFrame();
scrollFrame.Control = listView;
scrollFrame.ThemeName = "Office2019Colorful";

// Customize thumb width
scrollFrame.VerticalScrollBar.Style.ThumbWidth = 8;
scrollFrame.HorizontalScrollBar.Style.ThumbWidth = 8;
```

### Use Case 2: Panel with Fast Scrolling

**Scenario:** Panel containing many controls with increased scroll speed.

**Implementation:**
```csharp
// Setup Panel
Panel panel = new Panel { AutoScroll = true, Size = new Size(500, 400) };

// Attach scrollbar with fast scrolling
SfScrollFrame scrollFrame = new SfScrollFrame();
scrollFrame.Control = panel;
scrollFrame.VerticalScrollBar.SmallChange = 30;   // Faster vertical scroll
scrollFrame.HorizontalScrollBar.SmallChange = 30; // Faster horizontal scroll
```

### Use Case 3: Localized Application

**Scenario:** Multi-language application with localized scrollbar menus.

**Implementation:**
```csharp
// Set culture before InitializeComponent
System.Threading.Thread.CurrentThread.CurrentUICulture = 
    new System.Globalization.CultureInfo("de-DE");

InitializeComponent();

// SfScrollFrame automatically uses localized context menu strings
// if corresponding .resx files exist
```

### Use Case 4: Disabled Scrollbar Interaction

**Scenario:** Display-only scrollable content (no thumb dragging).

**Implementation:**
```csharp
SfScrollFrame scrollFrame = new SfScrollFrame();
scrollFrame.Control = listView;

// Disable thumb dragging (scrolling only via arrows/wheel)
scrollFrame.VerticalScrollBar.EnableThumb = false;
scrollFrame.HorizontalScrollBar.EnableThumb = false;

// Set disabled thumb color
scrollFrame.VerticalScrollBar.Style.ThumbDisabledColor = Color.LightGray;
scrollFrame.HorizontalScrollBar.Style.ThumbDisabledColor = Color.LightGray;
```

## Troubleshooting

### Issue: Scrollbars not appearing

**Solution:**
- Verify the target control is derived from `ScrollableControl`
- Check that content exceeds control size (scrollbars only appear when needed)
- Ensure `AutoScroll` property is `true` for Panel controls

### Issue: Theme not applying

**Solution:**
- Load theme assembly in `Program.cs` before running application
- Verify `ThemeName` spelling and casing
- Check theme assembly is referenced in project

### Issue: Context menu not showing

**Solution:**
- Right-click directly on the scrollbar track or thumb
- Verify `ContextMenuShowing` event isn't canceling the menu
- Check that control is focused

### Issue: SmallChange not working as expected

**Solution:**
- `SmallChange` affects arrow button clicks, not mouse wheel scrolling
- Mouse wheel scrolling is controlled by the attached control
- Use higher values (10-30) for noticeable speed changes

## Best Practices

1. **Attach early:** Set the `Control` property in form constructor after `InitializeComponent()`
2. **Load themes once:** Load theme assemblies in `Program.cs`, not per-form
3. **Consistent styling:** Apply the same theme across all SfScrollFrame instances in your application
4. **SmallChange tuning:** Test different values (10-50) to find optimal scroll speed
5. **Handle events:** Subscribe to `ContextMenuShowing` for custom menu behavior
6. **Test compatibility:** Verify SfScrollFrame works with your target control before deployment
7. **Localization setup:** Add resource files early in development for multi-language apps
8. **Performance:** Avoid excessive style property changes during runtime

