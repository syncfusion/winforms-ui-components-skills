# Serialization in Windows Forms CommandBar

## Table of Contents
- [Persistence Overview](#persistence-overview)
- [AppStateSerializer](#appstateserializer)
- [Implementation Workflow](#implementation-workflow)

## Persistence Overview

The CommandBar layout state can be saved and loaded in multiple formats:
- **XML** - Human-readable, file-based storage
- **Binary Format** - Compact file-based storage
- **Windows Registry** - System registry-based storage
- **Isolated Storage** - Application-isolated storage

### Enable persistence

Enable state persistence on the CommandBarController:

```csharp
this.commandBarController1.PersistState = true;
```

When enabled, CommandBar state changes are marked for persistence.

## AppStateSerializer

The `AppStateSerializer` class coordinates serialization behavior for CommandBar.

### Required namespaces

```csharp
using Syncfusion.Runtime.Serialization;
using System.IO;
using System.Xml;
using Microsoft.Win32;
using Syncfusion.Windows.Forms.Tools;
```

### Create XML serializer

```csharp
// Create XML serializer - stores in file
AppStateSerializer xmlSerializer = new AppStateSerializer(
    SerializeMode.XMLFile, 
    @"C:\AppData\CommandBarState");

// Use it to load/save
this.commandBarController1.LoadCommandBarState(xmlSerializer);
// ... user modifies bars ...
this.commandBarController1.SaveCommandBarState(xmlSerializer);
xmlSerializer.PersistNow();
```

### Create Binary serializer

```csharp
// Create binary serializer - compact file storage
AppStateSerializer binarySerializer = new AppStateSerializer(
    SerializeMode.BinaryFile, 
    @"C:\AppData\CommandBarState");

this.commandBarController1.LoadCommandBarState(binarySerializer);
this.commandBarController1.SaveCommandBarState(binarySerializer);
binarySerializer.PersistNow();
```

### Create Registry serializer

```csharp
// Create registry serializer
Microsoft.Win32.RegistryKey regKey = 
    Microsoft.Win32.Registry.CurrentUser.CreateSubKey(
        @"Software\MyApp\CommandBar\State");

AppStateSerializer regSerializer = new AppStateSerializer(
    SerializeMode.WindowsRegistry, 
    regKey);

this.commandBarController1.LoadCommandBarState(regSerializer);
this.commandBarController1.SaveCommandBarState(regSerializer);
regSerializer.PersistNow();
```

### Create IsolatedStorage serializer

```csharp
// Create isolated storage serializer - application-specific
AppStateSerializer isoSerializer = new AppStateSerializer(
    SerializeMode.IsolatedStorage,
    "CommandBarState",
    System.IO.IsolatedStorage.IsolatedStorageScope.User);

this.commandBarController1.LoadCommandBarState(isoSerializer);
this.commandBarController1.SaveCommandBarState(isoSerializer);
isoSerializer.PersistNow();
```

## Implementation Workflow

### Complete serialization example

```csharp
public partial class Form1 : Form
{
    private CommandBarController commandBarController1;
    private string persistenceMode = "XML";
    private RegistryKey rootKey;

    public Form1()
    {
        InitializeComponent();
    }

    private void Form1_Load(object sender, EventArgs e)
    {
        // Initialize CommandBar
        InitializeCommandBar();
        
        // Load saved state
        LoadCommandBarState();
    }

    private void InitializeCommandBar()
    {
        commandBarController1 = new CommandBarController();
        commandBarController1.HostForm = this;
        commandBarController1.PersistState = true;
        
        CommandBar bar1 = new CommandBar();
        bar1.Text = "Main Toolbar";
        commandBarController1.CommandBars.Add(bar1);
    }

    private void LoadCommandBarState()
    {
        // Get persistence preference
        rootKey = Registry.CurrentUser.OpenSubKey("Config");
        persistenceMode = (string)rootKey.GetValue("PersistType") ?? "XML";
        
        // Create appropriate serializer
        AppStateSerializer serializer = CreateSerializer(persistenceMode);
        
        // Load state if available
        if (serializer != null)
        {
            try
            {
                commandBarController1.LoadCommandBarState(serializer);
            }
            catch (Exception ex)
            {
                MessageBox.Show("Error loading state: " + ex.Message);
            }
        }
    }

    private void Form1_FormClosing(object sender, FormClosingEventArgs e)
    {
        // Save current state
        AppStateSerializer serializer = CreateSerializer(persistenceMode);
        
        if (serializer != null)
        {
            commandBarController1.SaveCommandBarState(serializer);
            serializer.PersistNow();
        }
    }

    private AppStateSerializer CreateSerializer(string mode)
    {
        switch (mode)
        {
            case "XML":
                return new AppStateSerializer(
                    SerializeMode.XMLFile, 
                    @"C:\AppData\CommandBarState");

            case "Binary":
                return new AppStateSerializer(
                    SerializeMode.BinaryFile, 
                    @"C:\AppData\CommandBarState");

            case "Registry":
                RegistryKey regKey = Registry.CurrentUser.CreateSubKey(
                    @"Software\MyApp\CommandBar");
                return new AppStateSerializer(
                    SerializeMode.WindowsRegistry, 
                    regKey);

            case "IsolatedStorage":
                return new AppStateSerializer(
                    SerializeMode.IsolatedStorage,
                    "CommandBarState",
                    System.IO.IsolatedStorage.IsolatedStorageScope.User);

            default:
                return null;
        }
    }
}
```

### Multiple serialization format options

```csharp
private AppStateSerializer GetSerializerByFormat(string format)
{
    AppStateSerializer serializer = null;

    switch (format.ToLower())
    {
        case "xml":
            // XML format - human readable
            string xmlPath = Path.Combine(
                Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
                "MyApp", "state.xml");
            serializer = new AppStateSerializer(SerializeMode.XMLFile, xmlPath);
            break;

        case "binary":
            // Binary format - compact
            string binPath = Path.Combine(
                Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
                "MyApp", "state.bin");
            serializer = new AppStateSerializer(SerializeMode.BinaryFile, binPath);
            break;

        case "registry":
            // Registry format - system storage
            RegistryKey key = Registry.CurrentUser.CreateSubKey(
                @"Software\MyApp\UI\CommandBar");
            serializer = new AppStateSerializer(
                SerializeMode.WindowsRegistry, key);
            break;

        case "isolated":
            // Isolated storage - app-specific
            serializer = new AppStateSerializer(
                SerializeMode.IsolatedStorage,
                "UIState",
                System.IO.IsolatedStorage.IsolatedStorageScope.User);
            break;
    }

    return serializer;
}
```

### Selective state persistence

```csharp
private void SaveSelectedBarsState(List<string> barNames)
{
    AppStateSerializer serializer = CreateSerializer("XML");
    
    if (serializer != null)
    {
        // Save only specified bars
        foreach (CommandBar bar in commandBarController1.CommandBars)
        {
            if (barNames.Contains(bar.Text))
            {
                commandBarController1.SaveCommandBarState(serializer);
            }
        }
        
        serializer.PersistNow();
    }
}

private void LoadSelectedBarsState(List<string> barNames)
{
    AppStateSerializer serializer = CreateSerializer("XML");
    
    if (serializer != null)
    {
        try
        {
            commandBarController1.LoadCommandBarState(serializer);
        }
        catch (Exception ex)
        {
            Console.WriteLine("Load error: " + ex.Message);
        }
    }
}
```

### State management with versioning

```csharp
private class StateManager
{
    private string version = "1.0";
    private AppStateSerializer serializer;

    public void SaveState(CommandBarController controller)
    {
        serializer = CreateVersionedSerializer();
        controller.SaveCommandBarState(serializer);
        serializer.PersistNow();
    }

    public void LoadState(CommandBarController controller)
    {
        serializer = CreateVersionedSerializer();
        
        try
        {
            controller.LoadCommandBarState(serializer);
        }
        catch (Exception ex)
        {
            Console.WriteLine("Incompatible version: " + ex.Message);
            // Fall back to default state
        }
    }

    private AppStateSerializer CreateVersionedSerializer()
    {
        string path = Path.Combine(
            Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData),
            "MyApp", $"state_v{version}.xml");
        
        return new AppStateSerializer(SerializeMode.XMLFile, path);
    }
}
```

## State persistence workflow

1. **Initialize**: Create CommandBar and set PersistState = true
2. **Load**: Call LoadCommandBarState in Form_Load event
3. **Use**: User customizes bars (dock, float, resize)
4. **Monitor**: Framework tracks state changes
5. **Save**: Call SaveCommandBarState in Form_Closing event
6. **Persist**: Call PersistNow() to write to storage
7. **Restore**: Next launch loads saved state automatically

## Edge cases and troubleshooting

**Issue: State not loading on application startup**

Solution: Ensure serializer is created with correct path/registry:

```csharp
// Verify path exists for file-based storage
string path = @"C:\AppData\CommandBarState";
if (!Directory.Exists(path))
{
    Directory.CreateDirectory(path);
}

AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.XMLFile, path);
```

**Issue: State corrupted or cannot be loaded**

Solution: Implement fallback mechanism:

```csharp
private void SafeLoadState(CommandBarController controller)
{
    try
    {
        AppStateSerializer serializer = CreateSerializer("XML");
        controller.LoadCommandBarState(serializer);
    }
    catch (Exception ex)
    {
        // Clear corrupted state
        ClearPersistedState();
        MessageBox.Show("State was corrupted and cleared. Using defaults.");
    }
}

private void ClearPersistedState()
{
    string path = @"C:\AppData\CommandBarState";
    if (File.Exists(Path.Combine(path, "state.xml")))
    {
        File.Delete(Path.Combine(path, "state.xml"));
    }
}
```

**Issue: Memory leak with IsolatedStorage**

Solution: Properly dispose serializer:

```csharp
AppStateSerializer serializer = null;

try
{
    serializer = new AppStateSerializer(
        SerializeMode.IsolatedStorage,
        "State",
        IsolatedStorageScope.User);
    
    commandBarController1.SaveCommandBarState(serializer);
    serializer.PersistNow();
}
finally
{
    serializer?.Dispose();
}
```
