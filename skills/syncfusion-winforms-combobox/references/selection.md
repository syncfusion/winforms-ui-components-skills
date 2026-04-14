---
layout: post
title: Selection in Windows Forms ComboBox control | Syncfusion
description: Learn about Selection support in Syncfusion Windows Forms ComboBox (SfComboBox) control and more details.
platform: windowsforms
control: SfComboBox
documentation: ug
---

# Selection in Windows Forms ComboBox (SfComboBox)

The [Windows Forms ComboBox](https://www.syncfusion.com/winforms-ui-controls/combobox) (SfComboBox) allows you to select single or multiple items in the drop-down list. The selection mode can be set by using the [ComboBoxMode](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfComboBox.html#Syncfusion_WinForms_ListView_SfComboBox_ComboBoxMode) property. 

Combo box has two different modes:

* SingleSelection: Selects single item.
* MultiSelection: Selects multiple items.

## Single selection

### Getting the selected index

Index of the selected item can be retrieved by using the [SelectedIndex](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfComboBox.html#Syncfusion_WinForms_ListView_SfComboBox_SelectedIndex) property.

### Getting the selected value

Value of the selected item can be retrieved by using the [SelectedValue](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfComboBox.html#Syncfusion_WinForms_ListView_SfComboBox_SelectedValue) property. It returns the property value bind to the [ValueMember](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfComboBox.html#Syncfusion_WinForms_ListView_SfComboBox_ValueMember) property. If the `ValueMember` is not initialized, it will return the value of the property bind to [DisplayMember](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfComboBox.html#Syncfusion_WinForms_ListView_SfComboBox_DisplayMember).
### Getting the selected item of underlying data object

The selected item of the SfComboBox can be retrieved by using the [SelectedItem](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfComboBox.html#Syncfusion_WinForms_ListView_SfComboBox_SelectedItem) property.


# Selection in Windows Forms ComboBox (SfComboBox)

The SfComboBox allows single or multiple selection. Set the selection mode with the `ComboBoxMode` property (`SingleSelection` or `MultiSelection`).

## Single selection

### Getting the selected index

Use the `SelectedIndex` property.

### Getting the selected value

Use the `SelectedValue` property (falls back to `DisplayMember` if `ValueMember` is unset).

### Getting the selected data item

Use the `SelectedItem` property.

### Events

#### SelectedIndexChanged event

The `SelectedIndexChanged` event is raised when the selection changes.

```csharp
sfComboBox1.SelectedIndexChanged += new EventHandler(SfComboBox1_SelectedIndexChanged);
private void SfComboBox1_SelectedIndexChanged(object sender, EventArgs e)
{
}
```

```vbnet
AddHandler sfComboBox1.SelectedIndexChanged, AddressOf SfComboBox1_SelectedIndexChanged
Private Sub SfComboBox1_SelectedIndexChanged(ByVal sender As Object, ByVal e As EventArgs)
End Sub
```

#### SelectedValueChanged event

```csharp
sfComboBox1.SelectedValueChanged += new EventHandler(SfComboBox1_SelectedValueChanged);
private void SfComboBox1_SelectedValueChanged(object sender, EventArgs e)
{
}
```

```vbnet
AddHandler sfComboBox1.SelectedValueChanged, AddressOf SfComboBox1_SelectedValueChanged
Private Sub SfComboBox1_SelectedValueChanged(ByVal sender As Object, ByVal e As EventArgs)
End Sub
```

## Multi-selection

Enable multi-selection:

```csharp
sfComboBox1.ComboBoxMode = ComboBoxMode.MultiSelection;
```

```vbnet
sfComboBox1.ComboBoxMode = ComboBoxMode.MultiSelection
```

![Selecting multiple items](Selection_images/Selection_img1.png)

### Select all

```csharp
sfComboBox1.AllowSelectAll = true;
```

```vbnet
sfComboBox1.AllowSelectAll = True
```

![Selecting all the items in drop-down](Selection_images/Selection_img2.png)

### Changing the delimiter character

```csharp
sfComboBox1.DelimiterChar = "-";
```

```vbnet
sfComboBox1.DelimiterChar = "-"
```

![Custom character to separate the items](Selection_images/Selection_img3.png)

### Hiding the buttons in the drop down

```csharp
sfComboBox1.DropDownControl.ShowButtons = false;
```

```vbnet
sfComboBox1.DropDownControl.ShowButtons = False
```

![Drop-Down without buttons](Selection_images/Selection_img4.png)

### Accessing the checked items

Use the `CheckedItems` property.

### Tooltip

Enable tooltip for multi-select display:

```csharp
sfComboBox1.ShowToolTip = true;
```

```vbnet
sfComboBox1.ShowToolTip = True
```

![ToolTip for selected items](Selection_images/Selection_img5.png)

#### Tooltip options (examples)

```csharp
sfComboBox1.ToolTipOption.InitialDelay = 3000;
sfComboBox1.ToolTipOption.AutoPopDelay = 2000;
sfComboBox1.ToolTipOption.ShadowVisible = false;
```

```vbnet
sfComboBox1.ToolTipOption.InitialDelay = 3000
sfComboBox1.ToolTipOption.AutoPopDelay = 2000
sfComboBox1.ToolTipOption.ShadowVisible = False
```

![ToolTip for the selected items with delay](Selection_images/Selection_img6.png)

#### Styling

```csharp
sfComboBox1.Style.ToolTipStyle.BackColor = Color.Green;
sfComboBox1.Style.ToolTipStyle.ForeColor = Color.White;
sfComboBox1.Style.ToolTipStyle.BorderColor = Color.Red;
sfComboBox1.Style.ToolTipStyle.BorderThickness = 5;
```

```vbnet
sfComboBox1.Style.ToolTipStyle.BackColor = Color.Green
sfComboBox1.Style.ToolTipStyle.ForeColor = Color.White
sfComboBox1.Style.ToolTipStyle.BorderColor = Color.Red
sfComboBox1.Style.ToolTipStyle.BorderThickness = 5
```

![BackColor for the tooltip](Selection_images/Selection_img7.png)

##### Conditional styling

```csharp
sfComboBox1.ToolTipOpening += new EventHandler<ComboBoxToolTipOpeningEventArgs>(SfComboBox1_ToolTipOpening);
private void SfComboBox1_ToolTipOpening(object sender, ComboBoxToolTipOpeningEventArgs e)
{
  if (e.DisplayText.Contains("California"))
  {
     sfComboBox1.Style.ToolTipStyle.ForeColor = Color.Red;
  }
}
```

```vbnet
AddHandler sfComboBox1.ToolTipOpening, AddressOf SfComboBox1_ToolTipOpening
Private Sub SfComboBox1_ToolTipOpening(ByVal sender As Object, ByVal e As ComboBoxToolTipOpeningEventArgs)
  If e.DisplayText.Contains("California") Then
     sfComboBox1.Style.ToolTipStyle.ForeColor = Color.Red
  End If
End Sub
```

![tooltip style based on condition](Selection_images/Selection_img8.png)

#### Canceling tooltip opening

```csharp
sfComboBox1.ToolTipOpening += new EventHandler<ComboBoxToolTipOpeningEventArgs>(SfComboBox1_ToolTipOpening);
private void SfComboBox1_ToolTipOpening(object sender, ComboBoxToolTipOpeningEventArgs e)
{
   e.Cancel = true;
}
```

```vbnet
AddHandler sfComboBox1.ToolTipOpening, AddressOf SfComboBox1_ToolTipOpening
Private Sub SfComboBox1_ToolTipOpening(ByVal sender As Object, ByVal e As ComboBoxToolTipOpeningEventArgs)
   e.Cancel = True
End Sub
```

## Clear selection

Use the `ShowClearButton` property to enable the clear button.

```csharp
sfComboBox1.ShowClearButton = true;
```

```vbnet
sfComboBox1.ShowClearButton = True
```

![WinForms SfComboBox Clear selection](Selection_images/WinForms-SfComboBox-Clear-selection.png)

### Clear button appearance

```csharp
sfComboBox1.Style.ClearButtonStyle.BackColor = Color.Yellow;
sfComboBox1.Style.ClearButtonStyle.ForeColor = Color.Red;
sfComboBox1.Style.ClearButtonStyle.HoverBackColor = Color.OrangeRed;
sfComboBox1.Style.ClearButtonStyle.HoverForeColor = Color.Yellow;
```

![WinForms SfComboBox ClearButton style](Selection_images/WinForms-SfComboBox-ClearButton-style.png)
#### Canceling tooltip opening
