# MultiColumn Dropdown in Windows Forms AutoComplete

The AutoComplete allows you to display multiple columns of information. The columns can be configured using the following properties:

## Columns

The `Columns` property specifies a collection of columns in the AutoComplete dropdown when the AutoCompleteModes enumerator value is AutoSuggest. Each column is represented by an AutoCompleteDataColumnInfo object. This class includes a definition for specifying whether the column is the matching column or the image column.

### Column configuration

The information needed for setting the attributes of a column in the drop-down list of the AutoComplete is handled using `AutoCompleteDataColumnInfo`. It specifies the appearance and behavior of each column that should be visible. The AutoCompleteDataColumnInfo properties are as follows.

| AutoCompleteDataColumn properties | Description |
|-----------------------------------|-------------|
| `ColumnHeaderText` | Represents the text for the column header. |
| `MatchingColumn` | Indicates whether the column that this item represents to be treated as the matching column. |
| `ImageColumn` | Indicates whether the column that this item represents to be treated as the image column. |
| `MinColumnWidth` | Sets minimum width for the column. |
| `Visible` | Shows or hides the column at run time. |

## Showing image in column

You can add a drop-down item with image to the AutoComplete popup.

An image list should be set to the `ImageList` property of AutoComplete component, and the `ImageColumn` property should be set to `true` for displaying images in the column. Specify the item text and the image index in the `AddHistoryItem` method.

```csharp
this.autoCompleteDataColumnInfo1.ColumnHeaderText = "Flag";
this.autoCompleteDataColumnInfo1.ImageColumn = true;
this.autoCompleteDataColumnInfo1.MatchingColumn = false;
this.autoCompleteDataColumnInfo1.Visible = true;

this.autoCompleteDataColumnInfo2.ColumnHeaderText = "Country";
this.autoCompleteDataColumnInfo2.ImageColumn = false;
this.autoCompleteDataColumnInfo2.MatchingColumn = false;
this.autoCompleteDataColumnInfo2.Visible = true;

this.autoComplete1.Columns.Add(this.autoCompleteDataColumnInfo2);
this.autoComplete1.Columns.Add(this.autoCompleteDataColumnInfo1);

// Add Images in the image list and set to this property.
this.autoComplete1.ImageList = this.imageList;

this.autoComplete1.AddHistoryItem("USA", 0);
this.autoComplete1.AddHistoryItem("China", 1);
```

```vb
Me.autoCompleteDataColumnInfo1.ColumnHeaderText = "Flag"
Me.autoCompleteDataColumnInfo1.ImageColumn = True
Me.autoCompleteDataColumnInfo1.MatchingColumn = False
Me.autoCompleteDataColumnInfo1.Visible = True

Me.autoCompleteDataColumnInfo2.ColumnHeaderText = "Country"
Me.autoCompleteDataColumnInfo2.ImageColumn = False
Me.autoCompleteDataColumnInfo2.MatchingColumn = True
Me.autoCompleteDataColumnInfo2.Visible = True

Me.autoComplete1.Columns.Add(Me.autoCompleteDataColumnInfo2)
Me.autoComplete1.Columns.Add(Me.autoCompleteDataColumnInfo1)

'Add Images in the image list and set to this property.
Me.autoComplete1.ImageList = Me.imageList

Me.autoComplete1.AddHistoryItem("USA", 0)
Me.autoComplete1.AddHistoryItem("China", 1)
```

![Winforms AutoComplete setting images in item list](../images/AutoComplete_Imagesetting.png)

## Example: Multi-column with DataTable

```csharp
DataTable dt = new DataTable();
dt.Columns.Add("Name", typeof(string));
dt.Columns.Add("Country", typeof(string));
dt.Columns.Add("City", typeof(string));

dt.Rows.Add("John Doe", "USA", "New York");
dt.Rows.Add("Jane Smith", "UK", "London");
dt.Rows.Add("Bob Johnson", "Canada", "Toronto");

autoComplete1.DataSource = dt;

// Configure columns
autoComplete1.RefreshColumns();
autoComplete1.Columns[0].ColumnHeaderText = "Name";
autoComplete1.Columns[0].MatchingColumn = true;
autoComplete1.Columns[1].ColumnHeaderText = "Country";
autoComplete1.Columns[2].ColumnHeaderText = "City";
```

## Best Practices

1. **Limit column count**: Keep the number of columns reasonable (3-5) for better usability
2. **Set appropriate widths**: Configure column widths to display all important information
3. **Use meaningful headers**: Provide clear column header text
4. **Designate matching column**: Always specify which column is used for matching
5. **Show/hide columns**: Use the Visible property to hide unnecessary columns dynamically
