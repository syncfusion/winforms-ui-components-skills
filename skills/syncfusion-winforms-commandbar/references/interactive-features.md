# Interactive Features in Windows Forms CommandBar

## Table of Contents
- [Floating Bars](#floating-bars)
- [Docking Modes](#docking-modes)
- [Event Handling](#event-handling)

## Floating Bars

### Enable/disable floating

By default, floating is enabled. Disable it to force docked mode only:

```csharp
// Disable floating - bar stays docked
this.commandBar1.DisableFloating = true;

// Enable floating (default)
this.commandBar1.DisableFloating = false;
```

### Check float state

```csharp
bool isCurrentlyFloating = this.commandBar1.Floating;

if (isCurrentlyFloating)
{
    Console.WriteLine("Bar is floating");
}
else
{
    Console.WriteLine("Bar is docked");
}
```

### Float mode wrapping

Enable content wrapping in float mode for space-constrained windows:

```csharp
// Enable wrapping
this.commandBar1.FloatModeWrapping = true;

// Disable wrapping (content extends beyond window)
this.commandBar1.FloatModeWrapping = false;
```

### Floating configuration example

```csharp
CommandBar toolBar = new CommandBar();
toolBar.Text = "Floating Toolbar";

// Allow floating with wrapping
toolBar.DisableFloating = false;
toolBar.FloatModeWrapping = true;

commandBarController1.CommandBars.Add(toolBar);
```

## Docking Modes

### Enable/disable docking

Disable docking to force float mode:

```csharp
// Disable docking - bar becomes floating
this.commandBar1.DisableDocking = true;

// Enable docking (default)
this.commandBar1.DisableDocking = false;
```

### Dock states

Set the dock position using `DockState`:

```csharp
// Dock to top (default)
this.commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Top;

// Dock to bottom
this.commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Bottom;

// Dock to left
this.commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Left;

// Dock to right
this.commandBar1.DockState = Syncfusion.Windows.Forms.Tools.CommandBarDockState.Right;
```

### Multi-bar docking

```csharp
// Create multiple docked bars
CommandBar bar1 = new CommandBar();
bar1.Text = "Main Tools";
bar1.DockState = CommandBarDockState.Top;

CommandBar bar2 = new CommandBar();
bar2.Text = "Formatting";
bar2.DockState = CommandBarDockState.Top;

CommandBar statusBar = new CommandBar();
statusBar.Text = "Status";
statusBar.DockState = CommandBarDockState.Bottom;

commandBarController1.CommandBars.Add(bar1);
commandBarController1.CommandBars.Add(bar2);
commandBarController1.CommandBars.Add(statusBar);
```

### Allowed dock borders

Restrict docking to specific sides of the form:

```csharp
// Allow docking only to left side
this.commandBar1.AllowedDockBorders = Syncfusion.Windows.Forms.Tools.CommandBarDockBorder.Left;

// Allow docking to top and bottom only
this.commandBar1.AllowedDockBorders = 
    Syncfusion.Windows.Forms.Tools.CommandBarDockBorder.Top | 
    Syncfusion.Windows.Forms.Tools.CommandBarDockBorder.Bottom;

// Allow docking to all sides (default)
this.commandBar1.AllowedDockBorders = 
    Syncfusion.Windows.Forms.Tools.CommandBarDockBorder.Top |
    Syncfusion.Windows.Forms.Tools.CommandBarDockBorder.Bottom |
    Syncfusion.Windows.Forms.Tools.CommandBarDockBorder.Left |
    Syncfusion.Windows.Forms.Tools.CommandBarDockBorder.Right;
```

## Event Handling

### Drop down clicked event

Fires when the drop down button is clicked:

```csharp
this.commandBar1.CommandBarDropDownClicked += CommandBar1_CommandBarDropDownClicked;

private void CommandBar1_CommandBarDropDownClicked(object sender, EventArgs e)
{
    MessageBox.Show("Drop down button clicked", "Info");
}
```

Use this to show/hide additional options or tools:

```csharp
private void ToolBar_CommandBarDropDownClicked(object sender, EventArgs e)
{
    CommandBar bar = sender as CommandBar;
    if (bar != null)
    {
        // Perform action based on which bar's dropdown was clicked
        Console.WriteLine($"Dropdown clicked on: {bar.Text}");
    }
}
```

### Command bar state changing event

Fires before transitioning between float/dock states:

```csharp
this.commandBar1.CommandBarStateChanging += CommandBar1_CommandBarStateChanging;

private void CommandBar1_CommandBarStateChanging(object obj, Syncfusion.Windows.Forms.Tools.CommandBarStateChangingEventArgs arg)
{
    Console.WriteLine("New dock state will be: " + arg.NewDockState);
    
    // Cancel state change if needed
    arg.Cancel = false;  // Set to true to prevent state change
}
```

Prevent invalid state transitions:

```csharp
private void CommandBar_StateChanging(object obj, CommandBarStateChangingEventArgs arg)
{
    // Prevent docking to certain states
    if (arg.NewDockState == CommandBarDockState.Left)
    {
        arg.Cancel = true;  // Don't allow left dock
        MessageBox.Show("Cannot dock to left side");
    }
}
```

### Command bar state changed event

Fires after successful state transition:

```csharp
this.commandBar1.CommandBarStateChanged += CommandBar1_CommandBarStateChanged;

private void CommandBar1_CommandBarStateChanged(object sender, EventArgs e)
{
    Console.WriteLine("Command bar state changed");
    
    CommandBar bar = sender as CommandBar;
    if (bar != null)
    {
        Console.WriteLine($"Bar {bar.Text} is now: {bar.DockState}");
    }
}
```

Update UI elements after state change:

```csharp
private void CommandBar_StateChanged(object sender, EventArgs e)
{
    // Refresh UI layout based on new state
    this.PerformLayout();
    
    // Update status
    statusLabel.Text = "Layout changed";
}
```

### Command bar user closed event

Fires when user clicks close button in float state:

```csharp
this.commandBar1.CommandBarUserClosed += CommandBar1_CommandBarUserClosed;

private void CommandBar1_CommandBarUserClosed(object sender, EventArgs e)
{
    Console.WriteLine("Command bar closed by user");
}
```

Hide/restore closed bars:

```csharp
private void ToolBar_Closed(object sender, EventArgs e)
{
    CommandBar bar = sender as CommandBar;
    if (bar != null)
    {
        // Optionally show it again or update menu
        Console.WriteLine($"{bar.Text} was closed. User can reopen from View menu.");
    }
}
```

### Command bar wrapping event

Fires when bar content wraps to new row due to resizing:

```csharp
this.commandBar1.CommandBarWrapping += CommandBar1_CommandBarWrapping;

private void CommandBar1_CommandBarWrapping(object obj, Syncfusion.Windows.Forms.Tools.CommandBarWrappingEventArgs arg)
{
    Console.WriteLine("Command bar resize type: " + arg.CommandBarResizeType);
    Console.WriteLine("New client size: " + arg.ClientSize);
}
```

### Complete event handling example

```csharp
public partial class Form1 : Form
{
    private CommandBar toolBar;

    private void InitializeCommandBar()
    {
        CommandBarController controller = new CommandBarController();
        controller.HostForm = this;
        
        toolBar = new CommandBar();
        toolBar.Text = "Main Toolbar";
        
        // Subscribe to all events
        toolBar.CommandBarDropDownClicked += ToolBar_DropDownClicked;
        toolBar.CommandBarStateChanging += ToolBar_StateChanging;
        toolBar.CommandBarStateChanged += ToolBar_StateChanged;
        toolBar.CommandBarUserClosed += ToolBar_Closed;
        toolBar.CommandBarWrapping += ToolBar_Wrapping;
        
        controller.CommandBars.Add(toolBar);
    }

    private void ToolBar_DropDownClicked(object sender, EventArgs e)
    {
        Console.WriteLine("Dropdown clicked");
    }

    private void ToolBar_StateChanging(object obj, CommandBarStateChangingEventArgs arg)
    {
        Console.WriteLine("State changing to: " + arg.NewDockState);
    }

    private void ToolBar_StateChanged(object sender, EventArgs e)
    {
        Console.WriteLine("State changed - new state: " + toolBar.DockState);
    }

    private void ToolBar_Closed(object sender, EventArgs e)
    {
        Console.WriteLine("Toolbar closed by user");
    }

    private void ToolBar_Wrapping(object obj, CommandBarWrappingEventArgs arg)
    {
        Console.WriteLine("Wrapping - resize type: " + arg.CommandBarResizeType);
    }
}
```

## Advanced patterns

### State-aware toolbar configuration

```csharp
private void ConfigureToolbarByState(CommandBar bar)
{
    if (bar.Floating)
    {
        // In float mode
        bar.FloatModeWrapping = true;
        bar.HideGripper = false;
    }
    else
    {
        // In dock mode
        bar.HideGripper = false;
        bar.AllowedDockBorders = CommandBarDockBorder.Top;
    }
}
```

### Event-driven layout management

```csharp
private int toolbarRow = 0;

private void ToolBar_StateChanged(object sender, EventArgs e)
{
    // Reorganize layout after state change
    RearrangeToolbars();
}

private void RearrangeToolbars()
{
    // Custom layout logic
    toolbarRow = 0;
    foreach (CommandBar bar in commandBarController1.CommandBars)
    {
        if (!bar.Floating && bar.DockState == CommandBarDockState.Top)
        {
            // Position logic
            toolbarRow++;
        }
    }
}
```
