# Statistical Formulas

Built-in statistical analysis functions for chart data.

## Available Formulas

- **Mean** - Average value
- **Median** - Middle value
- **Mode** - Most frequent value
- **StandardDeviation** - Data dispersion
- **Variance** - Squared deviation
- **Normal Distribution**
- **T-Test** - Statistical hypothesis testing
- **F-Test** - Variance comparison
- **Z-Test** - Population mean testing

## Applying Formulas

### Mean (Average)
```csharp
ChartSeries meanSeries = new ChartSeries("Mean");
meanSeries.Type = ChartSeriesType.Line;
double calculatedMean = BasicStatisticalFormulas.Mean(meanSeries);
chartControl1.Series.Add(meanSeries);
```

### Standard Deviation
```csharp
ChartSeries stdDevSeries = new ChartSeries("Std Dev");
double calculatedMean = BasicStatisticalFormulas.StandartDeviation(stdDevSeries, false);
chartControl1.Series.Add(stdDevSeries);
```

### Variance
```csharp
ChartSeries varianceSeries = new ChartSeries("Variance");
double calculatedMean = BasicStatisticalFormulas.Variance(varianceSeries, false);
chartControl1.Series.Add(varianceSeries);
```

## Common Statistical Patterns

### Trend Line
```csharp
ChartSeries trendSeries = new ChartSeries("Trend");
trendSeries.Type = ChartSeriesType.Line;

double calculatedMean = BasicStatisticalFormulas.Mean(trendSeries);

trendSeries.Style.Border.DashStyle = DashStyle.Dash;
chartControl1.Series.Add(trendSeries);
```