---
name: syncfusion-winforms-progress-bar
description: Implement Syncfusion ProgressBarAdv control in Windows Forms for displaying operation progress with customizable text formats. Use when creating progress indicators, loading bars, or percentage displays with Percentage, Value, or Custom text formats. Covers orientation (horizontal/vertical), themes, progress tracking, and progress events for file operations, installations, or download indicators.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Windows Forms Progress Bar (ProgressBarAdv)

The Syncfusion Windows Forms ProgressBarAdv control provides progress information during lengthy operations such as installation, copying, printing, and downloading. It displays progress with customizable text formats, colors, orientations, and visual styles.

## When to Use This Skill

Use this skill when you need to:

- **Display progress information** for lengthy operations (file operations, installations, downloads)
- **Show percentage completion** or current value out of total
- **Implement loading indicators** with custom text and animations
- **Create progress bars** with horizontal or vertical orientation
- **Customize progress appearance** with colors, gradients, borders, and images
- **Apply visual themes** (Office 2016, System, Tube, Gradient styles)
- **Handle progress events** (ValueChanged, custom rendering)
- **Display custom text** on progress bars (e.g., "Loading...", "Processing...")
- **Track time-consuming operations** with visual feedback

## Component Overview

**Key Features:**
- **Text formats:** Percentage, Value, or Custom text display
- **Progress images:** Use images for progress indication
- **Color customization:** Foreground, background, gradient, and border colors
- **Orientation:** Horizontal or vertical progress bars
- **Visual styles:** Office2016Colorful, Office2016White, Office2016DarkGray, Office2016Black, System, Tube, Gradient
- **Events:** ValueChanged, DrawWaitingCustomRender for custom rendering
- **Themes:** Built-in theme support with ThemesEnabled property

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when you need to:
- Install the ProgressBarAdv control via NuGet or designer
- Add the control to a Windows Forms project
- Create a ProgressBarAdv instance programmatically
- Set basic properties (Value, Minimum, Maximum)
- Configure initial progress bar setup
- Understand assembly deployment requirements

### Text and Display Settings
📄 **Read:** [references/text-display.md](references/text-display.md)

Read this reference when you need to:
- Configure TextStyle (Percentage, Value, Custom)
- Set custom text with CustomText property
- Adjust text alignment (Left, Center, Right)
- Configure text orientation
- Customize text color and font
- Implement display format patterns

### Appearance and Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)

Read this reference when you need to:
- Customize foreground colors and gradients
- Set background colors and images
- Configure border settings (style, color, sides)
- Add progress images
- Use ProgressStyle options
- Apply background styles
- Implement color customization patterns

### Orientation and Layout
📄 **Read:** [references/orientation-layout.md](references/orientation-layout.md)

Read this reference when you need to:
- Set horizontal or vertical orientation
- Configure progress bar layout
- Handle size and positioning
- Implement responsive design patterns
- Adjust for different form layouts

### Themes
📄 **Read:** [references/themes.md](references/themes.md)

Read this reference when you need to:
- Enable theme support with ThemesEnabled
- Apply built-in visual styles (Office2016Colorful, Office2016White, etc.)
- Configure ProgressStyle options
- Choose appropriate themes for your application
- Understand theme selection guidelines

### Events and Advanced Customization
📄 **Read:** [references/events-advanced.md](references/events-advanced.md)

Read this reference when you need to:
- Handle ValueChanged event
- Implement DrawWaitingCustomRender event
- Create custom rendering patterns
- Use WaitingCustomRender mode
- Configure ProgressFallBackStyle options
- Implement advanced customization scenarios

## Quick Start

### Basic Progress Bar

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create ProgressBarAdv instance
ProgressBarAdv progressBarAdv1 = new ProgressBarAdv();
progressBarAdv1.Location = new Point(20, 20);
progressBarAdv1.Size = new Size(300, 30);

// Set range and value
progressBarAdv1.Minimum = 0;
progressBarAdv1.Maximum = 100;
progressBarAdv1.Value = 60;

// Configure text display
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextAlignment = TextAlignment.Center;

// Apply visual style
progressBarAdv1.ProgressStyle = ProgressBarStyles.Office2016Colorful;

// Add to form
this.Controls.Add(progressBarAdv1);
```

## Common Patterns

### Pattern 1: Percentage Progress Bar

```csharp
// Display progress as percentage
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Minimum = 0;
progressBar.Maximum = 100;
progressBar.Value = 45;
progressBar.TextStyle = ProgressBarTextStyles.Percentage;
progressBar.TextAlignment = TextAlignment.Center;

this.Controls.Add(progressBar);
```

### Pattern 2: Custom Text Progress Bar

```csharp
// Display custom text (e.g., "Loading...")
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Minimum = 0;
progressBar.Maximum = 100;
progressBar.Value = 30;
progressBar.TextStyle = ProgressBarTextStyles.Custom;
progressBar.CustomText = "Loading files...";
progressBar.TextAlignment = TextAlignment.Center;

this.Controls.Add(progressBar);
```

### Pattern 3: Value Format Progress Bar

```csharp
// Display current value out of maximum (e.g., "60/100")
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Minimum = 0;
progressBar.Maximum = 100;
progressBar.Value = 60;
progressBar.TextStyle = ProgressBarTextStyles.Value;
progressBar.TextAlignment = TextAlignment.Center;

this.Controls.Add(progressBar);
```

### Pattern 4: Vertical Progress Bar

```csharp
// Create vertical progress bar
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.ProgressOrientation = Orientation.Vertical;
progressBar.Size = new Size(50, 200);
progressBar.Minimum = 0;
progressBar.Maximum = 100;
progressBar.Value = 75;
progressBar.TextStyle = ProgressBarTextStyles.Percentage;

this.Controls.Add(progressBar);
```

### Pattern 5: Themed Progress Bar with Custom Colors

```csharp
// Apply theme and custom colors
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.ProgressStyle = ProgressBarStyles.Office2016Colorful;
progressBar.Minimum = 0;
progressBar.Maximum = 100;
progressBar.Value = 50;

// Custom colors
progressBar.ForeColor = Color.Blue;
progressBar.BackColor = Color.LightGray;

// Text settings
progressBar.TextStyle = ProgressBarTextStyles.Percentage;
progressBar.TextAlignment = TextAlignment.Center;

this.Controls.Add(progressBar);
```

### Pattern 6: Progress Bar with ValueChanged Event

```csharp
// Create progress bar with event handling
ProgressBarAdv progressBar = new ProgressBarAdv();
progressBar.Minimum = 0;
progressBar.Maximum = 100;
progressBar.Value = 0;
progressBar.TextStyle = ProgressBarTextStyles.Custom;
progressBar.CustomText = "Starting...";

// Subscribe to ValueChanged event
progressBar.ValueChanged += ProgressBar_ValueChanged;

this.Controls.Add(progressBar);

// Event handler
private void ProgressBar_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv pb = sender as ProgressBarAdv;
    if (pb.Value < 100)
    {
        pb.CustomText = $"Processing... {pb.Value}%";
    }
    else
    {
        pb.CustomText = "Complete!";
    }
}
```

### Pattern 7: Simulated Long Operation

```csharp
// Simulate a long-running operation with progress updates
private async void StartOperation()
{
    ProgressBarAdv progressBar = new ProgressBarAdv();
    progressBar.Location = new Point(20, 20);
    progressBar.Size = new Size(400, 30);
    progressBar.Minimum = 0;
    progressBar.Maximum = 100;
    progressBar.Value = 0;
    progressBar.TextStyle = ProgressBarTextStyles.Percentage;
    progressBar.ProgressStyle = ProgressBarStyles.Office2016Colorful;
    
    this.Controls.Add(progressBar);
    
    // Simulate operation
    for (int i = 0; i <= 100; i += 10)
    {
        progressBar.Value = i;
        await Task.Delay(500); // Simulate work
    }
    
    MessageBox.Show("Operation complete!");
}
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Value` | `int` | Current progress value |
| `Minimum` | `int` | Minimum progress value (default: 0) |
| `Maximum` | `int` | Maximum progress value (default: 100) |
| `TextStyle` | `ProgressBarTextStyles` | Text format: Percentage, Value, Custom |
| `CustomText` | `string` | Custom text to display (when TextStyle = Custom) |
| `TextAlignment` | `TextAlignment` | Text alignment: Left, Center, Right |
| `ProgressOrientation` | `Orientation` | Horizontal or Vertical orientation |
| `ProgressStyle` | `ProgressBarStyles` | Visual style: Office2016Colorful, Tube, Gradient, etc. |
| `ThemesEnabled` | `bool` | Enable theme support |
| `ForeColor` | `Color` | Foreground/progress color |
| `BackColor` | `Color` | Background color |
| `BackgroundStyle` | `ProgressBarBackgroundStyles` | Background style options |

## Key Events

| Event | Description |
|-------|-------------|
| `ValueChanged` | Occurs when the Value property changes |
| `DrawWaitingCustomRender` | Allows custom rendering of waiting gradient |

## Text Format Options

| TextStyle | Display Format | Example |
|-----------|---------------|---------|
| `Percentage` | Shows percentage | "60%" |
| `Value` | Shows value/max | "60/100" |
| `Custom` | Shows CustomText | "Loading..." |

## Visual Style Options

| ProgressStyle | Description |
|---------------|-------------|
| `Office2016Colorful` | Vibrant Office 2016 theme with blue accents |
| `Office2016White` | Clean, light Office 2016 theme |
| `Office2016DarkGray` | Medium-dark Office 2016 theme |
| `Office2016Black` | High-contrast dark Office 2016 theme |
| `System` | Standard Windows system style |
| `Tube` | Tube-style progress indicator |
| `Gradient` | Gradient progress effect |