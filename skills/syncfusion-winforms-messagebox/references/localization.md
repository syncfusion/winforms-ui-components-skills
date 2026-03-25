# Localization Support

This guide covers implementing multilanguage support in MessageBoxAdv using the ILocalizationProvider interface to translate button text and UI elements.

## Table of Contents
- [Overview](#overview)
- [Localization Workflow](#localization-workflow)
- [ILocalizationProvider Interface](#ilocalizationprovider-interface)
- [ResourceIdentifiers](#resourceidentifiers)
- [Implementation Examples](#implementation-examples)
- [Best Practices](#best-practices)

---

## Overview

MessageBoxAdv supports complete localization, allowing you to translate button text and UI elements into any language. This is essential for international applications and multilingual user bases.

### What Can Be Localized?

- Button text (Yes, No, OK, Cancel, Retry, Abort, Ignore)
- "Details" button text
- "Close" button tooltip

### Localization Architecture

1. **ILocalizationProvider Interface:** Define localization logic
2. **LocalizationProvider.Provider:** Register your localizer
3. **GetLocalizedString():** Return translated strings
4. **ResourceIdentifiers:** Constants for localizable elements

---

## Localization Workflow

### Step-by-Step Process

**Step 1:** Create a class implementing `ILocalizationProvider`

**Step 2:** Implement `GetLocalizedString()` method

**Step 3:** Register provider **before** `InitializeComponent()`

**Step 4:** Use MessageBoxAdv normally (translations applied automatically)

### Quick Example

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Globalization;

// Step 1: Create localizer class
public class GermanLocalizer : ILocalizationProvider
{
    // Step 2: Implement GetLocalizedString
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes:
                return "Ja";
            case ResourceIdentifiers.No:
                return "Nein";
            case ResourceIdentifiers.OK:
                return "OK";
            case ResourceIdentifiers.Cancel:
                return "Abbrechen";
            default:
                return string.Empty;
        }
    }
}

// Step 3: Register provider in Form constructor
public Form1()
{
    LocalizationProvider.Provider = new GermanLocalizer();
    InitializeComponent();
}

// Step 4: Use MessageBoxAdv normally
MessageBoxAdv.Show(this, "Möchten Sie speichern?", "Bestätigen", 
    MessageBoxButtons.YesNo, MessageBoxIcon.Question);
// Buttons will show "Ja" and "Nein"
```

---

## ILocalizationProvider Interface

The `ILocalizationProvider` interface defines the contract for localization.

### Interface Definition

```csharp
public interface ILocalizationProvider
{
    string GetLocalizedString(CultureInfo culture, string name, object obj);
}
```

### GetLocalizedString Method

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `culture` | `CultureInfo` | Current culture (e.g., `de-DE`, `fr-FR`) |
| `name` | `string` | Resource identifier (from `ResourceIdentifiers` class) |
| `obj` | `object` | Context object (typically MessageBoxAdv instance) |

**Return Value:** Localized string for the given resource identifier

### Implementation Pattern

**C#:**
```csharp
public class MyLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        // Use switch statement for resource mapping
        switch (name)
        {
            case ResourceIdentifiers.Yes:
                return "Localized Yes";
            case ResourceIdentifiers.No:
                return "Localized No";
            // ... other cases ...
            default:
                return string.Empty; // Return empty for unmapped resources
        }
    }
}
```

**VB.NET:**
```vb
Public Class MyLocalizer
    Implements ILocalizationProvider

    Public Function GetLocalizedString(culture As CultureInfo, name As String, obj As Object) As String Implements ILocalizationProvider.GetLocalizedString
        Select Case name
            Case ResourceIdentifiers.Yes
                Return "Localized Yes"
            Case ResourceIdentifiers.No
                Return "Localized No"
            ' ... other cases ...
            Case Else
                Return String.Empty ' Return empty for unmapped resources
        End Select
    End Function
End Class
```

---

## ResourceIdentifiers

The `ResourceIdentifiers` class provides constants for all localizable elements.

### Available Resource Identifiers

| Constant | Default English Text | Appears In |
|----------|---------------------|------------|
| `ResourceIdentifiers.Yes` | "Yes" | YesNo, YesNoCancel buttons |
| `ResourceIdentifiers.No` | "No" | YesNo, YesNoCancel buttons |
| `ResourceIdentifiers.OK` | "OK" | OK, OKCancel buttons |
| `ResourceIdentifiers.Cancel` | "Cancel" | OKCancel, YesNoCancel, RetryCancel buttons |
| `ResourceIdentifiers.Retry` | "Retry" | RetryCancel, AbortRetryIgnore buttons |
| `ResourceIdentifiers.Abort` | "Abort" | AbortRetryIgnore buttons |
| `ResourceIdentifiers.Ignore` | "Ignore" | AbortRetryIgnore buttons |
| `ResourceIdentifiers.Details` | "Details" | Details view button |
| `ResourceIdentifiers.Close` | "Close" | Close button tooltip |

### Usage in Switch Statement

**C#:**
```csharp
switch (name)
{
    case ResourceIdentifiers.Yes:
        return "Sí";        // Spanish
    case ResourceIdentifiers.No:
        return "No";
    case ResourceIdentifiers.OK:
        return "Aceptar";
    case ResourceIdentifiers.Cancel:
        return "Cancelar";
    case ResourceIdentifiers.Retry:
        return "Reintentar";
    case ResourceIdentifiers.Abort:
        return "Abortar";
    case ResourceIdentifiers.Ignore:
        return "Ignorar";
    case ResourceIdentifiers.Details:
        return "Detalles";
    case ResourceIdentifiers.Close:
        return "Cerrar";
    default:
        return string.Empty;
}
```

---

## Implementation Examples

### Example 1: German Localization

Complete German localization implementation.

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Globalization;

public class GermanLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes:
                return "Ja";
            case ResourceIdentifiers.No:
                return "Nein";
            case ResourceIdentifiers.OK:
                return "OK";
            case ResourceIdentifiers.Cancel:
                return "Abbrechen";
            case ResourceIdentifiers.Retry:
                return "Wiederholen";
            case ResourceIdentifiers.Abort:
                return "Abbrechen";
            case ResourceIdentifiers.Ignore:
                return "Ignorieren";
            case ResourceIdentifiers.Details:
                return "Details";
            case ResourceIdentifiers.Close:
                return "Schließen";
            default:
                return string.Empty;
        }
    }
}

// Usage in Form
public class MainForm : Form
{
    public MainForm()
    {
        // Register German localizer BEFORE InitializeComponent
        LocalizationProvider.Provider = new GermanLocalizer();
        InitializeComponent();
        
        // Set theme
        MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016;
        MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful;
    }

    private void btnSave_Click(object sender, EventArgs e)
    {
        // Show German message box
        DialogResult result = MessageBoxAdv.Show(this,
            "Möchten Sie die Änderungen speichern?",
            "Ungespeicherte Änderungen",
            MessageBoxButtons.YesNoCancel,
            MessageBoxIcon.Question);

        // Buttons display: "Ja", "Nein", "Abbrechen"
        
        if (result == DialogResult.Yes)
        {
            SaveDocument();
        }
    }
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
Imports System.Globalization

Public Class GermanLocalizer
    Implements ILocalizationProvider

    Public Function GetLocalizedString(culture As CultureInfo, name As String, obj As Object) As String Implements ILocalizationProvider.GetLocalizedString
        Select Case name
            Case ResourceIdentifiers.Yes
                Return "Ja"
            Case ResourceIdentifiers.No
                Return "Nein"
            Case ResourceIdentifiers.OK
                Return "OK"
            Case ResourceIdentifiers.Cancel
                Return "Abbrechen"
            Case ResourceIdentifiers.Retry
                Return "Wiederholen"
            Case ResourceIdentifiers.Abort
                Return "Abbrechen"
            Case ResourceIdentifiers.Ignore
                Return "Ignorieren"
            Case ResourceIdentifiers.Details
                Return "Details"
            Case ResourceIdentifiers.Close
                Return "Schließen"
            Case Else
                Return String.Empty
        End Select
    End Function
End Class

' Usage in Form
Public Class MainForm
    Public Sub New()
        ' Register German localizer BEFORE InitializeComponent
        LocalizationProvider.Provider = New GermanLocalizer()
        InitializeComponent()
        
        ' Set theme
        MessageBoxAdv.MessageBoxStyle = MessageBoxAdv.Style.Office2016
        MessageBoxAdv.Office2016Theme = Office2016Theme.Colorful
    End Sub

    Private Sub btnSave_Click(sender As Object, e As EventArgs) Handles btnSave.Click
        ' Show German message box
        Dim result As DialogResult = MessageBoxAdv.Show(Me, _
            "Möchten Sie die Änderungen speichern?", _
            "Ungespeicherte Änderungen", _
            MessageBoxButtons.YesNoCancel, _
            MessageBoxIcon.Question)

        ' Buttons display: "Ja", "Nein", "Abbrechen"
        
        If result = DialogResult.Yes Then
            SaveDocument()
        End If
    End Sub
End Class
```

---

### Example 2: Spanish Localization

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Globalization;

public class SpanishLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes:
                return "Sí";
            case ResourceIdentifiers.No:
                return "No";
            case ResourceIdentifiers.OK:
                return "Aceptar";
            case ResourceIdentifiers.Cancel:
                return "Cancelar";
            case ResourceIdentifiers.Retry:
                return "Reintentar";
            case ResourceIdentifiers.Abort:
                return "Abortar";
            case ResourceIdentifiers.Ignore:
                return "Ignorar";
            case ResourceIdentifiers.Details:
                return "Detalles";
            case ResourceIdentifiers.Close:
                return "Cerrar";
            default:
                return string.Empty;
        }
    }
}

// Usage
public MainForm()
{
    LocalizationProvider.Provider = new SpanishLocalizer();
    InitializeComponent();
}

// Message box with Spanish buttons
MessageBoxAdv.Show(this,
    "¿Desea guardar los cambios?",
    "Cambios sin guardar",
    MessageBoxButtons.YesNoCancel,
    MessageBoxIcon.Question);
// Buttons: "Sí", "No", "Cancelar"
```

---

### Example 3: French Localization

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Globalization;

public class FrenchLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes:
                return "Oui";
            case ResourceIdentifiers.No:
                return "Non";
            case ResourceIdentifiers.OK:
                return "OK";
            case ResourceIdentifiers.Cancel:
                return "Annuler";
            case ResourceIdentifiers.Retry:
                return "Réessayer";
            case ResourceIdentifiers.Abort:
                return "Abandonner";
            case ResourceIdentifiers.Ignore:
                return "Ignorer";
            case ResourceIdentifiers.Details:
                return "Détails";
            case ResourceIdentifiers.Close:
                return "Fermer";
            default:
                return string.Empty;
        }
    }
}

// Usage
public MainForm()
{
    LocalizationProvider.Provider = new FrenchLocalizer();
    InitializeComponent();
}

// Message box with French buttons
MessageBoxAdv.Show(this,
    "Voulez-vous enregistrer les modifications?",
    "Modifications non enregistrées",
    MessageBoxButtons.YesNoCancel,
    MessageBoxIcon.Question);
// Buttons: "Oui", "Non", "Annuler"
```

---

### Example 4: Multi-Language Support with Culture Detection

Dynamically select language based on current culture.

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Globalization;

public class MultiLanguageLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        // Detect current culture
        string languageCode = culture.TwoLetterISOLanguageName;

        switch (languageCode)
        {
            case "de": // German
                return GetGermanString(name);
            case "es": // Spanish
                return GetSpanishString(name);
            case "fr": // French
                return GetFrenchString(name);
            case "it": // Italian
                return GetItalianString(name);
            case "pt": // Portuguese
                return GetPortugueseString(name);
            default: // English (fallback)
                return GetEnglishString(name);
        }
    }

    private string GetGermanString(string name)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes: return "Ja";
            case ResourceIdentifiers.No: return "Nein";
            case ResourceIdentifiers.OK: return "OK";
            case ResourceIdentifiers.Cancel: return "Abbrechen";
            case ResourceIdentifiers.Retry: return "Wiederholen";
            case ResourceIdentifiers.Abort: return "Abbrechen";
            case ResourceIdentifiers.Ignore: return "Ignorieren";
            case ResourceIdentifiers.Details: return "Details";
            case ResourceIdentifiers.Close: return "Schließen";
            default: return string.Empty;
        }
    }

    private string GetSpanishString(string name)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes: return "Sí";
            case ResourceIdentifiers.No: return "No";
            case ResourceIdentifiers.OK: return "Aceptar";
            case ResourceIdentifiers.Cancel: return "Cancelar";
            case ResourceIdentifiers.Retry: return "Reintentar";
            case ResourceIdentifiers.Abort: return "Abortar";
            case ResourceIdentifiers.Ignore: return "Ignorar";
            case ResourceIdentifiers.Details: return "Detalles";
            case ResourceIdentifiers.Close: return "Cerrar";
            default: return string.Empty;
        }
    }

    private string GetFrenchString(string name)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes: return "Oui";
            case ResourceIdentifiers.No: return "Non";
            case ResourceIdentifiers.OK: return "OK";
            case ResourceIdentifiers.Cancel: return "Annuler";
            case ResourceIdentifiers.Retry: return "Réessayer";
            case ResourceIdentifiers.Abort: return "Abandonner";
            case ResourceIdentifiers.Ignore: return "Ignorer";
            case ResourceIdentifiers.Details: return "Détails";
            case ResourceIdentifiers.Close: return "Fermer";
            default: return string.Empty;
        }
    }

    private string GetItalianString(string name)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes: return "Sì";
            case ResourceIdentifiers.No: return "No";
            case ResourceIdentifiers.OK: return "OK";
            case ResourceIdentifiers.Cancel: return "Annulla";
            case ResourceIdentifiers.Retry: return "Riprova";
            case ResourceIdentifiers.Abort: return "Interrompi";
            case ResourceIdentifiers.Ignore: return "Ignora";
            case ResourceIdentifiers.Details: return "Dettagli";
            case ResourceIdentifiers.Close: return "Chiudi";
            default: return string.Empty;
        }
    }

    private string GetPortugueseString(string name)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes: return "Sim";
            case ResourceIdentifiers.No: return "Não";
            case ResourceIdentifiers.OK: return "OK";
            case ResourceIdentifiers.Cancel: return "Cancelar";
            case ResourceIdentifiers.Retry: return "Tentar Novamente";
            case ResourceIdentifiers.Abort: return "Abortar";
            case ResourceIdentifiers.Ignore: return "Ignorar";
            case ResourceIdentifiers.Details: return "Detalhes";
            case ResourceIdentifiers.Close: return "Fechar";
            default: return string.Empty;
        }
    }

    private string GetEnglishString(string name)
    {
        switch (name)
        {
            case ResourceIdentifiers.Yes: return "Yes";
            case ResourceIdentifiers.No: return "No";
            case ResourceIdentifiers.OK: return "OK";
            case ResourceIdentifiers.Cancel: return "Cancel";
            case ResourceIdentifiers.Retry: return "Retry";
            case ResourceIdentifiers.Abort: return "Abort";
            case ResourceIdentifiers.Ignore: return "Ignore";
            case ResourceIdentifiers.Details: return "Details";
            case ResourceIdentifiers.Close: return "Close";
            default: return string.Empty;
        }
    }
}

// Usage - automatically uses current culture
public MainForm()
{
    LocalizationProvider.Provider = new MultiLanguageLocalizer();
    InitializeComponent();
}

// Message box will use appropriate language based on Thread.CurrentThread.CurrentCulture
```

---

### Example 5: Resource File Integration

Use .resx resource files for translation management.

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using System.Globalization;
using System.Resources;

public class ResourceFileLocalizer : ILocalizationProvider
{
    private ResourceManager _resourceManager;

    public ResourceFileLocalizer()
    {
        // Load resource file (MessageBoxResources.resx, MessageBoxResources.de.resx, etc.)
        _resourceManager = new ResourceManager("YourNamespace.Resources.MessageBoxResources", 
            typeof(ResourceFileLocalizer).Assembly);
    }

    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        try
        {
            // Look up string in resource file
            string resourceKey = GetResourceKey(name);
            string localizedString = _resourceManager.GetString(resourceKey, culture);
            
            return localizedString ?? string.Empty;
        }
        catch
        {
            return string.Empty;
        }
    }

    private string GetResourceKey(string resourceIdentifier)
    {
        // Map ResourceIdentifiers to resource file keys
        switch (resourceIdentifier)
        {
            case ResourceIdentifiers.Yes: return "MessageBox_Yes";
            case ResourceIdentifiers.No: return "MessageBox_No";
            case ResourceIdentifiers.OK: return "MessageBox_OK";
            case ResourceIdentifiers.Cancel: return "MessageBox_Cancel";
            case ResourceIdentifiers.Retry: return "MessageBox_Retry";
            case ResourceIdentifiers.Abort: return "MessageBox_Abort";
            case ResourceIdentifiers.Ignore: return "MessageBox_Ignore";
            case ResourceIdentifiers.Details: return "MessageBox_Details";
            case ResourceIdentifiers.Close: return "MessageBox_Close";
            default: return string.Empty;
        }
    }
}

// Resource file structure:
// MessageBoxResources.resx (English - default)
//   MessageBox_Yes = "Yes"
//   MessageBox_No = "No"
//   ...
//
// MessageBoxResources.de.resx (German)
//   MessageBox_Yes = "Ja"
//   MessageBox_No = "Nein"
//   ...
//
// MessageBoxResources.es.resx (Spanish)
//   MessageBox_Yes = "Sí"
//   MessageBox_No = "No"
//   ...

// Usage
public MainForm()
{
    LocalizationProvider.Provider = new ResourceFileLocalizer();
    InitializeComponent();
}
```

---

## Best Practices

### Initialization Timing

**✅ CORRECT - Before InitializeComponent:**
```csharp
public MainForm()
{
    LocalizationProvider.Provider = new GermanLocalizer();
    InitializeComponent(); // ✓ Localizer registered first
}
```

**❌ INCORRECT - After InitializeComponent:**
```csharp
public MainForm()
{
    InitializeComponent(); // ✗ Too late
    LocalizationProvider.Provider = new GermanLocalizer();
}
```

### Null Handling

Always return `string.Empty` for unmapped resources:

```csharp
public string GetLocalizedString(CultureInfo culture, string name, object obj)
{
    switch (name)
    {
        case ResourceIdentifiers.Yes:
            return "Ja";
        // ... other cases ...
        default:
            return string.Empty; // ✓ Return empty, not null
    }
}
```

### Culture-Specific vs. Language-Specific

**Language-specific** (recommended for most cases):
```csharp
string languageCode = culture.TwoLetterISOLanguageName; // "de", "en", "fr"
```

**Culture-specific** (for regional variations):
```csharp
string cultureCode = culture.Name; // "de-DE", "en-US", "fr-FR"

if (cultureCode == "en-GB")
    return "Colour"; // British spelling
else if (cultureCode == "en-US")
    return "Color"; // American spelling
```

### Performance

- Cache ResourceManager instances
- Avoid creating new localizer objects repeatedly
- Use static helper methods for common translations

### Testing

Test with different cultures:
```csharp
// Temporarily change culture for testing
CultureInfo originalCulture = Thread.CurrentThread.CurrentCulture;
Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");

MessageBoxAdv.Show(this, "Test", "Test");

Thread.CurrentThread.CurrentCulture = originalCulture;
```

---

## Translation Reference

Quick reference for common languages:

| English | German | Spanish | French | Italian | Portuguese | Russian |
|---------|--------|---------|--------|---------|------------|---------|
| Yes | Ja | Sí | Oui | Sì | Sim | Да |
| No | Nein | No | Non | No | Não | Нет |
| OK | OK | Aceptar | OK | OK | OK | ОК |
| Cancel | Abbrechen | Cancelar | Annuler | Annulla | Cancelar | Отмена |
| Retry | Wiederholen | Reintentar | Réessayer | Riprova | Tentar Novamente | Повторить |
| Abort | Abbrechen | Abortar | Abandonner | Interrompi | Abortar | Прервать |
| Ignore | Ignorieren | Ignorar | Ignorer | Ignora | Ignorar | Пропустить |
| Details | Details | Detalles | Détails | Dettagli | Detalhes | Подробности |
| Close | Schließen | Cerrar | Fermer | Chiudi | Fechar | Закрыть |

---

## Troubleshooting

### Buttons Still Show English

**Issue:** Localized strings not appearing.

**Solutions:**
1. Verify `LocalizationProvider.Provider` is set **before** `InitializeComponent()`
2. Check all `ResourceIdentifiers` are mapped in `GetLocalizedString()`
3. Ensure `GetLocalizedString()` returns non-empty strings
4. Test with breakpoint in `GetLocalizedString()` to verify it's called

### Wrong Language Displayed

**Issue:** Incorrect language shown.

**Solutions:**
1. Check `Thread.CurrentThread.CurrentCulture` value
2. Verify culture detection logic in multi-language localizers
3. Test culture-specific logic with different `CultureInfo` values

### Missing Translations

**Issue:** Some buttons show English, others show translated text.

**Solution:** Ensure all `ResourceIdentifiers` cases are handled in switch statement:
```csharp
case ResourceIdentifiers.Abort: // ✓ Don't forget less common ones
    return "Abortar";
```

---

## Next Steps

- **Visual Styles:** Apply themes to localized message boxes → [visual-styles.md](visual-styles.md)
- **Metro Customization:** Combine localization with custom Metro colors → [metro-customization.md](metro-customization.md)
- **Button Parameters:** Configure buttons and features → [button-parameters.md](button-parameters.md)
