# Split Behavior in Windows Forms Splitter Control

This guide covers how to configure the split behavior of the SplitterControl, including split directions, modes, and user interaction patterns.

## Understanding Split Behavior

The SplitterControl can be configured to support different types of splits:
- **Column splitting** (vertical split bars) - Creates side-by-side panes
- **Row splitting** (horizontal split bars) - Creates top-bottom panes
- **Both directions** - Enables full grid-style splitting (up to 4 panes)
- **None** - Disables splitting entirely

This is controlled through the `SplitBars` property using the `DynamicSplitBars` enumeration.

## DynamicSplitBars Enumeration

The `DynamicSplitBars` enum provides four options:

| Value | Description | Visual Result |
|-------|-------------|---------------|
| `None` | No splitting allowed | Single pane only |
| `SplitColumns` | Vertical split bar (side-by-side) | Left and right panes |
| `SplitRows` | Horizontal split bar (top-bottom) | Top and bottom panes |
| `Both` | Both vertical and horizontal | Up to 4 panes (grid layout) |

## Configuring Split Modes

### Configuring Split Modes

```csharp
// Column splitting (side-by-side, left/right panes)
this.splitterControl1.SplitBars = DynamicSplitBars.SplitColumns;
// Use for: Document comparison, code editors, before/after views

// Row splitting (top-bottom panes)
this.splitterControl1.SplitBars = DynamicSplitBars.SplitRows;
// Use for: Header/detail views, code editor with console, document with notes

// Both directions (quad-pane layout)
this.splitterControl1.SplitBars = DynamicSplitBars.Both;
// Use for: Excel-style viewers, data analysis, multi-section dashboards

// Disable splits
this.splitterControl1.SplitBars = DynamicSplitBars.None;
// Use for: Simple single-view interfaces
```

## Implementation Examples

```csharp
// Side-by-side comparison
splitterControl1.SplitBars = DynamicSplitBars.SplitColumns;
splitterControl1.ShowSizeGrip = true;

// Code editor with output panel
splitterControl1.SplitBars = DynamicSplitBars.SplitRows;
splitterControl1.ShowSizeGrip = true;

// Excel-style worksheet viewer
splitterControl1.SplitBars = DynamicSplitBars.Both;
splitterControl1.GridOfficeScrollBars = OfficeScrollBars.Office2010;
```

## User Interaction

### How Users Create Splits

Once `SplitBars` is configured, users can interact with the splitter:

1. **Initiate Split:**
   - User positions mouse over the split area
   - Visual indicator appears
   - Click and drag to create the split

2. **Adjust Split Position:**
   - User hovers over existing split bar
   - Cursor changes to resize cursor
   - Click and drag to reposition

3. **Independent Scrolling:**
   - Each pane scrolls independently
   - Content remains synchronized across views
   - Users can compare different sections simultaneously

### Split Bar Behavior

- **Draggable:** Users can freely drag split bars to adjust pane sizes
- **Visual Feedback:** Cursor changes when hovering over split bars
- **Smooth Resizing:** Panes resize dynamically as the split bar moves
- **Minimum Sizes:** System prevents panes from becoming too small

## Programmatic Split Control

```csharp
// Change split mode at runtime
private void EnableColumnSplitting()
{
    splitterControl1.SplitBars = DynamicSplitBars.SplitColumns;
}

private void EnableRowSplitting()
{
    splitterControl1.SplitBars = DynamicSplitBars.SplitRows;
}
```

## Best Practices

**Choose the right mode:**
- **SplitColumns:** Horizontal comparisons (side-by-side)
- **SplitRows:** Vertical layouts (header/content)
- **Both:** Complex data analysis
- **None:** Simple interfaces

**UX Tips:** Enable `ShowSizeGrip` for better discoverability, offer menu options for split mode selection.

## Troubleshooting

**Split bars not appearing:** Verify `SplitBars` property is not set to `None`.
**Can't drag split bars:** Ensure control has focus and mouse events aren't intercepted.
**Unexpected behavior:** `SplitColumns` = vertical bar (left/right), `SplitRows` = horizontal bar (top/bottom).

## Quick Reference

| Split Mode | Property Value | Visual Result |
|------------|---------------|---------------|
| Side-by-side | `DynamicSplitBars.SplitColumns` | Vertical split bar |
| Top-bottom | `DynamicSplitBars.SplitRows` | Horizontal split bar |
| Grid layout | `DynamicSplitBars.Both` | Both split bars |
| No splits | `DynamicSplitBars.None` | Single pane |
