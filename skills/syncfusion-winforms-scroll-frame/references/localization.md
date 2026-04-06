# Localization Support

## Table of Contents
- [Overview](#overview)
- [How Localization Works](#how-localization-works)
- [Step-by-Step Implementation](#step-by-step-implementation)
  - [Step 1: Create Resources Folder](#step-1-create-resources-folder)
  - [Step 2: Add Default Resource File](#step-2-add-default-resource-file)
  - [Step 3: Create Culture-Specific Resource File](#step-3-create-culture-specific-resource-file)
  - [Step 4: Add Localized Strings](#step-4-add-localized-strings)
  - [Step 5: Set Application Culture](#step-5-set-application-culture)
- [Editing Default Culture Resources](#editing-default-culture-resources)
- [Localizing from Different Assembly](#localizing-from-different-assembly)
- [Complete Example](#complete-example)

## Overview

Localization allows you to translate the SfScrollFrame context menu strings into different languages for specific cultures. The scrollbar context menu displays localized strings automatically when appropriate resource files are present.

### Localizable Strings

The following context menu items can be localized:

**Vertical Scrollbar:**
- Scroll Here
- Top
- Bottom
- Page Up
- Page Down
- Scroll Up
- Scroll Down

**Horizontal Scrollbar:**
- Scroll Here
- Left Edge
- Right Edge
- Page Left
- Page Right
- Scroll Left
- Scroll Right

## How Localization Works

1. **Resource Files (.resx):** Store localized strings in resource files
2. **Culture Naming:** Files are named `Syncfusion.Core.WinForms.<culture>.resx` (e.g., `Syncfusion.Core.WinForms.de-DE.resx` for German)
3. **CurrentUICulture:** Set the application's `CurrentUICulture` before controls are created
4. **Automatic Loading:** SfScrollFrame automatically loads strings from matching culture resource file

### Culture Name Format

Culture names follow the pattern: `<language>-<region>`

Examples:
- `de-DE` - German (Germany)
- `fr-FR` - French (France)
- `es-ES` - Spanish (Spain)
- `ja-JP` - Japanese (Japan)
- `zh-CN` - Chinese (Simplified, China)
- `pt-BR` - Portuguese (Brazil)

## Step-by-Step Implementation

### Step 1: Create Resources Folder

Create a folder named `Resources` in your application project:

```
MyApplication/
├── Properties/
├── Resources/          ← Create this folder
├── Form1.cs
├── Program.cs
└── MyApplication.csproj
```

**In Visual Studio:**
1. Right-click project in Solution Explorer
2. Select Add → New Folder
3. Name it "Resources"

### Step 2: Add Default Resource File

Download and add the default resource file to the Resources folder.

**Default Resource File:** `Syncfusion.Core.WinForms.resx`

**Download Location:** [Syncfusion.Core.WinForms.resx](https://www.syncfusion.com/downloads/support/directtrac/general/ze/Syncfusion.Core.WinForms-1127975576)

**Add to Project:**
1. Download `Syncfusion.Core.WinForms.resx`
2. Copy file to `Resources/` folder
3. Right-click `Resources` folder in Solution Explorer
4. Select Add → Existing Item
5. Browse to and select `Syncfusion.Core.WinForms.resx`
6. Click Add

```
MyApplication/
├── Resources/
│   └── Syncfusion.Core.WinForms.resx    ← Default resource file
```

### Step 3: Create Culture-Specific Resource File

Create a new resource file for your target culture.

**Naming Convention:** `Syncfusion.Core.WinForms.<culture-name>.resx`

**Example for German (Germany):** `Syncfusion.Core.WinForms.de-DE.resx`

**Steps:**
1. Right-click `Resources` folder
2. Select Add → New Item
3. In the Add New Item dialog:
   - Select "Resources File"
   - Name: `Syncfusion.Core.WinForms.de-DE.resx` (for German)
   - Click Add

```
MyApplication/
├── Resources/
│   ├── Syncfusion.Core.WinForms.resx         ← Default (English)
│   └── Syncfusion.Core.WinForms.de-DE.resx   ← German localization
```

**For Multiple Cultures:**
```
MyApplication/
├── Resources/
│   ├── Syncfusion.Core.WinForms.resx           ← Default
│   ├── Syncfusion.Core.WinForms.de-DE.resx     ← German
│   ├── Syncfusion.Core.WinForms.fr-FR.resx     ← French
│   ├── Syncfusion.Core.WinForms.es-ES.resx     ← Spanish
│   └── Syncfusion.Core.WinForms.ja-JP.resx     ← Japanese
```

### Step 4: Add Localized Strings

Open the culture-specific resource file and add name/value pairs for each localized string.

**Open Resource Designer:**
1. Double-click `Syncfusion.Core.WinForms.de-DE.resx` in Solution Explorer
2. The Resource Designer opens

**Add Translations:**

| Name (from default .resx) | Value (German translation) |
|---------------------------|----------------------------|
| ScrollHere | Hier scrollen |
| Top | Nach oben |
| Bottom | Nach unten |
| PageUp | Seite nach oben |
| PageDown | Seite nach unten |
| ScrollUp | Bildlauf nach oben |
| ScrollDown | Bildlauf nach unten |
| LeftEdge | Linke Kante |
| RightEdge | Rechte Kante |
| PageLeft | Seite nach links |
| PageRight | Seite nach rechts |
| ScrollLeft | Nach links scrollen |
| ScrollRight | Nach rechts scrollen |

**Resource Designer Screenshot Reference:**
- Name column: English keys from default resource file
- Value column: Translated strings in target language
- Comment column: Optional description

**Example Translations:**

**German (de-DE):**
```
ScrollHere → Hier scrollen
Top → Nach oben
Bottom → Nach unten
```

**French (fr-FR):**
```
ScrollHere → Défiler ici
Top → Haut
Bottom → Bas
```

**Spanish (es-ES):**
```
ScrollHere → Desplazarse aquí
Top → Arriba
Bottom → Abajo
```

### Step 5: Set Application Culture

Set the `CurrentUICulture` before creating any controls.

**In Form Constructor (Before InitializeComponent):**

```csharp
using System.Globalization;
using System.Threading;

public partial class MainForm : Form
{
    public MainForm()
    {
        // Set culture BEFORE InitializeComponent
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
        
        InitializeComponent();
        
        // Continue with form setup
        SetupControls();
    }
    
    private void SetupControls()
    {
        // Create and configure SfScrollFrame
        SfScrollFrame sfScrollFrame1 = new SfScrollFrame();
        sfScrollFrame1.Control = listView1;
        
        // Context menu will automatically use German strings
    }
}
```

**In Program.cs (Application-Wide):**

```csharp
using System;
using System.Globalization;
using System.Threading;
using System.Windows.Forms;

static class Program
{
    [STAThread]
    static void Main()
    {
        // Set culture for entire application
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
        
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new MainForm());
    }
}
```

**Dynamic Culture Selection:**

```csharp
// Allow user to select language at runtime
private void SetCulture(string cultureName)
{
    // Set current thread culture
    Thread.CurrentThread.CurrentUICulture = new CultureInfo(cultureName);
    
    // Recreate controls to apply new culture
    this.Controls.Clear();
    InitializeComponent();
}

// Usage
private void germanMenuItem_Click(object sender, EventArgs e)
{
    SetCulture("de-DE");
}

private void frenchMenuItem_Click(object sender, EventArgs e)
{
    SetCulture("fr-FR");
}

private void englishMenuItem_Click(object sender, EventArgs e)
{
    SetCulture("en-US");
}
```

## Editing Default Culture Resources

You can modify the default (English) strings by editing the `Syncfusion.Core.WinForms.resx` file in your Resources folder.

### Steps

1. **Add Default Resource File:**
   - Download `Syncfusion.Core.WinForms.resx` from [Syncfusion](https://www.syncfusion.com/downloads/support/directtrac/general/ze/Syncfusion.Core.WinForms-1127975576)
   - Add to your `Resources/` folder

2. **Open in Resource Designer:**
   - Double-click `Syncfusion.Core.WinForms.resx` in Solution Explorer

3. **Edit Values:**
   - Modify the Value column for any string
   - Example: Change "Top" to "Go to Top"

4. **Save Changes:**
   - Save the file
   - Rebuild application

### Example: Custom English Strings

```
Original:
Name: Top
Value: Top

Modified:
Name: Top
Value: Jump to Top
```

**Result:** Context menu will show "Jump to Top" instead of "Top" for all English users.

## Localizing from Different Assembly

If your resource files are in a different assembly or namespace, use the `SetResources` method.

### Scenario

Resource files are in:
- Different assembly: `MyResourceAssembly.dll`
- Different namespace: `MyCompany.MyApp.Resources`

### Implementation

```csharp
using System.Globalization;
using System.Threading;
using Syncfusion.WinForms.Controls;

public partial class MainForm : Form
{
    public MainForm()
    {
        // Set culture
        Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
        
        // Specify assembly and namespace containing resource files
        Syncfusion.WinForms.Controls.Localization.LocalizationResourceBase.SetResources(
            typeof(MyResourceClass).Assembly,   // Assembly containing resources
            "MyCompany.MyApp.Resources"         // Namespace of resources
        );
        
        InitializeComponent();
    }
}
```

### Parameters

```csharp
LocalizationResourceBase.SetResources(Assembly assembly, string nameSpace);
```

- `assembly`: Assembly object containing resource files
- `nameSpace`: Namespace where resource files are located

### Example with Explicit Assembly

```csharp
using System.Reflection;
using Syncfusion.WinForms.Controls;

// Load assembly
Assembly resourceAssembly = Assembly.LoadFrom("MyResourceAssembly.dll");

// Set resources
Syncfusion.WinForms.Controls.Localization.LocalizationResourceBase.SetResources(
    resourceAssembly,
    "MyCompany.Resources"
);
```

## Complete Example

Here's a complete working example with culture selection:

```csharp
using System;
using System.Drawing;
using System.Globalization;
using System.Threading;
using System.Windows.Forms;
using Syncfusion.WinForms.Controls;

namespace SfScrollFrameLocalizationDemo
{
    // Program.cs
    static class Program
    {
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Application.Run(new MainForm());
        }
    }
    
    // MainForm.cs
    public partial class MainForm : Form
    {
        private SfScrollFrame sfScrollFrame1;
        private ListView listView1;
        private MenuStrip menuStrip1;

        public MainForm()
        {
            // Default culture (can be changed via menu)
            Thread.CurrentThread.CurrentUICulture = new CultureInfo("en-US");
            
            InitializeComponent();
            SetupMenu();
            SetupControls();
        }

        private void SetupMenu()
        {
            menuStrip1 = new MenuStrip();
            
            ToolStripMenuItem languageMenu = new ToolStripMenuItem("Language");
            languageMenu.DropDownItems.Add("English", null, (s, e) => ChangeCulture("en-US"));
            languageMenu.DropDownItems.Add("Deutsch (German)", null, (s, e) => ChangeCulture("de-DE"));
            languageMenu.DropDownItems.Add("Français (French)", null, (s, e) => ChangeCulture("fr-FR"));
            languageMenu.DropDownItems.Add("Español (Spanish)", null, (s, e) => ChangeCulture("es-ES"));
            
            menuStrip1.Items.Add(languageMenu);
            this.MainMenuStrip = menuStrip1;
            this.Controls.Add(menuStrip1);
        }

        private void SetupControls()
        {
            // Create ListView
            listView1 = new ListView();
            listView1.View = View.Details;
            listView1.Size = new Size(400, 350);
            listView1.Location = new Point(20, 50);
            listView1.Columns.Add("Column 1", 190);
            listView1.Columns.Add("Column 2", 190);
            
            // Add items
            for (int i = 0; i < 100; i++)
            {
                listView1.Items.Add(new ListViewItem(new[] { $"Item {i}", $"Value {i}" }));
            }
            
            // Attach SfScrollFrame
            sfScrollFrame1 = new SfScrollFrame();
            sfScrollFrame1.Control = listView1;
            
            // Add instruction label
            Label label = new Label();
            label.Text = "Right-click on scrollbar to see localized context menu";
            label.AutoSize = true;
            label.Location = new Point(20, 410);
            
            this.Controls.Add(listView1);
            this.Controls.Add(label);
        }

        private void ChangeCulture(string cultureName)
        {
            // Set new culture
            Thread.CurrentThread.CurrentUICulture = new CultureInfo(cultureName);
            
            // Recreate controls to apply new culture
            this.Controls.Clear();
            
            // Rebuild form
            SetupMenu();
            SetupControls();
            
            // Show confirmation
            MessageBox.Show(
                $"Language changed to: {Thread.CurrentThread.CurrentUICulture.DisplayName}\n" +
                $"Right-click scrollbar to see localized menu.",
                "Culture Changed",
                MessageBoxButtons.OK,
                MessageBoxIcon.Information
            );
        }
    }
}
```

### Resource File Structure for Example

```
MyApplication/
├── Resources/
│   ├── Syncfusion.Core.WinForms.resx           (English - default)
│   ├── Syncfusion.Core.WinForms.de-DE.resx     (German)
│   ├── Syncfusion.Core.WinForms.fr-FR.resx     (French)
│   └── Syncfusion.Core.WinForms.es-ES.resx     (Spanish)
```

## Troubleshooting

### Localized strings not showing

**Check:**
1. Resource file is named correctly: `Syncfusion.Core.WinForms.<culture>.resx`
2. Resource file is in `Resources/` folder in project
3. Resource file Build Action is set to "Embedded Resource"
4. `CurrentUICulture` is set before `InitializeComponent()`
5. Culture name matches resource file name exactly

### How to verify resource file properties

1. Select resource file in Solution Explorer
2. Press F4 to open Properties window
3. Verify:
   - **Build Action:** Embedded Resource
   - **Copy to Output Directory:** Do not copy

### Culture not recognized

**Common mistakes:**
- `de-de` (lowercase) → Should be `de-DE`
- `en` (language only) → Should be `en-US` or `en-GB`
- Typos in culture name

**Valid culture names:** Use `CultureInfo.GetCultures(CultureTypes.AllCultures)` to list all valid names.

### Default strings showing instead of localized

**Possible causes:**
1. Resource file has wrong name/value pairs
2. Culture set after controls are created
3. Resource file not marked as Embedded Resource
4. Namespace mismatch (if using `SetResources`)

## Best Practices

1. **Set culture early:** Always set `CurrentUICulture` before creating any controls
2. **Consistent naming:** Follow the exact naming convention for resource files
3. **Test all cultures:** Verify each localized resource file displays correctly
4. **Include all strings:** Ensure all menu items are translated
5. **Use native speakers:** Have translations reviewed by native speakers
6. **Fallback to default:** If culture-specific file is missing, default (English) strings are used
7. **Document cultures:** List supported cultures in your application documentation
8. **Build Action:** Always set resource files to "Embedded Resource"
