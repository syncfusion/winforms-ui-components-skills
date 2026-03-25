# Child Button Customization

## Table of Contents
- [Button Types & Border Styles](#button-types--border-styles)
- [Button Alignment](#button-alignment)
- [Image & Text Settings](#image--text-settings)
- [Flat Style Configuration](#flat-style-configuration)
- [Visual Style Inheritance](#visual-style-inheritance)
- [Focus & Tab Navigation](#focus--tab-navigation)
- [Hiding Buttons](#hiding-buttons)

## Button Types & Border Styles

### Button Types

ButtonEditChildButton supports multiple button types for different visual presentations:

| Type | Description | Use Case |
|------|-------------|----------|
| **Normal** | Standard button | General purpose buttons |
| **Calculator** | Calculator-style | Mathematical operations |
| **Currency** | Currency-specific | Financial inputs |
| **Up/Down** | Spinner buttons | Increment/decrement |
| **ComboXPDown** | Dropdown arrow | Selection triggers |
| **Left/Right** | Navigation arrows | Direction controls |

### Border Styles

Customize button border appearance using BorderStyleAdv:

| Style | Description |
|-------|-------------|
| **None** | No border |
| **Default** | Default border |
| **Dashed** | Dashed line |
| **Dotted** | Dotted line |
| **Inset** | Inset effect |
| **Outset** | Outset effect |
| **Solid** | Solid line |
| **Bump** | Bumped texture |
| **Etched** | Etched appearance |
| **Flat** | Flat border |
| **Raised** | Raised border |
| **RaisedInner/Outer** | Raised inner or outer |
| **Sunken/SunkenInner/Outer** | Sunken effect |

### Example: Button Types with Borders

```csharp
ButtonEditChildButton btn1 = new ButtonEditChildButton();
btn1.ButtonType = ButtonType.Calculator;
btn1.BorderStyleAdv = ButtonAdvBorderStyle.Bump;

ButtonEditChildButton btn2 = new ButtonEditChildButton();
btn2.ButtonType = ButtonType.ComboXPDown;
btn2.BorderStyleAdv = ButtonAdvBorderStyle.Etched;

buttonEdit.Buttons.Add(btn1);
buttonEdit.Buttons.Add(btn2);
```

## Button Alignment

Control button position relative to the textbox.

### Alignment Options

| Option | Description |
|--------|-------------|
| **Left** | Button appears on left side of textbox |
| **Right** | Button appears on right side of textbox |

### Implementation

```csharp
// Left-aligned button
ButtonEditChildButton leftBtn = new ButtonEditChildButton();
leftBtn.Text = "◀";
leftBtn.ButtonAlign = ButtonAlignment.Left;

// Right-aligned button
ButtonEditChildButton rightBtn = new ButtonEditChildButton();
rightBtn.Text = "▶";
rightBtn.ButtonAlign = ButtonAlignment.Right;

buttonEdit.Buttons.Add(leftBtn);
buttonEdit.Buttons.Add(rightBtn);
```

### Multiple Buttons

You can have multiple buttons on each side:

```csharp
// Two left buttons
ButtonEditChildButton left1 = new ButtonEditChildButton();
left1.Text = "A";
left1.ButtonAlign = ButtonAlignment.Left;

ButtonEditChildButton left2 = new ButtonEditChildButton();
left2.Text = "B";
left2.ButtonAlign = ButtonAlignment.Left;

// Two right buttons
ButtonEditChildButton right1 = new ButtonEditChildButton();
right1.Text = "C";
right1.ButtonAlign = ButtonAlignment.Right;

ButtonEditChildButton right2 = new ButtonEditChildButton();
right2.Text = "D";
right2.ButtonAlign = ButtonAlignment.Right;

buttonEdit.Buttons.Add(left1);
buttonEdit.Buttons.Add(left2);
buttonEdit.Buttons.Add(right1);
buttonEdit.Buttons.Add(right2);
```

## Image & Text Settings

Customize button appearance with images and text.

### Text Settings

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();
btn.Text = "Browse";
btn.TextAlign = ContentAlignment.MiddleCenter;
btn.PreferredWidth = 64;  // Width for text button
```

### Image Settings

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();

// Set image from file
btn.Image = Image.FromFile(@"C:\Images\search.png");

// Or from resources
btn.Image = Properties.Resources.SearchIcon;

// Align image within button
btn.ImageAlign = ContentAlignment.MiddleCenter;

// Default button width for image
btn.PreferredWidth = 22;  // Typical icon button size
```

### Image Index from ImageList

```csharp
// Assuming ImageList bound to form
buttonEdit.ImageList = this.imageList1;

ButtonEditChildButton btn = new ButtonEditChildButton();
btn.ImageIndex = 0;  // Index in the ImageList
btn.ImageList = this.imageList1;
btn.PreferredWidth = 22;
```

### Text and Image Together

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();
btn.Image = Image.FromFile(@"C:\Images\search.png");
btn.Text = "Find";
btn.TextAlign = ContentAlignment.MiddleRight;
btn.ImageAlign = ContentAlignment.MiddleLeft;
btn.TextImageRelation = TextImageRelation.ImageBeforeText;
btn.PreferredWidth = 70;  // Wider for text + image
```

### PreferredWidth Values

```csharp
btn.PreferredWidth = 18;   // Icon-only button
btn.PreferredWidth = 22;   // Standard icon size
btn.PreferredWidth = 50;   // Short text (Clear, Save)
btn.PreferredWidth = 70;   // Medium text (Browse, Search)
btn.PreferredWidth = 100;  // Longer text (Calculate Total)
```

### Complete Image/Text Example

```csharp
ButtonEdit fileEdit = new ButtonEdit();

// Browse button with icon and text
ButtonEditChildButton browseBtn = new ButtonEditChildButton();
browseBtn.Image = Image.FromFile(@"C:\Images\folder.png");
browseBtn.Text = "Browse";
browseBtn.ImageAlign = ContentAlignment.MiddleLeft;
browseBtn.TextAlign = ContentAlignment.MiddleRight;
browseBtn.TextImageRelation = TextImageRelation.ImageBeforeText;
browseBtn.PreferredWidth = 80;
browseBtn.ButtonAlign = ButtonAlignment.Right;

fileEdit.Buttons.Add(browseBtn);
```

## Flat Style Configuration

Apply flat styling to child buttons.

### Flat Style Options

| Style | Description |
|-------|-------------|
| **Flat** | Borderless, flat appearance |
| **Popup** | Button raises when hovered |
| **Standard** | Standard 3D button |
| **System** | System default style |

### Basic Flat Configuration

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();
btn.FlatStyle = FlatStyle.Flat;
btn.FlatAppearance.BorderColor = Color.DodgerBlue;
btn.UseVisualStyleBackColor = false;
```

### FlatAppearance Properties

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();
btn.FlatStyle = FlatStyle.Flat;

// Border color
btn.FlatAppearance.BorderColor = Color.Crimson;

// Color when mouse is over
btn.FlatAppearance.MouseOverBackColor = Color.Pink;

// Background color when checked
btn.FlatAppearance.CheckedBackColor = Color.LightGray;

// Border size
btn.FlatAppearance.BorderSize = 2;
```

### Flat Example: Modern Minimal Design

```csharp
ButtonEdit modernEdit = new ButtonEdit();

ButtonEditChildButton btn1 = new ButtonEditChildButton();
btn1.FlatStyle = FlatStyle.Flat;
btn1.FlatAppearance.BorderColor = Color.LightGray;
btn1.FlatAppearance.MouseOverBackColor = Color.Gainsboro;
btn1.Text = "Clear";
btn1.PreferredWidth = 50;

ButtonEditChildButton btn2 = new ButtonEditChildButton();
btn2.FlatStyle = FlatStyle.Flat;
btn2.FlatAppearance.BorderColor = Color.LightGray;
btn2.FlatAppearance.MouseOverBackColor = Color.DodgerBlue;
btn2.FlatAppearance.MouseOverBackColor = Color.AliceBlue;
btn2.Text = "Submit";
btn2.PreferredWidth = 60;

modernEdit.Buttons.Add(btn1);
modernEdit.Buttons.Add(btn2);
```

## Visual Style Inheritance

Child buttons inherit visual styles from their parent ButtonEdit.

### Inherited Properties

By default, child buttons inherit:
- ButtonStyle from parent ButtonEdit
- Color scheme
- Theme

### Override Inherited Style

You can override parent style on individual buttons:

```csharp
// Parent uses Office2016
buttonEdit.UseVisualStyle = true;
buttonEdit.ButtonStyle = ButtonAppearance.Office2016Colorful;

// Child button can override
ButtonEditChildButton customBtn = new ButtonEditChildButton();
customBtn.UseVisualStyleBackColor = false;  // Use custom colors
customBtn.BackColor = Color.LightBlue;
```

### Color Scheme Override

```csharp
// Use parent's Office2007 blue
btn.Office2007ColorScheme = Office2007Theme.Blue;

// Or use managed custom colors
btn.Office2007ColorScheme = Office2007Theme.Managed;
Office2007Colors.ApplyManagedColors(this, Color.LightGreen);

// Or no visual style
btn.Office2010ColorScheme = Office2010Theme.None;
```

## Focus & Tab Navigation

Enable keyboard navigation and focus management.

### Tab Navigation

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();
btn.TabStop = true;        // Allow tabbing to this button
btn.TabIndex = 2;          // Tab order (0 = first, then 1, 2, etc.)
btn.KeepFocusRectangle = true;  // Show focus rectangle when focused
```

### Tab Order Example

```csharp
ButtonEditChildButton btn1 = new ButtonEditChildButton();
btn1.TabStop = true;
btn1.TabIndex = 0;

ButtonEditChildButton btn2 = new ButtonEditChildButton();
btn2.TabStop = true;
btn2.TabIndex = 1;

// Tab sequence: btn1 → btn2 → next control in form
```

### Focus Event Handling

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();
btn.TabStop = true;
btn.KeepFocusRectangle = true;

btn.GotFocus += (s, e) => 
{
    // Handle focus gained
};

btn.LostFocus += (s, e) => 
{
    // Handle focus lost
};
```

## Hiding Buttons

Show or hide buttons at runtime.

### HideButton Method

```csharp
ButtonEdit buttonEdit = new ButtonEdit();

// ... add buttons ...

// Hide button at index 0
buttonEdit.HideButton(0, false);  // false = hide

// Show button at index 0
buttonEdit.HideButton(0, true);   // true = show
```

### Example: Conditional Button Visibility

```csharp
ButtonEditChildButton btn1 = new ButtonEditChildButton();
btn1.Text = "Edit";
ButtonEditChildButton btn2 = new ButtonEditChildButton();
btn2.Text = "Delete";

buttonEdit.Buttons.Add(btn1);
buttonEdit.Buttons.Add(btn2);

// Hide delete button by default
buttonEdit.HideButton(1, false);

// Show delete button when item selected
if (itemSelected)
{
    buttonEdit.HideButton(1, true);
}
```

## Complete Child Button Example

```csharp
public Form1()
{
    InitializeComponent();
    
    ButtonEdit searchEdit = new ButtonEdit();
    searchEdit.Location = new Point(50, 50);
    searchEdit.Size = new Size(300, 21);
    
    // Left button - Clear
    ButtonEditChildButton clearBtn = new ButtonEditChildButton();
    clearBtn.Text = "Clear";
    clearBtn.ButtonAlign = ButtonAlignment.Left;
    clearBtn.PreferredWidth = 50;
    clearBtn.FlatStyle = FlatStyle.Flat;
    clearBtn.TabStop = true;
    clearBtn.TabIndex = 0;
    
    // Right button - Search
    ButtonEditChildButton searchBtn = new ButtonEditChildButton();
    searchBtn.Text = "Search";
    searchBtn.Image = Image.FromFile(@"C:\Images\search.png");
    searchBtn.TextImageRelation = TextImageRelation.ImageBeforeText;
    searchBtn.ButtonAlign = ButtonAlignment.Right;
    searchBtn.PreferredWidth = 70;
    searchBtn.FlatStyle = FlatStyle.Flat;
    searchBtn.TabStop = true;
    searchBtn.TabIndex = 1;
    
    searchEdit.Buttons.Add(clearBtn);
    searchEdit.Buttons.Add(searchBtn);
    
    this.Controls.Add(searchEdit);
}
```
