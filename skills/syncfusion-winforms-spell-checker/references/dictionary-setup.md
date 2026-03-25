# Dictionary Configuration & Multi-Language Support

## Table of Contents
- [Overview](#overview)
- [Default English Dictionary](#default-english-dictionary)
- [Hunspell Dictionary](#hunspell-dictionary)
- [Ispell Dictionary](#ispell-dictionary)
- [OpenOffice Dictionary](#openoffice-dictionary)
- [Custom Dictionary](#custom-dictionary)
- [Runtime Culture Switching](#runtime-culture-switching)

## Overview

SpellCheckerAdv supports multiple dictionary formats and languages. Choose the format based on your requirements and available dictionary files.

**Supported Formats:**
- **Hunspell** - Modern, feature-rich (used by Firefox, LibreOffice)
- **Ispell** - Legacy format, still widely available
- **OpenOffice** - Compatible with OpenOffice/LibreOffice
- **Custom** - Simple word lists for domain-specific terms

## Default English Dictionary

The simplest option for English-only applications.

### Setup

```csharp
SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.DictionaryPath = "Syncfusion_en_us.dic";
```

### Location Options

**From application folder:**
```csharp
spellChecker.DictionaryPath = "Syncfusion_en_us.dic";
```

**From custom folder:**
```csharp
string path = Path.Combine(Application.StartupPath, "Dictionaries", "Syncfusion_en_us.dic");
spellChecker.DictionaryPath = path;
```

**From resources:**
```csharp
string resourcePath = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "Resources", "Syncfusion_en_us.dic");
spellChecker.DictionaryPath = resourcePath;
```

## Hunspell Dictionary

Hunspell is the most common modern dictionary format. Each language requires two files:
- `*.dic` - Word list (basic words)
- `*.aff` - Affix rules (grammar and word variations)

### Adding Hunspell Dictionary

```csharp
using System.Globalization;
using Syncfusion.Windows.Forms.Tools;

// Create culture instance
CultureInfo frenchCulture = new CultureInfo("fr-FR");

// Create spell checker
SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.Dictionaries = new DictionaryCollection();

// Add French Hunspell dictionary
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = frenchCulture,
        DictionaryPath = @"Dictionaries\french.dic",
        GrammarPath = @"Dictionaries\french.aff"
    }
);

// Set active language
spellChecker.Culture = frenchCulture;
```

### Multiple Hunspell Dictionaries

```csharp
SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.Dictionaries = new DictionaryCollection();

// English
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = new CultureInfo("en-US"),
        DictionaryPath = @"Dictionaries\en-US.dic",
        GrammarPath = @"Dictionaries\en-US.aff"
    }
);

// French
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = new CultureInfo("fr-FR"),
        DictionaryPath = @"Dictionaries\fr-FR.dic",
        GrammarPath = @"Dictionaries\fr-FR.aff"
    }
);

// Spanish
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = new CultureInfo("es-ES"),
        DictionaryPath = @"Dictionaries\es-ES.dic",
        GrammarPath = @"Dictionaries\es-ES.aff"
    }
);

// Use English by default
spellChecker.Culture = new CultureInfo("en-US");
```

## Ispell Dictionary

Ispell is a legacy format but still available for many languages. File requirements:
- `*.dic` - Word list
- `*.aff` - Affix rules
- `*.xlg` - Extended word list (optional)

### Adding Ispell Dictionary

```csharp
CultureInfo spanishCulture = new CultureInfo("es-ES");

SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.Dictionaries = new DictionaryCollection();

spellChecker.Dictionaries.Add(
    new IspellDictionary()
    {
        Culture = spanishCulture,
        DictionaryPath = @"Dictionaries\spanish.dic",
        GrammarPath = @"Dictionaries\spanish.aff"
    }
);

spellChecker.Culture = spanishCulture;
```

## OpenOffice Dictionary

OpenOffice format is compatible with LibreOffice and similar tools. File requirements:
- `*.dic` - Word list
- `*.aff` - Affix rules

### Adding OpenOffice Dictionary

```csharp
CultureInfo germanCulture = new CultureInfo("de-DE");

SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.Dictionaries = new DictionaryCollection();

spellChecker.Dictionaries.Add(
    new OpenOfficeDictionary()
    {
        Culture = germanCulture,
        DictionaryPath = @"Dictionaries\german.dic",
        GrammarPath = @"Dictionaries\german.aff"
    }
);

spellChecker.Culture = germanCulture;
```

## Custom Dictionary

Custom dictionaries contain simple word lists (one word per line) with no grammar rules. Use for domain-specific terms or proprietary vocabulary.

### File Format

Create a text file `custom_words.txt`:
```
Syncfusion
SpellChecker
misspelled
refactor
```

### Adding Custom Dictionary

```csharp
CultureInfo culture = new CultureInfo("en-US");

SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.Dictionaries = new DictionaryCollection();

spellChecker.Dictionaries.Add(
    new CustomDictionary()
    {
        Culture = culture,
        DictionaryPath = @"Dictionaries\custom_words.txt"
    }
);
```

### Custom + Standard Dictionary Combination

Combine custom words with an official dictionary:

```csharp
CultureInfo culture = new CultureInfo("en-US");

SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.Dictionaries = new DictionaryCollection();

// Add custom company words
spellChecker.Dictionaries.Add(
    new CustomDictionary()
    {
        Culture = culture,
        DictionaryPath = @"Dictionaries\company_terms.txt"
    }
);

// Add standard English dictionary
spellChecker.Dictionaries.Add(
    new HunspellDictionary()
    {
        Culture = culture,
        DictionaryPath = @"Dictionaries\en-US.dic",
        GrammarPath = @"Dictionaries\en-US.aff"
    }
);

spellChecker.Culture = culture;
```

**Result:** Words in both dictionaries are recognized as correct.

## Runtime Culture Switching

Switch spell-check language at runtime based on user selection or document language.

### Basic Culture Switch

```csharp
// Configure multiple dictionaries
SpellCheckerAdv spellChecker = new SpellCheckerAdv();
spellChecker.Dictionaries = new DictionaryCollection();

// Add English
spellChecker.Dictionaries.Add(new HunspellDictionary()
{
    Culture = new CultureInfo("en-US"),
    DictionaryPath = @"en-US.dic",
    GrammarPath = @"en-US.aff"
});

// Add French
spellChecker.Dictionaries.Add(new HunspellDictionary()
{
    Culture = new CultureInfo("fr-FR"),
    DictionaryPath = @"fr-FR.dic",
    GrammarPath = @"fr-FR.aff"
});

// Switch at runtime
private void ChangeLanguage(string languageCode)
{
    spellChecker.Culture = new CultureInfo(languageCode);
}

// Usage
ChangeLanguage("fr-FR");  // Switch to French
ChangeLanguage("en-US");  // Switch to English
```

### Language Selection UI

```csharp
private void comboLanguage_SelectedIndexChanged(object sender, EventArgs e)
{
    string selectedLanguage = comboLanguage.SelectedItem as string;
    
    spellChecker.Culture = new CultureInfo(selectedLanguage);
    
    richTextBox.Focus();
}
```

### Per-Document Language Setting

```csharp
public class DocumentSpellChecker
{
    private SpellCheckerAdv spellChecker;
    private CultureInfo documentLanguage;

    public DocumentSpellChecker(string languageCode)
    {
        spellChecker = new SpellCheckerAdv();
        documentLanguage = new CultureInfo(languageCode);
        spellChecker.Culture = documentLanguage;
    }

    public void CheckDocument(RichTextBox textBox)
    {
        spellChecker.SpellCheck(new SpellCheckerAdvEditorWrapper(textBox));
    }

    public void SetLanguage(string languageCode)
    {
        documentLanguage = new CultureInfo(languageCode);
        spellChecker.Culture = documentLanguage;
    }
}
```

## Common Culture Codes

| Language | Code |
|----------|------|
| English (US) | en-US |
| English (GB) | en-GB |
| French | fr-FR |
| Spanish | es-ES |
| German | de-DE |
| Italian | it-IT |
| Portuguese (Brazil) | pt-BR |
| Russian | ru-RU |
| Chinese (Simplified) | zh-CN |
| Japanese | ja-JP |

## Troubleshooting Dictionary Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| "Dictionary not found" | File path incorrect | Verify path exists; use absolute path if needed |
| No suggestions appearing | Dictionary format mismatch | Verify file format matches configuration |
| Slow spell checking | Large dictionary file | Consider custom dictionaries for frequently used terms |
| Mixed language content | Single dictionary active | Add multiple dictionaries and switch as needed |
| Custom words not recognized | Custom dictionary not added | Ensure CustomDictionary is added to Dictionaries collection |
