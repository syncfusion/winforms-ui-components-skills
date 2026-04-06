# Balloon Style and Beak

This guide covers balloon-style tooltips with directional beaks, providing a more visually distinctive tooltip appearance.

## Understanding Tooltip Styles

The `SfToolTip` component supports two tooltip styles:

| Style | Description | Visual Appearance |
|-------|-------------|-------------------|
| **Rectangle** (default) | Standard rectangular tooltip with sharp corners | Clean, minimal design |
| **Balloon** | Tooltip with a triangular beak pointing to the control | Callout or speech bubble appearance |

**Use Rectangle Style When:**
- Maintaining a minimal, professional appearance
- Displaying dense information
- Following standard Windows UI conventions

**Use Balloon Style When:**
- Drawing attention to important tooltips
- Creating visually engaging interfaces
- Pointing to specific UI elements
- Mimicking callouts or speech bubbles

## Enabling Balloon Style

Set the `ToolTipStyle` property to `ToolTipStyle.Balloon` on the `ToolTipInfo` object.

### Basic Balloon Tooltip

```csharp
using Syncfusion.WinForms.Controls;

ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "This is a balloon-style tooltip";

toolTipInfo1.Items.Add(toolTipItem1);
sfToolTip1.SetToolTipInfo(this.button1, toolTipInfo1);
```

**Result:** Tooltip displays with a triangular beak pointing toward the control.

### Balloon Tooltip with Image and Text

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleLeft;
toolTipItem1.Image = global::GettingStarted.Properties.Resources.MORGK;
toolTipItem1.Style.ImageSize = new Size(100, 100);

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Use Case:** User profile cards, contact information, or any content requiring visual emphasis.

## Customizing Beak Back Color

The beak's background color can be customized independently using the `BeakBackColor` property.

### Setting Beak Color

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;
toolTipInfo1.BeakBackColor = Color.LightSkyBlue;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "David Carter\r\nPhone : +1 919.494.1974\r\nEmail : david@syncfusion.com";
toolTipItem1.Style.TextAlignment = ContentAlignment.MiddleLeft;
toolTipItem1.Image = global::GettingStarted.Properties.Resources.MORGK;
toolTipItem1.Style.ImageSize = new Size(100, 100);

toolTipInfo1.Items.AddRange(new ToolTipItem[] { toolTipItem1 });
sfToolTip1.SetToolTipInfo(this.button2, toolTipInfo1);
```

**Best Practice:** Match `BeakBackColor` to the tooltip's background color for visual cohesion.

### Matching Beak to Tooltip Background

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;
toolTipInfo1.BeakBackColor = Color.LightYellow;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "Warning: This action cannot be undone.";
toolTipItem1.Style.BackColor = Color.LightYellow; // Match beak color
toolTipItem1.Style.ForeColor = Color.DarkRed;
toolTipItem1.Style.Font = new Font("Arial", 9f, FontStyle.Bold);

toolTipInfo1.Items.Add(toolTipItem1);
sfToolTip1.SetToolTipInfo(this.deleteButton, toolTipInfo1);
```

**Result:** Seamless visual appearance where beak appears as part of the tooltip.

### Contrasting Beak Color

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;
toolTipInfo1.BeakBackColor = Color.Navy;

ToolTipItem toolTipItem1 = new ToolTipItem();
toolTipItem1.Text = "Premium Feature";
toolTipItem1.Style.BackColor = Color.LightSteelBlue;
toolTipItem1.Style.ForeColor = Color.White;

toolTipInfo1.Items.Add(toolTipItem1);
sfToolTip1.SetToolTipInfo(this.premiumButton, toolTipInfo1);
```

**Use Case:** Create emphasis or branding by using contrasting colors.

## Balloon Style with Multiple Items

Balloon style works seamlessly with multi-item tooltips.

```csharp
ToolTipInfo toolTipInfo1 = new ToolTipInfo();
toolTipInfo1.ToolTipStyle = ToolTipStyle.Balloon;
toolTipInfo1.BeakBackColor = Color.WhiteSmoke;

// Header item
ToolTipItem header = new ToolTipItem();
header.Text = "User Information";
header.Style.Font = new Font("Arial", 10f, FontStyle.Bold);
header.Style.BackColor = Color.LightBlue;
header.EnableSeparator = true;

// Details item
ToolTipItem details = new ToolTipItem();
details.Text = "Name: Sarah Johnson\nDepartment: Engineering\nStatus: Available";
details.Style.BackColor = Color.WhiteSmoke;

// Action hint item
ToolTipItem hint = new ToolTipItem();
hint.Text = "Click to view full profile";
hint.Style.ForeColor = Color.Blue;
hint.Style.BackColor = Color.WhiteSmoke;

toolTipInfo1.Items.AddRange(new ToolTipItem[] { header, details, hint });
sfToolTip1.SetToolTipInfo(this.userAvatar, toolTipInfo1);
```

**Note:** When using multiple items with different background colors, set `BeakBackColor` to match the first or most prominent item.

## Beak Positioning

The beak automatically positions itself to point toward the control the tooltip is associated with.

**Automatic Positioning Behavior:**
- Beak appears on the side closest to the control
- Adjusts based on screen edges and available space
- Maintains visual connection between tooltip and control

**Example - Tooltip Above Control:**
```csharp
// When tooltip displays above the control, beak points downward
ToolTipInfo toolTipInfo = new ToolTipInfo();
toolTipInfo.ToolTipStyle = ToolTipStyle.Balloon;
toolTipInfo.BeakBackColor = Color.White;

ToolTipItem item = new ToolTipItem();
item.Text = "Submit Form";
item.Style.BackColor = Color.White;

toolTipInfo.Items.Add(item);
sfToolTip1.SetToolTipInfo(this.submitButton, toolTipInfo);
```

**No Manual Control:** Beak direction cannot be manually specified; it's determined automatically based on tooltip position.

## Common Patterns

### Pattern 1: Warning Balloon

```csharp
ToolTipInfo warningInfo = new ToolTipInfo();
warningInfo.ToolTipStyle = ToolTipStyle.Balloon;
warningInfo.BeakBackColor = Color.MistyRose;
warningInfo.BorderColor = Color.DarkRed;
warningInfo.BorderThickness = 2;

ToolTipItem warningItem = new ToolTipItem();
warningItem.Text = "⚠ Warning: This action is irreversible";
warningItem.Style.BackColor = Color.MistyRose;
warningItem.Style.ForeColor = Color.DarkRed;
warningItem.Style.Font = new Font("Arial", 9f, FontStyle.Bold);

warningInfo.Items.Add(warningItem);
sfToolTip1.SetToolTipInfo(this.criticalActionButton, warningInfo);
```

### Pattern 2: Information Callout

```csharp
ToolTipInfo infoCallout = new ToolTipInfo();
infoCallout.ToolTipStyle = ToolTipStyle.Balloon;
infoCallout.BeakBackColor = Color.AliceBlue;

ToolTipItem infoItem = new ToolTipItem();
infoItem.Text = "ℹ Tip: Hold Shift while clicking for advanced options";
infoItem.Style.BackColor = Color.AliceBlue;
infoItem.Style.ForeColor = Color.DarkBlue;

infoCallout.Items.Add(infoItem);
sfToolTip1.SetToolTipInfo(this.toolButton, infoCallout);
```

### Pattern 3: Profile Card Balloon

```csharp
ToolTipInfo profileCard = new ToolTipInfo();
profileCard.ToolTipStyle = ToolTipStyle.Balloon;
profileCard.BeakBackColor = Color.White;
profileCard.BorderColor = Color.LightGray;
profileCard.BorderThickness = 1;

ToolTipItem profileItem = new ToolTipItem();
profileItem.Text = "Emily Rodriguez\nSenior Developer\nOnline";
profileItem.Image = Properties.Resources.ProfilePicture;
profileItem.Style.ImageSize = new Size(60, 60);
profileItem.Style.ImageAlignment = ToolTipImageAlignment.Left;
profileItem.Style.BackColor = Color.White;
profileItem.Style.ImageToTextOffset = 10;

profileCard.Items.Add(profileItem);
sfToolTip1.SetToolTipInfo(this.contactLink, profileCard);
```

## When to Use Balloon vs Rectangle

### Use Balloon Style For:
- **Call-to-action hints:** Drawing attention to important actions
- **Warnings and alerts:** Making critical information stand out
- **Tutorial or onboarding:** Guiding users through interface features
- **Profile cards:** Personal information with social context
- **Contextual help:** Pointing to specific UI elements
- **Branding:** Creating distinctive, memorable tooltips

### Use Rectangle Style For:
- **Dense information:** Technical data, specifications, or parameters
- **Professional applications:** Business or enterprise software
- **Subtle hints:** Non-intrusive help text
- **High-frequency tooltips:** Controls with many tooltip interactions
- **Consistency:** Matching standard Windows UI conventions

## Combining Balloon Style with Other Features

### With Gradient Background

```csharp
ToolTipInfo gradientBalloon = new ToolTipInfo();
gradientBalloon.ToolTipStyle = ToolTipStyle.Balloon;
gradientBalloon.BeakBackColor = Color.LightBlue; // Match gradient start

ToolTipItem item = new ToolTipItem();
item.Text = "Special Offer Available!";
item.EnableGradientBackground = true;
item.Style.GradientBrush = new BrushInfo(
    GradientStyle.Horizontal,
    new Color[] { Color.LightBlue, Color.LightGreen }
);
item.Style.Font = new Font("Arial", 10f, FontStyle.Bold);

gradientBalloon.Items.Add(item);
sfToolTip1.SetToolTipInfo(this.promoButton, gradientBalloon);
```

### With Shadow

```csharp
// Enable shadow at SfToolTip level
sfToolTip1.ShadowVisible = true;

ToolTipInfo balloonWithShadow = new ToolTipInfo();
balloonWithShadow.ToolTipStyle = ToolTipStyle.Balloon;
balloonWithShadow.BeakBackColor = Color.White;

ToolTipItem item = new ToolTipItem();
item.Text = "Enhanced tooltip with shadow effect";
item.Style.BackColor = Color.White;

balloonWithShadow.Items.Add(item);
sfToolTip1.SetToolTipInfo(this.button1, balloonWithShadow);
```

**Result:** Balloon tooltip with depth and elevation.

## Summary

This guide covered:
- **Balloon style basics:** Enabling ToolTipStyle.Balloon
- **Beak customization:** Setting BeakBackColor
- **Automatic positioning:** How beaks orient toward controls
- **Common patterns:** Warning, information, and profile balloons
- **Style selection:** When to use balloon vs rectangle

**Best Practices:**
1. Match beak color to tooltip background for cohesion
2. Use balloon style strategically for important tooltips
3. Combine with borders and shadows for enhanced appearance
4. Test positioning near screen edges
5. Maintain consistent style within application sections

**Next Steps:**
- Explore comprehensive styling in [appearance-customization.md](appearance-customization.md)
- Learn advanced features in [advanced-usage.md](advanced-usage.md)
