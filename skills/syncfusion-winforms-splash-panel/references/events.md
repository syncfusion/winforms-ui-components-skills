# SplashPanel Events

This reference guide covers all events available in the SplashPanel control, including lifecycle events, mouse interaction events, and event handling patterns.

## Table of Contents

- [Overview](#overview)
- [Event Summary](#event-summary)
- [BeforeSplash Event](#beforesplash-event)
  - [Event Data](#beforesplash-event-data)
  - [Canceling BeforeSplash](#canceling-beforesplash)
- [SplashDisplayed Event](#splashdisplayed-event)
- [SplashClosing Event](#splashclosing-event)
  - [Event Data](#splashclosing-event-data)
  - [Canceling SplashClosing](#canceling-splashclosing)
- [SplashClosed Event](#splashclosed-event)
  - [Event Data](#splashclosed-event-data)
  - [SplashFormClosedNotify Method](#splashformclosednotify-method)
- [SplashMouseEnter Event](#splashmouseenter-event)
- [SplashMouseLeave Event](#splashmouseleave-event)
- [Complete Event Handling Example](#complete-event-handling-example)
- [Next Steps](#next-steps)

## Overview

The SplashPanel control provides a comprehensive set of events that allow you to respond to various stages of the splash panel lifecycle and user interactions. These events enable you to:

- Control splash panel display behavior
- Cancel splash panel operations
- Track mouse interactions
- Perform custom actions at specific lifecycle stages
- Log splash panel activity

## Event Summary

The following table provides a complete list of events available in the SplashPanel control:

| Event | Description | Can Cancel |
|-------|-------------|-----------|
| BeforeSplash | Triggered before the splash image is displayed | Yes |
| SplashDisplayed | Triggered when splash image is displayed | No |
| SplashClosing | Triggered when the splash image is closing | Yes |
| SplashClosed | Triggered when the splash image is closed | No |
| SplashMouseEnter | Triggered when mouse enters the splash image | No |
| SplashMouseLeave | Triggered when the mouse leaves the splash image | No |

## BeforeSplash Event

The `BeforeSplash` event is triggered before the splash panel is displayed. This event allows you to perform initialization tasks or cancel the splash panel display if needed.

**Event Signature:**

```csharp
public event CancelEventHandler BeforeSplash
```

### BeforeSplash Event Data

The event handler receives an argument of type `CancelEventArgs` containing data related to this event.

**CancelEventArgs Properties:**

| Member | Type | Description |
|--------|------|-------------|
| Cancel | Boolean | Indicates whether the event should be canceled |

### Canceling BeforeSplash

You can prevent the splash panel from displaying by setting the `Cancel` property to `true` in the event handler.

**C# Example:**

```csharp
// Subscribe to the BeforeSplash event
this.splashPanel1.BeforeSplash += new CancelEventHandler(splashPanel1_BeforeSplash);

private void splashPanel1_BeforeSplash(object sender, System.ComponentModel.CancelEventArgs e)
{
    string message = String.Format("Event: {0} Object: {1}\r\n", "BeforeSplash", ((Control)sender).Name);
    textBox1.Text = textBox1.Text + message;

    // To cancel this event and prevent splash display
    // e.Cancel = true;
}
```

**VB.NET Example:**

```vb
' Subscribe to the BeforeSplash event
AddHandler Me.splashPanel1.BeforeSplash, AddressOf splashPanel1_BeforeSplash

Private Sub splashPanel1_BeforeSplash(ByVal sender As Object, ByVal e As System.ComponentModel.CancelEventArgs) Handles splashPanel1.BeforeSplash
    Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "BeforeSplash", (CType(sender, Control)).Name)
    If Me.InvokeRequired Then
        Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
    Else
        OutputText(message)
    End If

    ' To cancel this event and prevent splash display
    ' e.Cancel = True
End Sub
```

## SplashDisplayed Event

The `SplashDisplayed` event is triggered when the splash panel is successfully displayed on the screen. Use this event to perform actions after the splash panel becomes visible.

**Event Signature:**

```csharp
public event EventHandler SplashDisplayed
```

The event handler receives an argument of type `EventArgs` containing data related to this event.

**C# Example:**

```csharp
// Subscribe to the SplashDisplayed event
this.splashPanel1.SplashDisplayed += new EventHandler(splashPanel1_SplashDisplayed);

private void splashPanel1_SplashDisplayed(object sender, System.EventArgs e)
{
    string message = String.Format("Event: {0} Object: {1}\r\n", "SplashDisplayed", ((Control)sender).Name);
    if (this.InvokeRequired)
    {
        this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
    }
    else
    {
        textBox1.Text = textBox1.Text + message;
    }
}
```

**VB.NET Example:**

```vb
' Subscribe to the SplashDisplayed event
AddHandler Me.splashPanel1.SplashDisplayed, AddressOf splashPanel1_SplashDisplayed

Private Sub splashPanel1_SplashDisplayed(ByVal sender As Object, ByVal e As System.EventArgs) Handles splashPanel1.SplashDisplayed
    Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashDisplayed", (CType(sender, Control)).Name)
    If Me.InvokeRequired Then
        Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
    Else
        OutputText(message)
    End If
End Sub
```

## SplashClosing Event

The `SplashClosing` event is triggered when the splash panel is about to close. This event allows you to perform cleanup tasks or cancel the closing operation.

**Event Signature:**

```csharp
public event CancelEventHandler SplashClosing
```

### SplashClosing Event Data

The event handler receives an argument of type `SplashClosedEventArgs` containing data related to this event.

**SplashClosedEventArgs Properties:**

| Member | Type | Description |
|--------|------|-------------|
| Cancel | Boolean | Indicates whether the event should be canceled |

### Canceling SplashClosing

You can prevent the splash panel from closing by setting the `Cancel` property to `true` in the event handler.

**C# Example:**

```csharp
// Subscribe to the SplashClosing event
this.splashPanel1.SplashClosing += new CancelEventHandler(splashPanel1_SplashClosing);

private void splashPanel1_SplashClosing(object sender, Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs args)
{
    string message = String.Format("Event: {0} Object: {1}\r\n", "SplashClosing", ((Control)sender).Name);
    if (this.InvokeRequired)
    {
        this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
    }
    else
    {
        OutputText(message);
    }
    
    // Add the splash panel back to the form if needed
    if(this.Controls.Contains(this.splashPanel1) == false)
        this.Controls.Add(this.splashPanel1);
    
    this.splashPanel1.Location = this.currentPt1;
    this.splashPanel1.Visible = true;
    
    // To cancel closing
    // args.Cancel = true;
}
```

**VB.NET Example:**

```vb
' Subscribe to the SplashClosing event
AddHandler Me.splashPanel1.SplashClosing, AddressOf splashPanel1_SplashClosing

Private Sub splashPanel1_SplashClosing(ByVal sender As Object, ByVal args As Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs)
    Dim message As String = String.Format("Event: {0} Object: {1}" & Chr(13) & "" & Chr(10) & "", "SplashClosing", DirectCast(sender, Control).Name)
    If Me.InvokeRequired Then
        Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() {message})
    Else
        OutputText(message)
    End If
    
    ' Add the splash panel back to the form if needed
    If Me.Controls.Contains(Me.splashPanel1) = False Then
        Me.Controls.Add(Me.splashPanel1)
    End If
    
    Me.splashPanel1.Location = Me.currentPt1
    Me.splashPanel1.Visible = True
    
    ' To cancel closing
    ' args.Cancel = True
End Sub
```

## SplashClosed Event

The `SplashClosed` event is triggered after the splash panel has been closed. This is the final event in the splash panel lifecycle.

**Event Signature:**

```csharp
public event SplashClosedEventHandler SplashClosed
```

### SplashClosed Event Data

The event handler receives an argument of type `SplashClosedEventArgs` containing data related to this event.

**SplashClosedEventArgs Properties:**

| Member | Type | Description |
|--------|------|-------------|
| SplashCloseType | Enum | Returns the value which indicates the way in which the splash was closed |

The `SplashCloseType` property provides information about how the splash panel was closed, allowing you to respond appropriately based on the closing method.

**C# Example:**

```csharp
// Subscribe to the SplashClosed event
this.splashPanel1.SplashClosed += new SplashClosedEventHandler(splashPanel1_SplashClosed);

private void splashPanel1_SplashClosed(object sender, Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs args)
{
    string message = String.Format("Event: {0} Object: {1}\r\n", "SplashClosed", ((Control)sender).Name);
    if (this.InvokeRequired)
    {
        this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
    }
    else
    {
        OutputText(message);
    }
    
    // Add the splash panel back to the form if needed
    if(this.Controls.Contains(this.splashPanel1) == false)
        this.Controls.Add(this.splashPanel1);
    
    this.splashPanel1.Location = this.currentPt1;
    this.splashPanel1.Visible = true;

    // Get information about how the splash was closed
    string closeType = args.SplashCloseType.ToString();
    textBox1.Text += $"Close Type: {closeType}\r\n";
}
```

**VB.NET Example:**

```vb
' Subscribe to the SplashClosed event
AddHandler Me.splashPanel1.SplashClosed, AddressOf splashPanel1_SplashClosed

Private Sub splashPanel1_SplashClosed(ByVal sender As Object, ByVal args As Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs) Handles splashPanel1.SplashClosed
    Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashClosed", (CType(sender, Control)).Name)
    If Me.InvokeRequired Then
        Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
    Else
        OutputText(message)
    End If
    
    ' Add the splash panel back to the form if needed
    If Me.Controls.Contains(Me.splashPanel1) = False Then
        Me.Controls.Add(Me.splashPanel1)
    End If
    
    Me.splashPanel1.Location = Me.currentPt1
    Me.splashPanel1.Visible = True

    ' Get information about how the splash was closed
    Dim closeType As String = args.SplashCloseType.ToString()
    textBox1.Text += String.Format("Close Type: {0}" & Constants.vbCrLf, closeType)
End Sub
```

### SplashFormClosedNotify Method

The `SplashClosed` event is raised when the `SplashFormClosedNotify()` method is called. This method is an implementation of the ISplashWrapperFormListener for receiving notification from the SplashWrapperForm when the splash window is closed.

**C# Example:**

```csharp
// Manually trigger the SplashClosed event
this.splashPanel1.SplashFormClosedNotify();
```

**VB.NET Example:**

```vb
' Manually trigger the SplashClosed event
Me.splashPanel1.SplashFormClosedNotify()
```

## SplashMouseEnter Event

The `SplashMouseEnter` event is triggered when the mouse cursor enters the splash panel area while it is displayed. This event will not be triggered after the splash screen is closed.

**Event Signature:**

```csharp
public event EventHandler SplashMouseEnter
```

The event handler receives an argument of type `EventArgs` containing data related to this event.

**C# Example:**

```csharp
// Subscribe to the SplashMouseEnter event
this.splashPanel1.SplashMouseEnter += new EventHandler(splashPanel1_SplashMouseEnter);

private void splashPanel1_SplashMouseEnter(object sender, System.EventArgs e)
{
    string message = String.Format("Event: {0} Object: {1}\r\n", "SplashMouseEnter", ((Control)sender).Name);
    if (this.InvokeRequired)
    {
        this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
    }
    else
    {
        OutputText(message);
    }
}
```

**VB.NET Example:**

```vb
' Subscribe to the SplashMouseEnter event
AddHandler Me.splashPanel1.SplashMouseEnter, AddressOf splashPanel1_SplashMouseEnter

Private Sub splashPanel1_SplashMouseEnter(ByVal sender As Object, ByVal e As System.EventArgs) Handles splashPanel1.SplashMouseEnter
    Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashMouseEnter", (CType(sender, Control)).Name)
    If Me.InvokeRequired Then
        Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
    Else
        OutputText(message)
    End If
End Sub
```

## SplashMouseLeave Event

The `SplashMouseLeave` event is triggered when the mouse cursor leaves the splash panel area while it is displayed. This event will not be triggered after the splash screen is closed.

**Event Signature:**

```csharp
public event EventHandler SplashMouseLeave
```

The event handler receives an argument of type `EventArgs` containing data related to this event.

**C# Example:**

```csharp
// Subscribe to the SplashMouseLeave event
this.splashPanel1.SplashMouseLeave += new EventHandler(splashPanel1_SplashMouseLeave);

private void splashPanel1_SplashMouseLeave(object sender, System.EventArgs e)
{
    string message = String.Format("Event: {0} Object: {1}\r\n", "SplashMouseLeave", ((Control)sender).Name);
    if (this.InvokeRequired)
    {
        this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
    }
    else
    {
        string text = message;
        textBox1.Text = textBox1.Text + text;
    }
}
```

**VB.NET Example:**

```vb
' Subscribe to the SplashMouseLeave event
AddHandler Me.splashPanel1.SplashMouseLeave, AddressOf splashPanel1_SplashMouseLeave

Private Sub splashPanel1_SplashMouseLeave(ByVal sender As Object, ByVal e As System.EventArgs) Handles splashPanel1.SplashMouseLeave
    Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashMouseLeave", (CType(sender, Control)).Name)
    If Me.InvokeRequired Then
        Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
    Else
        OutputText(message)
    End If
End Sub
```

## Complete Event Handling Example

The following example demonstrates a complete implementation of all SplashPanel events with a TextBox to display event information.

**C# Example - Complete Setup:**

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class SplashEventForm : Form
{
    // Declare the controls
    private Syncfusion.Windows.Forms.Tools.SplashPanel splashPanel1;
    private System.Windows.Forms.TextBox textBox1;
    private delegate void SetStringDelegate(string val);
    private Point currentPt1;

    public SplashEventForm()
    {
        InitializeComponents();
        SubscribeToEvents();
    }

    private void InitializeComponents()
    {
        // Initialize the controls
        this.splashPanel1 = new Syncfusion.Windows.Forms.Tools.SplashPanel();
        this.textBox1 = new System.Windows.Forms.TextBox();

        // Set the properties for the textbox
        this.textBox1.Dock = System.Windows.Forms.DockStyle.Fill;
        this.textBox1.ForeColor = System.Drawing.Color.Black;
        this.textBox1.Location = new System.Drawing.Point(0, 0);
        this.textBox1.Multiline = true;
        this.textBox1.Name = "textBox1";
        this.textBox1.ScrollBars = System.Windows.Forms.ScrollBars.Vertical;
        this.textBox1.Size = new System.Drawing.Size(248, 142);
        this.textBox1.TabIndex = 4;
        this.textBox1.Text = "";

        // Add the textbox to the form
        this.Controls.Add(this.textBox1);
    }

    private void SubscribeToEvents()
    {
        // Subscribe to all events
        this.splashPanel1.BeforeSplash += new CancelEventHandler(splashPanel1_BeforeSplash);
        this.splashPanel1.SplashDisplayed += new EventHandler(splashPanel1_SplashDisplayed);
        this.splashPanel1.SplashClosing += new CancelEventHandler(splashPanel1_SplashClosing);
        this.splashPanel1.SplashClosed += new SplashClosedEventHandler(splashPanel1_SplashClosed);
        this.splashPanel1.SplashMouseEnter += new EventHandler(splashPanel1_SplashMouseEnter);
        this.splashPanel1.SplashMouseLeave += new EventHandler(splashPanel1_SplashMouseLeave);
    }

    private void OutputText(string text)
    {
        textBox1.Text = textBox1.Text + text;
    }

    private void splashPanel1_BeforeSplash(object sender, System.ComponentModel.CancelEventArgs e)
    {
        string message = String.Format("Event: {0} Object: {1}\r\n", "BeforeSplash", ((Control)sender).Name);
        textBox1.Text = textBox1.Text + message;
    }

    private void splashPanel1_SplashDisplayed(object sender, System.EventArgs e)
    {
        string message = String.Format("Event: {0} Object: {1}\r\n", "SplashDisplayed", ((Control)sender).Name);
        if (this.InvokeRequired)
        {
            this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
        }
        else
        {
            textBox1.Text = textBox1.Text + message;
        }
    }

    private void splashPanel1_SplashClosing(object sender, Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs args)
    {
        string message = String.Format("Event: {0} Object: {1}\r\n", "SplashClosing", ((Control)sender).Name);
        if (this.InvokeRequired)
        {
            this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
        }
        else
        {
            OutputText(message);
        }
    }

    private void splashPanel1_SplashClosed(object sender, Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs args)
    {
        string message = String.Format("Event: {0} Object: {1}\r\n", "SplashClosed", ((Control)sender).Name);
        if (this.InvokeRequired)
        {
            this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
        }
        else
        {
            OutputText(message);
        }
    }

    private void splashPanel1_SplashMouseEnter(object sender, System.EventArgs e)
    {
        string message = String.Format("Event: {0} Object: {1}\r\n", "SplashMouseEnter", ((Control)sender).Name);
        if (this.InvokeRequired)
        {
            this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
        }
        else
        {
            OutputText(message);
        }
    }

    private void splashPanel1_SplashMouseLeave(object sender, System.EventArgs e)
    {
        string message = String.Format("Event: {0} Object: {1}\r\n", "SplashMouseLeave", ((Control)sender).Name);
        if (this.InvokeRequired)
        {
            this.Invoke(new SetStringDelegate(OutputText), new object[] { message });
        }
        else
        {
            OutputText(message);
        }
    }
}
```

**VB.NET Example - Complete Setup:**

```vb
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class SplashEventForm
    Inherits Form
    
    ' Declare the controls
    Friend WithEvents splashPanel1 As Syncfusion.Windows.Forms.Tools.SplashPanel
    Friend WithEvents textBox1 As System.Windows.Forms.TextBox
    Private Delegate Sub SetStringDelegate(ByVal val As String)
    Private currentPt1 As Point

    Public Sub New()
        InitializeComponents()
        SubscribeToEvents()
    End Sub

    Private Sub InitializeComponents()
        ' Initialize the controls
        Me.splashPanel1 = New Syncfusion.Windows.Forms.Tools.SplashPanel
        Me.textBox1 = New System.Windows.Forms.TextBox

        ' Set the properties for the textbox
        Me.textBox1.Dock = System.Windows.Forms.DockStyle.Fill
        Me.textBox1.ForeColor = System.Drawing.Color.Black
        Me.textBox1.Location = New System.Drawing.Point(0, 0)
        Me.textBox1.Multiline = True
        Me.textBox1.Name = "textBox1"
        Me.textBox1.ScrollBars = System.Windows.Forms.ScrollBars.Vertical
        Me.textBox1.Size = New System.Drawing.Size(272, 190)
        Me.textBox1.TabIndex = 5
        Me.textBox1.Text = ""

        ' Add the textbox to the form
        Me.Controls.Add(Me.textBox1)
    End Sub

    Private Sub SubscribeToEvents()
        ' Subscribe to all events
        AddHandler Me.splashPanel1.BeforeSplash, AddressOf splashPanel1_BeforeSplash
        AddHandler Me.splashPanel1.SplashDisplayed, AddressOf splashPanel1_SplashDisplayed
        AddHandler Me.splashPanel1.SplashClosing, AddressOf splashPanel1_SplashClosing
        AddHandler Me.splashPanel1.SplashClosed, AddressOf splashPanel1_SplashClosed
        AddHandler Me.splashPanel1.SplashMouseEnter, AddressOf splashPanel1_SplashMouseEnter
        AddHandler Me.splashPanel1.SplashMouseLeave, AddressOf splashPanel1_SplashMouseLeave
    End Sub

    Private Sub OutputText(ByVal text As String)
        textBox1.Text = textBox1.Text & text
    End Sub

    Private Sub splashPanel1_BeforeSplash(ByVal sender As Object, ByVal e As System.ComponentModel.CancelEventArgs) Handles splashPanel1.BeforeSplash
        Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "BeforeSplash", (CType(sender, Control)).Name)
        If Me.InvokeRequired Then
            Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
        Else
            OutputText(message)
        End If
    End Sub

    Private Sub splashPanel1_SplashDisplayed(ByVal sender As Object, ByVal e As System.EventArgs) Handles splashPanel1.SplashDisplayed
        Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashDisplayed", (CType(sender, Control)).Name)
        If Me.InvokeRequired Then
            Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
        Else
            OutputText(message)
        End If
    End Sub

    Private Sub splashPanel1_SplashClosing(ByVal sender As Object, ByVal args As Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs)
        Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashClosing", DirectCast(sender, Control).Name)
        If Me.InvokeRequired Then
            Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() {message})
        Else
            OutputText(message)
        End If
    End Sub

    Private Sub splashPanel1_SplashClosed(ByVal sender As Object, ByVal args As Syncfusion.Windows.Forms.Tools.SplashClosedEventArgs) Handles splashPanel1.SplashClosed
        Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashClosed", (CType(sender, Control)).Name)
        If Me.InvokeRequired Then
            Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
        Else
            OutputText(message)
        End If
    End Sub

    Private Sub splashPanel1_SplashMouseEnter(ByVal sender As Object, ByVal e As System.EventArgs) Handles splashPanel1.SplashMouseEnter
        Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashMouseEnter", (CType(sender, Control)).Name)
        If Me.InvokeRequired Then
            Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
        Else
            OutputText(message)
        End If
    End Sub

    Private Sub splashPanel1_SplashMouseLeave(ByVal sender As Object, ByVal e As System.EventArgs) Handles splashPanel1.SplashMouseLeave
        Dim message As String = String.Format("Event: {0} Object: {1}" & Constants.vbCrLf, "SplashMouseLeave", (CType(sender, Control)).Name)
        If Me.InvokeRequired Then
            Me.Invoke(New SetStringDelegate(AddressOf OutputText), New Object() { message })
        Else
            OutputText(message)
        End If
    End Sub
End Class
```

## Next Steps

- **[Getting Started](./getting-started.md)** - Learn the basics of implementing SplashPanel
- **[Display Methods](./display-methods.md)** - Explore different ways to show and hide splash panels
- **[Animation and Appearance](./animation-appearance.md)** - Configure visual appearance and animations
- **[Slide Transitions](./slide-transitions.md)** - Configure advanced slide and marquee animations
