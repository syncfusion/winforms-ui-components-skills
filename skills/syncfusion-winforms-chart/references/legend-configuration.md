# Legend Configuration

## Table of Contents
- [Enabling Legend](#enabling-legend)
- [Position and Alignment](#position-and-alignment)
- [Legend Appearance](#legend-appearance)
- [Custom Legend Items](#custom-legend-items)
- [Multiple Legends](#multiple-legends)

## Enabling Legend

```csharp
// Show/hide legend
chartControl1.Legend.Visible = true;  // Enabled by default
```

## Position and Alignment

```csharp
// Position (docking)
chartControl1.Legend.Position = ChartDock.Bottom;
// Options: Top, Bottom, Left, Right

// Alignment
chartControl1.LegendAlignment = ChartAlignment.Center;
// Options: Near, Center, Far

// Placement (inside or outside chart area)
chartControl1.LegendsPlacement = ChartPlacement.Outside;
// Options: Inside, Outside
```

### Floating Legend
```csharp
chartControl1.Legend.DockingFree = true;
chartControl1.Legend.Location = new Point(100, 50);  // Custom position
```

## Legend Appearance

### Font and Colors
```csharp
chartControl1.Legend.Font = new Font("Segoe UI", 9);
chartControl1.Legend.ForeColor = Color.Black;
chartControl1.Legend.BackColor = Color.White;
```

### Border
```csharp
chartControl1.Legend.Border.BackColor = Color.Gray;
chartControl1.Legend.Border.ForeColor = Color.Red;
chartControl1.Legend.Border.Width = 1;
```

### Background
```csharp
chartControl1.Legend.BackInterior = new BrushInfo(Color.LightGray);
```

### Item Appearance
```csharp
chartControl1.Legend.ItemsSize = new Size(12, 12);  // Symbol size
chartControl1.Legend.ItemsAlignment = StringAlignment.Near;
```

## Custom Legend Items

Add items not tied to series:

```csharp
ChartLegendItem customItem = new ChartLegendItem();
customItem.Text = "Target Line";
customItem.Interior = new BrushInfo(Color.Red);
customItem.ItemStyle.Symbol.Shape = ChartSymbolShape.VertLine;

chartControl1.Legend.CustomItems = new ChartLegendItem[] { customItem };
```

### Modify Existing Items
```csharp
chartControl1.Legend.FilterItems += (sender, args) =>
{
    foreach (ChartLegendItem item in chartControl1.Legend.Items)
    {
        item.Text = item.Text.ToUpper();  // Customize text
    }
};
```

## Multiple Legends

```csharp
ChartLegend secondLegend = new ChartLegend();
secondLegend.Position = ChartDock.Right;
chartControl1.Legends.Add(secondLegend);

// Assign series to specific legend
series.Legend = secondLegend;
```

## Hide Specific Series from Legend

```csharp
series.LegendItem.Visible = false;
```

## Legend Orientation

```csharp
chartControl1.Legend.Orientation = ChartOrientation.Horizontal;
// Options: Horizontal, Vertical
```
