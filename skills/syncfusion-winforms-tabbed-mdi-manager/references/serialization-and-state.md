# Serialization & State Management

## Table of Contents
- [Serialization Overview](#serialization-overview)
- [Serialization Formats](#serialization-formats)
- [Save & Load Methods](#save--load-methods)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Serialization Overview

Serialization allows you to save and restore the state of your TabbedMDI layout between application sessions. This includes:
- Which documents were open
- Order of tabs
- Which tab group each document belonged to
- Tab group sizes and positions

### AppStateSerializer

The `AppStateSerializer` class handles all serialization operations:

```csharp
using Syncfusion.Runtime.Serialization;

// Create serializer with specific storage medium
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.XMLFile, "AppState");
```

## Serialization Formats

### 1. XML File Format (Recommended for Most Cases)

Store state in human-readable XML files:

```csharp
// Save state
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.XMLFile, "ApplicationState");
tabbedMDIManager.SaveTabGroupStates(serializer);
serializer.PersistNow();  // Writes to disk

// Load state
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.XMLFile, "ApplicationState");
tabbedMDIManager.LoadTabGroupStates(serializer);
```

**Advantages:**
- Human-readable files
- Easy to debug
- Can be edited manually if needed

**Storage Location:** Typically `C:\Users\[Username]\AppData\Local\[Company]\[AppName]\`

### 2. Binary File Format (Compact, Fast)

Store state in compressed binary format:

```csharp
// Save state
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.BinaryFile, "AppState");
tabbedMDIManager.SaveTabGroupStates(serializer);
serializer.PersistNow();

// Load state
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.BinaryFile, "AppState");
tabbedMDIManager.LoadTabGroupStates(serializer);
```

**Advantages:**
- Smaller file size
- Faster read/write
- Not human-readable (more secure if needed)

### 3. Isolated Storage (Sandboxed, Secure)

Store state in Windows Isolated Storage (sandbox environment):

```csharp
// Save state
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.IsolatedStorage, "AppState");
tabbedMDIManager.SaveTabGroupStates(serializer);
serializer.PersistNow();

// Load state
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.IsolatedStorage, "AppState");
tabbedMDIManager.LoadTabGroupStates(serializer);
```

**Advantages:**
- Sandboxed by user and application
- Automatically cleaned up with app
- Good for ClickOnce deployments

### 4. Memory Stream (In-Memory)

Store state in memory (useful for testing or temporary storage):

```csharp
// Save to memory
System.IO.MemoryStream ms = new System.IO.MemoryStream();
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.BinaryFmtStream, ms);
tabbedMDIManager.SaveTabGroupStates(serializer);
serializer.PersistNow();

// Load from memory
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.BinaryFmtStream, ms);
tabbedMDIManager.LoadTabGroupStates(serializer);
```

**Advantages:**
- Fast (in-memory)
- No disk I/O
- Good for testing

## Save & Load Methods

### SaveTabGroupStates Method

```csharp
// Save current tab layout
public void SaveApplicationState()
{
    try
    {
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLFile, "ApplicationState");

        tabbedMDIManager.SaveTabGroupStates(serializer);
        serializer.PersistNow();

        Console.WriteLine("Application state saved successfully");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error saving state: {ex.Message}");
    }
}
```

### LoadTabGroupStates Method

```csharp
// Restore previous tab layout
public void LoadApplicationState()
{
    try
    {
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLFile, "ApplicationState");

        tabbedMDIManager.LoadTabGroupStates(serializer);

        Console.WriteLine("Application state restored successfully");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error loading state: {ex.Message}");
    }
}
```

### ClearSavedTabGroupState Method

```csharp
// Clear saved state (reset to default)
public void ResetApplicationState()
{
    try
    {
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLFile, "ApplicationState");

        tabbedMDIManager.ClearSavedTabGroupState(serializer);
        serializer.PersistNow();

        Console.WriteLine("Saved state cleared");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error clearing state: {ex.Message}");
    }
}
```

## Singleton Pattern

### Using AppStateSerializer Singleton

For simplicity, initialize AppStateSerializer once at app startup:

```csharp
public partial class App : Form
{
    public static void Main()
    {
        // Initialize singleton at app start
        AppStateSerializer.InitializeSingleton(SerializeMode.XMLFile, "AppState");

        Application.Run(new MainForm());
    }
}

// Later, use default serializer
private void SaveState()
{
    tabbedMDIManager.SaveTabGroupStates();  // Uses default singleton
}

private void LoadState()
{
    tabbedMDIManager.LoadTabGroupStates();  // Uses default singleton
}
```

## Complete Examples

### Example 1: Basic Save & Load

```csharp
public partial class BasicSerializationForm : Form
{
    private TabbedMDIManager tabbedMDI;

    public BasicSerializationForm()
    {
        InitializeComponent();
        SetupMDI();
    }

    private void SetupMDI()
    {
        this.IsMdiContainer = true;
        this.Text = "MDI with Save/Load";

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        // Load previous state on startup
        LoadApplicationState();

        CreateMenu();
        FormClosed += (s, e) => SaveApplicationState();
    }

    private void CreateMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem fileMenu = menu.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New Document", null, (s, e) => CreateNewDocument());
        fileMenu.DropDownItems.Add("&Save Layout", null, (s, e) => SaveApplicationState());
        fileMenu.DropDownItems.Add("&Reset Layout", null, (s, e) => ResetApplicationState());
        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
    }

    private void CreateNewDocument()
    {
        Form doc = new Form();
        doc.Text = $"Document {this.MdiChildren.Length + 1}";
        doc.MdiParent = this;
        doc.Show();
    }

    private void SaveApplicationState()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile, "MyAppState");

            tabbedMDI.SaveTabGroupStates(serializer);
            serializer.PersistNow();

            Console.WriteLine("✓ Layout saved");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Save error: {ex.Message}");
        }
    }

    private void LoadApplicationState()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile, "MyAppState");

            tabbedMDI.LoadTabGroupStates(serializer);

            Console.WriteLine("✓ Layout restored");
        }
        catch
        {
            // No saved state yet - first run
            CreateInitialDocuments();
        }
    }

    private void ResetApplicationState()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile, "MyAppState");

            tabbedMDI.ClearSavedTabGroupState(serializer);
            serializer.PersistNow();

            // Clear current documents
            foreach (Form doc in this.MdiChildren.ToArray())
            {
                doc.Close();
            }

            CreateInitialDocuments();
            Console.WriteLine("✓ Layout reset to default");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Reset error: {ex.Message}");
        }
    }

    private void CreateInitialDocuments()
    {
        for (int i = 1; i <= 3; i++)
        {
            CreateNewDocument();
        }
    }
}
```

### Example 2: Session Recovery

```csharp
public partial class SessionRecoveryForm : Form
{
    private TabbedMDIManager tabbedMDI;
    private List<string> openDocuments = new List<string>();

    public SessionRecoveryForm()
    {
        InitializeComponent();
        SetupMDI();
    }

    private void SetupMDI()
    {
        this.IsMdiContainer = true;
        this.Text = "Session Recovery Demo";

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        // Track document changes
        tabbedMDI.BeforeMDIChildAdded += (s, e) =>
        {
            Form form = e.NewControl as Form;
            if (form != null && !openDocuments.Contains(form.Text))
            {
                openDocuments.Add(form.Text);
            }
        };

        // Load previous session or create new
        if (!TryRecoverSession())
        {
            CreateInitialDocuments();
        }

        CreateMenu();
        FormClosed += (s, e) => SaveSession();
    }

    private bool TryRecoverSession()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile, "SessionState");

            tabbedMDI.LoadTabGroupStates(serializer);

            MessageBox.Show("Previous session recovered!", "Recovery");
            return true;
        }
        catch
        {
            return false;  // No previous session
        }
    }

    private void SaveSession()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile, "SessionState");

            tabbedMDI.SaveTabGroupStates(serializer);
            serializer.PersistNow();

            Console.WriteLine("Session saved for recovery");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Session save error: {ex.Message}");
        }
    }

    private void CreateMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem fileMenu = menu.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New Document", null, (s, e) => CreateNewDocument());
        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
    }

    private void CreateNewDocument()
    {
        Form doc = new Form();
        string docName = $"Document {openDocuments.Count + 1}";
        doc.Text = docName;
        doc.MdiParent = this;
        doc.Show();
    }

    private void CreateInitialDocuments()
    {
        for (int i = 1; i <= 2; i++)
        {
            CreateNewDocument();
        }
    }
}
```

### Example 3: Multiple Serialization Formats

```csharp
public partial class MultiFormatSerializationForm : Form
{
    private TabbedMDIManager tabbedMDI;

    public MultiFormatSerializationForm()
    {
        InitializeComponent();
        SetupMDI();
    }

    private void SetupMDI()
    {
        this.IsMdiContainer = true;
        this.Text = "Multi-Format Serialization";

        tabbedMDI = new TabbedMDIManager();
        this.Controls.Add(tabbedMDI);
        tabbedMDI.AttachToMdiContainer(this);
        tabbedMDI.ThemesEnabled = true;

        CreateMenu();
        CreateInitialDocuments();
    }

    private void CreateMenu()
    {
        MenuStrip menu = new MenuStrip();
        this.Controls.Add(menu);
        this.MainMenuStrip = menu;

        ToolStripMenuItem fileMenu = menu.Items.Add("&File") as ToolStripMenuItem;
        fileMenu.DropDownItems.Add("&New Document", null, (s, e) => CreateNewDocument());

        ToolStripMenuItem saveMenu = fileMenu.DropDownItems.Add("&Save As...") as ToolStripMenuItem;
        saveMenu.DropDownItems.Add("&XML Format", null, (s, e) => SaveAsXML());
        saveMenu.DropDownItems.Add("&Binary Format", null, (s, e) => SaveAsBinary());
        saveMenu.DropDownItems.Add("&Isolated Storage", null, (s, e) => SaveAsIsolated());

        ToolStripMenuItem loadMenu = fileMenu.DropDownItems.Add("&Load...") as ToolStripMenuItem;
        loadMenu.DropDownItems.Add("&XML Format", null, (s, e) => LoadFromXML());
        loadMenu.DropDownItems.Add("&Binary Format", null, (s, e) => LoadFromBinary());
        loadMenu.DropDownItems.Add("&Isolated Storage", null, (s, e) => LoadFromIsolated());

        fileMenu.DropDownItems.AddSeparator();
        fileMenu.DropDownItems.Add("E&xit", null, (s, e) => this.Close());
    }

    private void SaveAsXML()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile, "StateXML");
            tabbedMDI.SaveTabGroupStates(serializer);
            serializer.PersistNow();
            MessageBox.Show("State saved as XML", "Success");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
    }

    private void SaveAsBinary()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.BinaryFile, "StateBinary");
            tabbedMDI.SaveTabGroupStates(serializer);
            serializer.PersistNow();
            MessageBox.Show("State saved as Binary", "Success");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
    }

    private void SaveAsIsolated()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.IsolatedStorage, "StateIsolated");
            tabbedMDI.SaveTabGroupStates(serializer);
            serializer.PersistNow();
            MessageBox.Show("State saved to Isolated Storage", "Success");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
    }

    private void LoadFromXML()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile, "StateXML");
            tabbedMDI.LoadTabGroupStates(serializer);
            MessageBox.Show("State loaded from XML", "Success");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
    }

    private void LoadFromBinary()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.BinaryFile, "StateBinary");
            tabbedMDI.LoadTabGroupStates(serializer);
            MessageBox.Show("State loaded from Binary", "Success");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
    }

    private void LoadFromIsolated()
    {
        try
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.IsolatedStorage, "StateIsolated");
            tabbedMDI.LoadTabGroupStates(serializer);
            MessageBox.Show("State loaded from Isolated Storage", "Success");
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
    }

    private void CreateNewDocument()
    {
        Form doc = new Form();
        doc.Text = $"Document {this.MdiChildren.Length + 1}";
        doc.MdiParent = this;
        doc.Show();
    }

    private void CreateInitialDocuments()
    {
        for (int i = 1; i <= 3; i++)
        {
            CreateNewDocument();
        }
    }
}
```

## Best Practices

1. **Save on exit** - Always save state when application closes
2. **Handle errors** - Wrap serialization in try-catch blocks
3. **First run** - Check if saved state exists before loading
4. **XML for development** - Use XML format during development for debugging
5. **Binary for production** - Consider binary format for smaller file sizes
6. **Document list** - Optionally maintain separate list of document paths for content recovery

## Troubleshooting

### Issue: LoadTabGroupStates Throws Exception
**Solution:** Check if saved state file exists first run:
```csharp
if (File.Exists(stateFilePath))
{
    tabbedMDI.LoadTabGroupStates(serializer);
}
```

### Issue: State Loads But Documents Are Empty
**Note:** TabbedMDIManager only saves layout, not document content. You must:
1. Save document content separately
2. Restore content after loading layout

### Issue: Serialization File Grows Large
**Solution:** Use Binary format instead of XML:
```csharp
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.BinaryFile, "AppState");
```
