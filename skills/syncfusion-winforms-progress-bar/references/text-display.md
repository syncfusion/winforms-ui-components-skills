# Text Display Configuration in ProgressBarAdv

This reference covers all text display options in ProgressBarAdv, including text styles, alignment, orientation, visibility, shadows, and custom text formatting.

## Table of Contents

- [Overview](#overview)
- [When to Read This](#when-to-read-this)
- [Text Style Options](#text-style-options)
  - [Value Style](#value-style)
  - [Percentage Style](#percentage-style)
  - [Custom Style](#custom-style)
- [Text Alignment](#text-alignment)
  - [Horizontal Alignment](#horizontal-alignment)
  - [Vertical Alignment Considerations](#vertical-alignment-considerations)
- [Text Orientation](#text-orientation)
  - [Horizontal Orientation](#horizontal-orientation)
  - [Vertical Orientation](#vertical-orientation)
- [Text Visibility](#text-visibility)
- [Text Shadow Effects](#text-shadow-effects)
- [Font and Color Settings](#font-and-color-settings)
  - [Font Configuration](#font-configuration)
  - [FontColor Property](#fontcolor-property)
- [Custom Text Implementation](#custom-text-implementation)
  - [Basic Custom Text](#basic-custom-text)
  - [Dynamic Custom Text](#dynamic-custom-text)
  - [Formatted Custom Text](#formatted-custom-text)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

ProgressBarAdv provides extensive text display capabilities allowing you to show progress information in various formats. The text can display the current value, percentage completion, or custom formatted text with full control over positioning, orientation, and appearance.

**Key Properties:**
- `TextStyle` - Determines what text is displayed (Value, Percentage, Custom)
- `TextAlignment` - Controls horizontal text alignment
- `TextOrientation` - Controls text orientation (horizontal/vertical)
- `TextVisible` - Shows or hides text
- `TextShadow` - Enables shadow effects
- `FontColor` - Sets text color

## When to Read This

Read this reference when:
- Implementing text display on progress bars
- Customizing text appearance and positioning
- Creating custom progress text formats
- Troubleshooting text display issues
- Showing progress information to users
- Aligning text with progress bar orientation

## Text Style Options

The `TextStyle` property controls what text is displayed on the progress bar.

### Value Style

Displays the current value of the progress bar.

**C#:**
```csharp
// Display current value
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Value;
progressBarAdv1.TextVisible = true;
progressBarAdv1.Minimum = 0;
progressBarAdv1.Maximum = 100;
progressBarAdv1.Value = 45;
// Displays: "45"
```

**VB.NET:**
```vbnet
' Display current value
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Value
progressBarAdv1.TextVisible = True
progressBarAdv1.Minimum = 0
progressBarAdv1.Maximum = 100
progressBarAdv1.Value = 45
' Displays: "45"
```

**Use Case:** File processing where you need to show items processed (e.g., "150" out of 500 files).

### Percentage Style

Displays the progress as a percentage.

**C#:**
```csharp
// Display percentage
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
progressBarAdv1.Minimum = 0;
progressBarAdv1.Maximum = 100;
progressBarAdv1.Value = 45;
// Displays: "45%"
```

**VB.NET:**
```vbnet
' Display percentage
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Percentage
progressBarAdv1.TextVisible = True
progressBarAdv1.Minimum = 0
progressBarAdv1.Maximum = 100
progressBarAdv1.Value = 45
' Displays: "45%"
```

**Use Case:** Download progress, installation progress, or any operation where percentage is more meaningful.

### Custom Style

Allows you to display custom formatted text using the `ValueChanged` event.

**C#:**
```csharp
// Setup custom text style
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Custom;
progressBarAdv1.TextVisible = true;

// Handle ValueChanged event to set custom text
progressBarAdv1.ValueChanged += ProgressBarAdv1_ValueChanged;

private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
{
    ProgressBarAdv progressBar = sender as ProgressBarAdv;
    if (progressBar != null)
    {
        int percentage = (int)((progressBar.Value - progressBar.Minimum) * 100.0 / 
                              (progressBar.Maximum - progressBar.Minimum));
        progressBar.Text = $"Loading... {percentage}% Complete";
    }
}
```

**VB.NET:**
```vbnet
' Setup custom text style
progressBarAdv1.TextStyle = Syncfusion.Windows.Forms.Tools.ProgressBarTextStyles.Custom
progressBarAdv1.TextVisible = True

' Handle ValueChanged event to set custom text
AddHandler progressBarAdv1.ValueChanged, AddressOf ProgressBarAdv1_ValueChanged

Private Sub ProgressBarAdv1_ValueChanged(sender As Object, e As EventArgs)
    Dim progressBar As ProgressBarAdv = TryCast(sender, ProgressBarAdv)
    If progressBar IsNot Nothing Then
        Dim percentage As Integer = CInt((progressBar.Value - progressBar.Minimum) * 100.0 / _
                                         (progressBar.Maximum - progressBar.Minimum))
        progressBar.Text = $"Loading... {percentage}% Complete"
    End If
End Sub
```

**Use Case:** Displaying detailed progress information like "Processing file 25 of 100 (25%)".

## Text Alignment

### Horizontal Alignment

The `TextAlignment` property controls horizontal text positioning.

**Available Options:**
- `Left` - Aligns text to the left
- `Center` - Centers text (default)
- `Right` - Aligns text to the right

**C#:**
```csharp
// Left alignment
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Left;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;

// Center alignment (default)
progressBarAdv2.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center;
progressBarAdv2.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv2.TextVisible = true;

// Right alignment
progressBarAdv3.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Right;
progressBarAdv3.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv3.TextVisible = true;
```

**VB.NET:**
```vbnet
' Left alignment
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Left
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage
progressBarAdv1.TextVisible = True

' Center alignment (default)
progressBarAdv2.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center
progressBarAdv2.TextStyle = ProgressBarTextStyles.Percentage
progressBarAdv2.TextVisible = True

' Right alignment
progressBarAdv3.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Right
progressBarAdv3.TextStyle = ProgressBarTextStyles.Percentage
progressBarAdv3.TextVisible = True
```

### Vertical Alignment Considerations

Text is automatically vertically centered. For vertical progress bars, consider using `TextOrientation` to rotate text.

## Text Orientation

### Horizontal Orientation

Default orientation where text reads left to right.

**C#:**
```csharp
progressBarAdv1.TextOrientation = System.Windows.Forms.Orientation.Horizontal;
progressBarAdv1.ProgressOrientation = System.Windows.Forms.Orientation.Horizontal;
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
```

**VB.NET:**
```vbnet
progressBarAdv1.TextOrientation = System.Windows.Forms.Orientation.Horizontal
progressBarAdv1.ProgressOrientation = System.Windows.Forms.Orientation.Horizontal
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage
progressBarAdv1.TextVisible = True
```

### Vertical Orientation

Text is rotated 90 degrees for vertical progress bars.

**C#:**
```csharp
progressBarAdv1.TextOrientation = System.Windows.Forms.Orientation.Vertical;
progressBarAdv1.ProgressOrientation = System.Windows.Forms.Orientation.Vertical;
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
progressBarAdv1.Size = new System.Drawing.Size(50, 200);
```

**VB.NET:**
```vbnet
progressBarAdv1.TextOrientation = System.Windows.Forms.Orientation.Vertical
progressBarAdv1.ProgressOrientation = System.Windows.Forms.Orientation.Vertical
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage
progressBarAdv1.TextVisible = True
progressBarAdv1.Size = New System.Drawing.Size(50, 200)
```

**Best Practice:** Match `TextOrientation` with `ProgressOrientation` for consistency.

## Text Visibility

Control whether text is displayed on the progress bar.

**C#:**
```csharp
// Show text
progressBarAdv1.TextVisible = true;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;

// Hide text
progressBarAdv2.TextVisible = false;

// Toggle text visibility
private void btnToggleText_Click(object sender, EventArgs e)
{
    progressBarAdv1.TextVisible = !progressBarAdv1.TextVisible;
}
```

**VB.NET:**
```vbnet
' Show text
progressBarAdv1.TextVisible = True
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage

' Hide text
progressBarAdv2.TextVisible = False

' Toggle text visibility
Private Sub btnToggleText_Click(sender As Object, e As EventArgs)
    progressBarAdv1.TextVisible = Not progressBarAdv1.TextVisible
End Sub
```

## Text Shadow Effects

Add depth to text with shadow effects.

**C#:**
```csharp
// Enable text shadow
progressBarAdv1.TextShadow = true;
progressBarAdv1.TextVisible = true;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.FontColor = System.Drawing.Color.White;
progressBarAdv1.BackColor = System.Drawing.Color.DarkBlue;

// Disable text shadow
progressBarAdv2.TextShadow = false;
progressBarAdv2.TextVisible = true;
progressBarAdv2.TextStyle = ProgressBarTextStyles.Percentage;
```

**VB.NET:**
```vbnet
' Enable text shadow
progressBarAdv1.TextShadow = True
progressBarAdv1.TextVisible = True
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage
progressBarAdv1.FontColor = System.Drawing.Color.White
progressBarAdv1.BackColor = System.Drawing.Color.DarkBlue

' Disable text shadow
progressBarAdv2.TextShadow = False
progressBarAdv2.TextVisible = True
progressBarAdv2.TextStyle = ProgressBarTextStyles.Percentage
```

**Note:** Text shadow provides better contrast on dark backgrounds.

## Font and Color Settings

### Font Configuration

**C#:**
```csharp
// Set custom font
progressBarAdv1.Font = new System.Drawing.Font("Segoe UI", 10F, System.Drawing.FontStyle.Bold);
progressBarAdv1.TextVisible = true;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center;

// Multiple fonts for different styles
progressBarAdv1.Font = new System.Drawing.Font("Arial", 12F, System.Drawing.FontStyle.Regular);
progressBarAdv2.Font = new System.Drawing.Font("Consolas", 10F, System.Drawing.FontStyle.Bold);
progressBarAdv3.Font = new System.Drawing.Font("Tahoma", 9F, System.Drawing.FontStyle.Italic);
```

**VB.NET:**
```vbnet
' Set custom font
progressBarAdv1.Font = New System.Drawing.Font("Segoe UI", 10.0F, System.Drawing.FontStyle.Bold)
progressBarAdv1.TextVisible = True
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center

' Multiple fonts for different styles
progressBarAdv1.Font = New System.Drawing.Font("Arial", 12.0F, System.Drawing.FontStyle.Regular)
progressBarAdv2.Font = New System.Drawing.Font("Consolas", 10.0F, System.Drawing.FontStyle.Bold)
progressBarAdv3.Font = New System.Drawing.Font("Tahoma", 9.0F, System.Drawing.FontStyle.Italic)
```

### FontColor Property

**C#:**
```csharp
// Set text color
progressBarAdv1.FontColor = System.Drawing.Color.SteelBlue;
progressBarAdv1.TextVisible = true;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;

// Contrast colors for visibility
progressBarAdv1.FontColor = System.Drawing.Color.White;
progressBarAdv1.ForeColor = System.Drawing.Color.DarkBlue;
progressBarAdv1.BackColor = System.Drawing.Color.LightBlue;

// Dynamic color based on progress
private void SetProgressTextColor()
{
    int percentage = (progressBarAdv1.Value * 100) / progressBarAdv1.Maximum;
    
    if (percentage < 30)
        progressBarAdv1.FontColor = System.Drawing.Color.Red;
    else if (percentage < 70)
        progressBarAdv1.FontColor = System.Drawing.Color.Orange;
    else
        progressBarAdv1.FontColor = System.Drawing.Color.Green;
}
```

**VB.NET:**
```vbnet
' Set text color
progressBarAdv1.FontColor = System.Drawing.Color.SteelBlue
progressBarAdv1.TextVisible = True
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage

' Contrast colors for visibility
progressBarAdv1.FontColor = System.Drawing.Color.White
progressBarAdv1.ForeColor = System.Drawing.Color.DarkBlue
progressBarAdv1.BackColor = System.Drawing.Color.LightBlue

' Dynamic color based on progress
Private Sub SetProgressTextColor()
    Dim percentage As Integer = (progressBarAdv1.Value * 100) \ progressBarAdv1.Maximum
    
    If percentage < 30 Then
        progressBarAdv1.FontColor = System.Drawing.Color.Red
    ElseIf percentage < 70 Then
        progressBarAdv1.FontColor = System.Drawing.Color.Orange
    Else
        progressBarAdv1.FontColor = System.Drawing.Color.Green
    End If
End Sub
```

## Custom Text Implementation

### Basic Custom Text

**C#:**
```csharp
public class CustomTextProgressBar : Form
{
    private ProgressBarAdv progressBarAdv1;
    
    public CustomTextProgressBar()
    {
        InitializeProgressBar();
    }
    
    private void InitializeProgressBar()
    {
        progressBarAdv1 = new ProgressBarAdv
        {
            Minimum = 0,
            Maximum = 100,
            Value = 0,
            TextStyle = ProgressBarTextStyles.Custom,
            TextVisible = true,
            TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center,
            Size = new System.Drawing.Size(400, 30)
        };
        
        progressBarAdv1.ValueChanged += ProgressBarAdv1_ValueChanged;
        this.Controls.Add(progressBarAdv1);
    }
    
    private void ProgressBarAdv1_ValueChanged(object sender, EventArgs e)
    {
        progressBarAdv1.Text = $"Progress: {progressBarAdv1.Value}%";
    }
}
```

**VB.NET:**
```vbnet
Public Class CustomTextProgressBar
    Inherits Form
    
    Private progressBarAdv1 As ProgressBarAdv
    
    Public Sub New()
        InitializeProgressBar()
    End Sub
    
    Private Sub InitializeProgressBar()
        progressBarAdv1 = New ProgressBarAdv With {
            .Minimum = 0,
            .Maximum = 100,
            .Value = 0,
            .TextStyle = ProgressBarTextStyles.Custom,
            .TextVisible = True,
            .TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center,
            .Size = New System.Drawing.Size(400, 30)
        }
        
        AddHandler progressBarAdv1.ValueChanged, AddressOf ProgressBarAdv1_ValueChanged
        Me.Controls.Add(progressBarAdv1)
    End Sub
    
    Private Sub ProgressBarAdv1_ValueChanged(sender As Object, e As EventArgs)
        progressBarAdv1.Text = $"Progress: {progressBarAdv1.Value}%"
    End Sub
End Class
```

### Dynamic Custom Text

**C#:**
```csharp
public class DynamicTextProgressBar : Form
{
    private ProgressBarAdv progressBarAdv1;
    private int totalItems = 100;
    private int currentItem = 0;
    
    private void InitializeProgressBar()
    {
        progressBarAdv1 = new ProgressBarAdv
        {
            Minimum = 0,
            Maximum = totalItems,
            Value = 0,
            TextStyle = ProgressBarTextStyles.Custom,
            TextVisible = true,
            Font = new System.Drawing.Font("Segoe UI", 9F, System.Drawing.FontStyle.Regular),
            FontColor = System.Drawing.Color.Black
        };
        
        progressBarAdv1.ValueChanged += UpdateDynamicText;
    }
    
    private void UpdateDynamicText(object sender, EventArgs e)
    {
        currentItem = progressBarAdv1.Value;
        int percentage = (currentItem * 100) / totalItems;
        TimeSpan estimated = EstimateTimeRemaining();
        
        progressBarAdv1.Text = $"{currentItem} of {totalItems} items ({percentage}%) - " +
                              $"ETA: {estimated:mm\\:ss}";
    }
    
    private TimeSpan EstimateTimeRemaining()
    {
        // Calculate based on current progress rate
        // This is a simplified example
        return TimeSpan.FromSeconds((totalItems - currentItem) * 0.5);
    }
}
```

**VB.NET:**
```vbnet
Public Class DynamicTextProgressBar
    Inherits Form
    
    Private progressBarAdv1 As ProgressBarAdv
    Private totalItems As Integer = 100
    Private currentItem As Integer = 0
    
    Private Sub InitializeProgressBar()
        progressBarAdv1 = New ProgressBarAdv With {
            .Minimum = 0,
            .Maximum = totalItems,
            .Value = 0,
            .TextStyle = ProgressBarTextStyles.Custom,
            .TextVisible = True,
            .Font = New System.Drawing.Font("Segoe UI", 9.0F, System.Drawing.FontStyle.Regular),
            .FontColor = System.Drawing.Color.Black
        }
        
        AddHandler progressBarAdv1.ValueChanged, AddressOf UpdateDynamicText
    End Sub
    
    Private Sub UpdateDynamicText(sender As Object, e As EventArgs)
        currentItem = progressBarAdv1.Value
        Dim percentage As Integer = (currentItem * 100) \ totalItems
        Dim estimated As TimeSpan = EstimateTimeRemaining()
        
        progressBarAdv1.Text = $"{currentItem} of {totalItems} items ({percentage}%) - " & _
                              $"ETA: {estimated:mm\:ss}"
    End Sub
    
    Private Function EstimateTimeRemaining() As TimeSpan
        ' Calculate based on current progress rate
        ' This is a simplified example
        Return TimeSpan.FromSeconds((totalItems - currentItem) * 0.5)
    End Function
End Class
```

### Formatted Custom Text

**C#:**
```csharp
public class FormattedTextProgressBar : Form
{
    private ProgressBarAdv progressBarAdv1;
    private long totalBytes = 0;
    private long downloadedBytes = 0;
    
    private void InitializeDownloadProgressBar()
    {
        progressBarAdv1 = new ProgressBarAdv
        {
            Minimum = 0,
            Maximum = 10000, // Using 10000 for precision
            Value = 0,
            TextStyle = ProgressBarTextStyles.Custom,
            TextVisible = true,
            TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center,
            Font = new System.Drawing.Font("Segoe UI", 9F)
        };
        
        progressBarAdv1.ValueChanged += UpdateDownloadText;
    }
    
    private void UpdateDownloadText(object sender, EventArgs e)
    {
        string downloaded = FormatBytes(downloadedBytes);
        string total = FormatBytes(totalBytes);
        int percentage = progressBarAdv1.Value / 100;
        
        progressBarAdv1.Text = $"Downloaded: {downloaded} of {total} ({percentage}%)";
    }
    
    private string FormatBytes(long bytes)
    {
        string[] sizes = { "B", "KB", "MB", "GB", "TB" };
        double len = bytes;
        int order = 0;
        
        while (len >= 1024 && order < sizes.Length - 1)
        {
            order++;
            len = len / 1024;
        }
        
        return $"{len:0.##} {sizes[order]}";
    }
    
    public void UpdateProgress(long downloaded, long total)
    {
        downloadedBytes = downloaded;
        totalBytes = total;
        
        if (total > 0)
        {
            progressBarAdv1.Value = (int)((downloaded * 10000) / total);
        }
    }
}
```

**VB.NET:**
```vbnet
Public Class FormattedTextProgressBar
    Inherits Form
    
    Private progressBarAdv1 As ProgressBarAdv
    Private totalBytes As Long = 0
    Private downloadedBytes As Long = 0
    
    Private Sub InitializeDownloadProgressBar()
        progressBarAdv1 = New ProgressBarAdv With {
            .Minimum = 0,
            .Maximum = 10000, ' Using 10000 for precision
            .Value = 0,
            .TextStyle = ProgressBarTextStyles.Custom,
            .TextVisible = True,
            .TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center,
            .Font = New System.Drawing.Font("Segoe UI", 9.0F)
        }
        
        AddHandler progressBarAdv1.ValueChanged, AddressOf UpdateDownloadText
    End Sub
    
    Private Sub UpdateDownloadText(sender As Object, e As EventArgs)
        Dim downloaded As String = FormatBytes(downloadedBytes)
        Dim total As String = FormatBytes(totalBytes)
        Dim percentage As Integer = progressBarAdv1.Value \ 100
        
        progressBarAdv1.Text = $"Downloaded: {downloaded} of {total} ({percentage}%)"
    End Sub
    
    Private Function FormatBytes(bytes As Long) As String
        Dim sizes() As String = { "B", "KB", "MB", "GB", "TB" }
        Dim len As Double = bytes
        Dim order As Integer = 0
        
        While len >= 1024 AndAlso order < sizes.Length - 1
            order += 1
            len = len / 1024
        End While
        
        Return $"{len:0.##} {sizes(order)}"
    End Function
    
    Public Sub UpdateProgress(downloaded As Long, total As Long)
        downloadedBytes = downloaded
        totalBytes = total
        
        If total > 0 Then
            progressBarAdv1.Value = CInt((downloaded * 10000) / total)
        End If
    End Sub
End Class
```

## Use Cases

### Use Case 1: File Processing with Item Count
Display number of files processed out of total.

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
progressBarAdv1.Minimum = 0;
progressBarAdv1.Maximum = totalFiles;
progressBarAdv1.ValueChanged += (s, e) => 
{
    progressBarAdv1.Text = $"Processing: {progressBarAdv1.Value} / {totalFiles} files";
};
```

### Use Case 2: Download Progress with Speed
Show download progress with current speed.

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
progressBarAdv1.ValueChanged += (s, e) =>
{
    progressBarAdv1.Text = $"{progressBarAdv1.Value}% - {currentSpeed:F2} MB/s";
};
```

### Use Case 3: Installation Progress
Display installation phase and percentage.

```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Custom;
progressBarAdv1.ValueChanged += (s, e) =>
{
    progressBarAdv1.Text = $"{currentPhase} - {progressBarAdv1.Value}% Complete";
};
```

## Best Practices

1. **Match Text to Context**
   - Use percentage for downloads, installations
   - Use value count for file processing, batch operations
   - Use custom text for complex scenarios

2. **Consider Text Readability**
   - Choose font colors with good contrast
   - Use appropriate font sizes (9-12pt recommended)
   - Enable text shadow for dark backgrounds

3. **Align Text with Orientation**
   - Match `TextOrientation` with `ProgressOrientation`
   - Center-align text for most scenarios
   - Use left/right alignment for specific UI requirements

4. **Keep Custom Text Concise**
   - Avoid overly long text strings
   - Consider progress bar width when formatting
   - Use abbreviations when appropriate (KB, MB, etc.)

5. **Update Text Efficiently**
   - Use `ValueChanged` event for custom text
   - Avoid unnecessary string allocations
   - Cache formatted strings when possible

## Common Scenarios

### Scenario 1: Basic Percentage Display
```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
progressBarAdv1.TextAlignment = Syncfusion.Windows.Forms.Tools.TextAlignment.Center;
```

### Scenario 2: File Processing Counter
```csharp
progressBarAdv1.TextStyle = ProgressBarTextStyles.Value;
progressBarAdv1.TextVisible = true;
progressBarAdv1.Maximum = totalFiles;
```

### Scenario 3: No Text Display
```csharp
progressBarAdv1.TextVisible = false;
// Progress shown only visually
```

### Scenario 4: Vertical Progress with Text
```csharp
progressBarAdv1.ProgressOrientation = System.Windows.Forms.Orientation.Vertical;
progressBarAdv1.TextOrientation = System.Windows.Forms.Orientation.Vertical;
progressBarAdv1.TextStyle = ProgressBarTextStyles.Percentage;
progressBarAdv1.TextVisible = true;
```

## Troubleshooting

### Issue: Text Not Visible

**Solutions:**
1. Verify `TextVisible = true`
2. Check `FontColor` has contrast with background
3. Ensure progress bar has sufficient size
4. Verify `Font` is not too large for progress bar height

### Issue: Custom Text Not Updating

**Solutions:**
1. Ensure `TextStyle` is set to `Custom`
2. Verify `ValueChanged` event is wired correctly
3. Check that `Text` property is being set in event handler
4. Ensure progress bar `Value` is actually changing

### Issue: Text Cut Off or Truncated

**Solutions:**
1. Increase progress bar width/height
2. Reduce font size
3. Use shorter text strings
4. Consider abbreviations or symbols

### Issue: Text Color Not Changing

**Solutions:**
1. Use `FontColor` property (not `ForeColor`)
2. Ensure theming is not overriding colors
3. Check `ThemesEnabled` property setting
4. Verify color is set after theme initialization

### Issue: Text Alignment Not Working

**Solutions:**
1. Verify `TextAlignment` property is set correctly
2. Check that text isn't too long (forces positioning)
3. Ensure sufficient padding/space in progress bar
4. Consider custom drawing for complex alignment needs

### Issue: Text Shadow Not Visible

**Solutions:**
1. Verify `TextShadow = true`
2. Ensure sufficient contrast between text and background
3. Use lighter text colors for shadow visibility
4. Check progress bar style supports shadows

## Related Topics

- **[appearance-styling.md](appearance-styling.md)** - Foreground and background colors
- **[orientation-layout.md](orientation-layout.md)** - Progress bar orientation settings
- **[themes.md](themes.md)** - Theme-based text styling
- **[events-advanced.md](events-advanced.md)** - ValueChanged event details
