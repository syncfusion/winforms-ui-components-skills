# Serialization

Guide to persisting and loading GradientLabel configurations using XML serialization for data preservation and user preferences.

## Overview

GradientLabel properties can be serialized to XML for:
- Saving user customizations
- Storing application themes
- Exporting/importing label configurations
- Preserving gradient settings across sessions

**Key Classes:**
- `XmlSerializer`: .NET serialization engine
- `BrushInfo`: Serializable gradient configuration
- `FileStream`: File I/O for XML storage

---

## Basic Serialization Concepts

### What Can Be Serialized?

**Serializable Properties:**
- BackgroundColor (BrushInfo)
- Text
- ForeColor
- Font (family, size, style)
- TextAlign
- BorderStyle, BorderSides, BorderColor
- Size, Location

**Non-Serializable:**
- Event handlers
- Parent references
- Design-time properties

---

## Saving BackgroundColor to XML

The most common serialization scenario is saving gradient configurations.

### Example 1: Serialize Single BrushInfo

**C# Example:**
```csharp
using System;
using System.IO;
using System.Xml.Serialization;
using Syncfusion.Drawing;

public void SaveGradientToXml(BrushInfo brushInfo, string filePath)
{
    try
    {
        XmlSerializer serializer = new XmlSerializer(typeof(BrushInfo));
        using (FileStream fs = new FileStream(filePath, FileMode.Create))
        {
            serializer.Serialize(fs, brushInfo);
        }
        MessageBox.Show("Gradient saved successfully!", "Success");
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error saving gradient: {ex.Message}", "Error");
    }
}

// Usage
BrushInfo gradient = new BrushInfo(
    GradientStyle.Horizontal,
    Color.Blue,
    Color.LightBlue
);
SaveGradientToXml(gradient, "gradient_config.xml");
```

**VB.NET Example:**
```vb
Imports System
Imports System.IO
Imports System.Xml.Serialization
Imports Syncfusion.Drawing

Public Sub SaveGradientToXml(brushInfo As BrushInfo, filePath As String)
    Try
        Dim serializer As New XmlSerializer(GetType(BrushInfo))
        Using fs As New FileStream(filePath, FileMode.Create)
            serializer.Serialize(fs, brushInfo)
        End Using
        MessageBox.Show("Gradient saved successfully!", "Success")
    Catch ex As Exception
        MessageBox.Show($"Error saving gradient: {ex.Message}", "Error")
    End Try
End Sub

' Usage
Dim gradient As New BrushInfo( _
    GradientStyle.Horizontal, _
    Color.Blue, _
    Color.LightBlue _
)
SaveGradientToXml(gradient, "gradient_config.xml")
```

---

## Loading BackgroundColor from XML

**C# Example:**
```csharp
public BrushInfo LoadGradientFromXml(string filePath)
{
    try
    {
        if (!File.Exists(filePath))
        {
            MessageBox.Show("Configuration file not found.", "Error");
            return null;
        }

        XmlSerializer serializer = new XmlSerializer(typeof(BrushInfo));
        using (FileStream fs = new FileStream(filePath, FileMode.Open))
        {
            BrushInfo brushInfo = (BrushInfo)serializer.Deserialize(fs);
            return brushInfo;
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error loading gradient: {ex.Message}", "Error");
        return null;
    }
}

// Usage
BrushInfo loadedGradient = LoadGradientFromXml("gradient_config.xml");
if (loadedGradient != null)
{
    gradientLabel.BackgroundColor = loadedGradient;
}
```

**VB.NET Example:**
```vb
Public Function LoadGradientFromXml(filePath As String) As BrushInfo
    Try
        If Not File.Exists(filePath) Then
            MessageBox.Show("Configuration file not found.", "Error")
            Return Nothing
        End If

        Dim serializer As New XmlSerializer(GetType(BrushInfo))
        Using fs As New FileStream(filePath, FileMode.Open)
            Dim brushInfo As BrushInfo = CType(serializer.Deserialize(fs), BrushInfo)
            Return brushInfo
        End Using
    Catch ex As Exception
        MessageBox.Show($"Error loading gradient: {ex.Message}", "Error")
        Return Nothing
    End Try
End Function

' Usage
Dim loadedGradient As BrushInfo = LoadGradientFromXml("gradient_config.xml")
If loadedGradient IsNot Nothing Then
    gradientLabel.BackgroundColor = loadedGradient
End If
```

---

## Complete Label Configuration Class

For full label persistence, create a serializable configuration class.

**C# Example:**
```csharp
using System;
using System.Drawing;
using System.Xml.Serialization;
using Syncfusion.Drawing;

[Serializable]
public class GradientLabelConfig
{
    public BrushInfo BackgroundColor { get; set; }
    public string Text { get; set; }
    
    // Serialize Color as ARGB int
    [XmlIgnore]
    public Color ForeColor { get; set; }
    
    [XmlElement("ForeColor")]
    public int ForeColorArgb
    {
        get { return ForeColor.ToArgb(); }
        set { ForeColor = Color.FromArgb(value); }
    }
    
    public string FontFamily { get; set; }
    public float FontSize { get; set; }
    public FontStyle FontStyle { get; set; }
    public ContentAlignment TextAlign { get; set; }
    public Border3DStyle BorderStyle { get; set; }
    public Border3DSide BorderSides { get; set; }
    
    [XmlIgnore]
    public Color BorderColor { get; set; }
    
    [XmlElement("BorderColor")]
    public int BorderColorArgb
    {
        get { return BorderColor.ToArgb(); }
        set { BorderColor = Color.FromArgb(value); }
    }
    
    public int Width { get; set; }
    public int Height { get; set; }

    // Default constructor for serialization
    public GradientLabelConfig()
    {
        // Set defaults
        Text = string.Empty;
        ForeColor = Color.Black;
        FontFamily = "Arial";
        FontSize = 10f;
        FontStyle = FontStyle.Regular;
        TextAlign = ContentAlignment.MiddleLeft;
        BorderStyle = Border3DStyle.Sunken;
        BorderSides = Border3DSide.All;
        BorderColor = Color.Gray;
        Width = 200;
        Height = 40;
    }
    
    // Apply configuration to label
    public void ApplyTo(GradientLabel label)
    {
        label.BackgroundColor = this.BackgroundColor;
        label.Text = this.Text;
        label.ForeColor = this.ForeColor;
        label.Font = new Font(this.FontFamily, this.FontSize, this.FontStyle);
        label.TextAlign = this.TextAlign;
        label.BorderStyle = this.BorderStyle;
        label.BorderSides = this.BorderSides;
        label.BorderColor = this.BorderColor;
        label.Size = new Size(this.Width, this.Height);
    }
    
    // Create configuration from label
    public static GradientLabelConfig FromLabel(GradientLabel label)
    {
        return new GradientLabelConfig
        {
            BackgroundColor = label.BackgroundColor,
            Text = label.Text,
            ForeColor = label.ForeColor,
            FontFamily = label.Font.FontFamily.Name,
            FontSize = label.Font.Size,
            FontStyle = label.Font.Style,
            TextAlign = label.TextAlign,
            BorderStyle = label.BorderStyle,
            BorderSides = label.BorderSides,
            BorderColor = label.BorderColor,
            Width = label.Width,
            Height = label.Height
        };
    }
}
```

---

## Save/Load Complete Configuration

**C# Example:**
```csharp
public class GradientLabelConfigManager
{
    public static void SaveConfig(GradientLabelConfig config, string filePath)
    {
        try
        {
            XmlSerializer serializer = new XmlSerializer(typeof(GradientLabelConfig));
            using (FileStream fs = new FileStream(filePath, FileMode.Create))
            {
                serializer.Serialize(fs, config);
            }
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error saving configuration: {ex.Message}", "Error");
        }
    }
    
    public static GradientLabelConfig LoadConfig(string filePath)
    {
        try
        {
            if (!File.Exists(filePath))
                return null;
                
            XmlSerializer serializer = new XmlSerializer(typeof(GradientLabelConfig));
            using (FileStream fs = new FileStream(filePath, FileMode.Open))
            {
                return (GradientLabelConfig)serializer.Deserialize(fs);
            }
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error loading configuration: {ex.Message}", "Error");
            return null;
        }
    }
}

// Usage: Save label configuration
GradientLabelConfig config = GradientLabelConfig.FromLabel(gradientLabel1);
GradientLabelConfigManager.SaveConfig(config, "label_theme.xml");

// Usage: Load and apply configuration
GradientLabelConfig loadedConfig = GradientLabelConfigManager.LoadConfig("label_theme.xml");
if (loadedConfig != null)
{
    loadedConfig.ApplyTo(gradientLabel1);
}
```

---

## Multi-Label Configuration

Save configurations for multiple labels.

**C# Example:**
```csharp
[Serializable]
public class LabelTheme
{
    public string ThemeName { get; set; }
    public List<GradientLabelConfig> Labels { get; set; }
    
    public LabelTheme()
    {
        Labels = new List<GradientLabelConfig>();
    }
}

// Save multiple labels
LabelTheme theme = new LabelTheme
{
    ThemeName = "Blue Corporate"
};

theme.Labels.Add(GradientLabelConfig.FromLabel(headerLabel));
theme.Labels.Add(GradientLabelConfig.FromLabel(statusLabel));
theme.Labels.Add(GradientLabelConfig.FromLabel(footerLabel));

XmlSerializer serializer = new XmlSerializer(typeof(LabelTheme));
using (FileStream fs = new FileStream("theme_corporate.xml", FileMode.Create))
{
    serializer.Serialize(fs, theme);
}

// Load and apply theme
using (FileStream fs = new FileStream("theme_corporate.xml", FileMode.Open))
{
    LabelTheme loadedTheme = (LabelTheme)serializer.Deserialize(fs);
    
    if (loadedTheme.Labels.Count >= 3)
    {
        loadedTheme.Labels[0].ApplyTo(headerLabel);
        loadedTheme.Labels[1].ApplyTo(statusLabel);
        loadedTheme.Labels[2].ApplyTo(footerLabel);
    }
}
```

---

## User Settings Persistence

**C# Example:**
```csharp
public class UserSettings
{
    private static string SettingsPath = 
        Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "YourApp",
            "label_settings.xml"
        );
    
    public static void SaveUserTheme(GradientLabelConfig config)
    {
        // Ensure directory exists
        Directory.CreateDirectory(Path.GetDirectoryName(SettingsPath));
        
        GradientLabelConfigManager.SaveConfig(config, SettingsPath);
    }
    
    public static GradientLabelConfig LoadUserTheme()
    {
        if (File.Exists(SettingsPath))
        {
            return GradientLabelConfigManager.LoadConfig(SettingsPath);
        }
        
        // Return default if no saved settings
        return new GradientLabelConfig();
    }
}

// Form Load: Restore user settings
private void Form1_Load(object sender, EventArgs e)
{
    GradientLabelConfig userConfig = UserSettings.LoadUserTheme();
    if (userConfig != null)
    {
        userConfig.ApplyTo(gradientLabel1);
    }
}

// Form Closing: Save user settings
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    GradientLabelConfig config = GradientLabelConfig.FromLabel(gradientLabel1);
    UserSettings.SaveUserTheme(config);
}
```

---

## Example XML Output

After saving a BrushInfo configuration:

```xml
<?xml version="1.0" encoding="utf-8"?>
<BrushInfo xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
           xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Style>Gradient</Style>
  <GradientStyle>Horizontal</GradientStyle>
  <BackColor>
    <R>0</R>
    <G>0</G>
    <B>255</B>
    <A>255</A>
  </BackColor>
  <ForeColor>
    <R>173</R>
    <G>216</G>
    <B>230</B>
    <A>255</A>
  </ForeColor>
  <GradientColors>
    <Color>
      <R>0</R>
      <G>0</G>
      <B>255</B>
      <A>255</A>
    </Color>
    <Color>
      <R>173</R>
      <G>216</G>
      <B>230</B>
      <A>255</A>
    </Color>
  </GradientColors>
</BrushInfo>
```

---

## Complete Application Example

**C# Form with Save/Load:**

```csharp
public partial class GradientLabelDesigner : Form
{
    private GradientLabel previewLabel;
    
    public GradientLabelDesigner()
    {
        InitializeComponent();
        InitializePreviewLabel();
    }
    
    private void InitializePreviewLabel()
    {
        previewLabel = new GradientLabel
        {
            Size = new Size(300, 60),
            Location = new Point(50, 50),
            Text = "Preview Label",
            Font = new Font("Arial", 14, FontStyle.Bold),
            ForeColor = Color.White,
            TextAlign = ContentAlignment.MiddleCenter
        };
        
        previewLabel.BackgroundColor = new BrushInfo(
            GradientStyle.Horizontal,
            Color.Blue,
            Color.LightBlue
        );
        
        this.Controls.Add(previewLabel);
    }
    
    private void btnSave_Click(object sender, EventArgs e)
    {
        SaveFileDialog saveDialog = new SaveFileDialog
        {
            Filter = "XML Files (*.xml)|*.xml",
            DefaultExt = "xml",
            FileName = "label_config.xml"
        };
        
        if (saveDialog.ShowDialog() == DialogResult.OK)
        {
            GradientLabelConfig config = GradientLabelConfig.FromLabel(previewLabel);
            GradientLabelConfigManager.SaveConfig(config, saveDialog.FileName);
            MessageBox.Show("Configuration saved successfully!", "Success");
        }
    }
    
    private void btnLoad_Click(object sender, EventArgs e)
    {
        OpenFileDialog openDialog = new OpenFileDialog
        {
            Filter = "XML Files (*.xml)|*.xml",
            DefaultExt = "xml"
        };
        
        if (openDialog.ShowDialog() == DialogResult.OK)
        {
            GradientLabelConfig config = GradientLabelConfigManager.LoadConfig(openDialog.FileName);
            if (config != null)
            {
                config.ApplyTo(previewLabel);
                MessageBox.Show("Configuration loaded successfully!", "Success");
            }
        }
    }
}
```

---

## Best Practices

### 1. Use Try-Catch for File Operations

```csharp
try
{
    // Serialization code
}
catch (IOException ex)
{
    MessageBox.Show($"File error: {ex.Message}");
}
catch (UnauthorizedAccessException ex)
{
    MessageBox.Show($"Access denied: {ex.Message}");
}
catch (Exception ex)
{
    MessageBox.Show($"Unexpected error: {ex.Message}");
}
```

### 2. Validate Before Applying

```csharp
public static bool ValidateConfig(GradientLabelConfig config)
{
    if (config == null) return false;
    if (config.Width <= 0 || config.Height <= 0) return false;
    if (config.FontSize <= 0) return false;
    if (string.IsNullOrEmpty(config.FontFamily)) return false;
    
    return true;
}
```

### 3. Provide Default Fallback

```csharp
GradientLabelConfig config = LoadConfig(filePath);
if (config == null || !ValidateConfig(config))
{
    config = new GradientLabelConfig();  // Use defaults
}
config.ApplyTo(label);
```

### 4. Version Your Configuration

```csharp
[Serializable]
public class GradientLabelConfig
{
    public int Version { get; set; } = 1;
    // ... other properties
    
    public void Upgrade()
    {
        if (Version < 2)
        {
            // Apply version 2 changes
            Version = 2;
        }
    }
}
```

---

## Troubleshooting

### Serialization Fails

**Common Causes:**
1. Properties not public
2. Missing default constructor
3. Non-serializable property types
4. File access permissions

**Solution:**
```csharp
// Ensure public properties
public BrushInfo BackgroundColor { get; set; }

// Provide default constructor
public GradientLabelConfig() { }

// Mark non-serializable properties
[XmlIgnore]
public EventHandler CustomEvent { get; set; }
```

### Color Serialization Issues

**Problem:** Color type doesn't serialize directly.

**Solution:** Use ARGB integer conversion.

```csharp
[XmlIgnore]
public Color ForeColor { get; set; }

[XmlElement("ForeColor")]
public int ForeColorArgb
{
    get { return ForeColor.ToArgb(); }
    set { ForeColor = Color.FromArgb(value); }
}
```

### File Not Found After Save

**Check:**
```csharp
string fullPath = Path.GetFullPath(fileName);
MessageBox.Show($"Saved to: {fullPath}");
```

---

## Related Topics

- **Background Styling**: Configure gradients → [background-styling.md](background-styling.md)
- **Getting Started**: Basic setup → [getting-started.md](getting-started.md)
- **Border Configuration**: Border settings → [border-configuration.md](border-configuration.md)
