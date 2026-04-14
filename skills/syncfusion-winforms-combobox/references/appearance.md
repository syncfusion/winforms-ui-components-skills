---
layout: post
title: Appearance in Windows Forms ComboBox control | Syncfusion
description: Learn about Appearance support in Syncfusion Windows Forms ComboBox (SfComboBox) control and more details.
platform: windowsforms
control: SfComboBox
documentation: ug
---

# Appearance in Windows Forms ComboBox (SfComboBox)

## Customizing editor appearance

Appearance of the editor portion can be customized by using the `Style.EditorStyle` property that contains all the needed properties for appearance customization.

{% tabs %}
{% highlight c# %}
sfComboBox1.Style.EditorStyle.BackColor = Color.Aqua;
sfComboBox1.Style.EditorStyle.BorderColor = Color.Red;
sfComboBox1.Style.EditorStyle.ForeColor = Color.Blue;
sfComboBox1.Style.EditorStyle.Font = new Font("Arial", 10F, FontStyle.Bold);
{% endhighlight %}
{% highlight vb %}
# Appearance in Windows Forms ComboBox (SfComboBox)

## Customizing editor appearance

Appearance of the editor portion can be customized using `Style.EditorStyle`.

```csharp
sfComboBox1.Style.EditorStyle.BackColor = Color.Aqua;
sfComboBox1.Style.EditorStyle.BorderColor = Color.Red;
sfComboBox1.Style.EditorStyle.ForeColor = Color.Blue;
sfComboBox1.Style.EditorStyle.Font = new Font("Arial", 10F, FontStyle.Bold);
```

```vbnet
sfComboBox1.Style.EditorStyle.BackColor = Color.Aqua
sfComboBox1.Style.EditorStyle.BorderColor = Color.Red
sfComboBox1.Style.EditorStyle.ForeColor = Color.Blue
sfComboBox1.Style.EditorStyle.Font = New Font("Arial", 10F, FontStyle.Bold)
```

![Customizing using EditorStyle](Appearance_images/Appearance_img1.png)

### Customizing non-editing mode appearance

```csharp
sfComboBox1.Style.ReadOnlyEditorStyle.BackColor = Color.Aqua;
sfComboBox1.Style.ReadOnlyEditorStyle.BorderColor = Color.Red;
sfComboBox1.Style.ReadOnlyEditorStyle.ForeColor = Color.Blue;
sfComboBox1.Style.ReadOnlyEditorStyle.Font = new Font("Arial", 10F, FontStyle.Bold);
```

```vbnet
sfComboBox1.Style.ReadOnlyEditorStyle.BackColor = Color.Aqua
sfComboBox1.Style.ReadOnlyEditorStyle.BorderColor = Color.Red
sfComboBox1.Style.ReadOnlyEditorStyle.ForeColor = Color.Blue
sfComboBox1.Style.ReadOnlyEditorStyle.Font = New Font("Arial", 10F, FontStyle.Bold)
```

![Customizing using ReadOnlyEditorStyle](Appearance_images/Appearance_img2.png)

## Customizing drop-down button appearance

```csharp
sfComboBox1.Style.DropDownButtonStyle.BackColor = Color.Blue;
sfComboBox1.Style.DropDownButtonStyle.HoverBackColor = Color.Red;
sfComboBox1.Style.DropDownButtonStyle.PressedBackColor = Color.Aqua;
sfComboBox1.Style.DropDownButtonStyle.FocusedBackColor = Color.Pink;
```

```vbnet
sfComboBox1.Style.DropDownButtonStyle.BackColor = Color.Blue
sfComboBox1.Style.DropDownButtonStyle.HoverBackColor = Color.Red
sfComboBox1.Style.DropDownButtonStyle.PressedBackColor = Color.Aqua
sfComboBox1.Style.DropDownButtonStyle.FocusedBackColor = Color.Pink
```

![customization the DropDownButtonStyle](Appearance_images/Appearance_img3.png)

## Customizing drop-down appearance

```csharp
sfComboBox1.Style.DropDownStyle.GripperBackColor = Color.Aqua;
sfComboBox1.Style.DropDownStyle.GripperForeColor = Color.Blue;
```

```vbnet
sfComboBox1.Style.DropDownStyle.GripperBackColor = Color.Aqua
sfComboBox1.Style.DropDownStyle.GripperForeColor = Color.Blue
```

![customization the DropDownStyle](Appearance_images/Appearance_img4.png)

### Setting image in drop-down list item

```csharp
sfComboBox1.DropDownListView.DrawItem += new EventHandler<Syncfusion.WinForms.ListView.Events.DrawItemEventArgs>(DropDownListView_DrawItem);
private void DropDownListView_DrawItem(object sender, Syncfusion.WinForms.ListView.Events.DrawItemEventArgs e)
{
   if (e.Text == "Spain")
     e.Image = Image.FromFile(@"..\..\Flags\Spain.png");
   else if (e.Text == "Germany")
     e.Image = Image.FromFile(@"..\..\Flags\Germany.png");
   // ... other countries ...
   e.ImageAlignment = ContentAlignment.BottomLeft;
}
```

```vbnet
AddHandler sfComboBox1.DropDownListView.DrawItem, AddressOf DropDownListView_DrawItem
Private Sub DropDownListView_DrawItem(ByVal sender As Object, ByVal e As Syncfusion.WinForms.ListView.Events.DrawItemEventArgs)
   If e.Text = "Spain" Then
     e.Image = Image.FromFile("..\..\Flags\Spain.png")
   End If
   e.ImageAlignment = ContentAlignment.BottomLeft
End Sub
```

![DropDown item with image](Appearance_images/Appearance_img5.png)

## Conditional styling

```csharp
sfComboBox1.DropDownListView.DrawItem += new EventHandler<Syncfusion.WinForms.ListView.Events.DrawItemEventArgs>(DropDownListView_DrawItem);
private void DropDownListView_DrawItem(object sender, Syncfusion.WinForms.ListView.Events.DrawItemEventArgs e)
{
  if((e.ItemData as CountryInfo).Continent == "Asia")
  {
     e.Style.BackColor = Color.Coral;
     e.Style.ForeColor = Color.White;
  }
}
```

```vbnet
AddHandler sfComboBox1.DropDownListView.DrawItem, AddressOf DropDownListView_DrawItem
Private Sub DropDownListView_DrawItem(ByVal sender As Object, ByVal e As Syncfusion.WinForms.ListView.Events.DrawItemEventArgs)
  If (TryCast(e.ItemData, CountryInfo)).Continent = "Asia" Then
     e.Style.BackColor = Color.Coral
     e.Style.ForeColor = Color.White
  End If
End Sub
```

![customization based on conditions](Appearance_images/Appearance_img6.png)

## Themes

Built-in themes:

- Office2016Colorful
- Office2016White
- Office2016DarkGray
- Office2016Black

### Loading theme assembly

Load the theme assembly once at application startup:

```csharp
using Syncfusion.WinForms.Controls;
static class Program
{
    static void Main()
    {
        SfSkinManager.LoadAssembly(typeof(Office2016Theme).Assembly);
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

```vbnet
Imports Syncfusion.WinForms.Controls
Module Program
    <STAThread>
    Sub Main()
        SfSkinManager.LoadAssembly(GetType(Office2016Theme).Assembly)
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Module
```

### Apply theme

```csharp
sfComboBox1.ThemeName = "Office2016Colorful";
```

```vbnet
sfComboBox1.ThemeName = "Office2016Colorful"
```

```csharp
sfComboBox1.ThemeName = "Office2016White";
```

```vbnet
sfComboBox1.ThemeName = "Office2016White"
```

```csharp
// Fixed: DarkGray example should use the DarkGray theme string
sfComboBox1.ThemeName = "Office2016DarkGray";
```

```vbnet
sfComboBox1.ThemeName = "Office2016DarkGray"
```

```csharp
sfComboBox1.ThemeName = "Office2016Black";
```

```vbnet
sfComboBox1.ThemeName = "Office2016Black"
```

![Office2016Black theme appearance](Appearance_images/Appearance_img10.png)
			Application.EnableVisualStyles()
