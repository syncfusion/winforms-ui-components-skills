# Chart Title

## Adding Titles

```csharp
ChartTitle title = new ChartTitle();
title.Text = "Sales Performance 2024";
chartControl1.Titles.Add(title);
```

## Title Appearance

### Font and Color
```csharp
title.Font = new Font("Arial", 14, FontStyle.Bold);
title.ForeColor = Color.Navy;
```

### Alignment
```csharp
title.Alignment = ChartAlignment.Center;
// Options: Near, Center, Far
```

### Position
```csharp
title.Position = ChartDock.Top;  // Top (default), Bottom, Left, Right
```

## Multiple Titles

```csharp
ChartTitle mainTitle = new ChartTitle();
mainTitle.Text = "Annual Sales Report";
mainTitle.Font = new Font("Arial", 16, FontStyle.Bold);

ChartTitle subTitle = new ChartTitle();
subTitle.Text = "Q1 - Q4 2024";
subTitle.Font = new Font("Arial", 11);

chartControl1.Titles.Add(mainTitle);
chartControl1.Titles.Add(subTitle);
```

## Background and Border

```csharp
title.BackColor = Color.LightGray;
title.Border.Color = Color.Black;
title.Border.Width = 1;
```

## Margins

```csharp
title.Margin = new System.Windows.Forms.Padding(10);
```
