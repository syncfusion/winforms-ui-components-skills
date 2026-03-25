# Serialization and State Management

DockingManager provides powerful serialization capabilities to save and restore dock layouts. Users can customize window arrangements and have them persist across application sessions.

## Table of Contents
- [Automatic Serialization](#automatic-serialization)
- [Manual Serialization](#manual-serialization)
- [Serialization Formats](#serialization-formats)
- [Selective Serialization](#selective-serialization)
- [Database Storage](#database-storage)
- [Isolated Storage](#isolated-storage)
- [Restore Initial State](#restore-initial-state)
- [Serialization Events](#serialization-events)
- [Dynamic Controls](#dynamic-controls)

## Automatic Serialization

### Enable Auto-Save on Close

```csharp
// Automatically save dock state when application closes
this.dockingManager1.PersistState = true;
```

When `true`, DockingManager automatically saves state to isolated storage on form close and restores it on next launch.

**VB.NET:**

```vb
' Enable automatic persistence
Me.dockingManager1.PersistState = True
```

## Manual Serialization

### Save Dock State

```csharp
// Save to isolated storage (default location)
this.dockingManager1.SaveDockState();
```

This saves the current dock layout to isolated storage.

### Load Dock State

```csharp
// Load from isolated storage
this.dockingManager1.LoadDockState();
```

Restores the previously saved dock layout.

**VB.NET:**

```vb
' Save and load state
Me.dockingManager1.SaveDockState()
Me.dockingManager1.LoadDockState()
```

## Serialization Formats

### XML File Format

```csharp
using System.IO;
using Syncfusion.Runtime.Serialization;

// Create serializer for XML format
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.XMLFile,
    "DockState"
);
serializer.InitializeSingleton();

// Save to XML file
this.dockingManager1.SaveDockState(serializer);

// Later, load from XML file
this.dockingManager1.LoadDockState(serializer);
```

**File location:** Creates `DockState.xml` in application folder.

**VB.NET:**

```vb
' XML serialization
Dim serializer As New AppStateSerializer(SerializeMode.XMLFile, "DockState")
serializer.InitializeSingleton()

' Save and load
Me.dockingManager1.SaveDockState(serializer)
Me.dockingManager1.LoadDockState(serializer)
```

### Binary File Format

```csharp
// Create serializer for binary format
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.BinaryFile,
    "DockState"
);

// Save to binary file
this.dockingManager1.SaveDockState(serializer);

// Load from binary file
this.dockingManager1.LoadDockState(serializer);
```

**File location:** Creates `DockState.bin` in application folder.

Binary format is faster and more compact than XML.

### Memory Stream Format

```csharp
using System.IO;

// Create serializer for binary stream
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.BinaryFmtStream,
    "DockState"
);

// Save to memory stream
this.dockingManager1.SaveDockState(serializer);
```

Use this for database storage or network transmission.

### Isolated Storage

```csharp
// Create serializer for isolated storage
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.IsolatedStorage,
    "DockState"
);

// Save to isolated storage
this.dockingManager1.SaveDockState(serializer);

// Load from isolated storage
this.dockingManager1.LoadDockState(serializer);
```

Isolated storage is user-specific and application-specific storage.

## Selective Serialization

### Save Specific Control

```csharp
// Create serializer
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.XMLFile,
    "ToolboxState"
);
serializer.InitializeSingleton();

// Save only panel1's state
this.dockingManager1.SaveDockState(serializer, panel1);

// Later, restore only panel1
this.dockingManager1.LoadDockState(serializer, panel1);
```

**Use case:** Save individual window states separately.

### Get Serialized Controls

```csharp
// Get list of controls that have saved state
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.XMLFile,
    "DockState"
);
serializer.InitializeSingleton();

Control[] serializedControls = 
    this.dockingManager1.GetSerializedControls(serializer);

foreach (Control ctrl in serializedControls)
{
    Console.WriteLine($"Saved state exists for: {ctrl.Name}");
}
```

**VB.NET:**

```vb
' Get serialized controls
Dim serializedControls As Control() = _
    Me.dockingManager1.GetSerializedControls(serializer)

For Each ctrl As Control In serializedControls
    Console.WriteLine("Saved state for: " & ctrl.Name)
Next
```

## Database Storage

### Save to Database

```csharp
using System.Data.SqlClient;
using System.IO;

// Create binary stream serializer
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.BinaryFmtStream,
    "DockState"
);

// Save state
this.dockingManager1.SaveDockState(serializer);

// Get byte array from PersistenceProvider
byte[] stateData = null;
MemoryStream ms = serializer.PersistenceProvider.GetStateStream(null);
if (ms != null)
{
    stateData = ms.ToArray();
}

// Save to database
using (SqlConnection conn = new SqlConnection(connectionString))
{
    conn.Open();
    SqlCommand cmd = new SqlCommand(
        "INSERT INTO DockLayouts (UserID, LayoutData, SaveDate) " +
        "VALUES (@UserID, @LayoutData, @SaveDate)", conn);
    cmd.Parameters.AddWithValue("@UserID", currentUserID);
    cmd.Parameters.AddWithValue("@LayoutData", stateData);
    cmd.Parameters.AddWithValue("@SaveDate", DateTime.Now);
    cmd.ExecuteNonQuery();
}
```

### Load from Database

```csharp
// Load from database
byte[] stateData = null;

using (SqlConnection conn = new SqlConnection(connectionString))
{
    conn.Open();
    SqlCommand cmd = new SqlCommand(
        "SELECT LayoutData FROM DockLayouts " +
        "WHERE UserID = @UserID ORDER BY SaveDate DESC", conn);
    cmd.Parameters.AddWithValue("@UserID", currentUserID);
    
    object result = cmd.ExecuteScalar();
    if (result != null && result != DBNull.Value)
    {
        stateData = (byte[])result;
    }
}

if (stateData != null)
{
    // Create memory stream from byte array
    MemoryStream ms = new MemoryStream(stateData);
    
    // Create serializer
    AppStateSerializer serializer = new AppStateSerializer(
        SerializeMode.BinaryFmtStream,
        "DockState"
    );
    
    // Set stream
    serializer.PersistenceProvider.SetStateStream(null, ms);
    
    // Load state
    this.dockingManager1.LoadDockState(serializer);
}
```

**VB.NET:**

```vb
' Save to database
Dim serializer As New AppStateSerializer(SerializeMode.BinaryFmtStream, "DockState")
Me.dockingManager1.SaveDockState(serializer)

Dim ms As MemoryStream = serializer.PersistenceProvider.GetStateStream(Nothing)
Dim stateData As Byte() = ms.ToArray()

' Insert into database
' (SQL code here)
```

## Isolated Storage

### Save to Isolated Storage

```csharp
// Default isolated storage location
this.dockingManager1.SaveDockState();

// Or explicitly use isolated storage serializer
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.IsolatedStorage,
    "MyAppDockState"
);

this.dockingManager1.SaveDockState(serializer);
```

**Storage location:** User-specific, app-specific isolated storage (not accessible as a regular file).

### Load from Isolated Storage

```csharp
// Load from default isolated storage
this.dockingManager1.LoadDockState();

// Or from named isolated storage
AppStateSerializer serializer = new AppStateSerializer(
    SerializeMode.IsolatedStorage,
    "MyAppDockState"
);

this.dockingManager1.LoadDockState(serializer);
```

## Restore Initial State

### Load Designer State

```csharp
// Restore to initial designer layout
this.dockingManager1.LoadDesignerDockState();
```

This resets the dock layout to the state defined in the form designer, discarding any runtime changes.

**Use case:** "Reset to Default Layout" button.

### Persist Changes Immediately

```csharp
// Commit pending changes to persistent storage
this.dockingManager1.PersistNow();
```

Forces immediate save when `PersistState` is `true`. Useful before critical operations.

## Serialization Events

### Before/After Load Events

```csharp
// Handle before load
this.dockingManager1.NewDockStateBeginLoad += (s, e) =>
{
    Console.WriteLine("Loading dock state...");
    // Prepare controls, show loading indicator
};

// Handle after load
this.dockingManager1.NewDockStateEndLoad += (s, e) =>
{
    Console.WriteLine("Dock state loaded successfully");
    // Update UI, apply additional settings
};
```

**VB.NET:**

```vb
Private Sub DockingManager1_NewDockStateEndLoad(sender As Object, e As EventArgs) _
    Handles dockingManager1.NewDockStateEndLoad
    
    MessageBox.Show("Layout restored")
End Sub
```

### InitializeControlOnLoad Event

```csharp
// Handle control initialization during load
this.dockingManager1.InitializeControlOnLoad += (s, e) =>
{
    // e.Control is the control being loaded
    // Recreate or initialize the control if it doesn't exist
    
    if (e.Control == null)
    {
        // Control doesn't exist, create it
        Panel newPanel = new Panel();
        newPanel.Name = e.ControlName;
        this.Controls.Add(newPanel);
        e.Control = newPanel;
    }
};
```

**Use case:** Dynamically recreate controls that were serialized but don't exist at load time.

## Dynamic Controls

### Handle Dynamic Children

When docking dynamically created controls:

```csharp
private void SetupDynamicDocking()
{
    // Create controls dynamically
    Panel panel1 = new Panel { Name = "Panel1", BackColor = Color.LightBlue };
    Panel panel2 = new Panel { Name = "Panel2", BackColor = Color.LightGreen };
    
    this.Controls.AddRange(new Control[] { panel1, panel2 });
    
    // Enable docking
    this.dockingManager1.SetEnableDocking(panel1, true);
    this.dockingManager1.SetEnableDocking(panel2, true);
    
    this.dockingManager1.SetDockLabel(panel1, "Dynamic Panel 1");
    this.dockingManager1.SetDockLabel(panel2, "Dynamic Panel 2");
    
    // Dock them
    this.dockingManager1.DockControl(panel1, this, DockingStyle.Left, 200);
    this.dockingManager1.DockControl(panel2, this, DockingStyle.Right, 200);
    
    // Handle control recreation on load
    this.dockingManager1.InitializeControlOnLoad += 
        DockingManager1_InitializeControlOnLoad;
}

private void DockingManager1_InitializeControlOnLoad(object sender, 
    InitializeControlOnLoadEventArgs e)
{
    // Recreate controls that were saved but don't exist
    if (e.Control == null)
    {
        Panel newPanel = new Panel { Name = e.ControlName };
        
        // Set properties based on name
        if (e.ControlName == "Panel1")
        {
            newPanel.BackColor = Color.LightBlue;
        }
        else if (e.ControlName == "Panel2")
        {
            newPanel.BackColor = Color.LightGreen;
        }
        
        this.Controls.Add(newPanel);
        this.dockingManager1.SetEnableDocking(newPanel, true);
        
        e.Control = newPanel;
    }
}
```

## Complete Example

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using System.IO;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Runtime.Serialization;

public class SerializationExample : Form
{
    private DockingManager dockingManager1;
    private Panel toolbox, properties, output;
    private MenuStrip menuStrip;
    private ToolStripMenuItem layoutMenu;
    private string layoutFile = "CustomLayout.xml";
    
    public SerializationExample()
    {
        InitializeComponent();
        SetupDocking();
        SetupMenu();
        LoadSavedLayout();
    }
    
    private void SetupDocking()
    {
        // Create DockingManager
        this.dockingManager1 = new DockingManager(this.components);
        this.dockingManager1.HostControl = this;
        
        // Enable automatic persistence
        this.dockingManager1.PersistState = true;
        
        // Create panels
        toolbox = new Panel { Name = "Toolbox", BackColor = Color.LightBlue };
        properties = new Panel { Name = "Properties", BackColor = Color.LightGreen };
        output = new Panel { Name = "Output", BackColor = Color.LightYellow };
        
        this.Controls.AddRange(new Control[] { toolbox, properties, output });
        
        // Enable docking
        this.dockingManager1.SetEnableDocking(toolbox, true);
        this.dockingManager1.SetEnableDocking(properties, true);
        this.dockingManager1.SetEnableDocking(output, true);
        
        // Set labels
        this.dockingManager1.SetDockLabel(toolbox, "Toolbox");
        this.dockingManager1.SetDockLabel(properties, "Properties");
        this.dockingManager1.SetDockLabel(output, "Output");
        
        // Arrange (default layout)
        this.dockingManager1.DockControl(toolbox, this, DockingStyle.Left, 200);
        this.dockingManager1.DockControl(properties, this, DockingStyle.Right, 250);
        this.dockingManager1.DockControl(output, this, DockingStyle.Bottom, 150);
        
        // Handle serialization events
        this.dockingManager1.NewDockStateBeginLoad += 
            DockingManager1_NewDockStateBeginLoad;
        this.dockingManager1.NewDockStateEndLoad += 
            DockingManager1_NewDockStateEndLoad;
        this.dockingManager1.InitializeControlOnLoad += 
            DockingManager1_InitializeControlOnLoad;
    }
    
    private void SetupMenu()
    {
        // Create menu
        menuStrip = new MenuStrip();
        
        layoutMenu = new ToolStripMenuItem("Layout");
        layoutMenu.DropDownItems.AddRange(new ToolStripItem[]
        {
            new ToolStripMenuItem("Save Layout", null, SaveLayout_Click),
            new ToolStripMenuItem("Load Layout", null, LoadLayout_Click),
            new ToolStripMenuItem("Reset to Default", null, ResetLayout_Click),
            new ToolStripSeparator(),
            new ToolStripMenuItem("Export to File...", null, ExportLayout_Click),
            new ToolStripMenuItem("Import from File...", null, ImportLayout_Click)
        });
        
        menuStrip.Items.Add(layoutMenu);
        this.MainMenuStrip = menuStrip;
        this.Controls.Add(menuStrip);
    }
    
    private void LoadSavedLayout()
    {
        // Load saved layout if exists
        if (File.Exists(layoutFile))
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile,
                Path.GetFileNameWithoutExtension(layoutFile)
            );
            serializer.InitializeSingleton();
            
            this.dockingManager1.LoadDockState(serializer);
        }
    }
    
    private void SaveLayout_Click(object sender, EventArgs e)
    {
        // Save current layout to XML file
        AppStateSerializer serializer = new AppStateSerializer(
            SerializeMode.XMLFile,
            Path.GetFileNameWithoutExtension(layoutFile)
        );
        serializer.InitializeSingleton();
        
        this.dockingManager1.SaveDockState(serializer);
        MessageBox.Show("Layout saved successfully!", "Save Layout");
    }
    
    private void LoadLayout_Click(object sender, EventArgs e)
    {
        // Load layout from XML file
        if (File.Exists(layoutFile))
        {
            AppStateSerializer serializer = new AppStateSerializer(
                SerializeMode.XMLFile,
                Path.GetFileNameWithoutExtension(layoutFile)
            );
            serializer.InitializeSingleton();
            
            this.dockingManager1.LoadDockState(serializer);
            MessageBox.Show("Layout loaded successfully!", "Load Layout");
        }
        else
        {
            MessageBox.Show("No saved layout found.", "Load Layout");
        }
    }
    
    private void ResetLayout_Click(object sender, EventArgs e)
    {
        // Reset to designer layout
        this.dockingManager1.LoadDesignerDockState();
        MessageBox.Show("Layout reset to default!", "Reset Layout");
    }
    
    private void ExportLayout_Click(object sender, EventArgs e)
    {
        // Export layout to user-selected file
        using (SaveFileDialog dlg = new SaveFileDialog())
        {
            dlg.Filter = "XML Files (*.xml)|*.xml|All Files (*.*)|*.*";
            dlg.DefaultExt = "xml";
            
            if (dlg.ShowDialog() == DialogResult.OK)
            {
                AppStateSerializer serializer = new AppStateSerializer(
                    SerializeMode.XMLFile,
                    Path.GetFileNameWithoutExtension(dlg.FileName)
                );
                serializer.InitializeSingleton();
                
                this.dockingManager1.SaveDockState(serializer);
                MessageBox.Show($"Layout exported to:\n{dlg.FileName}", 
                    "Export Layout");
            }
        }
    }
    
    private void ImportLayout_Click(object sender, EventArgs e)
    {
        // Import layout from user-selected file
        using (OpenFileDialog dlg = new OpenFileDialog())
        {
            dlg.Filter = "XML Files (*.xml)|*.xml|All Files (*.*)|*.*";
            
            if (dlg.ShowDialog() == DialogResult.OK)
            {
                AppStateSerializer serializer = new AppStateSerializer(
                    SerializeMode.XMLFile,
                    Path.GetFileNameWithoutExtension(dlg.FileName)
                );
                serializer.InitializeSingleton();
                
                this.dockingManager1.LoadDockState(serializer);
                MessageBox.Show($"Layout imported from:\n{dlg.FileName}", 
                    "Import Layout");
            }
        }
    }
    
    private void DockingManager1_NewDockStateBeginLoad(object sender, EventArgs e)
    {
        Console.WriteLine("Loading dock state...");
    }
    
    private void DockingManager1_NewDockStateEndLoad(object sender, EventArgs e)
    {
        Console.WriteLine("Dock state loaded successfully");
        this.Text = "Serialization Example - Layout Restored";
    }
    
    private void DockingManager1_InitializeControlOnLoad(object sender, 
        InitializeControlOnLoadEventArgs e)
    {
        // Recreate controls that don't exist
        if (e.Control == null)
        {
            Panel panel = new Panel { Name = e.ControlName };
            
            // Set properties based on control name
            switch (e.ControlName)
            {
                case "Toolbox":
                    panel.BackColor = Color.LightBlue;
                    break;
                case "Properties":
                    panel.BackColor = Color.LightGreen;
                    break;
                case "Output":
                    panel.BackColor = Color.LightYellow;
                    break;
            }
            
            this.Controls.Add(panel);
            this.dockingManager1.SetEnableDocking(panel, true);
            this.dockingManager1.SetDockLabel(panel, e.ControlName);
            
            e.Control = panel;
        }
    }
    
    protected override void OnFormClosing(FormClosingEventArgs e)
    {
        // Ensure state is saved before closing
        if (this.dockingManager1.PersistState)
        {
            this.dockingManager1.PersistNow();
        }
        
        base.OnFormClosing(e);
    }
}
```

## Best Practices

1. **Use PersistState for simplicity** - Automatic save/restore with no code
2. **Use XML for user editing** - XML files can be manually edited if needed
3. **Use binary for performance** - Faster serialization, smaller files
4. **Handle InitializeControlOnLoad** - Recreate dynamic controls properly
5. **Provide Reset to Default** - Let users restore initial layout
6. **Save to database for multi-user** - Store per-user layouts centrally
7. **Test load failures gracefully** - Handle corrupted or missing layout files
8. **Version your layouts** - Include version info for future compatibility

## Troubleshooting

**Layout not persisting:**
- Verify `PersistState` is `true` or call `SaveDockState()` explicitly
- Check file permissions for save location
- Ensure application closes properly (not killed)

**Load fails silently:**
- Check if save file exists at expected location
- Verify file isn't corrupted (try delete and recreate)
- Handle `NewDockStateEndLoad` event to detect issues

**Dynamic controls not recreating:**
- Implement `InitializeControlOnLoad` event handler
- Set `e.Control` to new control instance
- Ensure control `Name` property matches saved name

**State file too large:**
- Use `SerializeMode.BinaryFile` instead of XML
- Consider saving only specific controls with `SaveDockState(serializer, control)`
- Compress byte array before database storage
