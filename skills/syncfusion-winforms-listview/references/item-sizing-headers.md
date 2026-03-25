# Item Sizing and Headers/Footers

## Table of Contents
- [Item Sizing](#item-sizing)
  - [Customizing Item and Group Height](#customizing-item-and-group-height)
  - [On-Demand Item Height](#on-demand-item-height)
  - [Auto-Fit Items Based on Content](#auto-fit-items-based-on-content)
- [Headers and Footers](#headers-and-footers)
  - [Showing Headers and Footers](#showing-headers-and-footers)
  - [Setting Header and Footer Text](#setting-header-and-footer-text)
  - [Customizing Header and Footer Height](#customizing-header-and-footer-height)
  - [Loading Custom Controls](#loading-custom-controls)
  - [Styling Headers and Footers](#styling-headers-and-footers)
  - [Adding Images to Headers and Footers](#adding-images-to-headers-and-footers)
  - [Showing Summaries in Footer](#showing-summaries-in-footer)

---

## Item Sizing

Control the height of list items, group headers, and enable auto-fit behavior to match content.

### Customizing Item and Group Height

Set fixed heights for items and group headers using properties:

```csharp
sfListView1.ItemHeight = 35;
sfListView1.GroupHeaderHeight = 50;
```

**Properties:**
- `ItemHeight`: Height of each list item in pixels
- `GroupHeaderHeight`: Height of group header items in pixels

Use this for consistent, uniform item heights throughout the list.

### On-Demand Item Height

Customize the height of individual items using the `QueryItemHeight` event:

```csharp
sfListView1.QueryItemHeight += (sender, e) =>
{
    if (e.ItemIndex == 1)
    {
        e.ItemHeight = 70;
        e.Handled = true;
    }
};
```

**QueryItemHeightEventArgs Properties:**
- `ItemIndex`: Index of the item being queried
- `ItemData`: Underlying data object bound to the item
- `ItemHeight`: Height to assign to the item (set this property)
- `ItemType`: Type of item (e.g., record, group header)
- `Handled`: Set to `true` to apply the custom height

**Use case:** Set different heights based on item content, index, or data properties.

### Auto-Fit Items Based on Content

Dynamically adjust item heights to fit content using `AutoFitMode`:

```csharp
sfListView1.AutoFitMode = AutoFitMode.Height;
```

**AutoFitMode Options:**
- `Height`: Automatically adjusts item height based on content
- `None`: Uses fixed `ItemHeight` for all items (default)

**When to use:**
- Items have variable content lengths
- Text wrapping requires dynamic heights
- Complex item templates with varying sizes

---

## Headers and Footers

Add header and footer sections to your ListView that stick to the top and bottom of the view.

### Showing Headers and Footers

Enable headers and footers with properties:

```csharp
// Show header item
sfListView1.ShowHeader = true;

// Show footer item
sfListView1.ShowFooter = true;
```

**Default behavior:** Headers and footers stick to the top and bottom of the view during scrolling.

### Setting Header and Footer Text

Customize text displayed in headers and footers using the `DrawItem` event:

```csharp
sfListView1.DrawItem += (sender, e) =>
{
    if (e.ItemType == Syncfusion.WinForms.ListView.Enums.ItemType.Header)
    {
        e.Text = "List of US States";
    }

    if (e.ItemType == Syncfusion.WinForms.ListView.Enums.ItemType.Footer)
    {
        e.Text = "Filtered Items Count: " + sfListView1.View.Items.Count;
    }
};
```

**DrawItemEventArgs Properties:**
- `Text`: Text to display in the item
- `ItemType`: Identifies whether this is a Header, Footer, or other item type

### Customizing Header and Footer Height

Set specific heights for header and footer items:

```csharp
sfListView1.HeaderHeight = 30;
sfListView1.FooterHeight = 30;
```

**Properties:**
- `HeaderHeight`: Height of the header item in pixels
- `FooterHeight`: Height of the footer item in pixels

### Loading Custom Controls

Load custom user controls in the header or footer using `HeaderControl` and `FooterControl` properties:

```csharp
// Create custom user control (e.g., search textbox)
CustomHeaderUserControl customTextBox = new CustomHeaderUserControl(sfListView1);
customTextBox.TextBox.Font = sfListView1.Style.ItemStyle.Font;
customTextBox.Width = sfListView1.Size.Width - ListView.VerticalScroll.ScrollBar.Width;

// Assign to header
sfListView1.HeaderControl = customTextBox;
```

**Example custom control with filtering:**

```csharp
internal class CustomHeaderUserControl : Panel
{
    internal TextBox TextBox { get; set; }
    internal SfListView ListView { get; set; }

    internal CustomHeaderUserControl(SfListView listView)
    {
        this.ListView = listView;
        TextBox = new TextBox();
        TextBox.AutoSize = false;
        TextBox.Anchor = AnchorStyles.Left | AnchorStyles.Right;
        TextBox.BorderStyle = System.Windows.Forms.BorderStyle.None;
        this.Controls.Add(TextBox);
        
        TextBox.TextChanged += OnTextBoxTextChanged;
        ListView.View.Filter = FilterItem;
    }

    private void OnTextBoxTextChanged(object sender, EventArgs e)
    {
        this.ListView.View.RefreshFilter();
    }

    private bool FilterItem(object data)
    {
        if ((data as USState).LongName.ToLower().Contains(this.TextBox.Text.ToLower()))
            return true;
        return false;
    }

    protected override void OnPaint(PaintEventArgs e)
    {
        base.OnPaint(e);
        ControlPaint.DrawBorder(e.Graphics, this.ClientRectangle, 
            ColorTranslator.FromHtml("#7A7A7A"), ButtonBorderStyle.Solid);
    }
}
```

**Use cases for custom controls:**
- Search/filter textboxes in header
- Buttons for actions (clear, refresh, export)
- Summary statistics or counters
- Custom branding or logos

### Styling Headers and Footers

Customize appearance using the `Style.HeaderItemStyle` and `Style.FooterItemStyle` properties:

**Header styling:**
```csharp
sfListView1.Style.HeaderItemStyle.BackColor = Color.DarkCyan;
sfListView1.Style.HeaderItemStyle.ForeColor = Color.White;
sfListView1.Style.HeaderItemStyle.TextAlignment = ContentAlignment.MiddleCenter;
sfListView1.Style.HeaderItemStyle.Font = new Font("Segoe UI Semibold", 11);
```

**Footer styling:**
```csharp
sfListView1.Style.FooterItemStyle.BackColor = Color.DarkCyan;
sfListView1.Style.FooterItemStyle.ForeColor = Color.White;
sfListView1.Style.FooterItemStyle.TextAlignment = ContentAlignment.MiddleCenter;
sfListView1.Style.FooterItemStyle.Font = new Font("Segoe UI Semibold", 11);
```

**Available styling properties:**
- `BackColor`: Background color
- `ForeColor`: Text color
- `TextAlignment`: Text alignment (MiddleLeft, MiddleCenter, MiddleRight, etc.)
- `Font`: Font family, size, and style

### Adding Images to Headers and Footers

Load images in headers and footers using the `DrawItem` event:

**Header with image:**
```csharp
sfListView1.DrawItem += (sender, e) =>
{
    if (e.ItemType == Syncfusion.WinForms.ListView.Enums.ItemType.Header)
    {
        e.Text = "List of US States";
        e.Image = Image.FromFile("../../Icon/Flag.png");
        e.ImageAlignment = ContentAlignment.MiddleLeft;
        e.TextImageRelation = TextImageRelation.ImageBeforeText;
    }
};
```

**Footer with image:**
```csharp
sfListView1.DrawItem += (sender, e) =>
{
    if (e.ItemType == Syncfusion.WinForms.ListView.Enums.ItemType.Footer)
    {
        e.Text = "Total US States Count: " + sfListView1.View.Items.Count;
        e.Image = Image.FromFile("../../Icon/Flag.png");
        e.ImageAlignment = ContentAlignment.MiddleCenter;
        e.TextImageRelation = TextImageRelation.ImageBeforeText;
    }
};
```

**DrawItemEventArgs Image Properties:**
- `Image`: The image to display
- `ImageAlignment`: Position of image within the item bounds
- `TextImageRelation`: Relationship between text and image (ImageBeforeText, TextBeforeImage, etc.)

### Showing Summaries in Footer

Display summary information in the footer (e.g., item counts, totals):

```csharp
sfListView1.DrawItem += (sender, e) =>
{
    if (e.ItemType == Syncfusion.WinForms.ListView.Enums.ItemType.Footer)
    {
        e.Text = "Filtered Items Count: " + sfListView1.View.Items.Count;
    }
};
```

**Common summary scenarios:**
- Total item count
- Filtered item count
- Aggregate values (sum, average, max, min)
- Selection status ("3 of 10 selected")

---

## Common Scenarios

### Scenario 1: List with Custom Item Heights
```csharp
// Set default height
sfListView1.ItemHeight = 40;

// Override specific items
sfListView1.QueryItemHeight += (sender, e) =>
{
    // Make every 5th item taller
    if (e.ItemIndex % 5 == 0)
    {
        e.ItemHeight = 60;
        e.Handled = true;
    }
};
```

### Scenario 2: Searchable List with Header Filter
```csharp
// Enable header
sfListView1.ShowHeader = true;
sfListView1.HeaderHeight = 35;

// Add custom search control
CustomHeaderUserControl searchBox = new CustomHeaderUserControl(sfListView1);
sfListView1.HeaderControl = searchBox;

// Footer shows filtered count
sfListView1.ShowFooter = true;
sfListView1.DrawItem += (sender, e) =>
{
    if (e.ItemType == ItemType.Footer)
    {
        e.Text = $"Showing {sfListView1.View.Items.Count} items";
    }
};
```

### Scenario 3: Styled Header with Auto-Fit Content
```csharp
// Enable auto-fit for variable content
sfListView1.AutoFitMode = AutoFitMode.Height;

// Style header
sfListView1.ShowHeader = true;
sfListView1.Style.HeaderItemStyle.BackColor = Color.Navy;
sfListView1.Style.HeaderItemStyle.ForeColor = Color.White;
sfListView1.Style.HeaderItemStyle.Font = new Font("Arial", 12, FontStyle.Bold);

// Set header text
sfListView1.DrawItem += (sender, e) =>
{
    if (e.ItemType == ItemType.Header)
    {
        e.Text = "Product Catalog";
        e.Image = Image.FromFile("logo.png");
        e.TextImageRelation = TextImageRelation.ImageBeforeText;
    }
};
```

---

## Troubleshooting

**Issue: Custom item heights not applying**
- Ensure `Handled = true` in `QueryItemHeight` event handler
- Verify `AutoFitMode` is not set to `Height` (which overrides custom heights)
- Check that `ItemIndex` logic is correct

**Issue: Header/footer not visible**
- Confirm `ShowHeader` or `ShowFooter` is set to `true`
- Verify `HeaderHeight`/`FooterHeight` is greater than 0
- Check that the ListView has sufficient height to display content

**Issue: Custom control in header not responding**
- Ensure control is properly sized to fit within header bounds
- Handle ListView size changes to resize custom control
- Account for scrollbar width when sizing: `Width = ListView.Width - ScrollBar.Width`

**Issue: Auto-fit items causing performance issues**
- Auto-fit calculates height for each item on-demand, which can be slow with many items
- Consider using fixed heights with `QueryItemHeight` for specific items instead
- Use virtualization techniques for large datasets

**Issue: Images not showing in header/footer**
- Verify image file path is correct
- Check that `Image` property is set before `Text` property
- Ensure `TextImageRelation` and `ImageAlignment` are set appropriately

**Issue: Footer summary not updating**
- Hook into relevant events (FilterChanged, SelectionChanged, etc.)
- Refresh the footer display by invalidating the control
- Ensure `View.Items.Count` is accessed after data operations complete
