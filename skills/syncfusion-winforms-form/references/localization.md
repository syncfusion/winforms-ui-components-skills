# Localization

This guide covers localizing SfForm controls for multi-language support, allowing your Windows Forms application to display text in different languages based on user culture.

## Table of Contents
- [Overview](#overview)
- [Localize at Sample Level](#localize-at-sample-level)
- [Editing Default Resource File](#editing-default-resource-file)
- [Localize with Different Assembly or Namespace](#localize-with-different-assembly-or-namespace)

## Overview

Localization is the process of translating application resources into different languages for specific cultures. SfForm can be localized by adding resource (`.resx`) files that contain culture-specific text strings.

### How Localization Works

1. Create resource files for each supported culture
2. Add Name/Value pairs for translatable strings
3. Set `CurrentUICulture` before component initialization
4. SfForm automatically loads the appropriate resource file

### Culture Codes

Common culture codes follow the `language-COUNTRY` format:
- `en-US` - English (United States)
- `de-DE` - German (Germany)
- `fr-FR` - French (France)
- `es-ES` - Spanish (Spain)
- `ja-JP` - Japanese (Japan)
- `zh-CN` - Chinese (China)
- `pt-BR` - Portuguese (Brazil)
- `it-IT` - Italian (Italy)

### Default Resource File

Syncfusion provides a default resource file: `Syncfusion.Shared.Base.resx`  
Download from: [GitHub - Syncfusion WinForms Localization](https://github.com/syncfusion/winforms-controls-localization-resx-files/blob/master/Syncfusion.Shared.Base/Syncfusion.Shared.Base.resx)

## Localize at Sample Level

Follow these steps to localize SfForm at the application level:

### Step 1: Create Resources Folder

Create a new folder named `Resources` in your project:

```
MyProject/
├── Resources/              ← Create this folder
│   ├── Syncfusion.Shared.Base.resx (default)
│   ├── Syncfusion.Shared.Base.de-DE.resx (German)
│   └── Syncfusion.Shared.Base.fr-FR.resx (French)
├── Form1.cs
├── Program.cs
└── ...
```

### Step 2: Add Default Resource File

Download and add `Syncfusion.Shared.Base.resx` to the Resources folder:

**Download Link:**  
https://github.com/syncfusion/winforms-controls-localization-resx-files/blob/master/Syncfusion.Shared.Base/Syncfusion.Shared.Base.resx

**In Visual Studio:**
1. Right-click on `Resources` folder
2. Select **Add → Existing Item**
3. Browse to downloaded `Syncfusion.Shared.Base.resx`
4. Click **Add**

### Step 3: Create Culture-Specific Resource File

Create a new resource file for your target culture:

1. Right-click on `Resources` folder
2. Select **Add → New Item**
3. Select **Resource File**
4. Name it: `Syncfusion.Shared.Base.<culture-name>.resx`
   - For German: `Syncfusion.Shared.Base.de-DE.resx`
   - For French: `Syncfusion.Shared.Base.fr-FR.resx`
   - For Spanish: `Syncfusion.Shared.Base.es-ES.resx`
5. Click **Add**

### Step 4: Add Translated Strings

Open the culture-specific resource file in Visual Studio's Resource Designer:

1. Double-click `Syncfusion.Shared.Base.de-DE.resx` (or your culture file)
2. Add Name/Value pairs with translations:

| Name | Value (English) | Value (German) |
|------|----------------|----------------|
| FormText | Untitled | Unbenannt |
| CloseButton | Close | Schließen |
| MinimizeButton | Minimize | Minimieren |
| MaximizeButton | Maximize | Maximieren |
| RestoreButton | Restore | Wiederherstellen |

**Example Resource File Content:**
```
Name: FormText
Value: Unbenannt

Name: CloseButton  
Value: Schließen

Name: MinimizeButton
Value: Minimieren

Name: MaximizeButton
Value: Maximieren
```

### Step 5: Set CurrentUICulture

Set the culture **before** calling `InitializeComponent()`:

**C#:**
```csharp
using System.Globalization;
using System.Threading;

public partial class Form1 : SfForm
{
    public Form1()
    {
        // Set German culture
        Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
        
        // Initialize component (loads resources)
        InitializeComponent();
    }
}
```

**VB.NET:**
```vb
Imports System.Globalization
Imports System.Threading

Public Class Form1
    Inherits SfForm
    
    Public Sub New()
        ' Set German culture
        Thread.CurrentThread.CurrentCulture = New CultureInfo("de-DE")
        Thread.CurrentThread.CurrentUICulture = New CultureInfo("de-DE")
        
        ' Initialize component (loads resources)
        InitializeComponent()
    End Sub
End Class
```

### Complete Example: German Localization

**Resources/Syncfusion.Shared.Base.de-DE.resx:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="FormText">
    <value>Unbenannt</value>
  </data>
  <data name="CloseButton">
    <value>Schließen</value>
  </data>
  <data name="MinimizeButton">
    <value>Minimieren</value>
  </data>
  <data name="MaximizeButton">
    <value>Maximieren</value>
  </data>
  <data name="RestoreButton">
    <value>Wiederherstellen</value>
  </data>
</root>
```

**Form1.cs:**
```csharp
using System;
using System.Globalization;
using System.Threading;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace LocalizedApp
{
    public partial class Form1 : SfForm
    {
        public Form1()
        {
            // Set German culture
            Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");
            Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
            
            InitializeComponent();
            
            this.Text = "Lokalisierte Anwendung";
            this.Size = new Size(800, 600);
        }
    }
}
```

## Editing Default Resource File

You can modify the default English resource file to customize text even for the default language.

### Step 1: Add Default Resource File

Download `Syncfusion.Shared.Base.resx` and add it to your `Resources` folder:  
https://github.com/syncfusion/winforms-controls-localization-resx-files/blob/master/Syncfusion.Shared.Base/Syncfusion.Shared.Base.resx

### Step 2: Modify Values

Open `Syncfusion.Shared.Base.resx` in Resource Designer and change values:

**Before:**
```
Name: FormText
Value: Untitled
```

**After:**
```
Name: FormText
Value: New Document
```

### Step 3: Use Modified Defaults

The modified values will be used automatically without setting culture:

**C#:**
```csharp
public Form1()
{
    // No culture setting needed for default
    InitializeComponent();
    
    // Form will use "New Document" instead of "Untitled"
}
```

### Example: Custom English Text

**Resources/Syncfusion.Shared.Base.resx:**
```
Name: FormText
Value: My Custom Application

Name: CloseButton
Value: Exit

Name: MinimizeButton
Value: Collapse

Name: MaximizeButton
Value: Expand
```

This allows you to customize the default English text without creating a culture-specific file.

## Localize with Different Assembly or Namespace

If resource files are in a different assembly or namespace, use `SetResources` to specify the location.

### Scenario

Your project structure:
```
Solution/
├── MyApp (main project)
│   └── Form1.cs
└── MyApp.Resources (separate project)
    └── Resources/
        ├── Syncfusion.Shared.Base.resx
        └── Syncfusion.Shared.Base.de-DE.resx
```

### Using SetResources

**C#:**
```csharp
using System.Globalization;
using System.Threading;
using Syncfusion.WinForms.Core.Localization;

public Form1()
{
    // Set culture
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
    
    // Specify assembly and namespace containing resources
    LocalizationResourceBase.SetResources(
        typeof(MyApp.Resources.ResourceClass).Assembly,
        "MyApp.Resources"
    );
    
    InitializeComponent();
}
```

**VB.NET:**
```vb
Imports System.Globalization
Imports System.Threading
Imports Syncfusion.WinForms.Core.Localization

Public Sub New()
    ' Set culture
    Thread.CurrentThread.CurrentUICulture = New CultureInfo("de-DE")
    
    ' Specify assembly and namespace containing resources
    LocalizationResourceBase.SetResources(
        GetType(MyApp.Resources.ResourceClass).Assembly,
        "MyApp.Resources"
    )
    
    InitializeComponent()
End Sub
```

### Parameters

- **Assembly:** The assembly containing resource files
- **Namespace:** The namespace where resources are located

### Complete Example

**Separate Resources Project:**

Create a class library project: `MyApp.Localization`

**MyApp.Localization/ResourceAnchor.cs:**
```csharp
namespace MyApp.Localization
{
    // Empty class to reference assembly
    public class ResourceAnchor
    {
    }
}
```

**MyApp.Localization/Resources/Syncfusion.Shared.Base.de-DE.resx:**  
(Add resource file with German translations)

**MainApp/Form1.cs:**
```csharp
using System;
using System.Globalization;
using System.Threading;
using Syncfusion.WinForms.Controls;
using Syncfusion.WinForms.Core.Localization;
using MyApp.Localization;

public class Form1 : SfForm
{
    public Form1()
    {
        // Set German culture
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
        
        // Point to resources in separate assembly
        LocalizationResourceBase.SetResources(
            typeof(ResourceAnchor).Assembly,
            "MyApp.Localization.Resources"
        );
        
        InitializeComponent();
    }
}
```

## Advanced Localization Scenarios

### Dynamic Culture Switching

Allow users to change language at runtime:

**C#:**
```csharp
public class MainForm : SfForm
{
    private ComboBox languageSelector;
    
    public MainForm()
    {
        InitializeComponent();
        
        // Create language selector
        languageSelector = new ComboBox();
        languageSelector.Items.AddRange(new string[] { "English", "German", "French", "Spanish" });
        languageSelector.SelectedIndex = 0;
        languageSelector.SelectedIndexChanged += LanguageSelector_SelectedIndexChanged;
        
        this.Controls.Add(languageSelector);
    }
    
    private void LanguageSelector_SelectedIndexChanged(object sender, EventArgs e)
    {
        string culture = languageSelector.SelectedItem.ToString() switch
        {
            "German" => "de-DE",
            "French" => "fr-FR",
            "Spanish" => "es-ES",
            _ => "en-US"
        };
        
        SwitchLanguage(culture);
    }
    
    private void SwitchLanguage(string cultureName)
    {
        // Set new culture
        CultureInfo culture = new CultureInfo(cultureName);
        Thread.CurrentThread.CurrentCulture = culture;
        Thread.CurrentThread.CurrentUICulture = culture;
        
        // Recreate form to apply new language
        Form newForm = new MainForm();
        newForm.Show();
        this.Close();
    }
}
```

### User Preference Storage

Save user's language preference:

**C#:**
```csharp
// Save preference
private void SaveLanguagePreference(string cultureName)
{
    Properties.Settings.Default.PreferredLanguage = cultureName;
    Properties.Settings.Default.Save();
}

// Load preference
private void LoadLanguagePreference()
{
    string savedLanguage = Properties.Settings.Default.PreferredLanguage;
    if (!string.IsNullOrEmpty(savedLanguage))
    {
        CultureInfo culture = new CultureInfo(savedLanguage);
        Thread.CurrentThread.CurrentCulture = culture;
        Thread.CurrentThread.CurrentUICulture = culture;
    }
}

// In constructor
public Form1()
{
    LoadLanguagePreference();
    InitializeComponent();
}
```

### System Language Detection

Automatically use system language:

**C#:**
```csharp
public Form1()
{
    // Get system culture
    CultureInfo systemCulture = CultureInfo.CurrentCulture;
    
    // Check if we have resources for this culture
    if (IsCultureSupported(systemCulture.Name))
    {
        Thread.CurrentThread.CurrentUICulture = systemCulture;
    }
    else
    {
        // Fall back to English
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("en-US");
    }
    
    InitializeComponent();
}

private bool IsCultureSupported(string cultureName)
{
    string[] supportedCultures = { "en-US", "de-DE", "fr-FR", "es-ES" };
    return Array.Exists(supportedCultures, c => c == cultureName);
}
```

### Fallback Culture

Handle missing translations gracefully:

**C#:**
```csharp
public Form1()
{
    try
    {
        // Try to set requested culture
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
    }
    catch (CultureNotFoundException)
    {
        // Fall back to English if culture not supported
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("en-US");
    }
    
    InitializeComponent();
}
```

## Best Practices

### 1. Organize Resource Files

Keep all resource files in a dedicated `Resources` folder with clear naming:
```
Resources/
├── Syncfusion.Shared.Base.resx (default/English)
├── Syncfusion.Shared.Base.de-DE.resx (German)
├── Syncfusion.Shared.Base.fr-FR.resx (French)
└── Syncfusion.Shared.Base.es-ES.resx (Spanish)
```

### 2. Always Provide Default

Always include the default `Syncfusion.Shared.Base.resx` as fallback.

### 3. Set Culture Early

Set `CurrentUICulture` **before** `InitializeComponent()`:

```csharp
public Form1()
{
    // CORRECT: Set culture first
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
    InitializeComponent();  // Now loads German resources
}
```

### 4. Test All Cultures

Test your application with each supported culture to ensure:
- All text is translated
- UI layouts accommodate longer translations
- Date/number formats are correct
- No text is cut off

### 5. Handle Missing Translations

Use try-catch to handle cultures without resources:

```csharp
try
{
    Thread.CurrentThread.CurrentUICulture = new CultureInfo(requestedCulture);
}
catch
{
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("en-US");
}
```

### 6. Document Supported Languages

Clearly document which languages your application supports.

### 7. Consider Text Expansion

Some languages require more space (German text is often 30% longer than English):
- Design UI with flexible layouts
- Test with longest language
- Use appropriate control sizes

### 8. Localize Custom Text

Remember to localize your own text, not just SfForm resources:

```csharp
// Create your own resource files
string welcomeMessage = MyResources.WelcomeMessage;
string saveButton = MyResources.SaveButton;
```

## Troubleshooting

### Resources Not Loading
- Verify resource file name matches exactly: `Syncfusion.Shared.Base.<culture>.resx`
- Ensure resource file Build Action is set to "Embedded Resource"
- Check culture code is correct (e.g., `de-DE`, not `de`)
- Confirm `CurrentUICulture` is set before `InitializeComponent()`

### Wrong Language Displayed
- Check `CurrentUICulture`, not `CurrentCulture`
- Verify resource file exists for the culture
- Ensure resource file is included in project
- Rebuild solution after adding resource files

### Partial Localization
- Some strings may not have translations in resource file
- Add missing Name/Value pairs to culture-specific resource
- Check default resource file for complete list of strings

### Assembly Not Found
- Verify `LocalizationResourceBase.SetResources()` parameters
- Ensure resource assembly is referenced in project
- Check namespace matches resource file location

## Complete Localization Example

**Program.cs:**
```csharp
using System;
using System.Globalization;
using System.Threading;
using System.Windows.Forms;

namespace LocalizedApp
{
    static class Program
    {
        [STAThread]
        static void Main()
        {
            // Detect or set culture
            string savedCulture = Properties.Settings.Default.Culture;
            if (!string.IsNullOrEmpty(savedCulture))
            {
                Thread.CurrentThread.CurrentUICulture = new CultureInfo(savedCulture);
            }
            
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
}
```

**MainForm.cs:**
```csharp
using System;
using System.Globalization;
using System.Threading;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace LocalizedApp
{
    public class MainForm : SfForm
    {
        public MainForm()
        {
            InitializeComponent();
            
            this.Text = "Localized Application";
            this.Size = new Size(900, 600);
            
            CreateLanguageMenu();
        }
        
        private void CreateLanguageMenu()
        {
            MenuStrip menu = new MenuStrip();
            ToolStripMenuItem langMenu = new ToolStripMenuItem("Language");
            
            langMenu.DropDownItems.Add("English", null, (s, e) => ChangeLanguage("en-US"));
            langMenu.DropDownItems.Add("Deutsch", null, (s, e) => ChangeLanguage("de-DE"));
            langMenu.DropDownItems.Add("Français", null, (s, e) => ChangeLanguage("fr-FR"));
            langMenu.DropDownItems.Add("Español", null, (s, e) => ChangeLanguage("es-ES"));
            
            menu.Items.Add(langMenu);
            this.MainMenuStrip = menu;
            this.Controls.Add(menu);
        }
        
        private void ChangeLanguage(string cultureName)
        {
            // Save preference
            Properties.Settings.Default.Culture = cultureName;
            Properties.Settings.Default.Save();
            
            // Restart application to apply
            MessageBox.Show("Please restart the application to apply language change.");
        }
    }
}
```

This completes the localization guide for SfForm. With proper resource files and culture settings, your application can support multiple languages seamlessly.
