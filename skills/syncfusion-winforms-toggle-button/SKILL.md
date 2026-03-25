---
name: syncfusion-winforms-toggle-button
description: Implement the Syncfusion Windows Forms Toggle Button control for toggling between two states. Use this skill when users need to create toggle buttons with customizable active/inactive states, display modes, custom styling, and event handling. This skill covers toggle button implementation, state management, and customization in Windows Forms applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Windows Forms Toggle Button

Welcome to the Toggle Button implementation guide. The Toggle Button control allows you to toggle between two contrasting states (Active and Inactive) with full customization options.

## When to Use This Skill

Use this skill when you need to:
- Create toggle buttons with two contrasting states (ON/OFF, YES/NO, etc.)
- Customize the appearance of active and inactive states
- Switch between text and image display modes
- Implement custom styling with renderers
- Handle toggle events and state changes
- Build interactive Windows Forms applications with toggle controls

## Component Overview

The Toggle Button is a Windows Forms control that presents two distinct states:
- **Active State**: Typically used for "ON", "YES", or enabled conditions
- **Inactive State**: Typically used for "OFF", "NO", or disabled conditions

Users can toggle between states by clicking the button or pressing the Space key.

### Key Capabilities
- Configurable Active and Inactive states with independent styling
- Display modes: Text or Image
- Event handling for state changes
- Custom rendering support
- Full color and border customization per state

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly dependencies and NuGet setup
- Adding Toggle Button via Designer
- Adding Toggle Button via code
- Namespace imports and basic initialization

### Toggle States Configuration
📄 **Read:** [references/toggle-states.md](references/toggle-states.md)
- Understanding Active and Inactive states
- Setting the toggle state programmatically
- Customizing Active state (colors, text, hover effects)
- Customizing Inactive state (colors, text, hover effects)
- State transitions and toggling

### Display Modes
📄 **Read:** [references/display-modes.md](references/display-modes.md)
- Text display mode configuration
- Image display mode configuration
- Switching between display modes
- Use cases and best practices

### Custom Styling and Renderers
📄 **Read:** [references/custom-styling.md](references/custom-styling.md)
- Custom renderer implementation
- Implementing the IToggleButtonRenderer interface
- Assigning custom renderers
- Advanced styling and appearance customization

### Events and Interaction
📄 **Read:** [references/events-and-interaction.md](references/events-and-interaction.md)
- Handling state change events
- Mouse click interactions
- Keyboard interactions (Space key)
- Event patterns and best practices

## Quick Start Example

Here's a minimal example to create and configure a Toggle Button:

```csharp
using Syncfusion.Windows.Forms.Tools;
using System.Drawing;

// Create and add Toggle Button to form
ToggleButton toggleButton = new ToggleButton();
toggleButton.Location = new System.Drawing.Point(50, 50);
toggleButton.Size = new System.Drawing.Size(100, 40);
toggleButton.Name = "toggleButton1";

// Configure Active state
toggleButton.ActiveState.Text = "ON";
toggleButton.ActiveState.BackColor = Color.FromArgb(1, 115, 199);
toggleButton.ActiveState.ForeColor = Color.White;

// Configure Inactive state
toggleButton.InactiveState.Text = "OFF";
toggleButton.InactiveState.BackColor = Color.White;
toggleButton.InactiveState.ForeColor = Color.FromArgb(80, 80, 80);

// Set initial state
toggleButton.ToggleState = ToggleButtonState.Inactive;

// Add to form
this.Controls.Add(toggleButton);
```

## Common Patterns

### Pattern 1: Binary State Toggle
Create a simple ON/OFF toggle button for boolean settings:

```csharp
toggleButton.ActiveState.Text = "ON";
toggleButton.InactiveState.Text = "OFF";
toggleButton.ToggleState = ToggleButtonState.Inactive;
```

### Pattern 2: Yes/No Toggle
Create a decision toggle:

```csharp
toggleButton.ActiveState.Text = "YES";
toggleButton.InactiveState.Text = "NO";
```

### Pattern 3: State-Based Colors
Apply different colors to visually indicate state:

```csharp
// Active - Green
toggleButton.ActiveState.BackColor = Color.FromArgb(34, 177, 76);
toggleButton.ActiveState.ForeColor = Color.White;

// Inactive - Red
toggleButton.InactiveState.BackColor = Color.FromArgb(192, 0, 0);
toggleButton.InactiveState.ForeColor = Color.White;
```

### Pattern 4: Custom Text Per State
Provide descriptive labels for each state:

```csharp
toggleButton.ActiveState.Text = "Enabled";
toggleButton.InactiveState.Text = "Disabled";
```

## Key Properties

| Property | Purpose | Common Values |
|----------|---------|----------------|
| `ToggleState` | Current state of the button | `ToggleButtonState.Active`, `ToggleButtonState.Inactive` |
| `ActiveState.Text` | Text displayed when active | "ON", "YES", "Enabled" |
| `InactiveState.Text` | Text displayed when inactive | "OFF", "NO", "Disabled" |
| `ActiveState.BackColor` | Background color when active | Color values |
| `InactiveState.BackColor` | Background color when inactive | Color values |
| `DisplayMode` | What to display | `DisplayType.Text`, `DisplayType.Image` |
| `Renderer` | Custom appearance rendering | IToggleButtonRenderer implementation |

## Assembly Dependencies

To use Toggle Button, add these assemblies to your project:
- `Syncfusion.Grid.Base`
- `Syncfusion.Grid.Windows`
- `Syncfusion.Shared.Base`
- `Syncfusion.Shared.Windows`
- `Syncfusion.Tools.Base`
- `Syncfusion.Tools.Windows`

## Next Steps

1. **Start with Getting Started** - Install dependencies and add the control
2. **Configure States** - Customize active and inactive appearance
3. **Choose Display Mode** - Text or image display
4. **Add Styling** - Apply custom renderers if needed
5. **Handle Events** - Respond to state changes in your application
