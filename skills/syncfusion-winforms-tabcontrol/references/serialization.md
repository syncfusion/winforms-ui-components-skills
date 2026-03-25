# Serialization

Learn how to persist tab state (active page, tab order, and text) across application sessions using TabControlAdv's built-in serialization support.

## PersistTabState Property

The `PersistTabState` property enables automatic persistence of tab state information.

### Basic Usage

```csharp
// Enable automatic tab state persistence
tabControlAdv1.PersistTabState = true;

// Disable persistence (default)
tabControlAdv1.PersistTabState = false;
```

### What Gets Persisted

When `PersistTabState` is enabled, the following information is automatically saved and restored:

1. **Active Page** - Which tab was selected
2. **Tab Order** - The order of tabs (after drag-and-drop reordering)
3. **Tab Text** - The text of each tab (after editing)

## How Serialization Works

### Automatic Persistence

When `PersistTabState = true`:
- Tab state is automatically saved when the application closes
- Tab state is automatically loaded when the application starts
- Uses Windows registry or application settings (implementation-dependent)

**Example:**
```csharp
public class PersistentTabsForm : Form
{
    private TabControlAdv tabControl;
    
    public PersistentTabsForm()
    {
        InitializeTabControl();
        AddTabs();
    }
    
    private void InitializeTabControl()
    {
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Fill;
        
        // Enable all customization features
        tabControl.LabelEdit = true;
        tabControl.UserMoveTabs = true;
        
        // Enable automatic persistence
        tabControl.PersistTabState = true;
        
        this.Controls.Add(tabControl);
    }
    
    private void AddTabs()
    {
        // Add tabs - state will be restored automatically
        for (int i = 1; i <= 5; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Document {i}";
            tabControl.TabPages.Add(tab);
        }
        
        // If this is first run, no state to restore
        // If user previously reordered/renamed tabs, that state will be restored
    }
}
```

## Use Cases

### Use Case 1: Document Editor

Preserve which document was active and tab order:

```csharp
TabControlAdv documentTabs = new TabControlAdv();
documentTabs.Dock = DockStyle.Fill;
documentTabs.PersistTabState = true;
documentTabs.LabelEdit = true;
documentTabs.UserMoveTabs = true;

// User can:
// - Rename documents
// - Reorder tabs
// - Close and reopen app - state is preserved
```

### Use Case 2: Multi-View Application

Remember which view was active:

```csharp
TabControlAdv viewTabs = new TabControlAdv();
viewTabs.PersistTabState = true;

// Add views
TabPageAdv dashboardTab = new TabPageAdv { Text = "Dashboard" };
TabPageAdv dataTab = new TabPageAdv { Text = "Data" };
TabPageAdv reportsTab = new TabPageAdv { Text = "Reports" };

viewTabs.TabPages.Add(dashboardTab);
viewTabs.TabPages.Add(dataTab);
viewTabs.TabPages.Add(reportsTab);

// User's last selected view will be active on next launch
```

### Use Case 3: Customizable Workspace

Allow users to customize and persist their workspace layout:

```csharp
TabControlAdv workspaceTabs = new TabControlAdv();
workspaceTabs.PersistTabState = true;
workspaceTabs.LabelEdit = true;  // Rename tabs
workspaceTabs.UserMoveTabs = true;  // Reorder tabs

// Users can customize their workspace
// Changes are automatically persisted
```

## Manual Persistence (Alternative Approach)

If you need more control over serialization, implement manual persistence:

### Save Tab State

```csharp
public class TabState
{
    public int SelectedIndex { get; set; }
    public List<string> TabNames { get; set; }
    public List<int> TabOrder { get; set; }
}

private void SaveTabState()
{
    var state = new TabState
    {
        SelectedIndex = tabControlAdv1.SelectedIndex,
        TabNames = new List<string>(),
        TabOrder = new List<int>()
    };
    
    for (int i = 0; i < tabControlAdv1.TabPages.Count; i++)
    {
        state.TabNames.Add(tabControlAdv1.TabPages[i].Text);
        state.TabOrder.Add(i);
    }
    
    // Serialize to JSON or XML
    string json = JsonConvert.SerializeObject(state);
    File.WriteAllText("tabstate.json", json);
}
```

### Load Tab State

```csharp
private void LoadTabState()
{
    if (!File.Exists("tabstate.json"))
        return;
    
    string json = File.ReadAllText("tabstate.json");
    var state = JsonConvert.DeserializeObject<TabState>(json);
    
    // Restore tab names
    for (int i = 0; i < Math.Min(state.TabNames.Count, tabControlAdv1.TabPages.Count); i++)
    {
        tabControlAdv1.TabPages[i].Text = state.TabNames[i];
    }
    
    // Restore selected tab
    if (state.SelectedIndex >= 0 && state.SelectedIndex < tabControlAdv1.TabPages.Count)
    {
        tabControlAdv1.SelectedIndex = state.SelectedIndex;
    }
}
```

### Complete Manual Persistence Example

```csharp
public class ManualPersistenceForm : Form
{
    private TabControlAdv tabControl;
    private const string StateFile = "tabcontrol_state.json";
    
    public ManualPersistenceForm()
    {
        InitializeTabControl();
        AddTabs();
        LoadState();
        
        // Save state on closing
        this.FormClosing += (s, e) => SaveState();
    }
    
    private void InitializeTabControl()
    {
        tabControl = new TabControlAdv();
        tabControl.Dock = DockStyle.Fill;
        tabControl.LabelEdit = true;
        tabControl.UserMoveTabs = true;
        
        // Manual persistence - don't use automatic
        tabControl.PersistTabState = false;
        
        this.Controls.Add(tabControl);
    }
    
    private void AddTabs()
    {
        for (int i = 1; i <= 5; i++)
        {
            TabPageAdv tab = new TabPageAdv();
            tab.Text = $"Tab {i}";
            tab.Name = $"Tab{i}"; // Unique identifier
            tabControl.TabPages.Add(tab);
        }
    }
    
    private void SaveState()
    {
        try
        {
            var state = new
            {
                SelectedIndex = tabControl.SelectedIndex,
                Tabs = tabControl.TabPages.Cast<TabPageAdv>()
                    .Select(t => new { 
                        Name = t.Name, 
                        Text = t.Text 
                    }).ToList()
            };
            
            string json = JsonConvert.SerializeObject(state, Formatting.Indented);
            File.WriteAllText(StateFile, json);
            
            Console.WriteLine("Tab state saved");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error saving state: {ex.Message}");
        }
    }
    
    private void LoadState()
    {
        try
        {
            if (!File.Exists(StateFile))
                return;
            
            string json = File.ReadAllText(StateFile);
            var state = JsonConvert.DeserializeAnonymousType(json, new
            {
                SelectedIndex = 0,
                Tabs = new[] { new { Name = "", Text = "" } }.ToList()
            });
            
            // Restore tab texts
            foreach (var tabInfo in state.Tabs)
            {
                var tab = tabControl.TabPages.Cast<TabPageAdv>()
                    .FirstOrDefault(t => t.Name == tabInfo.Name);
                if (tab != null)
                {
                    tab.Text = tabInfo.Text;
                }
            }
            
            // Restore selected index
            if (state.SelectedIndex >= 0 && state.SelectedIndex < tabControl.TabPages.Count)
            {
                tabControl.SelectedIndex = state.SelectedIndex;
            }
            
            Console.WriteLine("Tab state loaded");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading state: {ex.Message}");
        }
    }
}
```

## Advanced Persistence Scenarios

### Persist to Application Settings

```csharp
// Save to application settings
Properties.Settings.Default.TabSelectedIndex = tabControlAdv1.SelectedIndex;
Properties.Settings.Default.TabNames = string.Join("|", 
    tabControlAdv1.TabPages.Cast<TabPageAdv>().Select(t => t.Text));
Properties.Settings.Default.Save();

// Load from settings
tabControlAdv1.SelectedIndex = Properties.Settings.Default.TabSelectedIndex;
string[] tabNames = Properties.Settings.Default.TabNames.Split('|');
for (int i = 0; i < Math.Min(tabNames.Length, tabControlAdv1.TabPages.Count); i++)
{
    tabControlAdv1.TabPages[i].Text = tabNames[i];
}
```

### Persist to Database

```csharp
public class TabStatePersistence
{
    public void SaveToDatabase(TabControlAdv tabControl, string userId)
    {
        using (var db = new AppDbContext())
        {
            // Delete old state
            var oldState = db.TabStates.FirstOrDefault(s => s.UserId == userId);
            if (oldState != null)
                db.TabStates.Remove(oldState);
            
            // Save new state
            var newState = new TabStateEntity
            {
                UserId = userId,
                SelectedIndex = tabControl.SelectedIndex,
                TabData = JsonConvert.SerializeObject(
                    tabControl.TabPages.Cast<TabPageAdv>()
                        .Select(t => new { t.Name, t.Text }))
            };
            
            db.TabStates.Add(newState);
            db.SaveChanges();
        }
    }
    
    public void LoadFromDatabase(TabControlAdv tabControl, string userId)
    {
        using (var db = new AppDbContext())
        {
            var state = db.TabStates.FirstOrDefault(s => s.UserId == userId);
            if (state == null)
                return;
            
            // Restore state
            var tabs = JsonConvert.DeserializeAnonymousType(state.TabData,
                new[] { new { Name = "", Text = "" } });
            
            foreach (var tabInfo in tabs)
            {
                var tab = tabControl.TabPages.Cast<TabPageAdv>()
                    .FirstOrDefault(t => t.Name == tabInfo.Name);
                if (tab != null)
                    tab.Text = tabInfo.Text;
            }
            
            tabControl.SelectedIndex = state.SelectedIndex;
        }
    }
}
```

## Best Practices

### When to Use Automatic Persistence
- Simple applications with basic tab state
- Single-user desktop applications
- When default registry/settings storage is acceptable

### When to Use Manual Persistence
- Need to persist additional custom data
- Multi-user applications
- Cloud/database storage required
- Complex state management needs
- Cross-platform requirements

### General Guidelines
- Always handle exceptions when loading state
- Provide default behavior if state file is missing/corrupted
- Validate loaded state before applying
- Consider versioning your state format
- Test state persistence across app restarts
- Clear old state when app structure changes

## Testing Persistence

### Test Checklist
1. ✓ Rename tabs, restart app → names preserved
2. ✓ Reorder tabs, restart app → order preserved
3. ✓ Switch active tab, restart app → selection preserved
4. ✓ Delete state file → app starts with defaults
5. ✓ Corrupt state file → app handles gracefully

### Test Example

```csharp
[TestMethod]
public void TestTabStatePersistence()
{
    // Create and configure tab control
    var tabControl = new TabControlAdv();
    tabControl.PersistTabState = true;
    
    // Add tabs
    for (int i = 1; i <= 3; i++)
    {
        var tab = new TabPageAdv { Text = $"Tab {i}" };
        tabControl.TabPages.Add(tab);
    }
    
    // Modify state
    tabControl.SelectedIndex = 2;
    tabControl.TabPages[0].Text = "Modified Tab";
    
    // Simulate app restart
    // (In real app, state would be saved/loaded automatically)
    
    // Verify state restored correctly
    Assert.AreEqual(2, tabControl.SelectedIndex);
    Assert.AreEqual("Modified Tab", tabControl.TabPages[0].Text);
}
```

## Common Issues

### Issue: State Not Persisting
**Solution:** Ensure `PersistTabState = true` is set before tabs are added.

### Issue: State Corrupted
**Solution:** Implement version checking and fallback to defaults.

### Issue: Different Users See Same State
**Solution:** Use user-specific storage or implement manual persistence with user IDs.

### Issue: Too Much Data Being Saved
**Solution:** Only persist essential state. Use manual persistence for fine-grained control.
