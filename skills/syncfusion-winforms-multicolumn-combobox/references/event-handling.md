# Event Handling in MultiColumnComboBox

This guide covers selection events, event timing, and practical patterns for responding to user interactions with the MultiColumnComboBox control.

## Table of Contents
- [Overview](#overview)
- [Selection Events](#selection-events)
- [Event Timing and Order](#event-timing-and-order)
- [Accessing Selected Data](#accessing-selected-data)
- [Practical Examples](#practical-examples)
- [Event Patterns](#event-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The MultiColumnComboBox provides three primary selection events inherited from ComboBoxAdv:

| Event | When Fired | Typical Use |
|-------|------------|-------------|
| `SelectionChangedCommitted` | When selection is committed (Enter, click, focus loss) | Update UI, save changes |
| `SelectedValueChanged` | When SelectedValue property changes | Track value changes |
| `SelectedIndexChanged` | When SelectedIndex property changes | React to any selection change |

## Selection Events

### SelectionChangedCommitted Event

Fires when the user commits a selection.

**Triggers:**
- User presses Enter key in text area
- User clicks an item in dropdown
- Control loses focus (DropDown mode only)
- Text property changed in code

**C#:**
```csharp
private void Form1_Load(object sender, EventArgs e)
{
    multiColumnComboBox1.SelectionChangedCommitted += MultiColumnComboBox1_SelectionChangedCommitted;
}

private void MultiColumnComboBox1_SelectionChangedCommitted(object sender, EventArgs e)
{
    ComboBoxBaseDataBound combo = sender as ComboBoxBaseDataBound;
    
    if (combo != null && combo.SelectedIndex != -1)
    {
        // Get selected value
        object selectedValue = combo.SelectedValue;
        string displayText = combo.Text;
        
        Console.WriteLine($"Selection committed: {displayText} (Value: {selectedValue})");
        
        // Perform action (update UI, save data, etc.)
        UpdateRelatedControls(selectedValue);
    }
}
```

**VB.NET:**
```vbnet
Private Sub Form1_Load(sender As Object, e As EventArgs)
    AddHandler multiColumnComboBox1.SelectionChangedCommitted, AddressOf MultiColumnComboBox1_SelectionChangedCommitted
End Sub

Private Sub MultiColumnComboBox1_SelectionChangedCommitted(sender As Object, e As EventArgs)
    Dim combo As ComboBoxBaseDataBound = TryCast(sender, ComboBoxBaseDataBound)
    
    If combo IsNot Nothing AndAlso combo.SelectedIndex <> -1 Then
        ' Get selected value
        Dim selectedValue As Object = combo.SelectedValue

        Dim displayText As String = combo.Text
