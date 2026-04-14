# Getting Started with ProgressBarAdv

This guide covers installation, setup, and basic usage of ProgressBarAdv in Windows Forms applications.

## When to Read This

Read this reference when:
- Setting up ProgressBarAdv for the first time
- Understanding assembly requirements
- Adding the control via designer or code
- Configuring value range and text format
- Troubleshooting setup issues

## Assembly Requirements

**Required assembly:** `Syncfusion.Shared.Base.dll`

**Namespace:**
```csharp
using Syncfusion.Windows.Forms.Tools;
```

## NuGet Installation

```powershell
Install-Package Syncfusion.Windows.Forms
```

Or via NuGet Package Manager UI: search for `Syncfusion.Windows.Forms`.

## Adding via Designer

1. Open your form in designer view.
2. Locate **ProgressBarAdv** in the Toolbox.
3. Drag and drop onto the form — the required assembly is referenced automatically.
4. Configure properties (`Value`, `Minimum`, `Maximum`, `TextStyle`) in the Properties window.

## Adding Programmatically

```csharp
using Syncfusion.Windows.Forms.Tools;

ProgressBarAdv progressBarAdv1 = new ProgressBarAdv();
progressBarAdv1.Location = new Point(20, 20);
progressBarAdv1.Size = new Size(300, 30);
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBarAdv1.Value = 60;
this.Controls.Add(progressBarAdv1);
```

## Setting Value Range

```csharp
progressBarAdv1.Minimum = 0;
progressBarAdv1.Maximum = 100;
progressBarAdv1.Value = 50;
```

**Important:**
- Default `Minimum` = 0, `Maximum` = 100.
- `Value` must be within `[Minimum, Maximum]`; setting it outside throws an exception.

### Custom Range Example

```csharp
// File sizes in MB
progressBarAdv1.Minimum = 0;
progressBarAdv1.Maximum = 1000;
progressBarAdv1.Value = 450;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Value;
```

## Configure Text Format

```csharp
// Percentage: "60%"
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;

// Value: "60" (or "60/100" depending on style)
progressBarAdv1.TextStyle = ProgressBarTextStyles.Value;

// Custom text
progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
progressBarAdv1.CustomText = "Loading...";

// Text alignment
progressBarAdv1.TextAlignment = TextAlignment.Center;
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class MainForm : Form
{
    private ProgressBarAdv progressBarAdv1;
    private Button startButton;
    private System.Windows.Forms.Timer progressTimer;

    public MainForm()
    {
        InitializeComponent();
        InitializeProgressBar();
    }

    private void InitializeProgressBar()
    {
        progressBarAdv1 = new ProgressBarAdv
        {
            Location = new Point(20, 20),
            Size = new Size(400, 30),
            Minimum = 0,
            Maximum = 100,
            Value = 0,
            ProgressStyle = ProgressBarStyles.Office2016Colorful,
            TextStyle = ProgressBarTextStyles.Percentage,
            TextAlignment = TextAlignment.Center
        };
        this.Controls.Add(progressBarAdv1);

        startButton = new Button
        {
            Text = "Start",
            Location = new Point(20, 60)
        };
        startButton.Click += StartButton_Click;
        this.Controls.Add(startButton);

        progressTimer = new System.Windows.Forms.Timer { Interval = 100 };
        progressTimer.Tick += ProgressTimer_Tick;
    }

    private void StartButton_Click(object sender, EventArgs e)
    {
        progressBarAdv1.Value = 0;
        startButton.Enabled = false;
        progressTimer.Start();
    }

    private void ProgressTimer_Tick(object sender, EventArgs e)
    {
        if (progressBarAdv1.Value < progressBarAdv1.Maximum)
        {
            progressBarAdv1.Value += 5;
        }
        else
        {
            progressTimer.Stop();
            startButton.Enabled = true;
            MessageBox.Show("Operation complete!");
        }
    }
}
```

## Best Practices

- Always bounds-check before setting `Value`:
  ```csharp
  if (newValue >= progressBarAdv1.Minimum && newValue <= progressBarAdv1.Maximum)
      progressBarAdv1.Value = newValue;
  ```
- Update the progress bar on the UI thread:
  ```csharp
  this.Invoke((Action)(() => progressBarAdv1.Value = i));
  ```
- Choose text style by context: **Percentage** for general use, **Value** for item counts, **Custom** for status messages.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Control not in Toolbox | Install NuGet, or right-click Toolbox ? Choose Items ? browse to `Syncfusion.Shared.Base.dll` |
| Namespace not found | Add reference to `Syncfusion.Shared.Base.dll`; add `using Syncfusion.Windows.Forms.Tools;` |
| Pages not displaying | Verify `Controls.Add(progressBarAdv1)` was called and `Size`/`Dock` is set |
| Value out of range exception | Ensure `Value` is between `Minimum` and `Maximum` before setting |

## Next Steps

- [text-display.md](text-display.md) — advanced text formatting
- [appearance-styling.md](appearance-styling.md) — colors, gradients, borders
- [orientation-layout.md](orientation-layout.md) — vertical progress bars
- [themes.md](themes.md) — visual styling options
- [events-advanced.md](events-advanced.md) — event handling and custom rendering
