# Item Customization

This guide covers text, color, and image customization options for GroupView items, including highlighting, offsets, and formatting settings.

## Table of Contents
- [Text Settings](#text-settings)
- [Color Settings](#color-settings)
- [Image Settings](#image-settings)

## Text Settings

Customize the appearance and behavior of text in GroupView items.

### Text Highlighting

Enable text highlighting when the mouse hovers over a GroupView item.

```csharp
// Enable text highlighting on mouse hover
this.groupView1.HighlightText = true;
```

**Effect:** When the mouse hovers over an item, its text color changes to the HighlightTextColor value.

**Prerequisite:** Most text-related properties require HighlightText = true to take effect.

### Text Offset Properties

Control the position offset of text in different item states.

#### Text Offset Properties

Configure text position offsets for different states:

```csharp
public void ConfigureTextOffsets()
{
    this.groupView1.HighlightText = true; // Required
    
    // Configure offsets for different states
    this.groupView1.HighlightTextOffset = new Point(5, 2);          // Hover
    this.groupView1.SelectedTextOffset = new Point(5, 2);           // Selected
    this.groupView1.SelectedHighlightTextOffset = new Point(7, 3);  // Selected + Hover
    this.groupView1.SelectingTextOffset = new Point(6, 3);          // Pressing
}
```

### Text Formatting Properties

#### Text Formatting Properties

```csharp
public void ConfigureTextFormatting()
{
    this.groupView1.TextSpacing = 10;     // Spacing between text and other elements
    this.groupView1.TextWrap = true;       // Enable wrapping for long labels
    this.groupView1.TextUnderline = false; // Underline text (hyperlink style)
}
```

### In-Place Renaming

```csharp
// Activate in-place editing for item at index
this.groupView1.InplaceRenameItem(2);

// Cancel active in-place edit
this.groupView1.CancelInplaceRenameItemAt();

// Handle rename completion
this.groupView1.GroupViewItemRenamed += (sender, e) =>
{
    var args = e as GroupItemRenamedEventArgs;
    MessageBox.Show($"Renamed: '{args.OldLabel}' → '{args.NewLabel}'");
};
```

## Color Settings

Customize colors for highlighting and selection states.

### Highlight and Selection Colors

Configure colors for different item states:

```csharp
public void ConfigureItemColors()
{
    this.groupView1.HighlightText = true; // Required
    
    // Highlight colors (mouse hover)
    this.groupView1.HighlightItemColor = Color.FromArgb(240, 248, 255);
    this.groupView1.HighlightTextColor = Color.DarkBlue;
    
    // Selection colors
    this.groupView1.SelectedItemColor = Color.LightGreen;
    this.groupView1.SelectedTextColor = Color.Blue;
    
    // Selected + highlighted
    this.groupView1.SelectedHighlightItemColor = Color.LightBlue;
    this.groupView1.SelectedHighlightTextColor = Color.Crimson;
    
    // Selecting (mouse button pressed)
    this.groupView1.SelectingItemColor = Color.PeachPuff;
    this.groupView1.SelectingTextColor = Color.Red;
}
```

### Complete Color Configuration Example

```csharp
public void ConfigureColors()
{
    // Enable highlighting (required for all color properties)
    this.groupView1.HighlightText = true;
    
    // Highlight colors (mouse hover, not selected)
    this.groupView1.HighlightItemColor = Color.FromArgb(230, 240, 250);
    this.groupView1.HighlightTextColor = Color.Navy;
    
    // Selection colors (selected, no hover)
    this.groupView1.SelectedItemColor = Color.FromArgb(51, 153, 255);
    this.groupView1.SelectedTextColor = Color.White;
    
    // Selected + highlighted colors (selected with hover)
    this.groupView1.SelectedHighlightItemColor = Color.FromArgb(0, 120, 215);
    this.groupView1.SelectedHighlightTextColor = Color.Yellow;
    
    // Selecting colors (mouse button pressed)
    this.groupView1.SelectingItemColor = Color.FromArgb(200, 230, 255);
    this.groupView1.SelectingTextColor = Color.DarkBlue;
}
```

### Windows 10-Style Color Scheme

```csharp
public void ApplyWindows10Colors()
{
    this.groupView1.HighlightText = true;
    
    // Subtle highlight (like Windows Explorer)
    this.groupView1.HighlightItemColor = Color.FromArgb(229, 243, 255);
    this.groupView1.HighlightTextColor = Color.Black;
    
    // Bold selection (Windows accent color)
    this.groupView1.SelectedItemColor = Color.FromArgb(0, 120, 215);
    this.groupView1.SelectedTextColor = Color.White;
    
    // Darker when selected + hovered
    this.groupView1.SelectedHighlightItemColor = Color.FromArgb(0, 102, 204);
    this.groupView1.SelectedHighlightTextColor = Color.White;
}
```

## Image Settings

Configure image display, highlighting, offsets, and spacing for GroupView items.

### Image List Configuration

Assign ImageList controls containing small or large images.

```csharp
// Assign small images (typically 16x16)
this.groupView1.SmallImageList = this.imageList1;
this.groupView1.SmallImageView = true;

// Assign large images (typically 32x32)
this.groupView1.LargeImageList = this.imageList2;
this.groupView1.SmallImageView = false; // Use large images
```

**Important:** Setting an ImageList does not automatically associate images with items. Each GroupViewItem must have its ImageIndex property set via the Collection Editor or code.

### Image Highlighting

Enable image highlighting when the mouse hovers over an item.

```csharp
// Enable image highlighting
this.groupView1.HighlightImage = true;
```

**Effect:** When the mouse hovers over an item, its image may shift position or change appearance based on offset settings.

**Prerequisite:** Most image-related properties require HighlightImage = true to take effect.

### Image Offset Properties

Configure image position offsets for different states:

```csharp
public void ConfigureImageOffsets()
{
    this.groupView1.HighlightImage = true; // Required
    
    // Configure offsets for visual feedback
    this.groupView1.HighlightImageOffset = new Point(3, 3);          // Hover
    this.groupView1.SelectedImageOffset = new Point(0, 0);           // Selected
    this.groupView1.SelectedHighlightImageOffset = new Point(2, 2);  // Selected + Hover
    this.groupView1.SelectingImageOffset = new Point(5, 5);          // Pressing
}
```

### Image Spacing

```csharp
// Set spacing between image and item edge (recommended: 5-10 pixels)
this.groupView1.HighlightImage = true; // Required
this.groupView1.ImageSpacing = 7;
```

### Complete Image Configuration Example

```csharp
public void ConfigureImages()
{
    // Setup ImageLists
    ImageList smallIcons = new ImageList();
    smallIcons.ImageSize = new Size(16, 16);
    smallIcons.Images.Add(Image.FromFile("icon1.png"));
    smallIcons.Images.Add(Image.FromFile("icon2.png"));
    smallIcons.Images.Add(Image.FromFile("icon3.png"));
    
    // Assign to GroupView
    this.groupView1.SmallImageList = smallIcons;
    this.groupView1.SmallImageView = true;
    
    // Enable highlighting
    this.groupView1.HighlightImage = true;
    
    // Configure spacing
    this.groupView1.ImageSpacing = 8;
    
    // Configure offsets for visual feedback
    this.groupView1.HighlightImageOffset = new Point(2, 2);
    this.groupView1.SelectedImageOffset = new Point(0, 0);
    this.groupView1.SelectedHighlightImageOffset = new Point(3, 3);
    this.groupView1.SelectingImageOffset = new Point(4, 4);
    
    // Add items with image indices
    this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
        new GroupViewItem("Document", 0, true, "Open document", "item1"),
        new GroupViewItem("Folder", 1, true, "Browse folder", "item2"),
        new GroupViewItem("Settings", 2, true, "Configure settings", "item3")
    });
}
```

## Comprehensive Customization Example

Combine text, color, and image settings for a fully customized GroupView:

```csharp
public partial class CustomizedGroupView : Form
{
    private GroupView groupView1;
    private ImageList imageList1;
    
    public CustomizedGroupView()
    {
        InitializeComponent();
        SetupCustomGroupView();
    }
    
    private void SetupCustomGroupView()
    {
        // Create GroupView
        this.groupView1 = new GroupView();
        this.groupView1.Location = new Point(20, 20);
        this.groupView1.Size = new Size(300, 400);
        this.groupView1.FlatLook = true;
        this.groupView1.BorderStyle = BorderStyle.FixedSingle;
        
        // Setup ImageList
        this.imageList1 = new ImageList();
        this.imageList1.ImageSize = new Size(16, 16);
        // Add images here...
        
        this.groupView1.SmallImageList = this.imageList1;
        this.groupView1.SmallImageView = true;
        
        // TEXT CUSTOMIZATION
        this.groupView1.HighlightText = true;
        this.groupView1.TextSpacing = 12;
        this.groupView1.TextWrap = true;
        this.groupView1.TextUnderline = false;
        
        // Text offsets
        this.groupView1.HighlightTextOffset = new Point(2, 1);
        this.groupView1.SelectedTextOffset = new Point(2, 1);
        
        // COLOR CUSTOMIZATION
        // Highlight colors (hover)
        this.groupView1.HighlightItemColor = Color.FromArgb(229, 243, 255);
        this.groupView1.HighlightTextColor = Color.Black;
        
        // Selection colors
        this.groupView1.SelectedItemColor = Color.FromArgb(0, 120, 215);
        this.groupView1.SelectedTextColor = Color.White;
        this.groupView1.SelectedHighlightItemColor = Color.FromArgb(0, 102, 204);
        this.groupView1.SelectedHighlightTextColor = Color.White;
        
        // IMAGE CUSTOMIZATION
        this.groupView1.HighlightImage = true;
        this.groupView1.ImageSpacing = 8;
        this.groupView1.HighlightImageOffset = new Point(2, 2);
        this.groupView1.SelectedImageOffset = new Point(0, 0);
        
        // Add items
        this.groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
            new GroupViewItem("My Documents", 0, true, "Access documents", "item1"),
            new GroupViewItem("Pictures", 1, true, "View pictures", "item2"),
            new GroupViewItem("Music", 2, true, "Play music", "item3"),
            new GroupViewItem("Videos", 3, true, "Watch videos", "item4")
        });
        
        // Enable in-place renaming via double-click
        this.groupView1.GroupViewItemDoubleClick += (sender, e) =>
        {
            var args = e as GroupViewItemDoubleClickEventArgs;
            this.groupView1.InplaceRenameItem(args.SelectedItem);
        };
        
        // Handle rename event
        this.groupView1.GroupViewItemRenamed += (sender, e) =>
        {
            var args = e as GroupItemRenamedEventArgs;
            this.Text = $"Renamed: {args.OldLabel} → {args.NewLabel}";
        };
        
        // Add to form
        this.Controls.Add(this.groupView1);
    }
}
```

## Best Practices

### Text Customization
- **Enable TextWrap** for items with long names or descriptions
- **Use TextSpacing** (8-15 pixels) to create breathing room around text
- **Avoid TextUnderline** unless creating hyperlink-style items
- **Set TextOffsets** subtly (1-3 pixels) for visual feedback without disruption

### Color Customization
- **Use high-contrast colors** for selected items (e.g., dark background + white text)
- **Keep highlight colors subtle** (10-15% darker/lighter than background)
- **Test color combinations** for readability in different lighting conditions
- **Follow platform guidelines** (Windows 10/11 color schemes for modern apps)

### Image Customization
- **Use consistent image sizes** (16x16 for small, 32x32 for large)
- **Set ImageSpacing** to prevent cramped appearance (5-10 pixels recommended)
- **Use minimal ImageOffsets** (2-5 pixels) to indicate state changes
- **Ensure images have transparent backgrounds** for better integration
