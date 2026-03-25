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

chartControl1.Series[0].CalculateStatisticalFormula(
    FinancialFormula.Mean,
    meanSeries
);

chartControl1.Series.Add(meanSeries);
```

### Standard Deviation
```csharp
ChartSeries stdDevSeries = new ChartSeries("Std Dev");
chartControl1.Series[0].CalculateStatisticalFormula(
    FinancialFormula.StandardDeviation,
    stdDevSeries,
    "1"  // Parameter (1 = sample, 0 = population)
);
chartControl1.Series.Add(stdDevSeries);
```

### Moving Average
```csharp
ChartSeries movingAvgSeries = new ChartSeries("Moving Average");
chartControl1.Series[0].CalculateStatisticalFormula(
    FinancialFormula.MovingAverage,
    movingAvgSeries,
    "5"  // 5-period moving average
);
chartControl1.Series.Add(movingAvgSeries);
```

## Common Statistical Patterns

### Trend Line
```csharp
ChartSeries trendSeries = new ChartSeries("Trend");
trendSeries.Type = ChartSeriesType.Line;

chartControl1.Series[0].CalculateStatisticalFormula(
    FinancialFormula.Linear Regression,
    trendSeries
);

trendSeries.Style.Border.DashStyle = DashStyle.Dash;
chartControl1.Series.Add(trendSeries);
```

### Bollinger Bands
```csharp
// Upper band
ChartSeries upperBand = new ChartSeries("Upper Band");
chartControl1.Series[0].CalculateStatisticalFormula(
    FinancialFormula.BollingerBands,
    upperBand,
    "20,2"  // 20-period, 2 standard deviations
);

// Lower band
ChartSeries lowerBand = new ChartSeries("Lower Band");
chartControl1.Series[0].CalculateStatisticalFormula(
    FinancialFormula.BollingerBands,
    lowerBand,
    "20,-2"
);

chartControl1.Series.Add(upperBand);
chartControl1.Series.Add(lowerBand);
```

## Parameters

Most formulas accept parameters as string:
- Single value: `"10"`
- Multiple values: `"20,2"`  (comma-separated)

Refer to formula documentation for specific parameter requirements.
