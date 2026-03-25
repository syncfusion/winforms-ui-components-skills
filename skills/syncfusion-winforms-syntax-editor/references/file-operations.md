# File Operations and Export

This guide covers file operations including loading, saving, exporting to various formats, and printing.

## When to Read This

Read this guide when you need to:
- Load files into the editor
- Save documents with encoding options
- Export content to XML, RTF, or HTML
- Print documents with syntax highlighting
- Handle file operations with dialogs
- Work with file streams
- Track file modification status

## Overview

EditControl provides comprehensive file management:
- **Load**: Open files with encoding support
- **Save**: Save with customizable encoding and line endings
- **Export**: XML, RTF, HTML formats with syntax highlighting
- **Print**: Print documents with headers, footers, and formatting
- **Streams**: Work with streams for advanced scenarios

## Loading Files

### Basic Load Operations

**C#:**
```csharp
// Load with dialog
editControl1.LoadFile();

// Load specific file
editControl1.LoadFile("Sample.cs");

// Load with encoding
editControl1.LoadFile("Sample.txt", Encoding.UTF8);

// Load from stream
using (FileStream stream = File.OpenRead("Sample.cs"))
{
    editControl1.LoadStream(stream);
}
```

**VB.NET:**
```vb
' Load with dialog
editControl1.LoadFile()

' Load specific file
editControl1.LoadFile("Sample.cs")

' Load with encoding
editControl1.LoadFile("Sample.txt", Encoding.UTF8)

' Load from stream
Using stream As FileStream = File.OpenRead("Sample.cs")
    editControl1.LoadStream(stream)
End Using
```

### Load with Configuration

**C#:**
```csharp
// Load and apply configuration
editControl1.LoadFile("Sample.cs");
editControl1.ApplyConfiguration(KnownLanguages.CSharp);

// Or load with stream and configuration
using (FileStream stream = File.OpenRead("Sample.xml"))
{
    IConfigLanguage config = editControl1.Configurator.Languages["XML"];
    editControl1.LoadStream(stream, config);
}
```

### File Properties

**C#:**
```csharp
// Get current file name
string fileName = editControl1.FileName;

// Get file stream
FileStream fileOpened = editControl1.FileOpened;

// Check if modified
bool isModified = editControl1.IsModified;
bool fileModified = editControl1.FileModified;
```

## Saving Files

### Basic Save Operations

**C#:**
```csharp
// Save with dialog
editControl1.Save();

// Save to specific file
editControl1.SaveFile("Output.txt");

// Save with encoding and line style
editControl1.SaveFile(
    "Output.txt",
    Encoding.UTF8,
    NewLineStyle.Windows  // Windows (CRLF), Unix (LF), or Mac (CR)
);

// Save as (show dialog)
editControl1.SaveAs();

// Save if modified
editControl1.SaveModified();
```

**VB.NET:**
```vb
' Save with dialog
editControl1.Save()

' Save to specific file
editControl1.SaveFile("Output.txt")

' Save with encoding and line style
editControl1.SaveFile(
    "Output.txt",
    Encoding.UTF8,
    NewLineStyle.Windows
)

' Save as (show dialog)
editControl1.SaveAs()

' Save if modified
editControl1.SaveModified()
```

### Save to Stream

**C#:**
```csharp
// Save to stream
using (FileStream stream = File.Create("Output.txt"))
{
    editControl1.SaveStream(
        stream,
        Encoding.UTF8,
        NewLineStyle.Windows
    );
}

// Save to memory stream
using (MemoryStream memStream = new MemoryStream())
{
    editControl1.SaveStream(memStream, Encoding.UTF8, NewLineStyle.Windows);
    byte[] content = memStream.ToArray();
    // Use content
}
```

### Save Options

**C#:**
```csharp
// Auto save on close
editControl1.SaveOnClose = true;

// Discard unsaved changes
editControl1.FlushChanges();
```

### Encoding Options

**C#:**
```csharp
// Common encodings
editControl1.SaveFile("file.txt", Encoding.UTF8);
editControl1.SaveFile("file.txt", Encoding.Unicode);
editControl1.SaveFile("file.txt", Encoding.ASCII);
editControl1.SaveFile("file.txt", Encoding.UTF32);
editControl1.SaveFile("file.txt", Encoding.BigEndianUnicode);
```

## File Operations

### New File

**C#:**
```csharp
// Create new empty file
editControl1.New();

// Create new with default configuration
editControl1.NewFile();

// Create new with specific configuration
IConfigLanguage csharpConfig = editControl1.Configurator.Languages["CSharp"];
editControl1.NewFile(csharpConfig);
```

### Insert File

**C#:**
```csharp
// Insert file at cursor with dialog
editControl1.InsertFile();

// Insert specific file at cursor
editControl1.InsertFile(@"..\..\Header.cs");
```

### Close File

**C#:**
```csharp
// Close current file
editControl1.Close();

// Handle close event
editControl1.Closing += (sender, e) =>
{
    if (editControl1.IsModified)
    {
        // Prompt user
        e.Action = SaveChangesAction.ShowDialog;
        
        // Or auto-save
        // e.Action = SaveChangesAction.Save;
        
        // Or discard
        // e.Action = SaveChangesAction.Discard;
    }
};
```

### Drag and Drop Files

**C#:**
```csharp
// Enable drag and drop
editControl1.AllowDrop = true;
editControl1.DropAllFiles = true;

// Filter file extensions
editControl1.FileExtensions = new string[] { ".cs", ".vb", ".xml", ".txt" };

// Insert dropped file into text (vs. replacing all)
editControl1.InsertDroppedFileIntoText = true;

// Show notification on drop
editControl1.ShowFileDropNotification = false;
```

## Export Formats

### Export to XML

**C#:**
```csharp
// Export to XML file
editControl1.SaveAsXML("output.xml");

// Get XML as string
string xmlContent = editControl1.GetTextAsXML();

// Export range to XML
string xmlRange = editControl1.GetTextAsXML(
    new Point(1, 1),    // Start
    new Point(10, 50)   // End
);
```

**VB.NET:**
```vb
' Export to XML file
editControl1.SaveAsXML("output.xml")

' Get XML as string
Dim xmlContent As String = editControl1.GetTextAsXML()

' Export range to XML
Dim xmlRange As String = editControl1.GetTextAsXML(
    New Point(1, 1),
    New Point(10, 50)
)
```

### Export to HTML

**C#:**
```csharp
// Export to HTML file (preserves syntax highlighting)
editControl1.SaveAsHTML("output.html");

// Get HTML as string
string htmlContent = editControl1.GetTextAsHTML();

// Export range to HTML
string htmlRange = editControl1.GetTextAsHTML(
    new Point(1, 1),
    new Point(10, 50)
);
```

### Export to RTF

**C#:**
```csharp
// Export to RTF file (preserves formatting)
editControl1.SaveAsRTF("output.rtf");

// Get RTF as string
string rtfContent = editControl1.GetTextAsRTF();

// Export range to RTF
string rtfRange = editControl1.GetTextAsRTF(
    new Point(1, 1),
    new Point(10, 50)
);
```

### Generate Bitmap

**C#:**
```csharp
// Create bitmap of editor content
Bitmap editorBitmap = editControl1.CreateBitmap();

// Save bitmap
editorBitmap.Save("editor_screenshot.png", ImageFormat.Png);
```

## Printing

### Basic Printing

**C#:**
```csharp
// Print with dialog
editControl1.Print();

// Print without dialog (use default printer)
editControl1.PrintNoDialog();

// Print current page
editControl1.PrintCurrentPage();

// Print selection only
editControl1.PrintSelection();

// Print page range
editControl1.PrintPages(1, 10);  // Pages 1 to 10
```

**VB.NET:**
```vb
' Print with dialog
editControl1.Print()

' Print without dialog
editControl1.PrintNoDialog()

' Print current page
editControl1.PrintCurrentPage()

' Print selection only
editControl1.PrintSelection()

' Print page range
editControl1.PrintPages(1, 10)
```

### Print Preview

**C#:**
```csharp
// Show print preview dialog
editControl1.PrintPreview();
```

### Headers and Footers

**C#:**
```csharp
// Enable headers and footers
editControl1.PageHeaderAndFooterVisible = true;

// Print document name in header
editControl1.PrintDocumentName = true;

// Print page numbers in footer
editControl1.PrintPageNumber = true;

// Custom header
editControl1.PrintHeader += (sender, e) =>
{
    e.Text = $"Company Name - {DateTime.Now:yyyy-MM-dd}";
};

// Custom footer
editControl1.PrintFooter += (sender, e) =>
{
    e.Text = $"Page {e.PageNumber} - Confidential";
};
```

### Page Borders

**C#:**
```csharp
// Add page border
editControl1.SetPageBorder(
    FrameBorderStyle.Solid,  // Style: Solid, Dashed, Dotted, etc.
    Color.Black,              // Color
    BorderWeight.Normal       // Weight: Thin, Normal, Bold, Heavy
);

// Remove page border
editControl1.RemovePageBorder();
```

## Complete File Operations Example

**C#:**
```csharp
using Syncfusion.Windows.Forms.Edit;
using Syncfusion.Windows.Forms.Edit.Enums;
using System;
using System.Drawing;
using System.IO;
using System.Text;
using System.Windows.Forms;

public class FileOperationsForm : Form
{
    private EditControl editControl1;
    private MenuStrip menuStrip;
    private string currentFilePath;
    
    public FileOperationsForm()
    {
        InitializeComponent();
        SetupEditor();
        SetupMenu();
    }
    
    private void SetupEditor()
    {
        editControl1 = new EditControl
        {
            Dock = DockStyle.Fill,
            ShowLineNumbers = true
        };
        
        editControl1.TextChanged += (s, e) =>
        {
            UpdateTitle();
        };
        
        // Auto-save on close
        editControl1.Closing += EditControl1_Closing;
        
        this.Controls.Add(editControl1);
    }
    
    private void SetupMenu()
    {
        menuStrip = new MenuStrip();
        
        // File menu
        ToolStripMenuItem fileMenu = new ToolStripMenuItem("&File");
        fileMenu.DropDownItems.Add("&New", null, NewFile_Click);
        fileMenu.DropDownItems.Add("&Open...", null, OpenFile_Click);
        fileMenu.DropDownItems.Add("&Save", null, SaveFile_Click);
        fileMenu.DropDownItems.Add("Save &As...", null, SaveFileAs_Click);
        fileMenu.DropDownItems.Add(new ToolStripSeparator());
        fileMenu.DropDownItems.Add("&Print...", null, Print_Click);
        fileMenu.DropDownItems.Add("Print Pre&view", null, PrintPreview_Click);
        fileMenu.DropDownItems.Add(new ToolStripSeparator());
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
        
        // Export menu
        ToolStripMenuItem exportMenu = new ToolStripMenuItem("&Export");
        exportMenu.DropDownItems.Add("Export as &HTML...", null, ExportHTML_Click);
        exportMenu.DropDownItems.Add("Export as &XML...", null, ExportXML_Click);
        exportMenu.DropDownItems.Add("Export as &RTF...", null, ExportRTF_Click);
        
        menuStrip.Items.Add(fileMenu);
        menuStrip.Items.Add(exportMenu);
        
        this.Controls.Add(menuStrip);
        this.MainMenuStrip = menuStrip;
    }
    
    private void NewFile_Click(object sender, EventArgs e)
    {
        if (PromptSaveIfModified())
        {
            editControl1.New();
            currentFilePath = null;
            UpdateTitle();
        }
    }
    
    private void OpenFile_Click(object sender, EventArgs e)
    {
        if (!PromptSaveIfModified()) return;
        
        OpenFileDialog dialog = new OpenFileDialog
        {
            Filter = "All Files (*.*)|*.*|C# Files (*.cs)|*.cs|VB Files (*.vb)|*.vb|XML Files (*.xml)|*.xml"
        };
        
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            editControl1.LoadFile(dialog.FileName);
            currentFilePath = dialog.FileName;
            
            // Auto-detect and apply language
            ApplyLanguageFromExtension(dialog.FileName);
            
            UpdateTitle();
        }
    }
    
    private void SaveFile_Click(object sender, EventArgs e)
    {
        if (string.IsNullOrEmpty(currentFilePath))
        {
            SaveFileAs_Click(sender, e);
        }
        else
        {
            editControl1.SaveFile(currentFilePath, Encoding.UTF8, NewLineStyle.Windows);
            UpdateTitle();
        }
    }
    
    private void SaveFileAs_Click(object sender, EventArgs e)
    {
        SaveFileDialog dialog = new SaveFileDialog
        {
            Filter = "All Files (*.*)|*.*|C# Files (*.cs)|*.cs|VB Files (*.vb)|*.vb|Text Files (*.txt)|*.txt"
        };
        
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            editControl1.SaveFile(dialog.FileName, Encoding.UTF8, NewLineStyle.Windows);
            currentFilePath = dialog.FileName;
            UpdateTitle();
        }
    }
    
    private void Print_Click(object sender, EventArgs e)
    {
        editControl1.PageHeaderAndFooterVisible = true;
        editControl1.PrintDocumentName = true;
        editControl1.PrintPageNumber = true;
        editControl1.Print();
    }
    
    private void PrintPreview_Click(object sender, EventArgs e)
    {
        editControl1.PageHeaderAndFooterVisible = true;
        editControl1.PrintDocumentName = true;
        editControl1.PrintPageNumber = true;
        editControl1.PrintPreview();
    }
    
    private void ExportHTML_Click(object sender, EventArgs e)
    {
        SaveFileDialog dialog = new SaveFileDialog
        {
            Filter = "HTML Files (*.html)|*.html"
        };
        
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            editControl1.SaveAsHTML(dialog.FileName);
            MessageBox.Show("Exported to HTML successfully!", "Export", 
                MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
    }
    
    private void ExportXML_Click(object sender, EventArgs e)
    {
        SaveFileDialog dialog = new SaveFileDialog
        {
            Filter = "XML Files (*.xml)|*.xml"
        };
        
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            editControl1.SaveAsXML(dialog.FileName);
            MessageBox.Show("Exported to XML successfully!", "Export",
                MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
    }
    
    private void ExportRTF_Click(object sender, EventArgs e)
    {
        SaveFileDialog dialog = new SaveFileDialog
        {
            Filter = "RTF Files (*.rtf)|*.rtf"
        };
        
        if (dialog.ShowDialog() == DialogResult.OK)
        {
            editControl1.SaveAsRTF(dialog.FileName);
            MessageBox.Show("Exported to RTF successfully!", "Export",
                MessageBoxButtons.OK, MessageBoxIcon.Information);
        }
    }
    
    private void ApplyLanguageFromExtension(string filePath)
    {
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
        }
    }
    
    private bool PromptSaveIfModified()
    {
        if (editControl1.IsModified)
        {
            DialogResult result = MessageBox.Show(
                "Do you want to save changes?",
                "Save Changes",
                MessageBoxButtons.YesNoCancel,
                MessageBoxIcon.Question
            );
            
            if (result == DialogResult.Yes)
            {
                SaveFile_Click(this, EventArgs.Empty);
                return true;
            }
            else if (result == DialogResult.Cancel)
            {
                return false;
            }
        }
        return true;
    }
    
    private void EditControl1_Closing(object sender, CloseEventArgs e)
    {
        if (editControl1.IsModified)
        {
            e.Action = SaveChangesAction.ShowDialog;
        }
    }
    
    private void UpdateTitle()
    {
        string fileName = string.IsNullOrEmpty(currentFilePath)
            ? "Untitled"
            : Path.GetFileName(currentFilePath);
        
        this.Text = editControl1.IsModified
            ? $"*{fileName} - File Editor"
            : $"{fileName} - File Editor";
    }
    
    private void InitializeComponent()
    {
        this.Text = "File Editor";
        this.Size = new Size(1000, 750);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
}
```

## Next Steps

- **[Advanced Features](advanced-features.md)** - Split views, find/replace, navigation, status bar, and more advanced capabilities
