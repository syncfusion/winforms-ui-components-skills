---
layout: post
title: Auto Complete in Windows Forms ComboBox control | Syncfusion
description: Learn about Auto Complete support in Syncfusion Windows Forms ComboBox (SfComboBox) control and more details.
 # Auto Complete in Windows Forms ComboBox (SfComboBox)

Auto complete can be enabled by using the `SfComboBox.AutoCompleteMode` property. Three display modes:

- **Suggest**: Displays suggestions in drop-down list.
- **Append**: Appends the first suggestion to text.
- **SuggestAppend**: Performs both behaviors.

## Auto complete modes

### Suggest

A list of probable matches will be suggested by setting `AutoCompleteMode = AutoCompleteMode.Suggest`.

```csharp
sfComboBox1.AutoCompleteMode = AutoCompleteMode.Suggest;
```

```vbnet
sfComboBox1.AutoCompleteMode = AutoCompleteMode.Suggest
```

![Suggest in WindowsForms AutComplete](autocomplete_images/windowsforms-autocomplete-suggest.png)

### Append

Enable append mode:

```csharp
sfComboBox1.AutoCompleteMode = AutoCompleteMode.Append;
```

```vbnet
sfComboBox1.AutoCompleteMode = AutoCompleteMode.Append
```

![Append in WindowsForms AutoComplete](autocomplete_images/windowsforms-autocomplete-append.png)

### Suggest append

```csharp
sfComboBox1.AutoCompleteMode = AutoCompleteMode.SuggestAppend;
```

```vbnet
sfComboBox1.AutoCompleteMode = AutoCompleteMode.SuggestAppend
```

![Suggest append in WindowsForms AutoComplete](autocomplete_images/windowsforms-autocomplete-suggest-append.png)

## Case sensitivity

By default auto-complete is case-insensitive. Enable with:

```csharp
sfComboBox1.AllowCaseSensitiveOnAutoComplete = true;
```

```vbnet
sfComboBox1.AllowCaseSensitiveOnAutoComplete = True
```

![Filter drop-down items with case sensitive in WindowsForms AutoComplete](autocomplete_images/windowsforms-autocomplete-case-sensitive.png)

## Suggest modes (StartsWith / Contains)

```csharp
sfComboBox1.AutoCompleteSuggestMode = AutoCompleteSuggestMode.Contains;
```

```vbnet
sfComboBox1.AutoCompleteSuggestMode = AutoCompleteSuggestMode.Contains
```

![Suggestion option as contains in WindowsForms AutoComplete](autocomplete_images/windowsforms-autocomplete-suggestion-items.png)

## Suggest delay

```csharp
sfComboBox1.AutoCompleteSuggestDelay = 1000;
```

```vbnet
sfComboBox1.AutoCompleteSuggestDelay = 1000
```

Note: `AutoCompleteSuggestDelay` applies to `Suggest` and `SuggestAppend` modes.
