# IntelliSense Features

## Table of Contents
- [Overview](#overview)
- [Context Choice (Auto-Complete)](#context-choice-auto-complete)
- [Context Prompt (Method Overloads)](#context-prompt-method-overloads)
- [Context Tooltip](#context-tooltip)
- [Auto Replace Triggers](#auto-replace-triggers)

This guide covers IntelliSense features including auto-complete, method overload prompts, context tooltips, and auto-correction.

## When to Read This

Read this guide when you need to:
- Configure auto-complete with predefined items
- Display method overload prompts
- Show context tooltips on hover
- Enable auto-correction for common typos
- Customize IntelliSense appearance and behavior
- Handle IntelliSense events

## Overview

EditControl provides comprehensive IntelliSense capabilities:
- **Context Choice**: Auto-complete dropdown with suggestions
- **Context Prompt**: Method parameter hints with overload selection
- **Context Tooltip**: Hover tooltips for collapsed code and custom content
- **Auto Replace**: Automatic correction of common typos

## Context Choice (Auto-Complete)

### XML Configuration

Configure auto-complete triggers in language XML:

```xml
<lexem BeginBlock="." Type="Operator" DropContextChoiceList="true"/>
```

### Populating Items

**C#:**
```csharp
// Subscribe to event
editControl1.ContextChoiceOpen += EditControl1_ContextChoiceOpen;

private void EditControl1_ContextChoiceOpen(ContextChoiceController controller)
{
    // Add simple items
    controller.Items.Add("Activate");
    controller.Items.Add("ActiveControl");
    controller.Items.Add("AllowDrop");
    
    // Add items with tooltips
    controller.Items.Add("BackColor", "Gets or sets the background color");
    controller.Items.Add("Controls", "Gets the collection of controls");
    
    // Add items with images
    int imageIndex = 0;
    foreach (Image img in imageList1.Images)
    {
        controller.AddImage($"Image{imageIndex}", img);
        imageIndex++;
    }
    
    controller.Items.Add("Method1", "Method description", controller.Images["Image0"]);
    controller.Items.Add("Property1", "Property description", controller.Images["Image1"]);
}
```

**VB.NET:**
```vb
' Subscribe to event
AddHandler editControl1.ContextChoiceOpen, AddressOf EditControl1_ContextChoiceOpen

Private Sub EditControl1_ContextChoiceOpen(controller As ContextChoiceController)
    ' Add simple items
    controller.Items.Add("Activate")
    controller.Items.Add("ActiveControl")
    controller.Items.Add("AllowDrop")
    
    ' Add items with tooltips
    controller.Items.Add("BackColor", "Gets or sets the background color")
    controller.Items.Add("Controls", "Gets the collection of controls")
End Sub
```

### Enable Auto-Completion

**C#:**
```csharp
// Enable automatic completion for single lexem
editControl1.AutoCompleteSingleLexem = true;

// Filter items as user types
editControl1.FilterAutoCompleteItems = true;
```

### Customization

**C#:**
```csharp
// Appearance
editControl1.ContextChoiceBorderColor = Color.Navy;
editControl1.ContextChoiceSize = new Size(200, 150);
editControl1.ContextChoiceBackColor = Color.AliceBlue;
editControl1.ContextChoiceForeColor = Color.Black;
editControl1.ContextChoiceFont = new Font("Consolas", 10F);

// Show/hide programmatically
editControl1.ShowContextChoice();
editControl1.CloseContextChoice();
```

### Events

**C#:**
```csharp
// Before opening
editControl1.ContextChoiceBeforeOpen += (sender, e) =>
{
    // Cancel if specific condition
    if (someCondition)
    {
        e.Cancel = true;
    }
};

// Item selected
editControl1.ContextChoiceItemSelected += (sender, e) =>
{
    string selectedText = e.SelectedItem.Text;
    string tooltip = e.SelectedItem.ToolTip;
    
    Console.WriteLine($"Selected: {selectedText}");
};

// Closing
editControl1.ContextChoiceClose += (controller, result) =>
{
    // Clean up
    controller.Items.Clear();
};
```

### Complete Auto-Complete Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
using System;
using System.Drawing;
using System.Windows.Forms;

public class AutoCompleteEditorForm : Form
{
    private EditControl editControl1;
    private ImageList imageList1;
    
    public AutoCompleteEditorForm()
    {
        InitializeComponent();
        SetupEditor();
        SetupAutoComplete();
    }
    
    private void SetupEditor()
    {
        editControl1 = new EditControl
        {
            Dock = DockStyle.Fill,
            ShowLineNumbers = true
        };
        editControl1.ApplyConfiguration(KnownLanguages.CSharp);
        
        this.Controls.Add(editControl1);
    }
    
    private void SetupAutoComplete()
    {
        // Enable features
        editControl1.AutoCompleteSingleLexem = true;
        editControl1.FilterAutoCompleteItems = true;
        
        // Create images for items
        imageList1 = new ImageList { ImageSize = new Size(16, 16) };
        imageList1.Images.Add("Method", Properties.Resources.MethodIcon);
        imageList1.Images.Add("Property", Properties.Resources.PropertyIcon);
        imageList1.Images.Add("Class", Properties.Resources.ClassIcon);
        
        // Subscribe to event
        editControl1.ContextChoiceOpen += PopulateContextChoice;
        
        // Customize appearance
        editControl1.ContextChoiceBackColor = Color.White;
        editControl1.ContextChoiceForeColor = Color.Black;
        editControl1.ContextChoiceFont = new Font("Consolas", 9.75F);
        editControl1.ContextChoiceSize = new Size(300, 200);
    }
    
    private void PopulateContextChoice(ContextChoiceController controller)
    {
        // Add images
        foreach (string key in imageList1.Images.Keys)
        {
            controller.AddImage(key, imageList1.Images[key]);
        }
        
        // Add C# common items
        controller.Items.Add("Console", "System.Console class", controller.Images["Class"]);
        controller.Items.Add("WriteLine", "Writes line to console", controller.Images["Method"]);
        controller.Items.Add("ReadLine", "Reads line from console", controller.Images["Method"]);
        controller.Items.Add("string", "System.String type", controller.Images["Class"]);
        controller.Items.Add("int", "System.Int32 type", controller.Images["Class"]);
        controller.Items.Add("Length", "Gets the length", controller.Images["Property"]);
        controller.Items.Add("ToString", "Converts to string", controller.Images["Method"]);
    }
    
    private void InitializeComponent()
    {
        this.Text = "Auto-Complete Editor";
        this.Size = new Size(900, 700);
    }
}
```

## Context Prompt (Method Overloads)

### XML Configuration

```xml
<lexem BeginBlock="(" Type="Operator" IsComplex="true" DropContextPrompt="true" />
```

### Populating Prompts

**C#:**
```csharp
editControl1.ContextPromptOpen += EditControl1_ContextPromptOpen;

private void EditControl1_ContextPromptOpen(object sender, ContextPromptUpdateEventArgs e)
{
    // Add method overloads
    var item1 = e.AddPrompt(
        "void WriteLine(string text)",
        "Writes a line of text to the console"
    );
    
    // Add bolded items for current parameter
    item1.BoldedItems.Add(16, 6, "string"); // Bold the "string" parameter
    
    var item2 = e.AddPrompt(
        "void WriteLine(string format, params object[] args)",
        "Writes a formatted string to the console"
    );
    item2.BoldedItems.Add(16, 6, "format");
    item2.BoldedItems.Add(31, 16, "params object[]");
    
    var item3 = e.AddPrompt(
        "void WriteLine()",
        "Writes an empty line to the console"
    );
}
```

### Update Current Parameter

**C#:**
```csharp
editControl1.ContextPromptUpdate += EditControl1_ContextPromptUpdate;

private void EditControl1_ContextPromptUpdate(object sender, ContextPromptUpdateEventArgs e)
{
    if (e.List.SelectedItem != null)
    {
        // Get lexems inside current parentheses
        IList list = editControl1.GetLexemsInsideCurrentStack(false);
        
        // Count commas to determine current parameter
        int parameterIndex = 0;
        foreach (ILexem lexem in list)
        {
            if (lexem.Text == ",")
            {
                parameterIndex++;
            }
        }
        
        // Update bolded item to current parameter
        if (parameterIndex < e.List.SelectedItem.BoldedItems.Count)
        {
            e.List.SelectedItem.BoldedItems.SelectedItem = 
                e.List.SelectedItem.BoldedItems[parameterIndex];
        }
    }
}
```

### Customization

**C#:**
```csharp
// Background gradient
editControl1.ContextPromptBackgroundBrush = new BrushInfo(
    GradientStyle.BackwardDiagonal,
    Color.White,
    Color.LightBlue
);

// Border color
editControl1.ContextPromptBorderColor = Color.Navy;

// Custom size
editControl1.UseCustomSizeContextPrompt = true;
editControl1.ContextPromptCustomSize = new Size(400, 100);

// Show/hide programmatically
editControl1.ShowContextPrompt();
editControl1.CloseContextPrompt();
```

## Context Tooltip

### Custom Tooltip Content

**C#:**
```csharp
editControl1.UpdateContextToolTip += EditControl1_UpdateContextToolTip;

private void EditControl1_UpdateContextToolTip(object sender, UpdateTooltipEventArgs e)
{
    if (e.Text == string.Empty)
    {
        // Convert mouse position to editor position
        Point pointVirtual = editControl1.PointToVirtualPosition(new Point(e.X, e.Y));
        
        if (pointVirtual.Y > 0)
        {
            ILexemLine line = editControl1.GetLine(pointVirtual.Y);
            if (line != null)
            {
                ILexem lexem = line.FindLexemByColumn(pointVirtual.X);
                if (lexem != null)
                {
                    // Set custom tooltip text
                    e.Text = $"Lexem: {lexem.Text}\nType: {lexem.Type}\nLine: {pointVirtual.Y}";
                }
            }
        }
    }
}
```

**VB.NET:**
```vb
AddHandler editControl1.UpdateContextToolTip, AddressOf EditControl1_UpdateContextToolTip

Private Sub EditControl1_UpdateContextToolTip(sender As Object, e As UpdateTooltipEventArgs)
    If e.Text = String.Empty Then
        ' Convert mouse position to editor position
        Dim pointVirtual As Point = editControl1.PointToVirtualPosition(New Point(e.X, e.Y))
        
        If pointVirtual.Y > 0 Then
            Dim line As ILexemLine = editControl1.GetLine(pointVirtual.Y)
            If line IsNot Nothing Then
                Dim lexem As ILexem = line.FindLexemByColumn(pointVirtual.X)
                If lexem IsNot Nothing Then
                    ' Set custom tooltip text
                    e.Text = $"Lexem: {lexem.Text}{vbCrLf}Type: {lexem.Type}{vbCrLf}Line: {pointVirtual.Y}"
                End If
            End If
        End If
    End If
End Sub
```

### Tooltip Customization

**C#:**
```csharp
// Enable tooltips
editControl1.ShowContextTooltip = true;

// Background
editControl1.ContextTooltipBackgroundBrush = new BrushInfo(
    PatternStyle.Percent05,
    Color.Yellow,
    Color.LightYellow
);

// Border
editControl1.ContextTooltipBorderColor = Color.Orange;

// Delay before showing (milliseconds)
editControl1.ToolTipDelay = 1000;
```

### Outlining Tooltips

Automatically show tooltips for collapsed code:

**C#:**
```csharp
// Enable outlining tooltips
editControl1.ShowOutliningTooltip = true;

// When user hovers over collapsed region, tooltip shows hidden code
```

## Auto Replace Triggers

### Enable Auto Replace

**C#:**
```csharp
// Enable auto-replacement
editControl1.UseAutoreplaceTriggers = true;

// Access triggers from language
editControl1.Language.AutoReplaceTriggers.AddRange(new AutoReplaceTrigger[]
{
    new AutoReplaceTrigger("tis", "this"),
    new AutoReplaceTrigger("fro", "for"),
    new AutoReplaceTrigger("retrun", "return"),
    new AutoReplaceTrigger("cosole", "console"),
    new AutoReplaceTrigger("stirng", "string"),
    new AutoReplaceTrigger("pubblic", "public")
});
```

### XML Configuration

```xml
<AutoReplaceTriggers>
    <AutoReplaceTrigger From="tis" To="this" />
    <AutoReplaceTrigger From="fro" To="for" />
    <AutoReplaceTrigger From="retrun" To="return" />
    <AutoReplaceTrigger From="cosole" To="console" />
</AutoReplaceTriggers>
```

### Add Triggers Programmatically

**C#:**
```csharp
// Add single trigger
editControl1.Language.AutoReplaceTriggers.Add(
    new AutoReplaceTrigger("teh", "the")
);

// Add multiple triggers
var commonTypos = new[]
{
    new AutoReplaceTrigger("recieve", "receive"),
    new AutoReplaceTrigger("seperate", "separate"),
    new AutoReplaceTrigger("definately", "definitely"),
    new AutoReplaceTrigger("occured", "occurred")
};

editControl1.Language.AutoReplaceTriggers.AddRange(commonTypos);
```

## Complete IntelliSense Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
using Syncfusion.Windows.Forms.Edit.Interfaces;
using System;
using System.Drawing;
using System.Windows.Forms;

public class IntelliSenseEditorForm : Form
{
    private EditControl editControl1;
    
    public IntelliSenseEditorForm()
    {
        InitializeComponent();
        SetupEditor();
        SetupIntelliSense();
    }
    
    private void SetupEditor()
    {
        editControl1 = new EditControl
        {
            Dock = DockStyle.Fill,
            ShowLineNumbers = true
        };
        editControl1.ApplyConfiguration(KnownLanguages.CSharp);
        
        this.Controls.Add(editControl1);
    }
    
    private void SetupIntelliSense()
    {
        // Context choice (auto-complete)
        editControl1.AutoCompleteSingleLexem = true;
        editControl1.FilterAutoCompleteItems = true;
        editControl1.ContextChoiceOpen += PopulateAutoComplete;
        
        // Context prompt (method overloads)
        editControl1.ContextPromptOpen += PopulateMethodPrompts;
        editControl1.ContextPromptUpdate += UpdateCurrentParameter;
        
        // Context tooltip
        editControl1.ShowContextTooltip = true;
        editControl1.UpdateContextToolTip += ShowCustomTooltip;
        
        // Auto replace
        editControl1.UseAutoreplaceTriggers = true;
        editControl1.Language.AutoReplaceTriggers.AddRange(new[]
        {
            new AutoReplaceTrigger("tis", "this"),
            new AutoReplaceTrigger("fro", "for"),
            new AutoReplaceTrigger("retrun", "return")
        });
        
        // Customize appearance
        editControl1.ContextChoiceBackColor = Color.White;
        editControl1.ContextChoiceFont = new Font("Consolas", 9.75F);
        editControl1.ContextPromptBorderColor = Color.Navy;
    }
    
    private void PopulateAutoComplete(ContextChoiceController controller)
    {
        controller.Items.Add("Console", "System.Console class");
        controller.Items.Add("WriteLine", "Writes to console");
        controller.Items.Add("ReadLine", "Reads from console");
        controller.Items.Add("string", "System.String");
        controller.Items.Add("int", "System.Int32");
        controller.Items.Add("foreach", "Foreach loop");
        controller.Items.Add("if", "If statement");
        controller.Items.Add("else", "Else statement");
    }
    
    private void PopulateMethodPrompts(object sender, ContextPromptUpdateEventArgs e)
    {
        var item1 = e.AddPrompt(
            "void WriteLine()",
            "Writes an empty line"
        );
        
        var item2 = e.AddPrompt(
            "void WriteLine(string text)",
            "Writes text to console"
        );
        item2.BoldedItems.Add(16, 6, "text");
        
        var item3 = e.AddPrompt(
            "void WriteLine(string format, params object[] args)",
            "Writes formatted text"
        );
        item3.BoldedItems.Add(16, 6, "format");
        item3.BoldedItems.Add(31, 16, "args");
    }
    
    private void UpdateCurrentParameter(object sender, ContextPromptUpdateEventArgs e)
    {
        if (e.List.SelectedItem != null)
        {
            int paramIndex = 0;
            var lexems = editControl1.GetLexemsInsideCurrentStack(false);
            foreach (ILexem lexem in lexems)
            {
                if (lexem.Text == ",") paramIndex++;
            }
            
            if (paramIndex < e.List.SelectedItem.BoldedItems.Count)
            {
                e.List.SelectedItem.BoldedItems.SelectedItem = 
                    e.List.SelectedItem.BoldedItems[paramIndex];
            }
        }
    }
    
    private void ShowCustomTooltip(object sender, UpdateTooltipEventArgs e)
    {
        if (e.Text == string.Empty)
        {
            Point pos = editControl1.PointToVirtualPosition(new Point(e.X, e.Y));
            if (pos.Y > 0)
            {
                ILexemLine line = editControl1.GetLine(pos.Y);
                if (line != null)
                {
                    ILexem lexem = line.FindLexemByColumn(pos.X);
                    if (lexem != null && lexem.Text.Length > 0)
                    {
                        e.Text = $"Element: {lexem.Text}\nType: {lexem.Type}";
                    }
                }
            }
        }
    }
    
    private void InitializeComponent()
    {
        this.Text = "IntelliSense Editor";
        this.Size = new Size(900, 700);
    }
}
```

## Next Steps

- **[Text Visualization](text-visualization.md)** - Enable line numbers, outlining, and bookmarks
- **[File Operations](file-operations.md)** - Load, save, and export files
- **[Advanced Features](advanced-features.md)** - Split views, find/replace, and navigation
