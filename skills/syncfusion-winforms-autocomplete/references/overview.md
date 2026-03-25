# Windows Forms AutoComplete Overview

The AutoComplete control is an extender control that provides AutoCompletion services to any edit control on the same form as the AutoComplete control.

AutoCompletion can be defined as prompting you with probable matches during data entry. This feature is similar to the AutoCompletion of the web addresses in the Internet Explorer address box.

![Overview for Autocomplete](../images/Overview_image1.png) 

AutoCompletion is a feature enhancement for edit controls. It expands strings that have been partially entered in an edit control into complete strings based on a list of previously specified strings.

![Overview for Autocomplete with previously specified strings](../images/Overview_image2.png)

For example, when you start to enter an URL in the Address edit control that is embedded in the Microsoft Internet Explorer navigation toolbar, autocompletion expands the string into one or more complete URLs that are consistent with the existing partial string. A partial URL string such as "sync" might be expanded to "http://www.syncfusion.com" or "http://www.syncfusion.com/faq/winforms". This feature is useful in windows applications that require collecting frequently repeated input from the user. 

The .NET Framework does not provide any built-in support for providing autocompletion in the System.Windows.Forms.Textbox or System.Windows.Forms.Combobox classes. The Essential Tools AutoComplete class provides an easy way of adding autocompletion to edit controls in an application. Autocompletion is typically used with edit controls (text box controls) or with controls that have an embedded edit control (such as combo box controls). 

The AutoComplete class is implemented as an Extender Provider similar to the ToolTip control in the Windows Forms package. 

## Key Features

### Multiple AutoComplete Modes

The AutoComplete control supports various modes:
- **AutoSuggest**: Displays a dropdown list of suggestions
- **AutoAppend**: Automatically appends the best match
- **Both**: Combines AutoSuggest and AutoAppend
- **MultiSuggest**: Extended suggestion mode matching from column start
- **MultiSuggestExtend**: Matches text anywhere in the column

### Flexible Data Binding

Supports multiple data sources:
- String arrays and collections
- DataTable and DataSet
- Generic collections (List<T>, BindingList<T>)
- Custom objects implementing IList, IBindingList
- Built-in sources (FileSystem, HistoryList, URLs)

### Multi-Column Display

Display multiple columns of information in the dropdown:
- Configure column appearance and behavior
- Show/hide columns dynamically
- Add images to columns
- Custom column headers and widths

### History Management

Maintain and persist user input history:
- Automatic history tracking
- Add items at runtime
- Delete individual items or clear entire history
- Persist history to various storage formats

### Rich Customization

Extensive customization options:
- Multiple visual themes (Office 2016, Metro, etc.)
- Custom fonts and colors
- Popup size and appearance
- Column header styles
- Custom rendering

### Event-Driven Architecture

Comprehensive event system:
- Item selection and browsing events
- Dropdown display/close events
- Custom matching logic
- Pre/post item add events
- Target control change events

## Use Cases

### 1. URL/Email Entry
Provide autocomplete for frequently used URLs or email addresses, similar to web browser address bars.

### 2. Search Boxes
Implement type-ahead search functionality with real-time suggestions from local or remote data sources.

### 3. Form Data Entry
Speed up data entry in forms by suggesting previously entered values or common options.

### 4. Code Editors
Add IntelliSense-like functionality to custom text editors or code entry fields.

### 5. Contact Selection
Enable quick user or contact selection in messaging or email applications.

### 6. Product/Item Selection
Facilitate product or item search in inventory or e-commerce applications.

### 7. Command Line Interfaces
Implement command completion in custom CLI or console applications.

## Architecture

The AutoComplete component follows the Extender Provider pattern:

1. **AutoComplete Component**: Main component added to the form
2. **Target Controls**: Any edit control (TextBox, ComboBox, etc.) on the same form
3. **AutoCompletePopup**: Internal popup window showing suggestions
4. **Data Source**: The collection of items for autocomplete suggestions
5. **Event System**: Events for customization and user interaction

### Component Lifecycle

1. **Initialization**: AutoComplete component is added to the form
2. **Association**: Target controls are associated via SetAutoComplete method
3. **User Input**: User types in the associated control
4. **Matching**: AutoComplete searches data source for matches
5. **Display**: Popup shows matching suggestions
6. **Selection**: User selects an item or continues typing
7. **Completion**: Selected value is applied to the target control

## Performance Considerations

1. **Large Datasets**: Use LoadOnDemand property for datasets with thousands of items
2. **Remote Data**: Implement caching to reduce network calls
3. **Filtering**: Use appropriate filter modes for your data type
4. **Column Count**: Limit multi-column displays to essential columns
5. **Event Handlers**: Keep event handler logic lightweight

## Integration Points

The AutoComplete control integrates with:
- Standard WinForms controls (TextBox, ComboBox, RichTextBox)
- Custom controls implementing IEditControlsEmbed interface
- Syncfusion controls (ComboBoxAdv, TextBoxExt)
- Data access layers (ADO.NET, Entity Framework, etc.)

## Comparison with .NET AutoComplete

| Feature | Syncfusion AutoComplete | .NET Built-in |
|---------|------------------------|---------------|
| Multi-column display | ✓ Yes | ✗ No |
| Multiple modes | ✓ Yes (6 modes) | Limited (3 modes) |
| Custom matching | ✓ Yes | ✗ No |
| Image support | ✓ Yes | ✗ No |
| History persistence | ✓ Multiple formats | Limited |
| Themes | ✓ Multiple themes | ✗ No |
| Events | ✓ Rich event system | Basic |
| RichTextBox support | ✓ Yes | ✗ No |

## See Also

- [Getting Started](getting-started.md)
- [Data Source](datasource.md)
- [Customization](customization.md)
- [Multi-Column Dropdown](multicolumn-dropdown.md)
- [Events](autocomplete-events.md)
