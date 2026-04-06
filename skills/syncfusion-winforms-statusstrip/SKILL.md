---
name: syncfusion-winforms-statusstrip
description: Implement Syncfusion StatusStripEx control for creating professional status bars in Windows Forms applications. Use when creating application status indicators with progress bars, status labels, buttons, and sizing grips. Covers StatusControl items (right-aligned), Notification items (left-aligned), ProgressBar integration, Office2007 color schemes (Silver/Blue/Black), Office2016 themes, custom managed colors, and sizing grip customization for bottom-docked status panels.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WinForms StatusStripEx

The **StatusStripEx** control provides an enhanced status strip that can be added to the bottom of forms or Ribbon controls. It supports various item types, Office themes, custom colors, and flexible positioning for creating professional status bars.

## When to Use This Skill

Use this skill when the user needs to:

- **Application Status Bars:** Display application status, user info, or system state at bottom of form
- **Progress Indicators:** Show progress bars or loading indicators in status strip
- **Ribbon Status Strips:** Add status bar to bottom of RibbonControlAdv
- **Left/Right Alignment:** Different item types for left-aligned (notifications) vs right-aligned (status)
- **Themed Status Bars:** Office2007 (Silver/Blue/Black) or Office2016 themes
- **Custom Status Items:** Buttons, labels, dropdowns, split buttons, progress bars, trackbars
- **Sizing Grip:** Resizable form indicator with customizable grip appearance
- **Status Information:** Display file info, record counts, zoom levels, connection status
- **Word-Style Context Menu:** Custom context menu for status items

## Component Overview

**StatusStripEx** (`Syncfusion.Windows.Forms.Tools.StatusStripEx`) is an enhanced status strip control supporting:

- **Two Item Types:**
  - **StatusControl Items:** Right-aligned (StatusLabel, ProgressBar, DropDownButton, SplitButton, PanelItem, TrackBarItem)
  - **Notification Items:** Left-aligned (StatusStripButton, StatusStripLabel, StatusStripProgressBar, etc.)
- **Office2007 Color Schemes:** Silver, Blue, Black
- **Office2016 Visual Styles:** Colorful, White, Black, DarkGray
- **Custom Managed Colors:** Apply custom color themes
- **Sizing Grip:** Customizable resize grip at bottom-right corner
- **Smart Tag Support:** Quick item addition via designer
- **Custom Context Menu:** Word2007-style status bar context menu
- **Ribbon Integration:** Works seamlessly with RibbonControlAdv

**Key Namespace:** `Syncfusion.Windows.Forms.Tools`

**Assembly:** `Syncfusion.Tools.Windows.dll` (and dependencies)

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** User needs to install, configure, or create their first StatusStripEx.

**Topics covered:**
- Assembly deployment and NuGet package installation
- Adding StatusStripEx via designer (toolbox)
- Adding StatusStripEx via code
- Docking to bottom of form or Ribbon
- Items Collection Editor for adding items
- Smart tag support for quick item addition
- Basic status strip setup

### Status Items and Collections
📄 **Read:** [references/status-items.md](references/status-items.md)

**When to read:** User wants to add status labels, progress bars, buttons, or understand item positioning.

**Topics covered:**
- StatusControl items (right-aligned): StatusLabel, ProgressBar, DropDownButton, SplitButton, PanelItem, TrackBarItem
- Notification items (left-aligned): StatusStripButton, StatusStripLabel, StatusStripProgressBar, StatusStripDropDownButton, etc.
- Difference between StatusControl and Notification items
- Adding items programmatically via Items collection
- Item positioning and alignment strategies
- Using different item types effectively

### Sizing Grip Customization
📄 **Read:** [references/sizing-grip.md](references/sizing-grip.md)

**When to read:** User needs to show/hide sizing grip or customize grip appearance.

**Topics covered:**
- Sizing grip overview and purpose
- SizingGrip property (true/false)
- GripStyle property (Visible, Hidden)
- GripMargin property for spacing
- Customizing grip appearance
- When to show or hide sizing grip

### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)

**When to read:** User wants Office2007/2016 themes or custom status bar colors.

**Topics covered:**
- Office2007 color schemes (Silver, Blue, Black)
- OfficeColorScheme property usage
- Office2016 visual styles (Colorful, White, Black, DarkGray)
- VisualStyle property configuration
- Custom managed colors with ApplyManagedColors
- Custom context menu (Word2007-style)
- StatusString property for context menu items

---

## Quick Start Example

### Basic StatusStripEx with Items

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Windows.Forms;

// Create StatusStripEx
StatusStripEx statusStripEx1 = new StatusStripEx();

// Create status label (right-aligned)
ToolStripStatusLabel statusLabel = new ToolStripStatusLabel();
statusLabel.Text = "Ready";

// Create progress bar (right-aligned)
ToolStripProgressBar progressBar = new ToolStripProgressBar();
progressBar.Value = 50;

// Add items to StatusStripEx
statusStripEx1.Items.AddRange(new ToolStripItem[] {
    statusLabel,
    progressBar
});

// Dock to bottom
statusStripEx1.Dock = DockStyleEx.Bottom;

// Add to form
this.Controls.Add(statusStripEx1);
```

### StatusStripEx with Office2016 Theme

```csharp
// Apply Office2016 Colorful theme
statusStripEx1.VisualStyle = StatusStripExStyle.Office2016Colorful;

// Enable sizing grip
statusStripEx1.SizingGrip = true;
statusStripEx1.GripStyle = ToolStripGripStyle.Visible;
```

---

## Common Patterns

### Application Status Bar with Multiple Items

```csharp
// Create StatusStripEx
StatusStripEx statusBar = new StatusStripEx();
statusBar.Dock = DockStyleEx.Bottom;

// Left-aligned notification item
ToolStripStatusLabel notificationLabel = new ToolStripStatusLabel();
notificationLabel.Text = "Connection: Active";
notificationLabel.Spring = true;  // Fill available space

// Right-aligned status items
ToolStripStatusLabel recordLabel = new ToolStripStatusLabel();
recordLabel.Text = "Records: 1,234";

ToolStripProgressBar loadingProgress = new ToolStripProgressBar();
loadingProgress.Visible = false;  // Show when loading

ToolStripStatusLabel zoomLabel = new ToolStripStatusLabel();
zoomLabel.Text = "Zoom: 100%";

// Add all items
statusBar.Items.AddRange(new ToolStripItem[] {
    notificationLabel,
    recordLabel,
    loadingProgress,
    zoomLabel
});

this.Controls.Add(statusBar);
```

**When to use:** Multi-purpose status bar showing various application states.

### Progress Indicator in Status Bar

```csharp
// Status bar with progress tracking
StatusStripEx statusBar = new StatusStripEx();

ToolStripStatusLabel statusText = new ToolStripStatusLabel();
statusText.Text = "Processing...";

ToolStripProgressBar progressBar = new ToolStripProgressBar();
progressBar.Style = ProgressBarStyle.Continuous;
progressBar.Maximum = 100;

statusBar.Items.AddRange(new ToolStripItem[] { statusText, progressBar });
statusBar.Dock = DockStyleEx.Bottom;

// Update progress
void UpdateProgress(int percent)
{
    progressBar.Value = percent;
    statusText.Text = $"Processing... {percent}%";
}
```

**When to use:** Long-running operations requiring progress feedback.

### Ribbon with StatusStripEx

```csharp
// StatusStripEx at bottom of Ribbon form
StatusStripEx ribbonStatusBar = new StatusStripEx();
ribbonStatusBar.Dock = DockStyleEx.Bottom;

// Apply matching Office2016 theme
ribbonStatusBar.VisualStyle = StatusStripExStyle.Office2016Colorful;

// Add status items
ToolStripStatusLabel pageLabel = new ToolStripStatusLabel();
pageLabel.Text = "Page 1 of 10";

ToolStripStatusLabel wordCount = new ToolStripStatusLabel();
wordCount.Text = "Words: 245";

ribbonStatusBar.Items.AddRange(new ToolStripItem[] { pageLabel, wordCount });

// Add to form (ensure it's added after Ribbon)
this.Controls.Add(ribbonStatusBar);
```

**When to use:** Ribbon-based applications requiring status information.

### Custom Themed Status Bar

```csharp
// Apply custom managed color
statusStripEx1.OfficeColorScheme = ToolStripEx.ColorScheme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.DarkBlue);

// Custom context menu for status items
ToolStripStatusLabel statusLabel = new ToolStripStatusLabel();
statusLabel.Text = "Pages";
statusLabel.StatusString = "1/1";  // Shows in Word-style context menu
```

**When to use:** Branded applications with custom color schemes.

---

## Key Properties

### Core Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| **Items** | `ToolStripItemCollection` | Collection of status items | Add/manage status strip items |
| **Dock** | `DockStyleEx` | Docking position (Bottom) | Position status strip at bottom |
| **SizingGrip** | `bool` | Show/hide sizing grip | Enable form resizing indicator |
| **GripStyle** | `ToolStripGripStyle` | Grip visibility style | Customize grip appearance |
| **GripMargin** | `Padding` | Grip margin spacing | Adjust grip positioning |

### Styling Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| **VisualStyle** | `StatusStripExStyle` | Office2016 visual style | Apply Office2016 themes |
| **OfficeColorScheme** | `ColorScheme` | Office2007 color scheme | Apply Silver/Blue/Black themes |

### Item Types

**StatusControl Items (Right-Aligned):**
- `StatusLabel` - Text label for status information
- `ProgressBar` - Progress indicator
- `DropDownButton` - Dropdown button for options
- `SplitButton` - Split button with dropdown
- `PanelItem` - Custom panel container
- `TrackBarItem` - Slider control

**Notification Items (Left-Aligned):**
- `StatusStripButton` - Button control
- `StatusStripLabel` - Text label
- `StatusStripProgressBar` - Progress indicator
- `StatusStripDropDownButton` - Dropdown button
- `StatusStripSplitButton` - Split button
- `StatusStripPanelItem` - Custom panel

---

## Common Use Cases

### 1. Document Editor Status Bar
**Scenario:** Word-like editor showing page, word count, zoom.
- Add StatusLabel for page number (e.g., "Page 1 of 5")
- Add StatusLabel for word count (e.g., "Words: 1,234")
- Add TrackBarItem for zoom control
- Use Office2016Colorful theme for modern look
- Read: [status-items.md](references/status-items.md), [styling-and-appearance.md](references/styling-and-appearance.md)

### 2. Data Application with Record Count
**Scenario:** Database application showing record count and connection status.
- Left-aligned notification label for connection status
- Right-aligned label for record count
- ProgressBar for data loading operations
- Read: [status-items.md](references/status-items.md)

### 3. File Manager with Status Info
**Scenario:** File manager showing selected items, total size.
- StatusLabel: "5 items selected"
- StatusLabel: "Total: 2.5 GB"
- StatusLabel: "Free space: 150 GB"
- Read: [getting-started.md](references/getting-started.md)

### 4. Application with Background Tasks
**Scenario:** Show background task progress in status bar.
- StatusStripLabel: Task description (left-aligned)
- StatusStripProgressBar: Task progress (left-aligned)
- Hide progress when task completes
- Read: [status-items.md](references/status-items.md)

### 5. Branded Application Status Bar
**Scenario:** Custom color theme matching brand identity.
- Set OfficeColorScheme to Managed
- Apply custom color via ApplyManagedColors
- Match status bar to application theme
- Read: [styling-and-appearance.md](references/styling-and-appearance.md)

### 6. Ribbon Application Status Bar
**Scenario:** Status bar for RibbonControlAdv application.
- Dock StatusStripEx to bottom
- Match VisualStyle to Ribbon theme
- Add relevant status items (page, zoom, etc.)
- Read: [getting-started.md](references/getting-started.md), [styling-and-appearance.md](references/styling-and-appearance.md)

---

## Best Practices

1. **Item Alignment:** Use StatusControl items for right-aligned, Notification items for left-aligned
2. **Theme Consistency:** Match StatusStripEx style to application theme (Office2007/2016)
3. **Sizing Grip:** Enable for resizable forms, disable for fixed-size forms
4. **Progress Indicators:** Show ProgressBar only during operations, hide when complete
5. **Spring Property:** Use Spring=true on left items to push right items to the right
6. **Item Count:** Keep status bar items minimal (3-6 items) for clarity
7. **Text Updates:** Update status text dynamically to reflect application state
8. **Ribbon Compatibility:** Add StatusStripEx after Ribbon to ensure proper docking

---

## See Also

- [Syncfusion StatusStripEx Documentation](https://help.syncfusion.com/windowsforms/statusstripex/overview)
- [StatusStripEx API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.StatusStripEx.html)
