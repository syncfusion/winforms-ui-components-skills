# Getting Started with Windows Forms Splitter Control

This guide walks through the installation, setup, and initial implementation of the Syncfusion Windows Forms SplitterControl.

## What is SplitterControl?

SplitterControl is a container control that enables displaying multiple views of the same grid by using split panes. This allows:
- Viewing more than one copy of a worksheet
- Scrolling through each pane independently
- Splitting horizontally and vertically
- Dragging the splitter to adjust pane sizes

The control is ideal for scenarios where users need to compare or reference different sections of the same data simultaneously.

## Installation and Setup

### NuGet Package Installation

- Important: Use version * (latest available).

Install via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Shared.Base
```

Or via .NET CLI:

```bash
dotnet add package Syncfusion.Shared.Base
```

The NuGet package automatically includes all required dependencies.

### Required Namespace and Assembly

```csharp
using Syncfusion.Windows.Forms;
```

**Required Assembly:** `Syncfusion.Shared.Base.dll`

## Creating a New Windows Forms Project

1. **Create Project:**
   - Open Visual Studio
   - File → New → Project
   - Select "Windows Forms App (.NET Framework)" or ".NET"
   - Name your project and click Create

2. **Add SplitterControl via Designer:**
   - Open the Toolbox in Visual Studio
   - Locate "SplitterControl" in the Syncfusion Tools section
   - Drag and drop it onto your Form

3. **Add SplitterControl via Code:**
   - Alternatively, create the control programmatically (see below)

## Basic Implementation via Designer

When you drag and drop SplitterControl from the Toolbox:

1. The control appears on your form
2. Default properties are set automatically
3. You can configure properties via the Properties window
4. The designer generates initialization code in `InitializeComponent()`

**Visual Result:**

The SplitterControl appears as a container ready to display grid content with split capabilities.

## Basic Implementation via Code

```csharp
using System.Windows.Forms;
using Syncfusion.Windows.Forms;

public class MyForm : Form
{
    private SplitterControl splitterControl1;
    
    public MyForm()
    {
        // Create SplitterControl
        splitterControl1 = new SplitterControl();
        splitterControl1.Dock = DockStyle.Fill;
        
        // Add to form
        this.Controls.Add(splitterControl1);
        this.ClientSize = new System.Drawing.Size(800, 600);
        this.Text = "SplitterControl Demo";
    }
}
```

## Initial Configuration

```csharp
SplitterControl splitterControl1 = new SplitterControl()
{
    Dock = DockStyle.Fill,
    SplitBars = DynamicSplitBars.SplitColumns,  // Enable splits
    ShowHorizontalScrollBar = true,
    ShowVerticalScrollBar = true,
    ShowSizeGrip = true
};
this.Controls.Add(splitterControl1);
```

## Next Steps

Now that you have a basic SplitterControl set up, explore these topics:

- **Configure split behavior** → Read `split-behavior.md` to learn about split modes (columns, rows, both)
- **Control scrollbar visibility** → Read `scrollbar-features.md` to show/hide scrollbars
- **Customize scrollbar styles** → Read `scrollbar-customization.md` for Office themes
- **Polish the appearance** → Read `visual-customization.md` for sizing grip and styles

## Common Gotchas

**Missing namespaces:** Add `using Syncfusion.Windows.Forms;`

**Control not visible:** Use `Dock = DockStyle.Fill` and ensure control is added to form's Controls collection.

**No split functionality:** Set `SplitBars` property (default is `None`):
```csharp
splitterControl1.SplitBars = DynamicSplitBars.SplitColumns;
```

## Quick Reference

| Task | Code |
|------|------|
| Create instance | `new SplitterControl()` |
| Add to form | `this.Controls.Add(splitterControl1)` |
| Fill entire form | `splitterControl1.Dock = DockStyle.Fill` |
| Enable column splits | `splitterControl1.SplitBars = DynamicSplitBars.SplitColumns` |
| Enable row splits | `splitterControl1.SplitBars = DynamicSplitBars.SplitRows` |
| Enable both | `splitterControl1.SplitBars = DynamicSplitBars.Both` |
