---
name: syncfusion-winforms-clock
description: Guide for implementing the Syncfusion WinForms Clock control in Windows Forms applications. Use this skill when the user needs to add a clock display, switch between analog and digital modes, customize clock appearance, apply custom renderers, configure frames or shapes for the digital clock, or freeze the clock at a fixed time. Covers ClockType, ClockFrame, ClockShape, ShowCustomTimeClock, ClockRenderer, and DigitalRenderer.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing the Syncfusion WinForms Clock Control

The Syncfusion `Clock` control (`Syncfusion.Windows.Forms.Tools.Clock`) is a display-only control that shows the current time as either an **analog** or **digital** clock. It supports extensive visual customization — hand colors and thickness, gradient backgrounds, frames and shapes for digital mode, transparency, and a fully replaceable rendering pipeline.

## When to Use This Skill

Use this skill when you need to:
- **Add a clock display** (analog or digital) to a WinForms form
- **Switch between analog and digital** modes via `ClockType`
- **Customize colors** — gradient background, hand colors, border, minute lines
- **Configure digital clock frames or shapes** — `ClockFrame`, `ClockShape`
- **Show a fixed or custom time** — `ShowCustomTimeClock` + `CustomTime`
- **Freeze the clock** at a specific time — `StopTimer`
- **Apply a custom renderer** — override `ClockRenderer.DrawInterior` or `DigitalClockRenderer.DrawDigitalClockFrame`

## Component Overview

| Property | Type | Description |
|---|---|---|
| `ClockType` | `ClockTypes` | `Analog` or `Digital` |
| `ShowCustomTimeClock` | `bool` | Enable custom (non-system) time display |
| `CustomTime` | `DateTime` | The custom time to display when `ShowCustomTimeClock` is true |
| `StopTimer` | `bool` | Freeze the clock at its current time |
| `IsTransparent` | `bool` | Transparent background (analog) |
| `Renderer` | `ClockRenderer` | Custom renderer for analog clock hands |
| `DigitalRenderer` | `DigitalClockRenderer` | Custom renderer for digital clock frame |

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly references and NuGet package setup
- Adding Clock via designer or programmatically
- Switching between analog and digital modes (`ClockType`)
- Displaying a fixed or custom time (`ShowCustomTimeClock`, `CustomTime`)
- Freezing the clock (`StopTimer`)

### Appearance and Color Customization
📄 **Read:** [references/appearance-and-color.md](references/appearance-and-color.md)
- Gradient background colors (`StartGradientBackColor`, `EndGradientBackColor`)
- Hand colors (`HourHandColor`, `MinuteHandColor`, `SecondHandColor`)
- Minute line and border colors (`MinuteColor`, `BorderColor`)
- Hand thickness (`HourHandThickness`, `MinuteHandThickness`, `SecondHandThickness`, `MinuteThickness`)
- Show/hide toggles (`ShowAMorPM`, `ShowBorder`, `ShowMinute`, `ShowSecondHand`)
- Transparent background (`IsTransparent`)

### Digital Clock
📄 **Read:** [references/digital-clock.md](references/digital-clock.md)
- Enabling digital mode
- Frames vs shapes — mutually exclusive; use `ShowClockFrame` to switch
- `ClockFrame` values: `RectangularFrame`, `CircularFrame`, `SquareFrame`
- `ClockShape` values: `Rectangle`, `RoundedRectangle`, `Circle`, `Square`, `RoundedSquare`
- Color customization: `ForeColor`, `BackgroundColor`, `BorderColor`
- Behavior: `DisplayDates`, `ShowHourDesignator`
- Custom time in digital mode
- Custom digital frame renderer (`DigitalRenderer`)

### Custom Renderer (Analog)
📄 **Read:** [references/custom-renderer.md](references/custom-renderer.md)
- Subclassing `ClockRenderer` and overriding `DrawInterior`
- `sender` values: `"SecondsHand"`, `"MinutesHand"`, `"HoursHand"`, minute lines (`else`)
- Customizing pens with `LineCap`, `DashStyle`
- Assigning the renderer to the clock

## Quick Start

**C# — Analog clock, programmatic:**
```csharp
using Syncfusion.Windows.Forms.Tools;

Clock clock1 = new Clock();
clock1.ClockType = ClockTypes.Analog;
clock1.StartGradientBackColor = Color.Black;
clock1.HourHandColor = Color.SkyBlue;
clock1.MinuteHandColor = Color.LightSeaGreen;
clock1.SecondHandColor = Color.LightSteelBlue;
this.Controls.Add(clock1);
```

**VB.NET — Digital clock:**
```vb
Imports Syncfusion.Windows.Forms.Tools

Dim clock1 As New Clock()
clock1.ClockType = ClockTypes.Digital
clock1.ShowClockFrame = True
clock1.ClockFrame = ClockFrames.RectangularFrame
Me.Controls.Add(clock1)
```

## Common Use Cases

| Scenario | Properties to Set |
|---|---|
| Show live analog clock | `ClockType = Analog` (default) |
| Show digital clock with frame | `ClockType = Digital`, `ShowClockFrame = true`, `ClockFrame = ...` |
| Freeze at a specific time | `StopTimer = true` |
| Show custom/fixed time | `ShowCustomTimeClock = true`, `CustomTime = new DateTime(...)` |
| Transparent analog overlay | `IsTransparent = true` |
| Custom hand rendering | Subclass `ClockRenderer`, set `clock1.Renderer` |
| Custom digital frame | Subclass `DigitalClockRenderer`, set `clock1.DigitalRenderer` |
