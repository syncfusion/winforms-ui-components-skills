# Appearance and Styling

This guide covers visual customization of the MultiColumnComboBox control, including built-in themes, color schemes, and custom styling options.

## When to Read This

Read this reference when:
- Applying consistent visual themes across your application
- Matching the combo box to your brand colors
- Implementing Office-style visual themes
- Customizing colors for specific UI requirements
- Creating dark mode or high-contrast interfaces

## Available Visual Styles

The MultiColumnComboBox provides 9 built-in visual styles through the `Style` property.

### Style Options

| Style | Description | Best For |
|-------|-------------|----------|
| `Office2003` | Classic Office 2003 look | Legacy applications, Windows XP era |
| `OfficeXP` | Office XP theme | Older enterprise applications |
| `VS2005` | Visual Studio 2005 style | Development tools, technical apps |
| `Office2007` | Office 2007 with Aero | Windows Vista/7 applications |
| `Metro` | Modern flat design | Modern, minimalist interfaces |
| `Office2016Colorful` | Office 2016 blue theme | Modern Office-style apps |
| `Office2016White` | Office 2016 light theme | Clean, bright interfaces |
| `Office2016Black` | Office 2016 dark theme | Dark mode applications |
| `Office2016DarkGray` | Office 2016 gray theme | Professional, subdued interfaces |

## Basic Style Application

### Setting a Style

**C#:**
```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

// Office 2016 Colorful (modern blue theme)
multiColumnComboBox1.Style = VisualStyle.Office2016Colorful;

// Office 2016 White (clean light theme)
multiColumnComboBox1.Style = VisualStyle.Office2016White;

// Office 2016 Black (dark theme)
multiColumnComboBox1.Style = VisualStyle.Office2016Black;

// Office 2016 Dark Gray
multiColumnComboBox1.Style = VisualStyle.Office2016DarkGray;

// Metro (flat design)
multiColumnComboBox1.Style = VisualStyle.Metro;

// Office 2007
multiColumnComboBox1.Style = VisualStyle.Office2007;

// Classic styles
multiColumnComboBox1.Style = VisualStyle.Office2003;
multiColumnComboBox1.Style = VisualStyle.OfficeXP;
multiColumnComboBox1.Style = VisualStyle.VS2005;
```

**VB.NET:**
```vbnet
Imports Syncfusion.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

' Office 2016 Colorful (modern blue theme)
multiColumnComboBox1.Style = VisualStyle.Office2016Colorful

' Office 2016 White (clean light theme)
multiColumnComboBox1.Style = VisualStyle.Office2016White

' Office 2016 Black (dark theme)
multiColumnComboBox1.Style = VisualStyle.Office2016Black

' Office 2016 Dark Gray
multiColumnComboBox1.Style = VisualStyle.Office2016DarkGray

' Metro (flat design)
multiColumnComboBox1.Style = VisualStyle.Metro

' Office 2007
multiColumnComboBox1.Style = VisualStyle.Office2007

' Classic styles
multiColumnComboBox1.Style = VisualStyle.Office2003
multiColumnComboBox1.Style = VisualStyle.OfficeXP
multiColumnComboBox1.Style = VisualStyle.VS2005
```

## Office2007 Color Themes

When using `Office2007` style, you can choose from three color schemes.

### Office2007ColorTheme Property

**C#:**
```csharp
using Syncfusion.Windows.Forms;

// Set Office2007 style
multiColumnComboBox1.Style = VisualStyle.Office2007;

// Blue theme (default)
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Blue;

// Silver theme
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Silver;

// Black theme
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Black;
```

**VB.NET:**
```vbnet
Imports Syncfusion.Windows.Forms

' Set Office2007 style
multiColumnComboBox1.Style = VisualStyle.Office2007

' Blue theme (default)
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Blue

' Silver theme
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Silver

' Black theme
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Black
```

### Complete Office2007 Example

**C#:**
```csharp
using System;
using System.Data;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class Office2007Form : Form
{
    private MultiColumnComboBox employeeCombo;
    
    public Office2007Form()
    {
        InitializeComponent();
        SetupOffice2007Combo();
    }
    
    private void SetupOffice2007Combo()
    {
        // Create combo
        employeeCombo = new MultiColumnComboBox
        {
            Location = new System.Drawing.Point(20, 20),
            Size = new System.Drawing.Size(300, 30),
            Style = VisualStyle.Office2007,
            Office2007ColorTheme = Office2007Theme.Blue,
            MultiColumn = true,
            ShowColumnHeader = true
        };
        
        // Bind data
        DataTable employees = CreateEmployeeData();
        employeeCombo.DataSource = employees;
        employeeCombo.DisplayMember = "Name";
        employeeCombo.ValueMember = "ID";
        
        this.Controls.Add(employeeCombo);
    }
    
    private DataTable CreateEmployeeData()
    {
        DataTable dt = new DataTable();
        dt.Columns.Add("ID", typeof(int));
        dt.Columns.Add("Name", typeof(string));
        dt.Columns.Add("Department", typeof(string));
        
        dt.Rows.Add(1, "John Smith", "Engineering");
        dt.Rows.Add(2, "Sarah Johnson", "Marketing");
        dt.Rows.Add(3, "Mike Brown", "Sales");
        
        return dt;
    }
}
```

## Custom Colors with Managed Theme

Apply custom colors to Office2007 style using the `Managed` theme.

### ApplyManagedColors Method

**C#:**
```csharp
using System.Drawing;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class CustomColorForm : Form
{
    private void ApplyCustomColors()
    {
        // Set Office2007 style with Managed theme
        multiColumnComboBox1.Style = VisualStyle.Office2007;
        multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Managed;
        
        // Apply custom color scheme to entire form
        Office2007Colors.ApplyManagedColors(this, Color.Orchid);
        
        // Other custom colors
        // Office2007Colors.ApplyManagedColors(this, Color.DarkOliveGreen);
        // Office2007Colors.ApplyManagedColors(this, Color.Teal);
        // Office2007Colors.ApplyManagedColors(this, Color.Crimson);
    }
}
```

**VB.NET:**
```vbnet
Imports System.Drawing
Imports Syncfusion.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class CustomColorForm
    Inherits Form
    
    Private Sub ApplyCustomColors()
        ' Set Office2007 style with Managed theme
        multiColumnComboBox1.Style = VisualStyle.Office2007
        multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Managed
        
        ' Apply custom color scheme to entire form
        Office2007Colors.ApplyManagedColors(Me, Color.Orchid)
        
        ' Other custom colors
        ' Office2007Colors.ApplyManagedColors(Me, Color.DarkOliveGreen)
        ' Office2007Colors.ApplyManagedColors(Me, Color.Teal)
        ' Office2007Colors.ApplyManagedColors(Me, Color.Crimson)
    End Sub
End Class
```

### Brand Color Example

Apply your organization's brand colors:

**C#:**
```csharp
private void ApplyBrandColors()
{
    // Company brand color
    Color brandColor = Color.FromArgb(0, 120, 215); // Custom blue
    
    // Apply to combo box
    multiColumnComboBox1.Style = VisualStyle.Office2007;
    multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Managed;
    Office2007Colors.ApplyManagedColors(this, brandColor);
    
    // Apply to all combos on form
    foreach (Control ctrl in this.Controls)
    {
        if (ctrl is MultiColumnComboBox combo)
        {
            combo.Style = VisualStyle.Office2007;
            combo.Office2007ColorTheme = Office2007Theme.Managed;
        }
    }
}
```

## Complete Styling Examples

### Example 1: Modern Application (Office 2016 Colorful)

**C#:**
```csharp
using System;
using System.Data;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ModernForm : Form
{
    private MultiColumnComboBox productCombo;
    
    public ModernForm()
    {
        InitializeComponent();
        SetupModernUI();
    }
    
    private void SetupModernUI()
    {
        // Modern Office 2016 look
        productCombo = new MultiColumnComboBox
        {
            Location = new Point(30, 30),
            Size = new Size(400, 30),
            Style = VisualStyle.Office2016Colorful,
            MultiColumn = true,
            ShowColumnHeader = true,
            AlphaBlendSelectionColor = Color.FromArgb(0, 120, 215), // Office blue
            DropDownWidth = 500
        };
        
        // Load data
        DataTable products = CreateProductData();
        productCombo.DataSource = products;
        productCombo.DisplayMember = "Product";
        productCombo.ValueMember = "ID";
        
        this.Controls.Add(productCombo);
        
        // Set form background to match theme
        this.BackColor = Color.White;
    }
    
    private DataTable CreateProductData()
    {
        DataTable dt = new DataTable();
        dt.Columns.Add("ID", typeof(int));
        dt.Columns.Add("Product", typeof(string));
        dt.Columns.Add("Category", typeof(string));
        dt.Columns.Add("Price", typeof(decimal));
        
        dt.Rows.Add(1, "Laptop Pro", "Electronics", 1299.99);
        dt.Rows.Add(2, "Wireless Mouse", "Accessories", 49.99);
        dt.Rows.Add(3, "USB-C Hub", "Accessories", 79.99);
        dt.Rows.Add(4, "4K Monitor", "Electronics", 599.99);
        
        return dt;
    }
}
```

### Example 2: Dark Mode Application

**C#:**
```csharp
using System;
using System.Data;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class DarkModeForm : Form
{
    private MultiColumnComboBox customerCombo;
    
    public DarkModeForm()
    {
        InitializeComponent();
        SetupDarkMode();
    }
    
    private void SetupDarkMode()
    {
        // Dark theme
        customerCombo = new MultiColumnComboBox
        {
            Location = new Point(30, 30),
            Size = new Size(400, 30),
            Style = VisualStyle.Office2016Black,
            MultiColumn = true,
            ShowColumnHeader = true,
            AlphaBlendSelectionColor = Color.FromArgb(60, 60, 60),
            DropDownWidth = 500
        };
        
        // Load data
        DataTable customers = CreateCustomerData();
        customerCombo.DataSource = customers;
        customerCombo.DisplayMember = "Company";
        customerCombo.ValueMember = "ID";
        
        this.Controls.Add(customerCombo);
        
        // Dark form background
        this.BackColor = Color.FromArgb(30, 30, 30);
        this.ForeColor = Color.White;
    }
    
    private DataTable CreateCustomerData()
    {
        DataTable dt = new DataTable();
        dt.Columns.Add("ID", typeof(int));
        dt.Columns.Add("Company", typeof(string));
        dt.Columns.Add("Contact", typeof(string));
        dt.Columns.Add("City", typeof(string));
        
        dt.Rows.Add(1, "Acme Corp", "John Smith", "New York");
        dt.Rows.Add(2, "TechStart Inc", "Sarah Lee", "San Francisco");
        dt.Rows.Add(3, "Global Solutions", "Mike Chen", "Chicago");
        
        return dt;
    }
}
```

### Example 3: Theme Switcher

Allow users to switch themes at runtime:

**C#:**
```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class ThemeSwitcherForm : Form
{
    private MultiColumnComboBox dataCombo;
    private ComboBox themeSwitcher;
    
    public ThemeSwitcherForm()
    {
        InitializeComponent();
        SetupThemeSwitcher();
    }
    
    private void SetupThemeSwitcher()
    {
        // Data combo
        dataCombo = new MultiColumnComboBox
        {
            Location = new System.Drawing.Point(30, 60),
            Size = new System.Drawing.Size(400, 30),
            MultiColumn = true,
            ShowColumnHeader = true
        };
        
        // Theme switcher combo
        themeSwitcher = new ComboBox
        {
            Location = new System.Drawing.Point(30, 20),
            Size = new System.Drawing.Size(200, 30),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        
        // Add theme options
        themeSwitcher.Items.AddRange(new object[]
        {
            "Office 2016 Colorful",
            "Office 2016 White",
            "Office 2016 Black",
            "Office 2016 Dark Gray",
            "Metro",
            "Office 2007 Blue",
            "Office 2007 Silver",
            "Office 2007 Black"
        });
        
        themeSwitcher.SelectedIndex = 0;
        themeSwitcher.SelectedIndexChanged += ThemeSwitcher_SelectedIndexChanged;
        
        // Load sample data
        LoadSampleData();
        
        this.Controls.Add(dataCombo);
        this.Controls.Add(themeSwitcher);
        
        // Apply initial theme
        ApplyTheme("Office 2016 Colorful");
    }
    
    private void ThemeSwitcher_SelectedIndexChanged(object sender, EventArgs e)
    {
        string selectedTheme = themeSwitcher.SelectedItem.ToString();
        ApplyTheme(selectedTheme);
    }
    
    private void ApplyTheme(string themeName)
    {
        switch (themeName)
        {
            case "Office 2016 Colorful":
                dataCombo.Style = VisualStyle.Office2016Colorful;
                break;
            
            case "Office 2016 White":
                dataCombo.Style = VisualStyle.Office2016White;
                break;
            
            case "Office 2016 Black":
                dataCombo.Style = VisualStyle.Office2016Black;
                this.BackColor = System.Drawing.Color.FromArgb(30, 30, 30);
                this.ForeColor = System.Drawing.Color.White;
                break;
            
            case "Office 2016 Dark Gray":
                dataCombo.Style = VisualStyle.Office2016DarkGray;
                break;
            
            case "Metro":
                dataCombo.Style = VisualStyle.Metro;
                break;
            
            case "Office 2007 Blue":
                dataCombo.Style = VisualStyle.Office2007;
                dataCombo.Office2007ColorTheme = Office2007Theme.Blue;
                break;
            
            case "Office 2007 Silver":
                dataCombo.Style = VisualStyle.Office2007;
                dataCombo.Office2007ColorTheme = Office2007Theme.Silver;
                break;
            
            case "Office 2007 Black":
                dataCombo.Style = VisualStyle.Office2007;
                dataCombo.Office2007ColorTheme = Office2007Theme.Black;
                break;
        }
        
        // Refresh the control
        dataCombo.Refresh();
    }
    
    private void LoadSampleData()
    {
        System.Data.DataTable dt = new System.Data.DataTable();
        dt.Columns.Add("ID");
        dt.Columns.Add("Item");
        dt.Columns.Add("Category");
        
        dt.Rows.Add("1", "Item A", "Category 1");
        dt.Rows.Add("2", "Item B", "Category 2");
        dt.Rows.Add("3", "Item C", "Category 1");
        
        dataCombo.DataSource = dt;
        dataCombo.DisplayMember = "Item";
        dataCombo.ValueMember = "ID";
    }
}
```

## Style Recommendations by Application Type

### Enterprise Applications

**Recommended:** Office2016White, Office2016Colorful
- Professional appearance
- Familiar to Office users
- Good readability

```csharp
multiColumnComboBox1.Style = VisualStyle.Office2016White;
```

### Modern Consumer Apps

**Recommended:** Metro, Office2016Colorful
- Clean, flat design
- Contemporary look
- Minimal distractions

```csharp
multiColumnComboBox1.Style = VisualStyle.Metro;
```

### Dark Mode Applications

**Recommended:** Office2016Black, Office2016DarkGray
- Reduced eye strain
- Modern aesthetic
- Popular for development tools

```csharp
multiColumnComboBox1.Style = VisualStyle.Office2016Black;
multiColumnComboBox1.AlphaBlendSelectionColor = Color.FromArgb(60, 60, 60);
```

### Legacy Applications

**Recommended:** Office2003, OfficeXP
- Matches older Windows UI
- Familiar to long-time users
- Good for XP/Vista era apps

```csharp
multiColumnComboBox1.Style = VisualStyle.Office2003;
```

## Best Practices

### Consistency

**Apply same theme to all controls:**
```csharp
private void ApplyThemeToAllControls(VisualStyle style)
{
    foreach (Control ctrl in this.Controls)
    {
        if (ctrl is MultiColumnComboBox combo)
        {
            combo.Style = style;
        }
    }
}
```

### User Preferences

**Remember user's theme choice:**
```csharp
// Save to application settings
Properties.Settings.Default.UserTheme = "Office2016Black";
Properties.Settings.Default.Save();

// Load on startup
string savedTheme = Properties.Settings.Default.UserTheme;
ApplyTheme(savedTheme);
```

### Performance

**Set style once, early:**
```csharp
// Good: Set in Form constructor or Load event
public MyForm()
{
    InitializeComponent();
    multiColumnComboBox1.Style = VisualStyle.Office2016Colorful;
}

// Avoid: Changing style repeatedly
// Don't change style on every paint or timer tick
```

## Troubleshooting

### Theme Not Applying

**Issue:** Style property set but no visual change.

**Solutions:**
1. Call `Refresh()` after setting style:
   ```csharp
   multiColumnComboBox1.Style = VisualStyle.Office2016Black;
   multiColumnComboBox1.Refresh();
   ```
2. Set style before binding data
3. Ensure Syncfusion.Shared.Base assembly is referenced

### Custom Color Not Working

**Issue:** ApplyManagedColors has no effect.

**Solutions:**
1. Verify Office2007 style is set:
   ```csharp
   multiColumnComboBox1.Style = VisualStyle.Office2007;
   ```
2. Set Managed theme:
   ```csharp
   multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Managed;
   ```
3. Call ApplyManagedColors AFTER setting theme:
   ```csharp
   multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Managed;
   Office2007Colors.ApplyManagedColors(this, Color.Orchid);
   ```

### Office2007ColorTheme Has No Effect

**Issue:** Changing Office2007ColorTheme doesn't change appearance.

**Solution:** Ensure Style is set to Office2007:
```csharp
// Required first
multiColumnComboBox1.Style = VisualStyle.Office2007;

// Then set color theme
multiColumnComboBox1.Office2007ColorTheme = Office2007Theme.Silver;
```
