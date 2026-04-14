---
layout: post
title: CheckBox SelectionMode in Windows Forms ListView | Syncfusion
description: This section explains about the CheckBox SelectionMode support in Syncfusion ListView (SfListView) control, and more.
platform: windowsforms
control: SfListView
documentation: ug
---

# Check Box Selection in Windows Forms ListView (SfListView)
The [Windows Forms ListView](https://www.syncfusion.com/winforms-ui-controls/listview) (SfListView) supports loading the checkBox to each item that allows the user to check or uncheck the corresponding item. You can display the check box in each item by setting the [SfListView.ShowCheckBoxes](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfListView.html#Syncfusion_WinForms_ListView_SfListView_ShowCheckBoxes) property to true.

{% tabs %}
{% highlight c# %}
sfListView1.ShowCheckBoxes = true;
{% endhighlight %}
{% highlight vb %}
sfListView1.ShowCheckBoxes = True
{% endhighlight %}
{% endtabs %}

![CheckBoxSelectionMode_images10](CheckBoxSelectionMode_images/CheckBoxSelectionMode_img10.png)

## Check box selection mode
The check box provides support to select in the context of state of the check box based on the [SfListView.CheckBoxSelectionMode](https://help.syncfusion.com/cr/windowsforms/Syncfusion.WinForms.ListView.SfListView.html#Syncfusion_WinForms_ListView_SfListView_CheckBoxSelectionMode) property.                                                                        

SfListView has the following modes for selection based on the check box state.

* Default: don’t want to affect the selection while checking/unchecking the item CheckBox, you need to set `SfListView.CheckBoxSelectionMode` as Default.

{% capture codesnippet1 %}
{% tabs %}
{% highlight c# %}
sfListView1.CheckBoxSelectionMode = CheckBoxSelectionMode.Default;
{% endhighlight %}
{% highlight vb %}
sfListView1.CheckBoxSelectionMode = CheckBoxSelectionMode.Default
{% endhighlight %}
{% endtabs %}
{% endcapture %}
{{ codesnippet1 | OrderList_Indent_Level_1 }}         

# Checkbox Selection

This reference documents checkbox selection support for `SfListView`.

## Enabling checkboxes

```csharp
sfListView1.ShowCheckBoxes = true;
```

## Checkbox Selection

This reference documents checkbox selection support for `SfListView`.

## Enabling checkboxes

```csharp
sfListView1.ShowCheckBoxes = true;
```

```vbnet
sfListView1.ShowCheckBoxes = True
```

## CheckBoxSelectionMode

Controls how checking interacts with selection. Options include `Default`, `SelectOnCheck`, `SynchronizeSelection`, and `CheckOnItemClick`.

```csharp
sfListView1.CheckBoxSelectionMode = CheckBoxSelectionMode.Default;
```

```vbnet
sfListView1.CheckBoxSelectionMode = CheckBoxSelectionMode.Default
```

## Tri-state mode

Enable tri-state (indeterminate) when the checked state is backed by a `CheckState` property in the data objects.

```csharp
sfListView1.AllowTriStateMode = true;
sfListView1.CheckedMember = "CheckedState";
```

## Clear checked items

```csharp
sfListView1.CheckedItems.Clear();
```

## Select All

```csharp
sfListView1.ShowCheckBoxes = true;
sfListView1.AllowSelectAll = true;
```

## Recursive checking

Enable recursive checking for grouped lists:

```csharp
sfListView1.ShowCheckBoxes = true;
sfListView1.AllowRecursiveChecking = true;
```

## Appearance

Customize checkbox visuals via `sfListView1.Style.CheckBoxStyle`.

```csharp
sfListView1.Style.CheckBoxStyle.CheckedBackColor = Color.BlueViolet;
sfListView1.Style.CheckBoxStyle.CheckedBorderColor = Color.Black;
sfListView1.Style.CheckBoxStyle.CheckedTickColor = Color.White;
```

## DrawCheckBox event

Customize checkbox rendering using `DrawCheckBox`.

```csharp
sfListView1.DrawCheckBox += new EventHandler<DrawCheckBoxEventArgs>(SfListView1_DrawCheckBox);
private void SfListView1_DrawCheckBox(object sender, DrawCheckBoxEventArgs e)
{
     if (e.ItemType == ItemType.Record && e.ItemIndex % 3 == 0)
     {
          // custom drawing
     }
}
```

## Events

### ItemChecking

Raised while an item is being checked (cancelable). Event args include `ItemData`, `ItemIndex`, `NewState`, `OldState`.

```csharp
sfListView1.ItemChecking += new EventHandler<ItemCheckingEventArgs>(SfListView1_ItemChecking);
private void SfListView1_ItemChecking(object sender, ItemCheckingEventArgs e)
{
     // Example: cancel based on data
     if ((sender as SfListView).CheckedItems.Count > 0)
          e.Cancel = true;
}
```

### ItemChecked

Raised after an item has changed checked state.

```csharp
sfListView1.ItemChecked += new EventHandler<Syncfusion.WinForms.ListView.Events.ItemCheckedEventArgs>(SfListView1_ItemChecked);
private void SfListView1_ItemChecked(object sender, Syncfusion.WinForms.ListView.Events.ItemCheckedEventArgs e)
{
     // post-check logic
}
```
