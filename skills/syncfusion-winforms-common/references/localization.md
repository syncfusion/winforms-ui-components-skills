# Localization of Syncfusion WinForms Controls

Localization makes an application multilingual by formatting content according to culture. Culture combines language and location — for example, `en-US` (English, US) or `de-DE` (German, Germany).

Syncfusion WinForms controls have built-in neutral (English) resources and support three localization approaches:

1. **ILocalizationProvider** — programmatic per-string override
2. **Satellite Assemblies** — compiled resource DLLs per culture
3. **.resx files** — resource files added to the application project

---

## 1. Using ILocalizationProvider

Use this approach when you want to provide custom string translations without compiling satellite assemblies.

**Steps:**

1. Add the required namespaces:

```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Edit;
```

```vb
Imports Syncfusion.Windows.Forms
Imports Syncfusion.Windows.Forms.Edit
```

2. Create a class that implements `ILocalizationProvider` (defined in `Syncfusion.Shared.Base.dll`):

```csharp
using Syncfusion.Windows.Forms.Localization.Localizer.EditResourceIdentifiers;
using Syncfusion.Windows.Forms.ResourceIdentifiers;

public class Localizer : ILocalizationProvider
{
    public string GetLocalizedString(
        System.Globalization.CultureInfo culture,
        string name,
        object obj)
    {
        switch (name)
        {
            case Localizer.EditResourceIdentifiers.FDbtnClose:
                return "schließen";
            case Localizer.EditResourceIdentifiers.FDbtnFind:
                return "finden";
            case Localizer.EditResourceIdentifiers.FDTitle:
                return "Kommentar";
            // Add more identifiers as needed
            default:
                return string.Empty; // Falls back to default English string
        }
    }
}
```

3. Register the provider **before** `InitializeComponent()`:

```csharp
LocalizationProvider.Provider = new Localizer();
```

```vb
LocalizationProvider.Provider = New Localizer()
```

> String identifiers are defined in the `ResourceIdentifiers` class (`Syncfusion.Shared.Base`) and control-specific identifier classes (e.g., `EditResourceIdentifiers` in `Syncfusion.Edit.Windows`).

---

## 2. Using Satellite Assemblies

Satellite assemblies are compiled resource DLLs for a specific culture. Place them in a culture-named subdirectory next to your executable.

**Steps (example: German `de-DE`):**

1. Locate neutral resource files from the Syncfusion installation:
   - Tools.Windows: `C:\Program Files\Syncfusion\Essential Studio\{version}\Windows\Tools.Windows\Localization\`
   - Shared.Base: `C:\Program Files\Syncfusion\Essential Studio\{version}\Base\Shared.Base\Localization\`

2. Open resource files using the **Resource Editor** (ResEditor) or **WinRes** (for Windows Forms resources).

3. Replace English strings with the target culture's equivalents and save with the culture suffix:
   - `Syncfusion.Windows.Forms.Tools.XPMenus.CustomizationPanel.de-DE.resources`

4. Compile the satellite assembly using the `al` (Assembly Linker) tool in the Visual Studio command prompt:

```console
al /t:lib /culture:de-DE /out:Syncfusion.Tools.Windows.resources.dll /v:19.1.0.54 ^
   /delay+ /keyf:sf.publicsnk ^
   /embed:Syncfusion.Windows.Forms.Tools.XPMenus.CustomizationPanel.de-DE.resources ^
   /embed:Syncfusion.Windows.Forms.Tools.SR.de-DE.resources
```

5. Skip strong-name verification for the satellite assembly:

```console
sn -Vr Syncfusion.Tools.Windows.resources.dll
```

6. Place the compiled DLL in a `de-DE` subfolder under your application's bin directory:
   ```
   bin\Debug\de-DE\Syncfusion.Tools.Windows.resources.dll
   ```

7. Set the UI culture **before** `InitializeComponent()` in the form constructor:

```csharp
Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("de-DE");
```

```vb
Thread.CurrentThread.CurrentUICulture = New System.Globalization.CultureInfo("de-DE")
```

---

## 3. Using .resx Files

This is the recommended approach for most applications — no compilation tools needed.

### Step 1: Set Culture Before Initialization

```csharp
public Form1()
{
    Thread.CurrentThread.CurrentCulture = new System.Globalization.CultureInfo("de-DE");
    Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("de-DE");
    InitializeComponent();
}
```

```vb
Public Sub New()
    Thread.CurrentThread.CurrentCulture = New System.Globalization.CultureInfo("de-DE")
    Thread.CurrentThread.CurrentUICulture = New System.Globalization.CultureInfo("de-DE")
    InitializeComponent()
End Sub
```

### Step 2: Create a Resources Folder

Right-click the project → **Add New Folder** → name it `Resources`.

### Step 3: Add Default Resource Files

Download the default `.resx` files from the Syncfusion GitHub repository:  
`https://github.com/syncfusion/winforms-controls-localization-resx-files`

Copy the `.resx` files for the libraries you use into the `Resources` folder.

> For example, if using `SfDataGrid`, copy `Syncfusion.SfDataGrid.WinForms.resx`. This shows you all key names and default English values.

### Step 4: Create Culture-Specific .resx Files

Right-click `Resources` → **Add > New Item** → select **Resources File**.  
Name the file: `Syncfusion.{LibraryName}.{culture}.resx`

**Examples:**
- `Syncfusion.SfDataGrid.WinForms.de-DE.resx` (German)
- `Syncfusion.SfDataGrid.WinForms.fr-FR.resx` (French)

### Step 5: Translate the Strings

Copy key names from the default `.resx` file into the culture-specific file and replace the values with the translated strings.

### Resource Files from Different Assemblies or Namespaces

If the `.resx` file is in a different namespace or assembly than the default, call `SetResources()` before `InitializeComponent()`:

```csharp
// Example: SfDataGrid resource in a "Localization" namespace
DataGridLocalizationResourceAccessor.Instance.SetResources(GetType().Assembly, "Localization");
```

| Assembly | Accessor Method |
|---|---|
| Syncfusion.SfDataGrid.WinForms | `DataGridLocalizationResourceAccessor.Instance.SetResources(...)` |
| Syncfusion.Shared.Base | `SharedLocalizationResourceAccessor.Instance.SetResources(...)` |
| Syncfusion.Tools.Windows | `ToolsLocalizationResourceAccessor.Instance.SetResources(...)` |
| Syncfusion.Grid.Windows | `GridLocalizationResourceAccessor.Instance.SetResources(...)` |
| Syncfusion.Edit.Windows | `EditLocalizationResourceAccessor.Instance.SetResources(...)` |
| Syncfusion.PdfViewer.Windows | `PdfViewerLocalizationResourceAccessor.Instance.SetResources(...)` |
| Syncfusion.Diagram.Windows | `DiagramLocalizationResourceAccessor.Instance.SetResources(...)` |
| Syncfusion.Spreadsheet.Windows | `SpreadsheetLocalizationResourceAccessor.Instance.SetResources(...)` |

### Editing Default Culture Strings

To change default English strings, add the default `.resx` file (from GitHub) to the `Resources` folder in your project. Syncfusion controls will read from this file instead of the embedded defaults.

---

## Gotchas

- **Culture must be set before `InitializeComponent()`** — setting it after has no effect on already-initialized controls.
- **Satellite assembly placement:** The DLL must be in a subfolder named after the culture code (e.g., `de-DE\`) next to your executable.
- **Empty string return:** When using `ILocalizationProvider`, returning `string.Empty` for an identifier causes the control to use its built-in default string for that key.
