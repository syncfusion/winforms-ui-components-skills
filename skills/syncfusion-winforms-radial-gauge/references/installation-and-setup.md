# Installation and Setup

## Assembly References

To use Syncfusion Gauge controls, reference the required assemblies in your Windows Forms project.

### Required Assemblies

```
Syncfusion.Gauge.Windows.dll
Syncfusion.Shared.Base.dll
```

**Why these assemblies:**
- `Syncfusion.Gauge.Windows.dll` - Contains RadialGauge, LinearGauge, and DigitalGauge controls
- `Syncfusion.Shared.Base.dll` - Provides base classes and common functionality

### Framework Support

Gauge controls support:
- .NET Framework 4.5, 4.5.1, 4.6, 4.7, 4.8
- .NET 6.0, .NET 7.0, .NET 8.0

## Installation Methods

### Method 1: NuGet Package Manager

**Via Package Manager Console:**

```powershell
Install-Package Syncfusion.Gauge.Windows
```

**Via NuGet Package Manager UI:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Gauge.Windows"
3. Select package → Install

**Note:** The Syncfusion.Gauge.Windows package includes gauge controls plus additional components.

### Method 2: Assembly Reference (Local Install)

If Syncfusion Essential Studio is installed locally:

1. Right-click project → Add Reference
2. Browse to installation location:
   - **Windows XP:** `C:\Program Files\Syncfusion\Essential Studio\Windows\{version}\precompiledassemblies\{framework version}`
   - **Windows 7/8/10/11:** `C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\{version}\precompiledassemblies\{framework version}`
3. Select required DLLs → OK

**Example path for .NET 4.5:**
```
C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\25.1.35.0\precompiledassemblies\4.5.1\Syncfusion.Gauge.Windows.dll
```

## Namespace Imports

Add these using directives to your code files:

```csharp
using Syncfusion.Windows.Forms.Gauge;
using System.Drawing;
using System.Windows.Forms;
```

**Namespace contents:**
- `Syncfusion.Windows.Forms.Gauge` - All three gauge controls (RadialGauge, LinearGauge, DigitalGauge), enumerations, and supporting classes

## Toolbox Configuration

### Adding Controls to Toolbox

After installing via NuGet or referencing assemblies:

1. Open Visual Studio Toolbox
2. Right-click → Add Tab → Name it "Syncfusion Gauges"
3. Right-click new tab → Choose Items
4. Browse to `Syncfusion.Gauge.Windows.dll`
5. Select all gauge controls → OK

**Toolbox controls added:**
- RadialGauge
- LinearGauge
- DigitalGauge

### Using Designer

Once in toolbox:
1. Drag gauge control onto form
2. Configure via Properties window
3. Preview in Designer

**Designer advantages:**
- Visual property configuration
- Immediate visual feedback
- Property grid with descriptions
- Smart tag actions (if available)

## Sample Browser and Examples

### Accessing Sample Browser

Syncfusion provides comprehensive samples demonstrating gauge features.

**Location:**
- **Windows 7/8/10/11:** 
  ```
  C:\Users\Public\Documents\Syncfusion\Windows\{version}\samples\2.0\GaugeSamples.sln
  ```
- **Sample Browser Application:**
  Start Menu → Syncfusion → Essential Studio → Dashboard → Sample Browser → Windows Forms → Gauge

**Sample categories:**
- RadialGauge samples (speedometer, compass, dashboard)
- LinearGauge samples (progress bars, level indicators)
- DigitalGauge samples (clocks, counters)
- Custom renderer examples
- Data binding demonstrations
- Theme showcases

### Source Code Location

View complete source code at:
```
C:\Program Files (x86)\Syncfusion\Essential Studio\Windows\{version}\Syncfusion.Windows.Forms.Gauge\Samples\
```

## Deployment Requirements

### Required Files for Deployment

When deploying applications using gauge controls, include:

**Assemblies:**
```
Syncfusion.Gauge.Windows.dll
Syncfusion.Shared.Base.dll
Syncfusion.Licensing.dll (for license validation)
```

**Framework dependencies:**
- .NET Framework Runtime (if targeting .NET Framework)
- .NET Desktop Runtime (if targeting .NET 6.0+)

### Licensing

**License key registration** (for applications using Syncfusion controls):

Add to `Program.cs` or `App.xaml.cs` before application startup:

```csharp
// Register Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");

Application.Run(new MainForm());
```

**License key locations:**
- Available from Syncfusion account after purchase
- Community license available for qualifying users
- Trial license auto-generated during trial period

**Without registration:**
- Controls display license banner at runtime
- Full functionality available during trial

## First Gauge Implementation

### Quick Start: RadialGauge in Designer

1. **Add control to form:**
   - Drag RadialGauge from toolbox onto form
   - Position and size (e.g., 300x300)

2. **Configure basic properties (Properties window):**
   ```
   MinimumValue: 0
   MaximumValue: 100
   Value: 50
   MajorDifference: 10
   MinorDifference: 2
   FrameType: HalfCircle
   ShowNeedle: True
   GaugeLabel: "Progress"
   ```

3. **Run application** - Gauge displays immediately

### Quick Start: LinearGauge in Code

```csharp
using Syncfusion.Windows.Forms.Gauge;

public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
        InitializeGauge();
    }

    private void InitializeGauge()
    {
        // Create gauge
        LinearGauge progressBar = new LinearGauge();
        progressBar.Size = new Size(400, 100);
        progressBar.Location = new Point(20, 20);

        // Configure properties
        progressBar.LinearFrameType = LinearFrameType.Horizontal;
        progressBar.MinimumValue = 0;
        progressBar.MaximumValue = 100;
        progressBar.Value = 75;
        progressBar.MajorDifference = 25;
        progressBar.ShowNeedle = true;

        // Add to form
        this.Controls.Add(progressBar);
    }
}
```

### Quick Start: DigitalGauge in Code

```csharp
using Syncfusion.Windows.Forms.Gauge;

private void InitializeDigitalClock()
{
    // Create gauge
    DigitalGauge clock = new DigitalGauge();
    clock.Size = new Size(300, 80);
    clock.Location = new Point(20, 20);

    // Configure properties
    clock.CharacterType = CharacterType.SevenSegment;
    clock.CharacterCount = 8;
    clock.Value = DateTime.Now.ToString("HH:mm:ss");

    // Add to form
    this.Controls.Add(clock);

    // Update timer
    Timer timer = new Timer();
    timer.Interval = 1000;
    timer.Tick += (s, e) => clock.Value = DateTime.Now.ToString("HH:mm:ss");
    timer.Start();
}
```

## Troubleshooting Installation Issues

### Issue: Controls not appearing in Toolbox

**Solutions:**
1. Verify assemblies are referenced in project
2. Clean and rebuild solution
3. Restart Visual Studio
4. Manually add via Choose Items (browse to DLL)
5. Check target framework matches assembly version

### Issue: License banner displayed

**Solutions:**
1. Register license key via `SyncfusionLicenseProvider.RegisterLicense()`
2. Verify license key is valid (not expired)
3. Call registration before any Syncfusion control instantiation
4. Check license key in Syncfusion account dashboard

### Issue: FileNotFoundException at runtime

**Solutions:**
1. Verify `Syncfusion.Gauge.Windows.dll` is copied to output directory
2. Check `Syncfusion.Shared.Base.dll` is also present
3. Set Copy Local = True for both assemblies in References
4. Verify target framework compatibility

### Issue: Designer errors when opening form

**Solutions:**
1. Clean solution, delete bin/obj folders, rebuild
2. Check assembly versions match across all Syncfusion references
3. Verify Visual Studio version supports designer features
4. Fall back to code-based initialization if designer issues persist

### Issue: NuGet package version conflicts

**Solutions:**
1. Use same Syncfusion package version across all packages
2. Update all Syncfusion packages together
3. Check for indirect dependencies requiring updates
4. Use `Package Manager Console` with `-Force` flag if needed

## Next Steps

Once installation is complete:

- **RadialGauge:** Read [radial-gauge.md](radial-gauge.md) for circular gauge implementation
- **LinearGauge:** Read [linear-gauge.md](linear-gauge.md) for bar-style gauges
- **DigitalGauge:** Read [digital-gauge.md](digital-gauge.md) for LED displays
- **Themes:** Read [visual-themes.md](visual-themes.md) for styling options
- **Data Binding:** Read [data-binding.md](data-binding.md) for real-time data integration
