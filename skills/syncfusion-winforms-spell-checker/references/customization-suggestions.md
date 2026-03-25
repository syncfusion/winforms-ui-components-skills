# Customization & Suggestion Retrieval

Configure spell-check rules and retrieve alternative word suggestions programmatically.

## Ignore Options

Customize which types of content should be excluded from spell checking. Use ignore options to avoid false positives for technical content, URLs, email addresses, and other special patterns.

### Available Ignore Options

```csharp
SpellCheckerAdv spellChecker = new SpellCheckerAdv();

// Ignore email addresses
spellChecker.IgnoreEmailAddress = true;

// Ignore website URLs and network paths
spellChecker.IgnoreUrl = true;

// Ignore file names and paths
spellChecker.IgnoreFileNames = true;

// Ignore HTML and XML tags
spellChecker.IgnoreHtmlTags = true;

// Ignore special symbols and punctuation
spellChecker.IgnoreSpecialSymbols = true;

// Ignore words with mixed case (e.g., camelCase, PascalCase)
spellChecker.IgnoreMixedCaseWords = true;

// Ignore ALL UPPERCASE words (acronyms, constants)
spellChecker.IgnoreUpperCaseWords = true;

// Ignore alphanumeric words (combinations of letters and numbers)
spellChecker.IgnoreAlphaNumericWords = true;
```

### Complete Customization Example

```csharp
public SpellCheckerAdv ConfigureForTechnicalContent()
{
    SpellCheckerAdv spellChecker = new SpellCheckerAdv();
    spellChecker.DictionaryPath = "Syncfusion_en_us.dic";

    // Ignore technical patterns
    spellChecker.IgnoreUrl = true;                    // Skip URLs
    spellChecker.IgnoreEmailAddress = true;           // Skip emails
    spellChecker.IgnoreAlphaNumericWords = true;      // Skip code identifiers
    spellChecker.IgnoreHtmlTags = true;               // Skip HTML tags
    spellChecker.IgnoreMixedCaseWords = true;         // Skip camelCase, PascalCase
    spellChecker.IgnoreUpperCaseWords = true;         // Skip constants, acronyms

    return spellChecker;
}
```

### Use Case Examples

**Example 1: Email/Blog Application**
```csharp
spellChecker.IgnoreEmailAddress = true;      // User emails
spellChecker.IgnoreUrl = true;               // Blog links
spellChecker.IgnoreHtmlTags = true;          // HTML formatting
```

**Example 2: Code Documentation**
```csharp
spellChecker.IgnoreAlphaNumericWords = true; // Function names: foo123
spellChecker.IgnoreMixedCaseWords = true;    // camelCase variable names
spellChecker.IgnoreUpperCaseWords = true;    // Constants like MAX_SIZE
```

**Example 3: General Content**
```csharp
spellChecker.IgnoreUrl = true;               // Web links
spellChecker.IgnoreEmailAddress = true;      // Email addresses
spellChecker.IgnoreFileNames = true;         // File paths
```

## Getting Suggestions for Misspelled Words

Retrieve correction suggestions programmatically. This is useful for implementing custom spell-check UI or analyzing text without showing dialogs.

### GetSuggestions Method

Returns a list of corrected word options for a misspelled word.

```csharp
// Get standard suggestions
List<string> suggestions = spellChecker.GetSuggestions("Textboxx") as List<string>;

// Result might be: ["Textbox", "Text", "box", "Textbooks"]
foreach (string suggestion in suggestions)
{
    Console.WriteLine(suggestion);
}
```

### Complete Suggestion Example

```csharp
public class SpellCheckHelper
{
    private SpellCheckerAdv spellChecker;

    public SpellCheckHelper()
    {
        spellChecker = new SpellCheckerAdv();
        spellChecker.DictionaryPath = "Syncfusion_en_us.dic";
    }

    public List<string> GetCorrections(string misspelledWord)
    {
        var suggestions = spellChecker.GetSuggestions(misspelledWord) as List<string>;
        return suggestions ?? new List<string>();
    }

    public void DisplaySuggestions(string word)
    {
        var suggestions = GetCorrections(word);
        Console.WriteLine($"Suggestions for '{word}':");
        
        foreach (var suggestion in suggestions)
        {
            Console.WriteLine($"  - {suggestion}");
        }
    }
}

// Usage
var helper = new SpellCheckHelper();
helper.DisplaySuggestions("writting");
// Output:
// Suggestions for 'writting':
//   - writing
//   - written
//   - writings
```

## Phonetic Alternatives

Get phonetically similar words. Useful for users who know how a word sounds but not the spelling.

### GetPhoneticWords Method

```csharp
List<string> phoneticWords = spellChecker.GetPhoneticWords("Textboxx") as List<string>;

// Result: Words that sound like "Textboxx" ["Textbox", "Text box"]
foreach (string word in phoneticWords)
{
    Console.WriteLine(word);
}
```

### Use Case: Phonetic Search

```csharp
public string FindPhoneticMatch(string userInput)
{
    var phoneticMatches = spellChecker.GetPhoneticWords(userInput) as List<string>;

    if (phoneticMatches != null && phoneticMatches.Count > 0)
    {
        return phoneticMatches[0];  // Return best match
    }

    return userInput;  // Return original if no match
}

// Usage: User types "shur" intending "sure"
string corrected = FindPhoneticMatch("shur");  // Returns "sure"
```

## Anagram Suggestions

Get anagrams (words containing the same letters rearranged).

### GetAnagrams Method

```csharp
List<string> anagrams = spellChecker.GetAnagrams("textbox") as List<string>;

// Result: Words with same letters rearranged
foreach (string anagram in anagrams)
{
    Console.WriteLine(anagram);
}
```

### Use Case: Word Games

```csharp
public class WordGameHelper
{
    private SpellCheckerAdv spellChecker;

    public WordGameHelper()
    {
        spellChecker = new SpellCheckerAdv();
        spellChecker.DictionaryPath = "Syncfusion_en_us.dic";
    }

    public List<string> FindAnagrams(string word)
    {
        return spellChecker.GetAnagrams(word) as List<string> ?? new List<string>();
    }

    public void ShowWordGameOptions(string word)
    {
        var anagrams = FindAnagrams(word);
        Console.WriteLine($"Anagrams for '{word}':");
        
        foreach (var anagram in anagrams)
        {
            Console.WriteLine($"  - {anagram}");
        }
    }
}

// Usage
var gameHelper = new WordGameHelper();
gameHelper.ShowWordGameOptions("listen");
// Output:
// Anagrams for 'listen':
//   - silent
//   - enlist
//   - inlets
```

## Suggestion Comparison

| Method | Purpose | Use When |
|--------|---------|----------|
| `GetSuggestions()` | Standard spelling corrections | User misspelled a word |
| `GetPhoneticWords()` | Sound-alike alternatives | User knows pronunciation but not spelling |
| `GetAnagrams()` | Same letters rearranged | Playing word games or exploring variants |

## Practical Implementation: Custom Spell Check UI

### Build a Custom Suggestion Dialog

```csharp
public class CustomSpellCheckDialog : Form
{
    private Label misspelledWordLabel;
    private ListBox suggestionsListBox;
    private Button replaceButton;
    private Button ignoreButton;
    private TextBox textBoxToCheck;
    private SpellCheckerAdv spellChecker;

    public CustomSpellCheckDialog(string misspelledWord, TextBox sourceTextBox)
    {
        spellChecker = new SpellCheckerAdv();
        spellChecker.DictionaryPath = "Syncfusion_en_us.dic";
        textBoxToCheck = sourceTextBox;

        // Setup UI
        this.Text = "Spell Check";
        this.Size = new System.Drawing.Size(400, 300);

        // Display misspelled word
        misspelledWordLabel = new Label()
        {
            Text = $"Misspelled: {misspelledWord}",
            Left = 10,
            Top = 10,
            Width = 370
        };
        this.Controls.Add(misspelledWordLabel);

        // Suggestions list
        suggestionsListBox = new ListBox()
        {
            Left = 10,
            Top = 40,
            Width = 370,
            Height = 150
        };

        // Populate suggestions
        var suggestions = spellChecker.GetSuggestions(misspelledWord) as List<string>;
        if (suggestions != null)
        {
            foreach (var suggestion in suggestions)
            {
                suggestionsListBox.Items.Add(suggestion);
            }
        }

        this.Controls.Add(suggestionsListBox);

        // Replace button
        replaceButton = new Button()
        {
            Text = "Replace",
            Left = 10,
            Top = 200,
            Width = 80
        };
        replaceButton.Click += (s, e) =>
        {
            if (suggestionsListBox.SelectedItem != null)
            {
                ReplaceWord(misspelledWord, suggestionsListBox.SelectedItem.ToString());
                this.Close();
            }
        };
        this.Controls.Add(replaceButton);

        // Ignore button
        ignoreButton = new Button()
        {
            Text = "Ignore",
            Left = 100,
            Top = 200,
            Width = 80
        };
        ignoreButton.Click += (s, e) => this.Close();
        this.Controls.Add(ignoreButton);
    }

    private void ReplaceWord(string original, string replacement)
    {
        string text = textBoxToCheck.Text;
        textBoxToCheck.Text = text.Replace(original, replacement);
    }
}

// Usage
private void CheckTextForErrors()
{
    string misspelledWord = "writting";
    var dialog = new CustomSpellCheckDialog(misspelledWord, myTextBox);
    dialog.ShowDialog();
}
```

## Advanced: Batch Word Verification

Check multiple words programmatically:

```csharp
public class BatchSpellChecker
{
    private SpellCheckerAdv spellChecker;

    public BatchSpellChecker()
    {
        spellChecker = new SpellCheckerAdv();
        spellChecker.DictionaryPath = "Syncfusion_en_us.dic";
    }

    public Dictionary<string, List<string>> CheckWords(string[] words)
    {
        var results = new Dictionary<string, List<string>>();

        foreach (var word in words)
        {
            var suggestions = spellChecker.GetSuggestions(word) as List<string>;
            results[word] = suggestions ?? new List<string>();
        }

        return results;
    }

    public void ReportMisspellings(string[] words)
    {
        var results = CheckWords(words);

        foreach (var kvp in results)
        {
            if (kvp.Value.Count > 0)
            {
                Console.WriteLine($"'{kvp.Key}' → {string.Join(", ", kvp.Value)}");
            }
        }
    }
}

// Usage
var batchChecker = new BatchSpellChecker();
batchChecker.ReportMisspellings(new[] { "writting", "recieve", "definately" });
// Output:
// 'writting' → writing, written, writings
// 'recieve' → receive, receiver, deceive
// 'definately' → definitely, final, definitely
```

## Troubleshooting Suggestions

| Issue | Cause | Solution |
|-------|-------|----------|
| No suggestions returned | Dictionary not loaded | Verify DictionaryPath is set and file exists |
| Wrong suggestions | Misspelled word too different | Try GetPhoneticWords() instead |
| Suggestions include proper nouns | Dictionary includes names | Filter results in application logic |
| Performance slow for many words | Large dictionary | Cache suggestions or use GetSuggestions() with limit |
| Anagrams not found | Word too short or no valid anagrams | Check dictionary supports anagrams |
