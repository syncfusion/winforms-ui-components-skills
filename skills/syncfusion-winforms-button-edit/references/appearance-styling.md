# Appearance & Styling

## Table of Contents
- [Visual Styles](#visual-styles)
- [Custom Colors](#custom-colors)
- [Border Styles](#border-styles)
- [Size Settings](#size-settings)
- [Foreground Settings](#foreground-settings)
- [Character Casing](#character-casing)

## Visual Styles

ButtonEdit supports multiple visual styles to match your application's look and feel.

### Enabling Visual Styles

To apply a visual style, first enable visual styling:

```csharp
buttonEdit.UseVisualStyle = true;
buttonEdit.ButtonStyle = ButtonAppearance.Office2007Blue;
```

### Available ButtonStyles

| Style | Description | Best For |
|-------|-------------|----------|
| **Classic** | Traditional Windows classic style | Legacy applications |
| **Office2000** | Microsoft Office 2000 appearance | Office-like UI |
| **WindowsXP** | Windows XP theme | Windows XP compatibility |
| **OfficeXP** | Microsoft OfficeXP style | Professional look |
| **Office2003** | Office 2003 blue theme | Corporate applications |
| **Office2007** | Office 2007 style | Modern professional UI |
| **Metro** | Flat modern Metro style | Minimalist design |
| **Office2016Colorful** | Office 2016 colorful theme | Contemporary look |
| **Office2016White** | Office 2016 white theme | Clean white background |
| **Office2016DarkGray** | Office 2016 dark gray theme | Dark theme |
| **Office2016Black** | Office 2016 black theme | Dark modern theme |

### Example: Applying Different Styles

```csharp
// Metro style
buttonEdit.UseVisualStyle = true;
buttonEdit.ButtonStyle = ButtonAppearance.Metro;

// Office 2016
buttonEdit.UseVisualStyle = true;
buttonEdit.ButtonStyle = ButtonAppearance.Office2016Colorful;

// Office 2007
buttonEdit.UseVisualStyle = true;
buttonEdit.ButtonStyle = ButtonAppearance.Office2007Blue;
```

## Custom Colors

Apply custom colors to ButtonEdit using Office2007ColorScheme.

### Setting Managed Colors

```csharp
// Set child buttons to use managed colors
buttonEditChildButton1.Office2007ColorScheme = Office2007Theme.Managed;
buttonEditChildButton2.Office2007ColorScheme = Office2007Theme.Managed;
buttonEditChildButton3.Office2007ColorScheme = Office2007Theme.Managed;

// Apply a custom color scheme
Office2007Colors.ApplyManagedColors(this, Color.LightGreen);
```

### Common Color Schemes

```csharp
// Light green theme
Office2007Colors.ApplyManagedColors(this, Color.LightGreen);

// Light blue theme
Office2007Colors.ApplyManagedColors(this, Color.LightBlue);

// Custom brand color
Office2007Colors.ApplyManagedColors(this, Color.FromArgb(51, 102, 204));
```

## Border Styles

Customize the border appearance of ButtonEdit.

### Border 3D Styles

The Border3DStyle property controls the 3D effect:

| Style | Description |
|-------|-------------|
| **RaisedOuter** | Raised outer edge |
| **RaisedInner** | Raised inner edge |
| **SunkenOuter** | Sunken outer edge |
| **SunkenInner** | Sunken inner edge |
| **Raised** | Full raised effect |
| **Sunken** | Full sunken effect (default) |
| **Etched** | Etched appearance |
| **Flat** | Flat no-border look |
| **Adjust** | Adjusted appearance |
| **Bump** | Bumped texture |

### Example: 3D Border Styles

```csharp
buttonEdit.Border3DStyle = Border3DStyle.Raised;
```

### Border Sides

Control which sides have borders:

```csharp
buttonEdit.BorderSides = BorderSides.All;           // All four sides
buttonEdit.BorderSides = BorderSides.Left | BorderSides.Right;  // Left and right only
buttonEdit.BorderSides = BorderSides.Top;           // Top only
```

### Flat Style Configuration

For a modern flat appearance without visual styles:

```csharp
buttonEdit.UseVisualStyle = false;
buttonEdit.FlatStyle = FlatStyle.Flat;
buttonEdit.FlatBorderColor = Color.DodgerBlue;
```

### Complete Border Example

```csharp
// Flat with custom border color
buttonEdit.UseVisualStyle = false;
buttonEdit.FlatBorderColor = Color.Red;
buttonEdit.FlatStyle = FlatStyle.Flat;
buttonEdit.BorderSides = BorderSides.All;

// Or 3D sunken style
buttonEdit.Border3DStyle = Border3DStyle.Sunken;
```

## Size Settings

Control the dimensions of ButtonEdit.

### Minimum and Maximum Size

```csharp
// Set size constraints
buttonEdit.MinimumSize = new Size(100, 21);  // Minimum width 100
buttonEdit.MaximumSize = new Size(400, 21);  // Maximum width 400
```

### Typical Sizes

```csharp
// Standard height for single-line input
buttonEdit.Height = 21;

// Wider for file paths
buttonEdit.Width = 300;

// Compact
buttonEdit.Size = new Size(150, 21);

// Large
buttonEdit.Size = new Size(500, 21);
```

### Multiline Textbox

For multiline input, increase height:

```csharp
// Enable multiline in embedded textbox
buttonEdit.TextBox.Multiline = true;
buttonEdit.TextBox.Height = 80;  // Taller for multiple lines
buttonEdit.AutoSize = false;
```

## Foreground Settings

Control text appearance (font, color).

### Font Configuration

```csharp
// Set font for ButtonEdit
buttonEdit.Font = new Font("Verdana", 9F);
buttonEdit.Font = new Font("Arial", 10F, FontStyle.Bold);

// Set foreground color
buttonEdit.ForeColor = Color.SteelBlue;
```

### Override with Textbox Settings

Textbox font and color override ButtonEdit settings:

```csharp
buttonEdit.Font = new Font("Arial", 9F);  // Base font
buttonEdit.TextBox.Font = new Font("Courier New", 10F);  // Textbox overrides

buttonEdit.ForeColor = Color.Black;  // Base color
buttonEdit.TextBox.ForeColor = Color.DarkBlue;  // Textbox overrides
```

### Set Foreground for Child Buttons

```csharp
ButtonEditChildButton btn = new ButtonEditChildButton();
btn.Font = new Font("Arial", 8F);
btn.ForeColor = Color.DarkGray;
```

### Common Font Combinations

```csharp
// Professional corporate
buttonEdit.Font = new Font("Segoe UI", 9F);

// Monospace for data
buttonEdit.TextBox.Font = new Font("Courier New", 10F);

// Larger for visibility
buttonEdit.Font = new Font("Arial", 12F);

// System default
buttonEdit.Font = SystemFonts.DefaultFont;
```

## Character Casing

Automatically convert typed characters to upper or lower case.

### Casing Options

| Option | Description |
|--------|-------------|
| **Normal** | No conversion (default) |
| **Upper** | All characters uppercase |
| **Lower** | All characters lowercase |

### Implementation

```csharp
// Convert to uppercase
buttonEdit.CharacterCasing = CharacterCasing.Upper;

// Convert to lowercase
buttonEdit.CharacterCasing = CharacterCasing.Lower;

// No conversion
buttonEdit.CharacterCasing = CharacterCasing.Normal;
```

### Example: Case Conversion

```csharp
ButtonEdit codeEdit = new ButtonEdit();
codeEdit.CharacterCasing = CharacterCasing.Upper;  // Ensure code in uppercase
codeEdit.TextBox.Text = "code123";  // Displayed as "CODE123"

ButtonEdit emailEdit = new ButtonEdit();
emailEdit.CharacterCasing = CharacterCasing.Lower;  // Email always lowercase
emailEdit.TextBox.Text = "User@Example.Com";  // Displayed as "user@example.com"
```

### Textbox Override

```csharp
// ButtonEdit default is uppercase
buttonEdit.CharacterCasing = CharacterCasing.Upper;

// Textbox can override
buttonEdit.TextBox.CharacterCasing = CharacterCasing.Normal;
```

## Complete Styling Example

```csharp
public Form1()
{
    InitializeComponent();
    
    ButtonEdit buttonEdit = new ButtonEdit();
    buttonEdit.Location = new Point(50, 50);
    buttonEdit.Size = new Size(300, 21);
    
    // Apply modern style
    buttonEdit.UseVisualStyle = true;
    buttonEdit.ButtonStyle = ButtonAppearance.Office2016Colorful;
    
    // Border styling
    buttonEdit.Border3DStyle = Border3DStyle.Flat;
    
    // Size constraints
    buttonEdit.MinimumSize = new Size(200, 21);
    buttonEdit.MaximumSize = new Size(500, 21);
    
    // Text formatting
    buttonEdit.Font = new Font("Segoe UI", 9F);
    buttonEdit.ForeColor = Color.DarkSlateGray;
    
    // Character casing
    buttonEdit.CharacterCasing = CharacterCasing.Upper;
    
    this.Controls.Add(buttonEdit);
}
```
