# Appearance and Color Customization (Analog Clock)

## Table of Contents
- [Color Properties Overview](#color-properties-overview)
- [Gradient Background](#gradient-background)
- [Hand Colors](#hand-colors)
- [Minute Line and Border Colors](#minute-line-and-border-colors)
- [Hand Thickness](#hand-thickness)
- [Show / Hide Toggles](#show--hide-toggles)
- [Transparent Background](#transparent-background)

---

## Color Properties Overview

| Property | Description |
|---|---|
| `StartGradientBackColor` | Starting color of the background gradient |
| `EndGradientBackColor` | Ending color of the background gradient |
| `HourHandColor` | Color of the hour hand |
| `MinuteHandColor` | Color of the minute hand |
| `SecondHandColor` | Color of the second hand |
| `MinuteColor` | Color of the minute tick marks on the face |
| `BorderColor` | Color of the clock border ring |

---

## Gradient Background

The Clock face uses a two-color gradient background. Set both start and end colors:

**C#:**
```csharp
this.clock1.StartGradientBackColor = Color.Black;
this.clock1.EndGradientBackColor = Color.RoyalBlue;
```

**VB.NET:**
```vb
Me.clock1.StartGradientBackColor = Color.Black
Me.clock1.EndGradientBackColor = Color.RoyalBlue
```

> Set both to the same color for a flat (non-gradient) background.

---

## Hand Colors

Each clock hand has its own color property:

**C#:**
```csharp
this.clock1.HourHandColor   = Color.SkyBlue;
this.clock1.MinuteHandColor = Color.LightSeaGreen;
this.clock1.SecondHandColor = Color.LightSteelBlue;
```

**VB.NET:**
```vb
Me.clock1.HourHandColor   = Color.SkyBlue
Me.clock1.MinuteHandColor = Color.LightSeaGreen
Me.clock1.SecondHandColor = Color.LightSteelBlue
```

---

## Minute Line and Border Colors

**C#:**
```csharp
this.clock1.MinuteColor = Color.LightPink;   // tick marks on the face
this.clock1.BorderColor = Color.Violet;       // outer border ring
```

**VB.NET:**
```vb
Me.clock1.MinuteColor = Color.LightPink
Me.clock1.BorderColor = Color.Violet
```

### Full Color Example

**C#:**
```csharp
this.clock1.StartGradientBackColor = Color.Black;
this.clock1.EndGradientBackColor   = Color.RoyalBlue;
this.clock1.HourHandColor          = Color.SkyBlue;
this.clock1.MinuteHandColor        = Color.LightSeaGreen;
this.clock1.SecondHandColor        = Color.LightSteelBlue;
this.clock1.MinuteColor            = Color.LightPink;
this.clock1.BorderColor            = Color.Violet;
```

---

## Hand Thickness

Adjust the thickness (in pixels) of each hand and the minute tick marks:

| Property | Controls |
|---|---|
| `HourHandThickness` | Thickness of the hour hand |
| `MinuteHandThickness` | Thickness of the minute hand |
| `SecondHandThickness` | Thickness of the second hand |
| `MinuteThickness` | Thickness of the minute tick lines on the face |

**C#:**
```csharp
this.clock1.HourHandThickness   = 7;
this.clock1.MinuteHandThickness = 5;
this.clock1.SecondHandThickness = 2;
this.clock1.MinuteThickness     = 4;
```

**VB.NET:**
```vb
Me.clock1.HourHandThickness   = 7
Me.clock1.MinuteHandThickness = 5
Me.clock1.SecondHandThickness = 2
Me.clock1.MinuteThickness     = 4
```

> Larger values produce bolder hands. The second hand is typically the thinnest for visual clarity.

---

## Show / Hide Toggles

Use these boolean properties to enable or disable individual visual elements:

| Property | Default | Description |
|---|---|---|
| `ShowAMorPM` | `false` | Show the AM/PM indicator on the face |
| `ShowBorder` | `true` | Show the outer border ring |
| `ShowMinute` | `true` | Show the minute tick marks |
| `ShowSecondHand` | `true` | Show the second hand |

**C#:**
```csharp
this.clock1.ShowAMorPM    = true;   // display AM/PM
this.clock1.ShowBorder    = false;  // hide border
this.clock1.ShowMinute    = false;  // hide minute ticks
this.clock1.ShowSecondHand = false; // hide second hand
```

**VB.NET:**
```vb
Me.clock1.ShowAMorPM     = True
Me.clock1.ShowBorder     = False
Me.clock1.ShowMinute     = False
Me.clock1.ShowSecondHand = False
```

> Hiding the border and minute ticks gives a minimalist, hands-only appearance that works well on dark or transparent backgrounds.

---

## Transparent Background

Make the clock face transparent so the form background (or underlying controls) shows through:

**C#:**
```csharp
this.clock1.IsTransparent = true;
```

**VB.NET:**
```vb
Me.clock1.IsTransparent = True
```

> `IsTransparent` applies to analog mode only. Use it to overlay the clock on background images or colored panels. The hands and tick marks remain visible.
