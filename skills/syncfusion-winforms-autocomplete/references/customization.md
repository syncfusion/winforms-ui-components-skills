# Customization in Windows Forms AutoComplete

This section explains about the customization of the AutoComplete component.

## Item customization

### Appearance 

The appearance of the AutoComplete popup window and the items can be customized using the following properties:

| AutoComplete properties | Description |
|------------------------|-------------|
| `HeaderForeColor` | Specifies the HeaderForeColor of the AutoCompletePopup. |
| `TextColor` | Specifies the item text ForeColor of the AutoCompletePopup. |
| `HeaderFont` | Specifies the header font of the AutoCompletePopup. |
| `ItemFont` | Specifies the item font of the AutoCompletePopup. |
| `HeaderStyle` | Specifies the AutoCompletePopup column header style. |

```csharp
// Specifies the header fore color of the AutoComplete popup.
this.autoComplete1.HeaderForeColor = Color.Red;

// Specifies the item fore color of the AutoComplete popup.
this.autoComplete1.TextColor = Color.Blue;

// Specifies the font for ColumnHeader in the AutoComplete popup.
this.autoComplete1.HeaderFont = new System.Drawing.Font("Arial Black", 10.25F);

// Specifies the font of items in the AutoComplete Popup.
this.autoComplete1.ItemFont = new System.Drawing.Font("Monotype Corsiva", 9.25F);

// Specifies the style of ColumnHeader in the AutoComplete popup.
this.autoComplete1.HeaderStyle = ColumnHeaderStyle.Clickable;
```

```vb
'Specifies the header fore color of the AutoComplete popup.
Me.autoComplete1.HeaderForeColor = Color.Red

'Specifies the item fore color of the AutoComplete popup.
Me.autoComplete1.TextColor = Color.Blue

'Specifies the font of the ColumnHeader in the AutoComplete popup.
Me.autoComplete1.HeaderFont = New System.Drawing.Font("Arial Black", 10.25F)

'Specifies the font of the items in the AutoComplete popup.
Me.autoComplete1.ItemFont = New System.Drawing.Font("Monotype Corsiva", 9.25F)

'Specifies the style of ColumnHeader in the AutoComplete popup.
Me.autoComplete1.HeaderStyle = ColumnHeaderStyle.Clickable
```

![Windows Forms AutoComplete Maximum number of suggestions](../images/AutoComplete_ItemCustomization.png)

## Popup customization

### Close button

To show or hide the `CloseButton` at the bottom-left of the DropDownContainer, use the `ShowCloseButton` property. The default value of this property is `true`.

> **Note**: The AutoComplete dropdown can be closed by calling the `CloseDropDown` method.

### Resize gripper

To show or hide the resizing gripper at the bottom-right of the DropDownContainer, use the `ShowGripper` property. The default value of this property is `true`.

### Column Header

To show or hide the column header, use the `ShowColumnHeader` property.

```csharp
this.autoComplete1.ShowCloseButton = false;
this.autoComplete1.ShowGripper = false;
this.autoComplete1.ShowColumnHeader = true;
```

```vb
Me.autoComplete1.ShowCloseButton = False
Me.autoComplete1.ShowGripper = False
Me.autoComplete1.ShowColumnHeader = True
```

### Size Settings

The properties that can control the height and width of the AutoCompletePopup are as follows.

| AutoComplete properties | Description |
|------------------------|-------------|
| `AdjustHeightToItemCount` | Specifies whether the height of the dropdown should be adjusted automatically based on the number of items. |
| `AutoPersistentDropDownSize` | The dropdown size of Autocomplete component is automatically persistent when this property is set to true. |
| `PreferredHeight` | Specifies preferred height for the dropdown displayed by the AutoComplete component when AdjustHeightToItemCount property is false. The default value of this property is 200. |
| `PreferredWidth` | Specifies preferred width for the dropdown displayed by the AutoComplete component when AdjustHeightToItemCount property is false. The default value of this property is -1. |

```csharp
this.autoComplete1.AdjustHeightToItemCount = false;
this.autoComplete1.AutoPersistentDropDownSize = true;
this.autoComplete1.PreferredHeight = 100;
this.autoComplete1.PreferredWidth = 300;
```

```vb
Me.autoComplete1.AdjustHeightToItemCount = False
Me.autoComplete1.AutoPersistentDropDownSize = True
Me.autoComplete1.PreferredHeight = 100
Me.autoComplete1.PreferredWidth = 300
```

## Visual styles 

The built-in themes for professional representation of AutoComplete are as follows.

### Office2019Colorful

Sets the Office2019Colorful theme.

```csharp
this.autoComplete1.ThemeName = "Office2019Colorful";
```

```vb
Me.autoComplete1.ThemeName = "Office2019Colorful"
```

![Windows Forms AutoComplete Office2019Colorful theme](../images/AutoComplete_Office2019Col.png)

### HighContrastBlack

Sets the HighContrastBlack theme.

```csharp
this.autoComplete1.ThemeName = "HighContrastBlack";
```

```vb
Me.autoComplete1.ThemeName = "HighContrastBlack"
```

![Windows Forms AutoComplete HighcontrastBlack theme](../images/AutoComplete_Highcontrast.png)

### Office2016Colorful

Sets the Office2016Colorful theme.

```csharp
this.autoComplete1.Style = AutoCompleteStyle.Office2016Colorful;
```

```vb
Me.autoComplete1.Style = AutoCompleteStyle.Office2016Colorful
```

![Windows Forms AutoComplete Office2016Colorful theme](../images/AutoComplete_Office2016Col.png)

### Office2016White

Sets the Office2016White theme.

```csharp
this.autoComplete1.Style = AutoCompleteStyle.Office2016White;
```

```vb
Me.autoComplete1.Style = AutoCompleteStyle.Office2016White
```

![Windows Forms AutoComplete Office2016White theme](../images/AutoComplete_Office2016white.png)

### Office2016Black

Sets the Office2016Black theme.

```csharp
this.autoComplete1.Style = AutoCompleteStyle.Office2016Black;
```

```vb
Me.autoComplete1.Style = AutoCompleteStyle.Office2016Black
```

### Office2016DarkGray

Sets the Office2016DarkGray theme.

```csharp
this.autoComplete1.Style = AutoCompleteStyle.Office2016DarkGray;
```

```vb
Me.autoComplete1.Style = AutoCompleteStyle.Office2016DarkGray
```

![Windows Forms AutoComplete Office2016DarkGray theme](../images/AutoComplete_Office2016darkgray.png)

### Metro

Sets the Metro theme.

```csharp
this.autoComplete1.Style = AutoCompleteStyle.Metro;
```

```vb
Me.autoComplete1.Style = AutoCompleteStyle.Metro
```

![Windows Forms AutoComplete Metro theme](../images/AutoComplete_Metro.png)

### Default

Sets the default theme.

## Persistence

The history list of AutoComplete component can be saved in the following formats:

* Binary Format
* XML Format
* IsolatedStorage medium
* MemoryStream
* PersistState property

The AutoComplete component has a fully built-in serialization feature that provides automatic serialization for the AutoComplete's history list. The serialization mechanism is implemented using the standardized Syncfusion.Windows.Forms.AppStateSerializer component that acts as a central coordinator for all the Essential tools components and provides options to read or write to different media such as the default isolated storage, XML file, XML stream, binary file, binary stream, and the Windows Registry.

### Persisting AutoComplete's data in the default storage

The data of AutoComplete component can be persisted by setting the `AutoSerialize` property to `true`. It specifies whether the AutoComplete component can persist its data. This information is stored in the isolated storage.

```csharp
this.autoComplete1.AutoSerialize = true;
```

```vb
Me.autoComplete1.AutoSerialize = True
```

### Persisting in XML file

To save and load the AutoComplete data in XML.

```csharp
using Syncfusion.Runtime.Serialization;

// To save
AppStateSerializer aser = new AppStateSerializer(SerializeMode.XMLFile, @"C:\info.xml");
this.autoComplete1.SaveCurrentState(aser);

// To load
AppStateSerializer aser = new AppStateSerializer(SerializeMode.XMLFile, @"C:\info.xml");
this.autoComplete1.LoadCurrentState(aser);
```

```vb
Imports Syncfusion.Runtime.Serialization

' To save
Private aser As AppStateSerializer = New AppStateSerializer(SerializeMode.XMLFile, "C:\info.xml")
Me.autoComplete1.SaveCurrentState(aser)

' To load
Private aser As AppStateSerializer = New AppStateSerializer(SerializeMode.XMLFile, "C:\info.xml")
Me.autoComplete1.LoadCurrentState(aser)
```

### Persisting in memory stream

It serializes the data into a memory stream. The following methods are storing and retrieving data in AutoComplete.

#### Storing state

The current internal list information is stored to the persistence medium using the `SaveCurrentState` method. 

```csharp
MemoryStream ms = new MemoryStream();
AppStateSerializer aser = new AppStateSerializer(SerializeMode.BinaryFmtStream, ms);
this.autoComplete1.SaveCurrentState(aser);
aser.PersistNow();
```

```vb
Dim ms As MemoryStream = New MemoryStream()
Private aser As AppStateSerializer = New AppStateSerializer(SerializeMode.BinaryFmtStream, ms)
Me.autoComplete1.SaveCurrentState(aser)
aser.PersistNow()
```

### Retrieving state

Retrieving state reads the previously serialized internal history list using the `LoadCurrentState` method.

```csharp
// Code to retrieve data(stream) from database.
MemoryStream ms = new MemoryStream(val);
ms.Position = 0;
AppStateSerializer aser = new AppStateSerializer(SerializeMode.BinaryFmtStream, ms);
this.autoComplete1.LoadCurrentState(aser);
```

```vb
' Code to retrieve data(stream) from database.
Dim ms As MemoryStream = New MemoryStream(value)
ms.Position = 0
Dim aser As AppStateSerializer = New AppStateSerializer(SerializeMode.BinaryFmtStream, ms)
this.autoComplete1.LoadCurrentState(aser)
```

#### To serialize in binary format

```csharp
// To save
AppStateSerializer aser = new AppStateSerializer(SerializeMode.BinaryFile,"myfile");
this.autoComplete1.SaveCurrentState(aser);
aser.PersistNow();

// To load
AppStateSerializer aser = new AppStateSerializer(SerializeMode.BinaryFile,"myfile");
this.autoComplete1.LoadCurrentState(aser);
```

```vb
' To save
Private aser As AppStateSerializer = New AppStateSerializer(SerializeMode.BinaryFile, "myfile")
Me.autoComplete1.SaveCurrentState(aser)
aser.PersistNow()

' To load
Private aser As AppStateSerializer = New AppStateSerializer(SerializeMode.BinaryFile, "myfile")
Me.autoComplete1.LoadCurrentState(aser)
```

#### To serialize in isolated storage medium

```csharp
// To save
AppStateSerializer aser = new AppStateSerializer(SerializeMode.IsolatedStorage, "myfile");
this.autoComplete1.SaveCurrentState(aser);
aser.PersistNow();

// To load
AppStateSerializer serializer = new AppStateSerializer(SerializeMode.IsolatedStorage, "myfile");
this.autoComplete1.LoadCurrentState(aser);
```

```vb
' To save
Private aser As AppStateSerializer = New AppStateSerializer(SerializeMode.IsolatedStorage, "myfile")
Me.autoComplete1.SaveCurrentState(aser)
aser.PersistNow()

' To load
Private serializer As AppStateSerializer = New AppStateSerializer(SerializeMode.IsolatedStorage, "myfile")
Me.autoComplete1.LoadCurrentState(aser)
```
