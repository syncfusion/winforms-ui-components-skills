# Context Menu Integration for Real-Time Spell Checking

Enable Microsoft Office-style context menus that appear when users right-click on misspelled words.

## Overview

Context menu spell checking provides:
- Right-click detection of misspelled words
- Dropdown suggestions for corrections
- "Add to Dictionary" option
- "Ignore All" option
- No modal dialogs required

## Implementing ISpellCheckerAdvEditorTools Interface

All context menu features require implementing the `ISpellCheckerAdvEditorTools` interface.

### Complete TextBoxSpellEditor Implementation

```csharp
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

/// <summary>
/// Adapter class that implements ISpellCheckerAdvEditorTools for TextBox and RichTextBox controls.
/// This allows SpellCheckerAdv to interact with standard text controls for context menu spell checking.
/// </summary>
public class TextBoxSpellEditor : ISpellCheckerAdvEditorTools
{
    /// <summary>
    /// The underlying TextBoxBase control (TextBox or RichTextBox).
    /// </summary>
    private TextBoxBase textBox;

    /// <summary>
    /// Initializes a new instance of the TextBoxSpellEditor class.
    /// </summary>
    /// <param name="control">The TextBox or RichTextBox control to spell check.</param>
    public TextBoxSpellEditor(Control control)
    {
        Control = control;
    }

    /// <summary>
    /// Gets or sets the control whose text is to be spell checked.
    /// </summary>
    public Control Control
    {
        get { return textBox; }
        set { textBox = value as TextBoxBase; }
    }

    /// <summary>
    /// Gets or sets the current misspelled word detected by the spell checker.
    /// </summary>
    public string CurrentWord { get; set; }

    /// <summary>
    /// Gets or sets the full text content of the control.
    /// </summary>
    public string Text
    {
        get { return textBox.Text; }
        set { textBox.Text = value; }
    }

    /// <summary>
    /// Gets or sets the control whose text is to be spell checked.
    /// (Same as Control property - required by interface)
    /// </summary>
    public Control ControlToCheck
    {
        get { return textBox; }
        set { textBox = value as TextBoxBase; }
    }

    /// <summary>
    /// Selects a word in the text control by its position and length.
    /// This highlights the word for user visibility.
    /// </summary>
    /// <param name="selectionStart">Zero-based index of the word's start position.</param>
    /// <param name="selectionLength">Length of the word to select.</param>
    public void SelectText(int selectionStart, int selectionLength)
    {
        textBox.Select(selectionStart, selectionLength);
        textBox.Focus();
    }
}
```

## Setting Up Context Menu Spell Checking

### Basic Setup

```csharp
private SpellCheckerAdv spellChecker;
private RichTextBox richTextBox;

public void InitializeContextMenuSpellCheck()
{
    // Create controls
    spellChecker = new SpellCheckerAdv();
    richTextBox = new RichTextBox();

    // Configure dictionary
    spellChecker.DictionaryPath = "Syncfusion_en_us.dic";

    // Create adapter
    TextBoxSpellEditor editor = new TextBoxSpellEditor(richTextBox);

    // Enable context menu spell checking
    spellChecker.PerformSpellCheckUsingContextMenu(editor);
}
```

### Advanced Setup with Multiple Dictionaries

```csharp
public void InitializeAdvancedContextMenu()
{
    spellChecker = new SpellCheckerAdv();
    richTextBox = new RichTextBox();

    // Configure multiple language dictionaries
    spellChecker.Dictionaries = new DictionaryCollection();
    spellChecker.Dictionaries.Add(
        new HunspellDictionary()
        {
            Culture = new System.Globalization.CultureInfo("en-US"),
            DictionaryPath = @"Dictionaries\en-US.dic",
            GrammarPath = @"Dictionaries\en-US.aff"
        }
    );

    spellChecker.Culture = new System.Globalization.CultureInfo("en-US");

    // Enable context menu
    TextBoxSpellEditor editor = new TextBoxSpellEditor(richTextBox);
    spellChecker.PerformSpellCheckUsingContextMenu(editor);
}
```

## User Workflow

### How Users Interact with Context Menu

1. **User types text** in RichTextBox or TextBox
2. **Misspelled word gets underlined** (optional visual indicator)
3. **User right-clicks** on the underlined word
4. **Context menu appears** with:
   - Suggested corrections (selectable)
   - "Add to Dictionary" button
   - "Ignore All" option
5. **User selects correction** → Word is replaced automatically
   - OR clicks "Add to Dictionary" → Word is added to custom dictionary
   - OR clicks "Ignore All" → Word is ignored for rest of session

## Code Examples

### Example 1: Simple Email Editor

```csharp
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class EmailEditorForm : Form
{
    private RichTextBox emailBody;
    private SpellCheckerAdv spellChecker;
    private TextBoxSpellEditor editor;

    public EmailEditorForm()
    {
        // Setup form
        this.Text = "Email Editor with Spell Check";
        this.Size = new System.Drawing.Size(500, 400);

        // Setup RichTextBox
        emailBody = new RichTextBox();
        emailBody.Dock = DockStyle.Fill;
        emailBody.Text = "Compose your email hear...";
        this.Controls.Add(emailBody);

        // Setup spell checker
        spellChecker = new SpellCheckerAdv();
        spellChecker.DictionaryPath = "Syncfusion_en_us.dic";

        // Enable context menu
        editor = new TextBoxSpellEditor(emailBody);
        spellChecker.PerformSpellCheckUsingContextMenu(editor);
    }
}
```

### Example 2: Document Editor with Language Support

```csharp
public class DocumentEditorForm : Form
{
    private RichTextBox documentText;
    private ComboBox languageSelector;
    private SpellCheckerAdv spellChecker;
    private TextBoxSpellEditor editor;

    public DocumentEditorForm()
    {
        // Setup controls
        this.Text = "Document Editor";
        this.Size = new System.Drawing.Size(600, 500);

        // Language selector
        languageSelector = new ComboBox();
        languageSelector.Items.AddRange(new[] { "English (US)", "French", "Spanish" });
        languageSelector.SelectedIndex = 0;
        languageSelector.SelectedIndexChanged += LanguageSelector_SelectedIndexChanged;
        languageSelector.Dock = DockStyle.Top;
        languageSelector.Height = 30;
        this.Controls.Add(languageSelector);

        // Document text area
        documentText = new RichTextBox();
        documentText.Dock = DockStyle.Fill;
        this.Controls.Add(documentText);

        // Setup spell checker with multiple languages
        spellChecker = new SpellCheckerAdv();
        spellChecker.Dictionaries = new DictionaryCollection();

        // Add English
        spellChecker.Dictionaries.Add(new HunspellDictionary()
        {
            Culture = new System.Globalization.CultureInfo("en-US"),
            DictionaryPath = @"Dictionaries\en-US.dic",
            GrammarPath = @"Dictionaries\en-US.aff"
        });

        // Add French
        spellChecker.Dictionaries.Add(new HunspellDictionary()
        {
            Culture = new System.Globalization.CultureInfo("fr-FR"),
            DictionaryPath = @"Dictionaries\fr-FR.dic",
            GrammarPath = @"Dictionaries\fr-FR.aff"
        });

        // Add Spanish
        spellChecker.Dictionaries.Add(new HunspellDictionary()
        {
            Culture = new System.Globalization.CultureInfo("es-ES"),
            DictionaryPath = @"Dictionaries\es-ES.dic",
            GrammarPath = @"Dictionaries\es-ES.aff"
        });

        spellChecker.Culture = new System.Globalization.CultureInfo("en-US");

        // Enable context menu
        editor = new TextBoxSpellEditor(documentText);
        spellChecker.PerformSpellCheckUsingContextMenu(editor);
    }

    private void LanguageSelector_SelectedIndexChanged(object sender, System.EventArgs e)
    {
        string selectedLanguage = languageSelector.SelectedItem.ToString();

        switch (selectedLanguage)
        {
            case "French":
                spellChecker.Culture = new System.Globalization.CultureInfo("fr-FR");
                break;
            case "Spanish":
                spellChecker.Culture = new System.Globalization.CultureInfo("es-ES");
                break;
            default:
                spellChecker.Culture = new System.Globalization.CultureInfo("en-US");
                break;
        }
    }
}
```

### Example 3: Form Input Field with Context Menu

```csharp
public class CustomerFormWithSpellCheck : Form
{
    private TextBox nameField;
    private TextBox addressField;
    private SpellCheckerAdv spellChecker;

    public CustomerFormWithSpellCheck()
    {
        this.Text = "Customer Information";
        this.Size = new System.Drawing.Size(400, 300);

        // Name field (no spell check needed)
        Label nameLabel = new Label() { Text = "Name:", Left = 10, Top = 10 };
        nameField = new TextBox() { Left = 10, Top = 30, Width = 360 };
        this.Controls.Add(nameLabel);
        this.Controls.Add(nameField);

        // Address field (spell check enabled)
        Label addressLabel = new Label() { Text = "Address:", Left = 10, Top = 70 };
        addressField = new TextBox()
        {
            Left = 10,
            Top = 90,
            Width = 360,
            Height = 80,
            Multiline = true,
            WordWrap = true,
            AcceptsReturn = true
        };
        this.Controls.Add(addressLabel);
        this.Controls.Add(addressField);

        // Setup spell check for address field only
        spellChecker = new SpellCheckerAdv();
        spellChecker.DictionaryPath = "Syncfusion_en_us.dic";

        TextBoxSpellEditor editor = new TextBoxSpellEditor(addressField);
        spellChecker.PerformSpellCheckUsingContextMenu(editor);
    }
}
```

## Method Reference

### PerformSpellCheckUsingContextMenu

Enables context menu spell checking for a text control.

```csharp
spellChecker.PerformSpellCheckUsingContextMenu(ISpellCheckerAdvEditorTools editor);
```

**Parameters:**
- `editor`: Instance of ISpellCheckerAdvEditorTools wrapping the text control

**Returns:** void

**Notes:**
- Must be called after dictionary configuration
- Can be called multiple times to add multiple controls
- Right-click on underlined words to access context menu

## Common Patterns

### Pattern 1: Multiple Controls with Different Dictionaries

```csharp
// English spell check for English fields
var englishEditor = new TextBoxSpellEditor(englishTextBox);
spellChecker.Culture = new CultureInfo("en-US");
spellChecker.PerformSpellCheckUsingContextMenu(englishEditor);
```

### Pattern 2: Disable Context Menu

To disable context menu spell checking:

```csharp
// Create new spell checker instance without calling PerformSpellCheckUsingContextMenu
SpellCheckerAdv newSpellChecker = new SpellCheckerAdv();
// Or remove the editor reference
editor = null;
```

### Pattern 3: Switch Between Dialog and Context Menu

```csharp
private void ButtonSpellCheckDialog_Click(object sender, EventArgs e)
{
    // Dialog-based: opens modal window
    spellChecker.SpellCheck(new SpellCheckerAdvEditorWrapper(richTextBox));
}

private void InitializeContextMenu()
{
    // Context menu-based: right-click interaction
    var editor = new TextBoxSpellEditor(richTextBox);
    spellChecker.PerformSpellCheckUsingContextMenu(editor);
}
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Context menu doesn't appear | Ensure `PerformSpellCheckUsingContextMenu()` is called after dictionary setup |
| No suggestions showing | Verify dictionary is valid and dictionary path is correct |
| Words not being recognized as misspelled | Check dictionary language matches text content |
| "Add to Dictionary" not working | Ensure custom dictionary is added to Dictionaries collection |
