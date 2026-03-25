# Syntax Highlighting

## Table of Contents
- [Overview](#overview)
- [Built-in Languages](#built-in-languages)
- [Custom Language Configuration](#custom-language-configuration)
- [Programmatic Language Configuration](#programmatic-language-configuration)
- [Advanced Configuration](#advanced-configuration)

This guide covers syntax highlighting features including built-in language support and custom language configuration.

## When to Read This

Read this guide when you need to:
- Apply syntax highlighting for C#, VB.NET, XML, HTML, Java, SQL, PowerShell, or other built-in languages
- Create custom language configurations using XML
- Define lexems, formats, and syntax rules for custom languages
- Configure collapsible code regions
- Implement language-specific coloring and formatting
- Support multiple programming languages in one application

## Overview

EditControl provides comprehensive syntax highlighting through:
- **Built-in Languages**: 11 pre-configured popular languages
- **Custom Languages**: XML-based configuration for any language
- **Programmatic API**: Create languages via code
- **Extensible**: Support for regex patterns, collapsible regions, and auto-replacement

## Built-in Languages

### Supported Languages

| Language | Enum Value | Common Extensions |
|----------|-----------|-------------------|
| **C#** | `KnownLanguages.CSharp` | .cs |
| **VB.NET** | `KnownLanguages.VB` | .vb |
| **XML** | `KnownLanguages.XML` | .xml, .config |
| **HTML** | `KnownLanguages.HTML` | .html, .htm |
| **Java** | `KnownLanguages.Java` | .java |
| **SQL** | `KnownLanguages.SQL` | .sql |
| **PowerShell** | `KnownLanguages.PowerShell` | .ps1 |
| **C** | `KnownLanguages.C` | .c, .h |
| **JavaScript** | `KnownLanguages.JScript` | .js |
| **VBScript** | `KnownLanguages.VBScript` | .vbs |
| **Delphi** | `KnownLanguages.Delphi` | .pas |

### Applying Built-in Languages

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit.Enums;

// Apply C# syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.CSharp);

// Load C# file
editControl1.LoadFile("Sample.cs");
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Edit.Enums

' Apply C# syntax highlighting
editControl1.ApplyConfiguration(KnownLanguages.CSharp)

' Load C# file
editControl1.LoadFile("Sample.cs")
```

### Language-Specific Examples

**XML Highlighting:**
```csharp
editControl1.ApplyConfiguration(KnownLanguages.XML);
editControl1.LoadFile("Config.xml");
```

**HTML Highlighting:**
```csharp
editControl1.ApplyConfiguration(KnownLanguages.HTML);
editControl1.LoadFile("Index.html");
```

**SQL Highlighting:**
```csharp
editControl1.ApplyConfiguration(KnownLanguages.SQL);
editControl1.Text = @"
SELECT * FROM Customers
WHERE Country = 'USA'
ORDER BY Name;
";
```

**PowerShell Highlighting:**
```csharp
editControl1.ApplyConfiguration(KnownLanguages.PowerShell);
editControl1.LoadFile("Deploy.ps1");
```

### Auto-Detection by Extension

**C#:**
```csharp
private void LoadFileWithAutoLanguage(string filePath)
{
    editControl1.LoadFile(filePath);
    
    string ext = Path.GetExtension(filePath).ToLower();
    
    switch (ext)
    {
        case ".cs":
            editControl1.ApplyConfiguration(KnownLanguages.CSharp);
            break;
        case ".vb":
            editControl1.ApplyConfiguration(KnownLanguages.VB);
            break;
        case ".xml":
        case ".config":
            editControl1.ApplyConfiguration(KnownLanguages.XML);
            break;
        case ".html":
        case ".htm":
            editControl1.ApplyConfiguration(KnownLanguages.HTML);
            break;
        case ".java":
            editControl1.ApplyConfiguration(KnownLanguages.Java);
            break;
        case ".sql":
            editControl1.ApplyConfiguration(KnownLanguages.SQL);
            break;
        case ".ps1":
            editControl1.ApplyConfiguration(KnownLanguages.PowerShell);
            break;
        case ".js":
            editControl1.ApplyConfiguration(KnownLanguages.JScript);
            break;
        case ".c":
        case ".h":
            editControl1.ApplyConfiguration(KnownLanguages.C);
            break;
        default:
            // Use plain text or default configuration
            break;
    }
}
```

## Custom Language Configuration

### XML Configuration Structure

Create an XML file to define a custom language:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ArrayOfConfigLanguage>
    <ConfigLanguage name="CustomLang" CaseInsensitive="true">
        
        <!-- Define text formats -->
        <formats>
            <format name="Text" Font="Consolas, 12pt" FontColor="Black" />
            <format name="KeyWord" Font="Consolas, 12pt, style=Bold" FontColor="Blue" />
            <format name="Operator" Font="Consolas, 10pt" FontColor="Brown" />
            <format name="String" Font="Consolas, 12pt" FontColor="Red" />
            <format name="Comment" Font="Consolas, 10pt, style=Italic" FontColor="Green" />
            <format name="Number" Font="Consolas, 12pt" FontColor="DarkMagenta" />
            <format name="Error" Font="Consolas, 10pt" underline="Wave" LineColor="Red" />
        </formats>
        
        <!-- Define file extensions -->
        <extensions>
            <extension>custom</extension>
            <extension>cst</extension>
        </extensions>
        
        <!-- Define lexems (keywords, operators, etc.) -->
        <lexems>
            <!-- Keywords -->
            <lexem BeginBlock="function" Type="KeyWord" />
            <lexem BeginBlock="if" Type="KeyWord" />
            <lexem BeginBlock="else" Type="KeyWord" />
            <lexem BeginBlock="while" Type="KeyWord" />
            <lexem BeginBlock="return" Type="KeyWord" />
            
            <!-- Operators -->
            <lexem BeginBlock="+" Type="Operator" />
            <lexem BeginBlock="-" Type="Operator" />
            <lexem BeginBlock="*" Type="Operator" />
            <lexem BeginBlock="=" Type="Operator" />
            
            <!-- Strings -->
            <lexem BeginBlock="&quot;" EndBlock="&quot;" Type="String" />
            <lexem BeginBlock="'" EndBlock="'" Type="String" />
            
            <!-- Comments -->
            <lexem BeginBlock="//" Type="Comment" />
            <lexem BeginBlock="/*" EndBlock="*/" Type="Comment" />
            
            <!-- Collapsible blocks -->
            <lexem BeginBlock="{" EndBlock="}" Type="Operator" 
                   IsCollapsable="true" CollapseName="{...}">
                <SubLexems>
                    <lexem BeginBlock="\n" IsBeginRegex="true" />
                </SubLexems>
            </lexem>
        </lexems>
        
        <!-- Define region split keywords -->
        <splits>
            <split>#region</split>
            <split>#endregion</split>
        </splits>
        
        <!-- Auto-replacement triggers -->
        <AutoReplaceTriggers>
            <AutoReplaceTrigger From="teh" To="the" />
            <AutoReplaceTrigger From="retrun" To="return" />
        </AutoReplaceTriggers>
        
    </ConfigLanguage>
</ArrayOfConfigLanguage>
```

### Loading Custom Configuration

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using System.IO;

private void LoadCustomLanguage()
{
    // Path to configuration file
    string configPath = Path.Combine(
        Application.StartupPath, 
        "CustomLanguage.xml"
    );
    
    // Load configuration
    editControl1.Configurator.Open(configPath);
    
    // Apply the custom language
    editControl1.ApplyConfiguration("CustomLang");
    
    // Load file with custom syntax
    editControl1.LoadFile("Sample.custom");
}
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Edit
Imports System.IO

Private Sub LoadCustomLanguage()
    ' Path to configuration file
    Dim configPath As String = Path.Combine(
        Application.StartupPath,
        "CustomLanguage.xml"
    )
    
    ' Load configuration
    editControl1.Configurator.Open(configPath)
    
    ' Apply the custom language
    editControl1.ApplyConfiguration("CustomLang")
    
    ' Load file with custom syntax
    editControl1.LoadFile("Sample.custom")
End Sub
```

### Format Attributes

| Attribute | Values | Description |
|-----------|--------|-------------|
| **name** | String | Format identifier (Text, KeyWord, String, etc.) |
| **Font** | Font specification | Font name, size, and style (e.g., "Consolas, 12pt, style=Bold") |
| **FontColor** | Color name or RGB | Text color |
| **BackColor** | Color name or RGB | Background color (optional) |
| **underline** | None, Solid, Dash, Wave, Dot | Underline style |
| **LineColor** | Color name or RGB | Underline color |

### Lexem Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| **BeginBlock** | String | Starting text or pattern |
| **EndBlock** | String | Ending text (for multi-character lexems) |
| **Type** | FormatType | Format to apply (KeyWord, Operator, String, Comment) |
| **IsBeginRegex** | Boolean | Use regex for BeginBlock |
| **IsEndRegex** | Boolean | Use regex for EndBlock |
| **IsCollapsable** | Boolean | Can collapse/expand |
| **CollapseName** | String | Text shown when collapsed |
| **CaseInsensitive** | Boolean | Ignore case for matching |

### Complex Language Example (LISP)

```xml
<?xml version="1.0" encoding="utf-8"?>
<ArrayOfConfigLanguage>
    <ConfigLanguage name="LISP" CaseInsensitive="false">
        <formats>
            <format name="Text" Font="Courier New, 10pt" FontColor="Black" />
            <format name="KeyWord" Font="Courier New, 10pt, style=Bold" FontColor="Blue" />
            <format name="String" Font="Courier New, 10pt" FontColor="Red" />
            <format name="Operator" Font="Courier New, 10pt" FontColor="DarkCyan" />
            <format name="Comment" Font="Courier New, 10pt, style=Italic" FontColor="Green" />
        </formats>
        
        <extensions>
            <extension>lsp</extension>
            <extension>lisp</extension>
        </extensions>
        
        <lexems>
            <!-- Operators -->
            <lexem BeginBlock="(" Type="Operator" />
            <lexem BeginBlock=")" Type="Operator" />
            <lexem BeginBlock="'" Type="Operator" />
            
            <!-- Keywords -->
            <lexem BeginBlock="car" Type="KeyWord" />
            <lexem BeginBlock="cdr" Type="KeyWord" />
            <lexem BeginBlock="cons" Type="KeyWord" />
            <lexem BeginBlock="defun" Type="KeyWord" />
            <lexem BeginBlock="lambda" Type="KeyWord" />
            <lexem BeginBlock="let" Type="KeyWord" />
            <lexem BeginBlock="setq" Type="KeyWord" />
            
            <!-- Comments -->
            <lexem BeginBlock=";" Type="Comment" />
            
            <!-- Strings -->
            <lexem BeginBlock="&quot;" EndBlock="&quot;" Type="String" />
        </lexems>
        
        <splits>
            <split>;; Region</split>
            <split>;; End Region</split>
        </splits>
    </ConfigLanguage>
</ArrayOfConfigLanguage>
```

## Programmatic Language Configuration

### Creating Language via Code

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Interfaces;
using System.Drawing;

private void CreateCustomLanguageProgrammatically()
{
    // Create new language configuration
    IConfigLanguage lang = editControl1.Configurator.CreateLanguageConfiguration("MyLanguage");
    
    // Define formats
    ISnippetFormat keywordFormat = lang.Formats.Add("Keyword");
    keywordFormat.FontColor = Color.Blue;
    keywordFormat.Font = new Font("Consolas", 12, FontStyle.Bold);
    
    ISnippetFormat stringFormat = lang.Formats.Add("String");
    stringFormat.FontColor = Color.Red;
    stringFormat.Font = new Font("Consolas", 12);
    
    ISnippetFormat commentFormat = lang.Formats.Add("Comment");
    commentFormat.FontColor = Color.Green;
    commentFormat.Font = new Font("Consolas", 10, FontStyle.Italic);
    
    // Add lexems
    ConfigLexem ifLexem = new ConfigLexem("if", "", FormatType.Custom, false);
    ifLexem.FormatName = "Keyword";
    lang.Lexems.Add(ifLexem);
    
    ConfigLexem elseLexem = new ConfigLexem("else", "", FormatType.Custom, false);
    elseLexem.FormatName = "Keyword";
    lang.Lexems.Add(elseLexem);
    
    ConfigLexem stringLexem = new ConfigLexem("\"", "\"", FormatType.Custom, false);
    stringLexem.FormatName = "String";
    lang.Lexems.Add(stringLexem);
    
    ConfigLexem commentLexem = new ConfigLexem("//", "", FormatType.Custom, false);
    commentLexem.FormatName = "Comment";
    lang.Lexems.Add(commentLexem);
    
    // Add collapsible block
    ConfigLexem blockLexem = new ConfigLexem("{", "}", FormatType.Operator, true);
    blockLexem.IsCollapsable = true;
    blockLexem.CollapseName = "{...}";
    
    // Add sub-lexem for line breaks
    ConfigLexem newLineLexem = new ConfigLexem("\n", "", FormatType.Text, false);
    newLineLexem.IsBeginRegex = true;
    blockLexem.SubLexems.Add(newLineLexem);
    
    lang.Lexems.Add(blockLexem);
    
    // Apply configuration
    editControl1.ApplyConfiguration(lang);
    editControl1.Language.ResetCaches();
}
```

### Modifying Existing Language

**C#:**
```csharp
private void ModifyExistingLanguage()
{
    // Apply base configuration
    editControl1.ApplyConfiguration(KnownLanguages.CSharp);
    
    // Get current language
    IConfigLanguage language = editControl1.Language;
    
    // Add custom keywords
    ConfigLexem customKeyword = new ConfigLexem("mycustomkeyword", "", FormatType.Custom, false);
    customKeyword.FormatName = "KeyWord";
    language.Lexems.Add(customKeyword);
    
    // Add auto-replacement triggers
    language.AutoReplaceTriggers.Add(new AutoReplaceTrigger("cosole", "console"));
    language.AutoReplaceTriggers.Add(new AutoReplaceTrigger("stirng", "string"));
    
    // Reset caches to apply changes
    editControl1.Language.ResetCaches();
}
```

## Advanced Configuration

### Regex Patterns in Lexems

Use regex for flexible pattern matching:

```xml
<lexem BeginBlock="\b[0-9]+\b" IsBeginRegex="true" Type="Number" />
<lexem BeginBlock="0x[0-9A-Fa-f]+" IsBeginRegex="true" Type="Number" />
<lexem BeginBlock="\b[A-Z_][A-Z0-9_]*\b" IsBeginRegex="true" Type="Constant" />
```

**C#:**
```csharp
// Programmatic regex lexem
ConfigLexem numberLexem = new ConfigLexem(@"\b[0-9]+\b", "", FormatType.Custom, false);
numberLexem.IsBeginRegex = true;
numberLexem.FormatName = "Number";
language.Lexems.Add(numberLexem);
```

### Nested Collapsible Regions

```xml
<lexem BeginBlock="{" EndBlock="}" IsCollapsable="true" CollapseName="{...}">
    <SubLexems>
        <lexem BeginBlock="\n" IsBeginRegex="true" />
        <!-- Support nested braces -->
        <lexem BeginBlock="{" EndBlock="}" IsCollapsable="true" CollapseName="{...}">
            <SubLexems>
                <lexem BeginBlock="\n" IsBeginRegex="true" />
            </SubLexems>
        </lexem>
    </SubLexems>
</lexem>
```

### Multiple Languages in One Application

**C#:**
```csharp
private Dictionary<string, KnownLanguages> languageMap = new Dictionary<string, KnownLanguages>
{
    { ".cs", KnownLanguages.CSharp },
    { ".vb", KnownLanguages.VB },
    { ".xml", KnownLanguages.XML },
    { ".html", KnownLanguages.HTML },
    { ".sql", KnownLanguages.SQL }
};

private void LoadFileWithLanguage(string filePath)
{
    editControl1.LoadFile(filePath);
    
    string extension = Path.GetExtension(filePath).ToLower();
    
    if (languageMap.ContainsKey(extension))
    {
        editControl1.ApplyConfiguration(languageMap[extension]);
    }
}
```

### Complete Custom Language Example

**Create Configuration File (JSON-like syntax):**

```xml
<?xml version="1.0" encoding="utf-8"?>
<ArrayOfConfigLanguage>
    <ConfigLanguage name="JSON" CaseInsensitive="false">
        <formats>
            <format name="Text" Font="Consolas, 11pt" FontColor="Black" />
            <format name="String" Font="Consolas, 11pt" FontColor="Brown" />
            <format name="Number" Font="Consolas, 11pt" FontColor="DarkMagenta" />
            <format name="KeyWord" Font="Consolas, 11pt, style=Bold" FontColor="Blue" />
            <format name="Operator" Font="Consolas, 11pt" FontColor="Gray" />
        </formats>
        
        <extensions>
            <extension>json</extension>
        </extensions>
        
        <lexems>
            <!-- Strings -->
            <lexem BeginBlock="&quot;" EndBlock="&quot;" Type="String" />
            
            <!-- Numbers -->
            <lexem BeginBlock="-?[0-9]+\.?[0-9]*" IsBeginRegex="true" Type="Number" />
            
            <!-- Keywords (boolean and null) -->
            <lexem BeginBlock="true" Type="KeyWord" />
            <lexem BeginBlock="false" Type="KeyWord" />
            <lexem BeginBlock="null" Type="KeyWord" />
            
            <!-- Operators -->
            <lexem BeginBlock=":" Type="Operator" />
            <lexem BeginBlock="," Type="Operator" />
            
            <!-- Collapsible objects -->
            <lexem BeginBlock="{" EndBlock="}" IsCollapsable="true" CollapseName="{...}">
                <SubLexems>
                    <lexem BeginBlock="\n" IsBeginRegex="true" />
                </SubLexems>
            </lexem>
            
            <!-- Collapsible arrays -->
            <lexem BeginBlock="[" EndBlock="]" IsCollapsable="true" CollapseName="[...]">
                <SubLexems>
                    <lexem BeginBlock="\n" IsBeginRegex="true" />
                </SubLexems>
            </lexem>
        </lexems>
    </ConfigLanguage>
</ArrayOfConfigLanguage>
```

**Load and Apply:**
```csharp
editControl1.Configurator.Open("JSON.xml");
editControl1.ApplyConfiguration("JSON");
editControl1.LoadFile("config.json");
```

## Next Steps

- **[Editing Features](editing-features.md)** - Learn about clipboard, undo/redo, and text manipulation
- **[IntelliSense](intellisense.md)** - Configure auto-complete and context prompts
- **[Text Visualization](text-visualization.md)** - Enable line numbers, outlining, and bookmarks
