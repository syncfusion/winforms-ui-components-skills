# Getting Started with SpellCheckerAdv

This guide covers installation, control setup, and basic spell-checking integration with text controls.

## Installation & Assembly References

### Required Assemblies

Add these assemblies to your Windows Forms project:

- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.SpellChecker.Base.dll`
- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`

### NuGet Installation

Install the SpellChecker package via NuGet Package Manager:

```bash
Install-Package Syncfusion.Shared.Base
```

This automatically includes all required dependencies.

## Adding Control via Designer

1. Open your Windows Forms project in Visual Studio
2. Open the Toolbox and search for **SpellCheckerAdv**
3. Drag it onto your form
4. Required assemblies are added automatically

## Adding Control Manually via Code

### Step 1: Add Namespace

```csharp
using Syncfusion.Windows.Forms.Tools;
```

### Step 2: Create SpellCheckerAdv Instance

```csharp
SpellCheckerAdv spellCheckerAdv1 = new SpellCheckerAdv();
```

### Step 3: Configure for RichTextBox

Create a wrapper class implementing `ISpellCheckerAdvEditorTools`:

```csharp
class TextBoxSpellEditor : ISpellCheckerAdvEditorTools
{
    private TextBoxBase textBox;

    public TextBoxSpellEditor(Control control)
    {
        Control = control;
    }

    public Control Control
    {
        get { return textBox; }
        set { textBox = value as TextBoxBase; }
    }

    public string CurrentWord { get; set; }

    public string Text
    {
        get { return textBox.Text; }
        set { textBox.Text = value; }
    }

    public Control ControlToCheck
    {
        get { return textBox; }
        set { textBox = value as TextBoxBase; }
    }

    public void SelectText(int selectionStart, int selectionLength)
    {
        textBox.Select(selectionStart, selectionLength);
    }
}
```

## Configuring SpellCheckerAdv with RichTextBox

### Step 1: Create Form Controls

```csharp
public class SpellCheckForm : Form
{
    private RichTextBox richTextBox1;
    private Button buttonSpellCheck;
    private SpellCheckerAdv spellCheckerAdv1;

    public SpellCheckForm()
    {
        // Create RichTextBox
        richTextBox1 = new RichTextBox();
        richTextBox1.Dock = DockStyle.Fill;
        richTextBox1.Text = "Add your text hear for spell chekcing.";

        // Create Button
        buttonSpellCheck = new Button();
        buttonSpellCheck.Text = "Spell Check";
        buttonSpellCheck.Click += ButtonSpellCheck_Click;

        // Create SpellCheckerAdv
        spellCheckerAdv1 = new SpellCheckerAdv();
        spellCheckerAdv1.DictionaryPath = "Syncfusion_en_us.dic";

        // Add controls to form
        this.Controls.Add(richTextBox1);
        this.Controls.Add(buttonSpellCheck);
    }

    private void ButtonSpellCheck_Click(object sender, EventArgs e)
    {
        spellCheckerAdv1.SpellCheck(new SpellCheckerAdvEditorWrapper(richTextBox1));
    }
}
```

### Step 2: Trigger Spell Check

Use the `SpellCheck()` method with `SpellCheckerAdvEditorWrapper`:

```csharp
private void buttonSpellCheck_Click(object sender, EventArgs e)
{
    spellCheckerAdv1.SpellCheck(new SpellCheckerAdvEditorWrapper(richTextBox1));
}
```

**What happens:**
1. Dialog opens showing the first misspelled word
2. User selects a suggestion or clicks "Ignore" / "Replace"
3. Dialog moves to next misspelled word
4. Process repeats until all words checked

## Configuring SpellCheckerAdv with TextBox

The same `TextBoxSpellEditor` class works for standard TextBox controls:

```csharp
private void SetupSpellCheck()
{
    TextBox textBox = new TextBox();
    spellCheckerAdv1.PerformSpellCheckForControl(new TextBoxSpellEditor(textBox));
}
```

## Dictionary Setup

### Set Built-In English Dictionary

```csharp
spellCheckerAdv1.DictionaryPath = "Syncfusion_en_us.dic";
```

The built-in English dictionary is included with Syncfusion.

### Custom Dictionary Location

If your dictionary is in a custom folder:

```csharp
string dictionaryPath = Application.StartupPath + @"\Dictionaries\Syncfusion_en_us.dic";
spellCheckerAdv1.DictionaryPath = dictionaryPath;
```

## Complete Example: Minimal Spell Check App

```csharp
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class MinimalSpellCheckApp : Form
{
    private RichTextBox richTextBox;
    private Button checkButton;
    private SpellCheckerAdv spellChecker;

    public MinimalSpellCheckApp()
    {
        // Setup form
        this.Text = "Spell Checker Example";
        this.Size = new System.Drawing.Size(600, 400);

        // Setup RichTextBox
        richTextBox = new RichTextBox();
        richTextBox.Dock = DockStyle.Fill;
        richTextBox.Text = "This is a simple spell chek example.";
        this.Controls.Add(richTextBox);

        // Setup Button
        checkButton = new Button();
        checkButton.Text = "Check Spelling";
        checkButton.Dock = DockStyle.Bottom;
        checkButton.Height = 40;
        checkButton.Click += (s, e) => CheckSpelling();
        this.Controls.Add(checkButton);

        // Setup SpellChecker
        spellChecker = new SpellCheckerAdv();
        spellChecker.DictionaryPath = "Syncfusion_en_us.dic";
    }

    private void CheckSpelling()
    {
        spellChecker.SpellCheck(new SpellCheckerAdvEditorWrapper(richTextBox));
    }

    [System.STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.Run(new MinimalSpellCheckApp());
    }
}
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Dictionary not found | Verify file path is correct; check file exists in specified location |
| No suggestions appearing | Ensure dictionary file is valid and language matches content |
| Control not responding | Verify all required assemblies are referenced |
| Context menu not showing | Ensure `PerformSpellCheckUsingContextMenu()` is called before right-click |
