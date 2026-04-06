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

### Other Serialization Formats

**Binary File** - Smaller file size, faster read/write:
```csharp
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.BinaryFile, "AppState");
```

**Isolated Storage** - Sandboxed environment, good for ClickOnce:
```csharp
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.IsolatedStorage, "AppState");
```

**Memory Stream** - In-memory, useful for testing:
```csharp
System.IO.MemoryStream ms = new System.IO.MemoryStream();
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.BinaryFmtStream, ms);
```

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

## Complete Example

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
