# Getting Started with MainFrameBarManager

This guide covers the initial setup and assembly configuration needed to use the MainFrameBarManager component in your Windows Forms application.

## Assembly References

Add the following Syncfusion assemblies to your project:

- **Syncfusion.Tools.Base.dll** - Core tools framework
- **Syncfusion.Tools.Windows.dll** - Windows Forms tools implementation
- **Syncfusion.Shared.Base.dll** - Shared base classes
- **Syncfusion.Shared.Windows.dll** - Shared Windows Forms components
- **Syncfusion.Grid.Base.dll** - Grid base infrastructure
- **Syncfusion.Grid.Windows.dll** - Grid Windows Forms implementation
- **Syncfusion.Licensing.dll** - Licensing support
- **Syncfusion.SpellChecker.Base.dll** - SpellChecker framework

### Adding via NuGet

Use the NuGet Package Manager to install Syncfusion WinForms Tools:

```
Install-Package Syncfusion.Tools.Windows
```

This automatically adds required dependencies. Refer to [installation guide](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages) for detailed instructions.

## Namespace Imports

Include the required namespace in your code:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

Or in VB.NET:

```vb
Imports Syncfusion.Windows.Forms.Tools
```

## License Key Registration

Starting with v16.2.0.x, a valid Syncfusion license key is required:

```csharp
// Register license key at application startup
Syncfusion.Licensing.SyncfusionLicensing.RegisterLicense("YOUR_LICENSE_KEY");
```

For trial versions, visit [Licensing Overview](https://help.syncfusion.com/common/essential-studio/licensing/overview) to obtain a temporary key.

## Creating MainFrameBarManager Instance

### Basic Setup

```csharp
// Create instance
MainFrameBarManager mainFrameBarManager1 = new MainFrameBarManager();

// Set visual style
this.mainFrameBarManager1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful;

// Associate with parent form
this.mainFrameBarManager1.Form = this;
```

### VB.NET Equivalent

```vb
' Create instance
Dim mainFrameBarManager1 As New MainFrameBarManager()

' Set visual style
Me.mainFrameBarManager1.Style = Syncfusion.Windows.Forms.VisualStyle.Office2016Colorful

' Associate with parent form
Me.mainFrameBarManager1.Form = Me
```

## Available Visual Styles

The Style property supports multiple professional visual styles:

- **Office2016Colorful** - Modern colorful Office 2016 theme
- **Office2016White** - Clean white Office 2016 theme
- **Office2016Black** - Dark Office 2016 theme
- **Office2016DarkGray** - Gray Office 2016 theme
- **Office2007Blue** - Blue Office 2007 theme
- **Office2007Black** - Black Office 2007 theme
- **Office2007Silver** - Silver Office 2007 theme
- **XPBlue** - Classic XP Blue theme
- **XPSilver** - Classic XP Silver theme
- **XPOlive** - Classic XP Olive theme

## Designer Integration

To add MainFrameBarManager via the designer:

1. Open your Windows Forms designer
2. Locate **MainFrameBarManager** in the toolbox (under Syncfusion Tools)
3. Drag it onto your form
4. All required assembly references are automatically added
5. Configure properties in the Properties panel

## Next Steps

Once you've created the MainFrameBarManager instance:
- Add bars and menu items using [menu-items-via-code.md](menu-items-via-code.md) or [menu-items-via-designer.md](menu-items-via-designer.md)
- Configure menu item types using [menu-item-types.md](menu-item-types.md)
- Add interactive features from [interactive-features.md](interactive-features.md)
- Implement keyboard shortcuts with [keyboard-support.md](keyboard-support.md)
- Enable persistence with [state-persistence-mdi.md](state-persistence-mdi.md)

## Troubleshooting

**Assembly Not Found:** Ensure all Syncfusion assemblies are properly installed via NuGet or added to the project references.

**License Key Error:** Register the license key before creating any Syncfusion components, ideally in Program.Main().

**Designer Not Showing:** Rebuild the project and close/reopen the designer window.

**Control Not Appearing:** Verify the Form property is set to the correct parent form instance.
