# Localization

## Using ILocalizationProvider

```csharp
// Program.cs (or before any form is created)
using System.Globalization;
using System.Threading;
using Syncfusion.Windows.Forms.Localization;

Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");
Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");
LocalizationProvider.Provider = new MyLocalizer(); // register provider
```

```csharp
// MyLocalizer.cs
using System.Globalization;
using Syncfusion.Windows.Forms.Localization;
using Syncfusion.Windows.Forms.ResourceIdentifiers;

public sealed class MyLocalizer : ILocalizationProvider
{
    public string GetLocalizedString(CultureInfo culture, string name, object obj)
    {
        // Map Syncfusion string IDs → localized text (add only what you need)
        return name switch
        {
            ResourceIdentifiers.OK     => "OK",
            ResourceIdentifiers.Cancel => "Abbrechen",
            ResourceIdentifiers.Apply  => "Übernehmen",
            _ => string.Empty // return empty to fall back to Syncfusion defaults
        };
    }
}
```

Registers a custom localizer and returns translated strings for Syncfusion‑defined identifiers; empty string falls back to defaults.

## Using Resource-Based Localization

Create resource files (e.g., `ChartResources.de-DE.resx`) for text elements:

```csharp
// Form1.cs (constructor)
using System.Globalization;
using System.Threading;
using Syncfusion.Windows.Forms.Chart;
using Syncfusion.Windows.Forms.Localization;

public Form1()
{
    Thread.CurrentThread.CurrentCulture  = new CultureInfo("de-DE");
    Thread.CurrentThread.CurrentUICulture = new CultureInfo("de-DE");

    // If your .resx lives in a project folder/namespace called "Resources"
    SharedLocalizationResourceAccessor
        .Instance.SetResources(this.GetType().Assembly, "Resources");

    InitializeComponent();

    // Apply your own .resx strings to chart text elements
    chartControl1.Titles[0].Text        = ChartResources.ChartTitle;   // from ChartResources.de-DE.resx
    chartControl1.PrimaryXAxis.Title    = ChartResources.XAxisTitle;
    chartControl1.PrimaryYAxis.Title    = ChartResources.YAxisTitle;

    ChartSeries series = chartControl1.Series[0];
    series.Text = ChartResources.Series1Name;

    // Culture-aware formatting (labels respect CurrentCulture)
    chartControl1.PrimaryXAxis.ValueType = ChartValueType.DateTime;
    chartControl1.PrimaryXAxis.DateTimeFormat = "d";
}
```

## Right-to-Left Support

```csharp
chartControl1.RightToLeft = RightToLeft.Yes;
```
