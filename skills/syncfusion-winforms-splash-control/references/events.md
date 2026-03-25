# SplashControl Events

## Table of Contents
- [Overview](#overview)
- [Event Lifecycle](#event-lifecycle)
- [BeforeSplash Event](#beforesplash-event)
- [SplashDisplayed Event](#splashdisplayed-event)
- [SplashClosing Event](#splashclosing-event)
- [SplashClosed Event](#splashclosed-event)
- [Event Notification Methods](#event-notification-methods)
- [Complete Event Examples](#complete-event-examples)
- [Best Practices](#best-practices)

## Overview

The SplashControl exposes four key events that allow you to respond to the splash screen lifecycle. These events enable logging, initialization coordination, cancellation, and post-splash actions.

## Event Lifecycle

The events fire in this sequence:

```
1. BeforeSplash        ← Cancelable, fired before display
         ↓
2. SplashDisplayed     ← Fired after splash becomes visible
         ↓
   (Splash displays for TimerInterval duration)
         ↓
3. SplashClosing       ← Cancelable, fired before close
         ↓
4. SplashClosed        ← Fired after splash is hidden
```

## BeforeSplash Event

### Overview

The **BeforeSplash** event is raised before the splash screen is displayed. This event can be canceled to prevent the splash from showing.

### Event Signature

```csharp
public event CancelEventHandler BeforeSplash
```

### Event Arguments

**Type:** `System.ComponentModel.CancelEventArgs`

| Member | Type | Description |
|--------|------|-------------|
| **Cancel** | bool | Set to `true` to prevent splash display |

### Basic Usage

**C# Example:**

```csharp
public Form1()
{
    InitializeComponent();
    
    // Subscribe to event
    splashControl1.BeforeSplash += SplashControl1_BeforeSplash;
}

private void SplashControl1_BeforeSplash(object sender, CancelEventArgs e)
{
    // Log before splash displays
    Debug.WriteLine("Splash screen is about to display");
    
    // Optionally cancel
    // e.Cancel = true;
}
```

**VB.NET Example:**

```vb
Public Sub New()
    InitializeComponent()
    
    ' Subscribe to event
    AddHandler splashControl1.BeforeSplash, AddressOf SplashControl1_BeforeSplash
End Sub

Private Sub SplashControl1_BeforeSplash(sender As Object, e As CancelEventArgs)
    ' Log before splash displays
    Debug.WriteLine("Splash screen is about to display")
    
    ' Optionally cancel
    ' e.Cancel = True
End Sub
```

### Canceling Splash Display

You can prevent the splash from showing based on conditions:

```csharp
private void SplashControl1_BeforeSplash(object sender, CancelEventArgs e)
{
    // Don't show splash if command-line argument present
    string[] args = Environment.GetCommandLineArgs();
    if (args.Contains("/nosplash"))
    {
        e.Cancel = true;
        Debug.WriteLine("Splash display cancelled via command line");
        return;
    }
    
    // Don't show if already shown recently
    DateTime? lastShown = GetLastSplashTime();
    if (lastShown.HasValue && 
        (DateTime.Now - lastShown.Value).TotalHours < 1)
    {
        e.Cancel = true;
        Debug.WriteLine("Splash cancelled - shown recently");
    }
}
```

### Logging Example

```csharp
private StringBuilder eventLog = new StringBuilder();

private void SplashControl1_BeforeSplash(object sender, CancelEventArgs e)
{
    string eventMessage = $"[{DateTime.Now:HH:mm:ss.fff}] BeforeSplash fired";
    eventLog.AppendLine(eventMessage);
    Debug.WriteLine(eventMessage);
    
    // Could also log to file or database
    LogToFile(eventMessage);
}
```

### Triggering Method

The BeforeSplash event is raised when the `BeforeSplashNotify()` method is called:

```csharp
// This triggers the BeforeSplash event
splashPanel1.BeforeSplashNotify();
```

## SplashDisplayed Event

### Overview

The **SplashDisplayed** event is raised after the splash screen becomes visible on the screen.

### Event Signature

```csharp
public event EventHandler SplashDisplayed
```

### Event Arguments

**Type:** `System.EventArgs`

Standard EventArgs with no additional data.

### Basic Usage

**C# Example:**

```csharp
public Form1()
{
    InitializeComponent();
    
    // Subscribe to event
    splashControl1.SplashDisplayed += SplashControl1_SplashDisplayed;
}

private void SplashControl1_SplashDisplayed(object sender, EventArgs e)
{
    // Splash is now visible
    Debug.WriteLine("Splash screen is now displayed");
    
    // Start background initialization
    StartBackgroundTasks();
}
```

**VB.NET Example:**

```vb
Public Sub New()
    InitializeComponent()
    
    ' Subscribe to event
    AddHandler splashControl1.SplashDisplayed, AddressOf SplashControl1_SplashDisplayed
End Sub

Private Sub SplashControl1_SplashDisplayed(sender As Object, e As EventArgs)
    ' Splash is now visible
    Debug.WriteLine("Splash screen is now displayed")
    
    ' Start background initialization
    StartBackgroundTasks()
End Sub
```

### Use Cases

**1. Start Background Initialization:**

```csharp
private void SplashControl1_SplashDisplayed(object sender, EventArgs e)
{
    // Start loading data while splash is visible
    Task.Run(async () =>
    {
        await LoadApplicationData();
        await InitializeComponents();
    });
}
```

**2. Update Analytics:**

```csharp
private void SplashControl1_SplashDisplayed(object sender, EventArgs e)
{
    // Track application startup
    Analytics.LogEvent("SplashDisplayed", new Dictionary<string, string>
    {
        { "Version", Application.ProductVersion },
        { "StartTime", DateTime.Now.ToString() }
    });
}
```

**3. Show Notification:**

```csharp
private void SplashControl1_SplashDisplayed(object sender, EventArgs e)
{
    // Update system tray
    if (notifyIcon1 != null)
    {
        notifyIcon1.ShowBalloonTip(
            2000,
            "Application Starting",
            "Please wait while the application initializes...",
            ToolTipIcon.Info
        );
    }
}
```

### Logging Example with TextBox

```csharp
private TextBox logTextBox;

private void SplashControl1_SplashDisplayed(object sender, EventArgs e)
{
    string eventMessage = $"Event: SplashDisplayed Object: {sender}\r\n";
    
    if (logTextBox != null && logTextBox.InvokeRequired)
    {
        logTextBox.Invoke(new Action(() =>
        {
            logTextBox.Text += eventMessage;
        }));
    }
    else if (logTextBox != null)
    {
        logTextBox.Text += eventMessage;
    }
}
```

### Triggering Method

The SplashDisplayed event is raised when the `SplashDisplayedNotify()` method is called:

```csharp
// This triggers the SplashDisplayed event
splashPanel1.SplashDisplayedNotify();
```

## SplashClosing Event

### Overview

The **SplashClosing** event is raised when the splash screen is about to close. This event can be canceled to keep the splash open longer.

### Event Signature

```csharp
public event CancelEventHandler SplashClosing
```

### Event Arguments

**Type:** `System.ComponentModel.CancelEventArgs`

| Member | Type | Description |
|--------|------|-------------|
| **Cancel** | bool | Set to `true` to prevent splash from closing |

### Basic Usage

**C# Example:**

```csharp
public Form1()
{
    InitializeComponent();
    
    // Subscribe to event
    splashControl1.SplashClosing += SplashControl1_SplashClosing;
}

private void SplashControl1_SplashClosing(object sender, CancelEventArgs e)
{
    // Log before splash closes
    Debug.WriteLine("Splash screen is closing");
    
    // Optionally prevent closing
    // e.Cancel = true;
}
```

**VB.NET Example:**

```vb
Public Sub New()
    InitializeComponent()
    
    ' Subscribe to event
    AddHandler splashControl1.SplashClosing, AddressOf SplashControl1_SplashClosing
End Sub

Private Sub SplashControl1_SplashClosing(sender As Object, e As CancelEventArgs)
    ' Log before splash closes
    Debug.WriteLine("Splash screen is closing")
    
    ' Optionally prevent closing
    ' e.Cancel = True
End Sub
```

### Preventing Close Until Ready

Keep splash open until initialization completes:

```csharp
private bool initializationComplete = false;

private async void Form1_Load(object sender, EventArgs e)
{
    splashControl1.ShowSplash(true);
    
    // Perform initialization
    await PerformLongInitialization();
    
    initializationComplete = true;
}

private void SplashControl1_SplashClosing(object sender, CancelEventArgs e)
{
    if (!initializationComplete)
    {
        // Keep splash open
        e.Cancel = true;
        Debug.WriteLine("Initialization not complete, keeping splash open");
    }
}

private async Task PerformLongInitialization()
{
    await Task.Delay(5000); // Simulate long operation
}
```

### Conditional Closing

```csharp
private bool userClickedSkip = false;

private void SplashControl1_SplashClosing(object sender, CancelEventArgs e)
{
    // Only close if minimum time elapsed or user clicked skip
    TimeSpan elapsed = DateTime.Now - splashStartTime;
    
    if (elapsed.TotalSeconds < 2 && !userClickedSkip)
    {
        e.Cancel = true;
        Debug.WriteLine("Splash closing too early, extending display");
    }
}
```

### Logging Example

```csharp
private void SplashControl1_SplashClosing(object sender, CancelEventArgs e)
{
    string eventMessage = $"[{DateTime.Now:HH:mm:ss.fff}] SplashClosing fired";
    eventLog.AppendLine(eventMessage);
    
    if (logTextBox != null)
    {
        logTextBox.Text += eventMessage + "\r\n";
    }
}
```

### Triggering Method

The SplashClosing event is raised when the `SplashClosingNotify()` method is called:

```csharp
// This triggers the SplashClosing event
splashControl1.SplashClosingNotify();
```

## SplashClosed Event

### Overview

The **SplashClosed** event is raised after the splash screen has been closed and is no longer visible.

### Event Signature

```csharp
public event EventHandler SplashClosed
```

### Event Arguments

**Type:** `System.EventArgs`

Standard EventArgs with no additional data.

### Basic Usage

**C# Example:**

```csharp
public Form1()
{
    InitializeComponent();
    
    // Subscribe to event
    splashControl1.SplashClosed += SplashControl1_SplashClosed;
}

private void SplashControl1_SplashClosed(object sender, EventArgs e)
{
    // Splash has closed
    Debug.WriteLine("Splash screen has closed");
    
    // Show welcome message
    ShowWelcomeDialog();
}
```

**VB.NET Example:**

```vb
Public Sub New()
    InitializeComponent()
    
    ' Subscribe to event
    AddHandler splashControl1.SplashClosed, AddressOf SplashControl1_SplashClosed
End Sub

Private Sub SplashControl1_SplashClosed(sender As Object, e As EventArgs)
    ' Splash has closed
    Debug.WriteLine("Splash screen has closed")
    
    ' Show welcome message
    ShowWelcomeDialog()
End Sub
```

### Common Use Cases

**1. Display Welcome or What's New:**

```csharp
private void SplashControl1_SplashClosed(object sender, EventArgs e)
{
    // Check if this is first run
    if (Properties.Settings.Default.FirstRun)
    {
        ShowWelcomeWizard();
        Properties.Settings.Default.FirstRun = false;
        Properties.Settings.Default.Save();
    }
    else if (IsNewVersion())
    {
        ShowWhatsNewDialog();
    }
}
```

**2. Finalize UI Setup:**

```csharp
private void SplashControl1_SplashClosed(object sender, EventArgs e)
{
    // Ensure main form has focus
    this.Activate();
    this.BringToFront();
    
    // Show status message
    statusLabel.Text = $"Welcome, {Environment.UserName}!";
    
    // Enable controls
    EnableAllControls();
}
```

**3. Start Scheduled Tasks:**

```csharp
private void SplashControl1_SplashClosed(object sender, EventArgs e)
{
    // Start background workers
    backgroundWorker1.RunWorkerAsync();
    
    // Start timers
    updateCheckTimer.Start();
    autoSaveTimer.Start();
    
    Debug.WriteLine("Background tasks started");
}
```

**4. Clean Up Resources:**

```csharp
private void SplashControl1_SplashClosed(object sender, EventArgs e)
{
    // Dispose of splash-related resources
    if (splashControl1.CustomSplashPanel != null)
    {
        splashControl1.CustomSplashPanel.Dispose();
        splashControl1.CustomSplashPanel = null;
    }
    
    // Free memory
    GC.Collect();
}
```

### Logging Example with TextBox

```csharp
private TextBox logTextBox;

private void SplashControl1_SplashClosed(object sender, EventArgs e)
{
    string eventMessage = $"Event: SplashClosed Object: {sender}\r\n";
    
    if (logTextBox != null)
    {
        logTextBox.Text += eventMessage;
    }
    
    Debug.WriteLine(eventMessage);
}
```

### Triggering Method

The SplashClosed event is raised when the `SplashClosedNotify()` method is called:

```csharp
// This triggers the SplashClosed event
splashPanel1.SplashClosedNotify();
```

## Event Notification Methods

The SplashControl and SplashPanel provide notification methods that trigger the corresponding events. These are implementations of the `ISplashWrapperFormListener` interface.

### Available Notification Methods

| Method | Triggers Event | Purpose |
|--------|---------------|---------|
| **BeforeSplashNotify()** | BeforeSplash | Notify before splash displays |
| **SplashDisplayedNotify()** | SplashDisplayed | Notify after splash is visible |
| **SplashClosingNotify()** | SplashClosing | Notify before splash closes |
| **SplashClosedNotify()** | SplashClosed | Notify after splash is closed |

### Manual Event Triggering

You can manually trigger events using these methods:

```csharp
// Trigger BeforeSplash event
splashPanel1.BeforeSplashNotify();

// Trigger SplashDisplayed event
splashPanel1.SplashDisplayedNotify();

// Trigger SplashClosing event
splashControl1.SplashClosingNotify();

// Trigger SplashClosed event
splashPanel1.SplashClosedNotify();
```

**Note:** These are typically called internally by the framework, but can be used in custom scenarios.

## Complete Event Examples

### Example 1: Full Event Logging

```csharp
public class EventLoggingExample : Form
{
    private SplashControl splashControl1;
    private TextBox logTextBox;
    
    public EventLoggingExample()
    {
        InitializeComponent();
        SetupEventLogging();
    }
    
    private void SetupEventLogging()
    {
        // Create log textbox
        logTextBox = new TextBox
        {
            Dock = DockStyle.Fill,
            Multiline = true,
            ScrollBars = ScrollBars.Vertical
        };
        this.Controls.Add(logTextBox);
        
        // Subscribe to all events
        splashControl1.BeforeSplash += LogBeforeSplash;
        splashControl1.SplashDisplayed += LogSplashDisplayed;
        splashControl1.SplashClosing += LogSplashClosing;
        splashControl1.SplashClosed += LogSplashClosed;
    }
    
    private void LogBeforeSplash(object sender, CancelEventArgs e)
    {
        LogEvent("BeforeSplash", sender);
    }
    
    private void LogSplashDisplayed(object sender, EventArgs e)
    {
        LogEvent("SplashDisplayed", sender);
    }
    
    private void LogSplashClosing(object sender, CancelEventArgs e)
    {
        LogEvent("SplashClosing", sender);
    }
    
    private void LogSplashClosed(object sender, EventArgs e)
    {
        LogEvent("SplashClosed", sender);
    }
    
    private void LogEvent(string eventName, object sender)
    {
        string timestamp = DateTime.Now.ToString("HH:mm:ss.fff");
        string message = $"[{timestamp}] Event: {eventName} | Sender: {sender}\r\n";
        
        if (logTextBox.InvokeRequired)
        {
            logTextBox.Invoke(new Action(() => logTextBox.Text += message));
        }
        else
        {
            logTextBox.Text += message;
        }
    }
}
```

### Example 2: Coordinated Initialization

```csharp
public class CoordinatedInitExample : Form
{
    private SplashControl splashControl1;
    private bool dataLoaded = false;
    private bool servicesStarted = false;
    
    public CoordinatedInitExample()
    {
        InitializeComponent();
        SetupSplashEvents();
    }
    
    private void SetupSplashEvents()
    {
        splashControl1.BeforeSplash += OnBeforeSplash;
        splashControl1.SplashDisplayed += OnSplashDisplayed;
        splashControl1.SplashClosing += OnSplashClosing;
        splashControl1.SplashClosed += OnSplashClosed;
    }
    
    private void OnBeforeSplash(object sender, CancelEventArgs e)
    {
        Debug.WriteLine("Preparing to show splash...");
        
        // Could cancel based on conditions
        if (IsDebugMode())
        {
            e.Cancel = true;
            Debug.WriteLine("Debug mode: skipping splash");
        }
    }
    
    private async void OnSplashDisplayed(object sender, EventArgs e)
    {
        Debug.WriteLine("Splash visible, starting initialization...");
        
        // Start background tasks
        await Task.WhenAll(
            LoadDataAsync(),
            StartServicesAsync()
        );
    }
    
    private void OnSplashClosing(object sender, CancelEventArgs e)
    {
        // Ensure initialization is complete
        if (!dataLoaded || !servicesStarted)
        {
            Debug.WriteLine("Initialization incomplete, keeping splash open");
            e.Cancel = true;
        }
    }
    
    private void OnSplashClosed(object sender, EventArgs e)
    {
        Debug.WriteLine("Splash closed, finalizing startup...");
        
        // Finalize UI
        this.Activate();
        ShowStatusMessage("Ready");
    }
    
    private async Task LoadDataAsync()
    {
        await Task.Delay(2000); // Simulate loading
        dataLoaded = true;
        Debug.WriteLine("Data loaded");
    }
    
    private async Task StartServicesAsync()
    {
        await Task.Delay(1500); // Simulate startup
        servicesStarted = true;
        Debug.WriteLine("Services started");
    }
    
    private bool IsDebugMode()
    {
        #if DEBUG
        return true;
        #else
        return false;
        #endif
    }
    
    private void ShowStatusMessage(string message)
    {
        // Update status bar or similar
    }
}
```

### Example 3: Conditional Splash Based on Settings

```csharp
public class ConditionalSplashExample : Form
{
    private SplashControl splashControl1;
    
    public ConditionalSplashExample()
    {
        InitializeComponent();
        splashControl1.BeforeSplash += CheckSplashSettings;
        splashControl1.SplashClosed += SaveSplashPreference;
    }
    
    private void CheckSplashSettings(object sender, CancelEventArgs e)
    {
        // Check user preference
        bool showSplash = Properties.Settings.Default.ShowSplashScreen;
        
        if (!showSplash)
        {
            e.Cancel = true;
            Debug.WriteLine("User disabled splash screen");
            return;
        }
        
        // Check if shown recently (once per day)
        DateTime? lastShown = Properties.Settings.Default.LastSplashShown;
        if (lastShown.HasValue && lastShown.Value.Date == DateTime.Today)
        {
            e.Cancel = true;
            Debug.WriteLine("Splash already shown today");
        }
    }
    
    private void SaveSplashPreference(object sender, EventArgs e)
    {
        // Record that splash was shown
        Properties.Settings.Default.LastSplashShown = DateTime.Now;
        Properties.Settings.Default.Save();
    }
}
```

## Best Practices

### Event Subscription

1. **Subscribe in constructor or Form_Load:**
   ```csharp
   public Form1()
   {
       InitializeComponent();
       splashControl1.SplashClosed += OnSplashClosed;
   }
   ```

2. **Unsubscribe when disposing:**
   ```csharp
   protected override void Dispose(bool disposing)
   {
       if (disposing)
       {
           splashControl1.SplashClosed -= OnSplashClosed;
       }
       base.Dispose(disposing);
   }
   ```

### Error Handling

Always handle exceptions in event handlers:

```csharp
private void SplashControl1_SplashDisplayed(object sender, EventArgs e)
{
    try
    {
        PerformInitialization();
    }
    catch (Exception ex)
    {
        Debug.WriteLine($"Error in SplashDisplayed: {ex.Message}");
        LogError(ex);
    }
}
```

### Threading Considerations

When updating UI from events triggered by background threads:

```csharp
private void UpdateUIFromEvent(string message)
{
    if (this.InvokeRequired)
    {
        this.Invoke(new Action(() => UpdateUIFromEvent(message)));
        return;
    }
    
    // Safe to update UI here
    statusLabel.Text = message;
}
```

### Performance

1. **Keep event handlers fast:** Don't block the UI thread
2. **Use async for long operations:** Start tasks, don't wait
3. **Avoid heavy logging:** Log only essential information

### Event Usage Guidelines

| Event | Primary Use | Avoid |
|-------|------------|-------|
| **BeforeSplash** | Conditional cancellation, preparation | Heavy operations |
| **SplashDisplayed** | Start background tasks | Blocking operations |
| **SplashClosing** | Conditional cancellation | Infinite loops preventing close |
| **SplashClosed** | Finalize setup, show dialogs | Starting splash again |

### Debugging Events

Use Debug.WriteLine for event tracking during development:

```csharp
private void SplashControl1_SplashDisplayed(object sender, EventArgs e)
{
    Debug.WriteLine($"[{DateTime.Now:HH:mm:ss.fff}] SplashDisplayed");
    Debug.WriteLine($"  Sender: {sender.GetType().Name}");
    Debug.WriteLine($"  Thread: {Thread.CurrentThread.ManagedThreadId}");
}
```
