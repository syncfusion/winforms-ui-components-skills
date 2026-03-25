# Localization

## Culture-Specific Formatting

### Setting Culture
```csharp
using System.Globalization;

// Set chart culture
chartControl1.Culture = new CultureInfo("de-DE");  // German
```

### Number Formatting
```csharp
// Uses current culture for number formatting
chartControl1.PrimaryYAxis.LabelNumberFormat = "N2";  // Respects culture

// German: 1.234,56
// US: 1,234.56
```

### Date Formatting
```csharp
chartControl1.PrimaryXAxis.ValueType = ChartValueType.DateTime;
chartControl1.PrimaryXAxis.DateTimeFormat = "d";  // Short date (culture-specific)

// German: 21.03.2024
// US: 3/21/2024
```

## Custom Axis Labels

### Manual Translation
```csharp
chartControl1.ChartFormatAxisLabel += (sender, e) =>
{
    if (e.AxisOrientation == ChartOrientation.Horizontal)
    {
        // Translate month names
        Dictionary<string, string> translations = new Dictionary<string, string>
        {
            { "January", "Januar" },
            { "February", "Februar" },
            { "March", "März" }
            // ... more translations
        };
        
        if (translations.ContainsKey(e.Label))
        {
            e.Label = translations[e.Label];
        }
    }
};
```

## Resource-Based Localization

Create resource files (e.g., `ChartResources.de-DE.resx`) for text elements:

```csharp
// Set localized strings
chartControl1.Titles[0].Text = Resources.ChartTitle;
chartControl1.PrimaryXAxis.Title = Resources.XAxisTitle;
chartControl1.PrimaryYAxis.Title = Resources.YAxisTitle;
series.Text = Resources.SeriesName;
```

## Right-to-Left Support

```csharp
chartControl1.RightToLeft = RightToLeft.Yes;
```
