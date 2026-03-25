# Toggle States Configuration

## Table of Contents
- [Overview](#overview)
- [Setting Toggle State](#setting-toggle-state)
- [Active State Customization](#active-state-customization)
- [Inactive State Customization](#inactive-state-customization)
- [State Transitions](#state-transitions)
- [Common Scenarios](#common-scenarios)

## Overview

The Toggle Button control manages two distinct states: **Active** and **Inactive**. Each state is fully independent and can have its own:
- Display text
- Background color
- Border color
- Foreground (text) color
- Hover color

This separation allows you to visually represent completely different conditions, making it clear to users what each state means.

## Setting Toggle State

The `ToggleState` property controls which state the button currently displays:

```csharp
// Set to Active state
this.toggleButton1.ToggleState = ToggleButtonState.Active;

// Set to Inactive state
this.toggleButton1.ToggleState = ToggleButtonState.Inactive;
```

### Visual Basic

```vb
' Set to Active state
Me.toggleButton1.ToggleState = ToggleButtonState.Active

' Set to Inactive state
Me.toggleButton1.ToggleState = ToggleButtonState.Inactive
```

### Reading Current State

To check the current state of the button:

```csharp
if (this.toggleButton1.ToggleState == ToggleButtonState.Active)
{
    // Button is currently in Active state
    MessageBox.Show("Toggle is Active");
}
else
{
    // Button is currently in Inactive state
    MessageBox.Show("Toggle is Inactive");
}
```

## Active State Customization

The Active state represents the "on", "yes", or enabled condition. Configure it using the `ActiveState` property:

### Basic Configuration

```csharp
this.toggleButton1.ActiveState.Text = "ON";
this.toggleButton1.ActiveState.BackColor = Color.FromArgb(1, 115, 199);
this.toggleButton1.ActiveState.BorderColor = Color.FromArgb(1, 115, 199);
this.toggleButton1.ActiveState.ForeColor = Color.White;
this.toggleButton1.ActiveState.HoverColor = Color.FromArgb(0, 103, 176);
```

### Visual Basic

```vb
Me.toggleButton1.ActiveState.Text = "ON"
Me.toggleButton1.ActiveState.BackColor = Color.FromArgb(1, 115, 199)
Me.toggleButton1.ActiveState.BorderColor = Color.FromArgb(1, 115, 199)
Me.toggleButton1.ActiveState.ForeColor = Color.White
Me.toggleButton1.ActiveState.HoverColor = Color.FromArgb(0, 103, 176)
```

### Active State Properties

| Property | Purpose | Example |
|----------|---------|---------|
| `Text` | Text displayed when active | "ON", "YES", "Enabled" |
| `BackColor` | Background color when active | Color.Green |
| `BorderColor` | Border color when active | Color.DarkGreen |
| `ForeColor` | Text color when active | Color.White |
| `HoverColor` | Background color on hover | Color.LimeGreen |

### Example: Green Active State

```csharp
this.toggleButton1.ActiveState.Text = "Enabled";
this.toggleButton1.ActiveState.BackColor = Color.Green;
this.toggleButton1.ActiveState.BorderColor = Color.DarkGreen;
this.toggleButton1.ActiveState.ForeColor = Color.White;
this.toggleButton1.ActiveState.HoverColor = Color.LimeGreen;
```

## Inactive State Customization

The Inactive state represents the "off", "no", or disabled condition. Configure it using the `InactiveState` property:

### Basic Configuration

```csharp
this.toggleButton1.InactiveState.Text = "OFF";
this.toggleButton1.InactiveState.BackColor = Color.White;
this.toggleButton1.InactiveState.BorderColor = Color.FromArgb(150, 150, 150);
this.toggleButton1.InactiveState.ForeColor = Color.FromArgb(80, 80, 80);
this.toggleButton1.InactiveState.HoverColor = Color.White;
```

### Visual Basic

```vb
Me.toggleButton1.InactiveState.Text = "OFF"
Me.toggleButton1.InactiveState.BackColor = Color.White
Me.toggleButton1.InactiveState.BorderColor = Color.FromArgb(150, 150, 150)
Me.toggleButton1.InactiveState.ForeColor = Color.FromArgb(80, 80, 80)
Me.toggleButton1.InactiveState.HoverColor = Color.White
```

### Inactive State Properties

| Property | Purpose | Example |
|----------|---------|---------|
| `Text` | Text displayed when inactive | "OFF", "NO", "Disabled" |
| `BackColor` | Background color when inactive | Color.LightGray |
| `BorderColor` | Border color when inactive | Color.Gray |
| `ForeColor` | Text color when inactive | Color.Black |
| `HoverColor` | Background color on hover | Color.Gainsboro |

### Example: Gray Inactive State

```csharp
this.toggleButton1.InactiveState.Text = "Disabled";
this.toggleButton1.InactiveState.BackColor = Color.LightGray;
this.toggleButton1.InactiveState.BorderColor = Color.Gray;
this.toggleButton1.InactiveState.ForeColor = Color.DarkGray;
this.toggleButton1.InactiveState.HoverColor = Color.Gainsboro;
```

## State Transitions

Users can toggle between states in two ways:

### Method 1: Mouse Click
Clicking the button automatically toggles to the opposite state:

```csharp
// Before click: State = Inactive
// After click: State = Active
```

### Method 2: Space Key
When the button has focus, pressing the Space key toggles the state:

```csharp
// Focus on button and press Space to toggle
```

### Method 3: Programmatic Change
You can change the state in code:

```csharp
private void ToggleButtonProgrammatically()
{
    if (this.toggleButton1.ToggleState == ToggleButtonState.Active)
    {
        this.toggleButton1.ToggleState = ToggleButtonState.Inactive;
    }
    else
    {
        this.toggleButton1.ToggleState = ToggleButtonState.Active;
    }
}
```

## Common Scenarios

### Scenario 1: Feature Toggle
Create a toggle for enabling/disabling a feature:

```csharp
// Setup
toggleButton.ActiveState.Text = "Feature ON";
toggleButton.ActiveState.BackColor = Color.Green;
toggleButton.InactiveState.Text = "Feature OFF";
toggleButton.InactiveState.BackColor = Color.Red;

// Usage
if (toggleButton.ToggleState == ToggleButtonState.Active)
{
    EnableFeature();
}
else
{
    DisableFeature();
}
```

### Scenario 2: Sound/Notification Toggle
Toggle audio or notifications:

```csharp
toggleButton.ActiveState.Text = "🔊 Sound ON";
toggleButton.InactiveState.Text = "🔇 Sound OFF";

// In your form's event handler
toggleButton.ToggleState = ToggleButtonState.Active;
MuteAudio(toggleButton.ToggleState == ToggleButtonState.Inactive);
```

### Scenario 3: Professional Yes/No Toggle
Create a yes/no toggle with professional styling:

```csharp
// Active (YES)
toggleButton.ActiveState.Text = "YES";
toggleButton.ActiveState.BackColor = Color.FromArgb(34, 177, 76);
toggleButton.ActiveState.ForeColor = Color.White;

// Inactive (NO)
toggleButton.InactiveState.Text = "NO";
toggleButton.InactiveState.BackColor = Color.FromArgb(192, 0, 0);
toggleButton.InactiveState.ForeColor = Color.White;

// Initial state
toggleButton.ToggleState = ToggleButtonState.Inactive;
```

### Scenario 4: User Preference Toggle
Toggle user preferences with clear labeling:

```csharp
// Dark Mode Toggle
toggleButton.ActiveState.Text = "Dark Mode";
toggleButton.ActiveState.BackColor = Color.Black;
toggleButton.ActiveState.ForeColor = Color.White;

toggleButton.InactiveState.Text = "Light Mode";
toggleButton.InactiveState.BackColor = Color.White;
toggleButton.InactiveState.ForeColor = Color.Black;
```

### Scenario 5: Connection Status
Show connection status with dynamic states:

```csharp
toggleButton.ActiveState.Text = "Connected";
toggleButton.ActiveState.BackColor = Color.Green;

toggleButton.InactiveState.Text = "Disconnected";
toggleButton.InactiveState.BackColor = Color.Red;

// Update based on actual connection
if (IsConnectedToServer())
{
    toggleButton.ToggleState = ToggleButtonState.Active;
}
```

## Best Practices

1. **Consistent Styling**: Use colors that clearly indicate state differences
2. **Descriptive Text**: Use text that explains what each state means
3. **Color Contrast**: Ensure text is readable on background colors
4. **Hover Feedback**: Set HoverColor to provide user feedback on hover
5. **Logical Mapping**: Use ON/OFF, YES/NO, or similar logical pairs
6. **State Persistence**: Save the user's toggle preference if needed
