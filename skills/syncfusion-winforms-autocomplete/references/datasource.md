# DataSource in Windows Forms AutoComplete

The AutoComplete component supports variety of data sources such as DataTables, DataSets, or any component that implement interfaces such as IList, IBindingList, ITypedList, and IListSource. For assigning data source to AutoComplete, use the `DataSource` property. This section explains about the different types of data binding mechanisms and data settings supported by the AutoComplete component.

## Data settings

The data for the autocompletion is maintained by the AutoComplete component itself. This is referred to as History Data List mode. The following properties deal with data settings.

| AutoComplete properties | Description |
|------------------------|-------------|
| `CategoryName` | Specifies a unique or shared name that can be given to an AutoComplete component, so that it can persist the values under that name. For example, when the CategoryName "URL" is provided for an AutoComplete component on a particular form, all the values persisted by that AutoComplete component are also accessible to other AutoComplete component on others forms or on the same form with the CategoryName "URL". |
| `DataSource` | Sets the Datasource to the Autocomplete component. The AutoComplete component automatically picks the "History Data List" mode or "Data source" mode based on the values set for the DataSource property. When the datasource property is set to NULL (default value is NULL), the component defaults to History Data List mode. It is to be remembered that the properties CategoryName, AutoAddItem, and AutoSerialize have to be set appropriately for the History Data List mode to work properly. |

```csharp
this.autoComplete1.CategoryName = "FTP";
this.autoComplete1.DataSource = DataTable1;
```

```vb
Me.autoComplete1.CategoryName = "FTP"
Me.autoComplete1.DataSource = DataTable1
```

## Dynamic source at run time

Enabling the `AutoAddItem` property allows you to save your entries at run time, and pressing the `Enter` key saves your entry.

## Built-in source

The different built-in source (FileSystem, HistoryList, AllUrl, etc) can be set to the AutoComplete component using the `AutoCompleteSource` property of the editor control.

| Built-in DataSource Support | Description |
|------------------------------|-------------|
| **FileSystem** | It specifies file system as source. |
| **HistoryList** | Includes all the URLs in the history list. |
| **RecentlyUsedList** | Includes a list of most recently used URLs. |
| **AllUrl** | Equivalent source of HistoryList and RecentlyUsedList as the source. |
| **AllSystemSources** | Equivalent source of AllUrls and FileSystem as the source (default value of AutoCompleteSource when AutoCompleteMode is set to some values other than default value). |
| **ListItems** | Specifies the items in the control. |
| **FileSystemDirectories** | Specifies directory names alone without file names. |
| **CustomSource** | Uses the string values entered in AutoCompleteCustomSource property. |
| **None** | There is no source for the auto completion. |

```csharp
this.textBox1.AutoCompleteSource = System.Windows.Forms.AutoCompleteSource.HistoryList;
```

```vb
Me.textBox1.AutoCompleteSource = System.Windows.Forms.AutoCompleteSource.HistoryList
```

![Windows Forms AutoComplete History List in datasource](../images/AutoComplete_Historylist.png)

## Custom source

The AutoComplete component allows you add a set of text using `String Collection Editor`, and this editor window will be shown by clicking the `AutoCompleteCustomSource` property in property window of the editor control. The `AutoCompleteSource` property should be set to `CustomSource` for using custom items added through String Collection Editor.

![Windows Forms AutoComplete Customsource datasource](../images/AutoComplete_Customsource.png) 

```csharp
this.textBox1.AutoCompleteSource = System.Windows.Forms.AutoCompleteSource.CustomSource;

this.textBox1.AutoCompleteCustomSource.AddRange(new string[] {"Syncfusion", "AutoComplete",
"Item Customization", "Popup Customization", "DataBinding"});
```

```vb
Me.textBox1.AutoCompleteSource = System.Windows.Forms.AutoCompleteSource.CustomSource

Me.textBox1.AutoCompleteCustomSource.AddRange(New String() {"Syncfusion", "AutoComplete",
"Item Customization", "Popup Customization", "DataBinding"})
```

![Windows Forms AutoComplete Customsource datasource](../images/AutoComplete_Customsourcecode.png) 

## Binding custom collections 

The different custom collections that can be bound to the `DataSource` property of the AutoComplete component are listed as follows:

* BindingList Collection
* ArrayList Collection
* Observable Collection
* Collection Base
* Generic Collections
* DataTable

### Example: Binding ArrayList

```csharp
ArrayList data = new ArrayList();
data.Add("Australia");
data.Add("Brazil");
data.Add("Canada");
data.Add("China");

autoComplete1.DataSource = data;
```

### Example: Binding Generic List

```csharp
List<string> countries = new List<string>();
countries.Add("Australia");
countries.Add("Brazil");
countries.Add("Canada");

autoComplete1.DataSource = countries;
```

### Example: Binding DataTable

```csharp
DataTable dt = new DataTable();
dt.Columns.Add("Name", typeof(string));
dt.Columns.Add("Code", typeof(string));

dt.Rows.Add("Australia", "AU");
dt.Rows.Add("Brazil", "BR");
dt.Rows.Add("Canada", "CA");

autoComplete1.DataSource = dt;
autoComplete1.DisplayMember = "Name";
```

## Best Practices

1. **Use appropriate data structures**: Choose the right collection type based on your data size and update frequency
2. **Set DisplayMember**: When binding to complex objects, always specify which property to display
3. **Cache data**: For remote data sources, cache results to improve performance
4. **Lazy loading**: For very large datasets, consider implementing lazy loading or virtual mode
5. **Error handling**: Always handle exceptions when loading data from external sources
