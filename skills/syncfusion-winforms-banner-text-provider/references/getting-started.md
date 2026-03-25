# Getting Started with BannerTextProvider

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Adding via Designer](#adding-via-designer)
- [Adding via Code](#adding-via-code)
- [Assigning Banner Text](#assigning-banner-text)
- [Basic Configuration](#basic-configuration)

## Assembly Deployment

### NuGet Installation
The BannerTextProvider requires the Syncfusion Windows Forms NuGet package. Install it using the Package Manager:

```powershell
Install-Package Syncfusion.Shared.Base
```

Or use the NuGet Package Manager UI in Visual Studio:
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Shared.Base"
3. Click Install

**Required Assembly:** `Syncfusion.Shared.Base.dll`

### Manual Assembly Reference
If using manual DLL references:
1. In Visual Studio, right-click Project → Add Reference
2. Browse to the Syncfusion installation folder (typically `C:\Program Files\Syncfusion\Essential Studio\Windows\Assemblies`)
3. Select `Syncfusion.Shared.Base.dll`
4. Click Add

**Namespace:** `using Syncfusion.Windows.Forms;`

## Adding via Designer

### Step 1: Open Toolbox
1. In Visual Studio, open the Toolbox (View → Toolbox or Ctrl+Alt+X)
2. Locate **BannerTextProvider** in the Syncfusion Windows Forms components section

### Step 2: Add to Form
1. Drag **BannerTextProvider** from the Toolbox onto your Form
2. The component appears in the component tray (below the form design surface)
3. Syncfusion assemblies are automatically added to the project

### Step 3: Add Editor Control
1. Drag a **TextBoxExt** or other editor control from the Toolbox onto the form
2. The editor control appears as a regular control on the form surface

### Step 4: Configure in Designer
1. Select the editor control (e.g., TextBoxExt)
2. In the Properties panel, find the **bannerTextProvider1.BannerText** property
3. Click the ellipsis (...) button to open the BannerTextInfo editor
4. Enter your banner text and configure appearance
5. Click OK

**Result:** Banner text is automatically displayed in the editor control when empty

## Adding via Code

### Step 1: Import Namespace
```csharp
using Syncfusion.Windows.Forms;
using Syncfusion.Windows.Forms.Tools; // For TextBoxExt and other controls
```

### Step 2: Create BannerTextProvider Instance
Add this code in your Form class (typically in Form_Load or Form constructor):

```csharp
BannerTextProvider bannerTextProvider1 = new BannerTextProvider(this.components);
```

**Note:** Pass `this.components` to ensure proper disposal and component management

### Step 3: Create Editor Control
```csharp
TextBoxExt textBoxExt = new TextBoxExt()
{
    Size = new Size(150, 30),
    Location = new Point(50, 50)
};

this.Controls.Add(textBoxExt);
```

### Step 4: Create BannerTextInfo and Assign
```csharp
BannerTextInfo bannerTextInfo = new BannerTextInfo()
{
    Text = "Type here...",
    Visible = true
};

bannerTextProvider1.SetBannerText(textBoxExt, bannerTextInfo);
```

**Complete Example:**
```csharp
public partial class MainForm : Form
{
    private BannerTextProvider bannerTextProvider1;
    private TextBoxExt textBoxExt;

    public MainForm()
    {
        InitializeComponent();
    }

    private void Form_Load(object sender, EventArgs e)
    {
        // Initialize BannerTextProvider
        bannerTextProvider1 = new BannerTextProvider(this.components);

        // Create TextBoxExt
        textBoxExt = new TextBoxExt()
        {
            Size = new Size(200, 30),
            Location = new Point(20, 20),
            Text = "" // Important: Clear default text
        };
        this.Controls.Add(textBoxExt);

        // Set banner text
        BannerTextInfo bannerInfo = new BannerTextInfo()
        {
            Text = "Enter your name...",
            Visible = true
        };

        bannerTextProvider1.SetBannerText(textBoxExt, bannerInfo);
    }
}
```

## Assigning Banner Text

### Basic Assignment
```csharp
bannerTextProvider1.SetBannerText(textBoxExt, 
    new BannerTextInfo("Enter text...", true));
```

### With Full Configuration
```csharp
BannerTextInfo bannerInfo = new BannerTextInfo()
{
    Text = "Type here...",
    Visible = true,
    Font = new Font("Verdana", 9, FontStyle.Italic),
    Color = Color.Gray
};

bannerTextProvider1.SetBannerText(textBoxExt, bannerInfo);
```

### Clearing Banner Text
```csharp
bannerTextProvider1.SetBannerText(textBoxExt, null);
```

## Basic Configuration

### BannerTextInfo Properties

| Property | Type | Purpose |
|----------|------|---------|
| **Text** | string | The watermark text to display |
| **Visible** | bool | Show/hide the banner text |
| **Color** | Color | Text foreground color |
| **Font** | Font | Text font and style |
| **Mode** | BannerTextMode | FocusMode or EditMode |

### Simple Configuration
```csharp
var banner = new BannerTextInfo("Enter value", true);
bannerTextProvider1.SetBannerText(control, banner);
```

### Configuration with Constructor
```csharp
var banner = new BannerTextInfo(
    text: "Email address",
    visible: true,
    font: new Font("Arial", 9),
    color: Color.Silver,
    mode: BannerTextMode.EditMode
);

bannerTextProvider1.SetBannerText(emailTextBox, banner);
```

## Important Notes

✓ **Always clear Text property** before setting banner text to avoid conflicts
✓ **Use this.components** when creating BannerTextProvider for proper resource management
✓ **Banner text is automatic** - no need to manually hide/show (managed by Mode setting)
✓ **Lightweight component** - can handle multiple controls with single BannerTextProvider instance

---

**Next:** Configure appearance and modes in [banner-text-configuration.md](banner-text-configuration.md)
