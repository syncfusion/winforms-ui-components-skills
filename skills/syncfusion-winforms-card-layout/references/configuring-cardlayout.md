# Configuring CardLayout

## Table of Contents
- [Card Naming](#card-naming)
- [Card Index](#card-index)
- [Aspect Ratio](#aspect-ratio)
- [Layout Modes](#layout-modes)
- [Size Configuration](#size-configuration)

## Card Naming

By default, CardLayout automatically generates unique names for each card added to the layout. You can customize these names to make card selection and management more intuitive.

### Setting Custom Card Names

Use the `SetCardName()` method to assign meaningful names to cards:

```csharp
// Set custom name for a card
this.cardLayout1.SetCardName(this.panel1, "WelcomePage");
this.cardLayout1.SetCardName(this.panel2, "FormPage");
this.cardLayout1.SetCardName(this.panel3, "SummaryPage");
```

```vb
' Set custom name for a card
Me.cardLayout1.SetCardName(Me.panel1, "WelcomePage")
Me.cardLayout1.SetCardName(Me.panel2, "FormPage")
Me.cardLayout1.SetCardName(Me.panel3, "SummaryPage")
```

### Getting Card Names

Retrieve a single card name or all card names in the layout:

```csharp
// Get the name of a specific card
string cardName = this.cardLayout1.GetCardName(this.panel1);

// Get all card names as array
string[] allCardNames = this.cardLayout1.GetCardNames();

// Get a control by its card name
Control cardControl = this.cardLayout1.GetComponentFromName("WelcomePage");
```

```vb
' Get the name of a specific card
Dim cardName As String = Me.cardLayout1.GetCardName(Me.panel1)

' Get all card names as array
Dim allCardNames As String() = Me.cardLayout1.GetCardNames()

' Get a control by its card name
Dim cardControl As Control = Me.cardLayout1.GetComponentFromName("WelcomePage")
```

### Generating Unique Card Names

When adding new cards dynamically, use `GetNewCardName()` to automatically generate unique names:

```csharp
// Generate a new unique card name
string newCardName = this.cardLayout1.GetNewCardName();

// Create and add new card
Panel newCard = new Panel();
cardLayoutPanel.Controls.Add(newCard);
this.cardLayout1.SetCardName(newCard, newCardName);
```

```vb
' Generate a new unique card name
Dim newCardName As String = Me.cardLayout1.GetNewCardName()

' Create and add new card
Dim newCard As New Panel()
cardLayoutPanel.Controls.Add(newCard)
Me.cardLayout1.SetCardName(newCard, newCardName)
```

### Card Naming Methods Reference

| Method | Returns | Description |
|--------|---------|-------------|
| `GetCardName(Control)` | string | Returns the name of the specified card |
| `GetCardNames()` | string[] | Returns an array of all card names |
| `GetComponentFromName(string)` | Control | Returns the control associated with the given card name |
| `GetNewCardName()` | string | Generates a new unique card name |
| `SetCardName(Control, string)` | void | Assigns a name to the specified card |

## Card Index

CardLayout provides properties to determine the indices of the previous and next cards. This is useful for implementing navigation controls and determining card position.

### Index Properties

```csharp
// Get the index of the next card
int nextIndex = this.cardLayout1.NextCardIndex;

// Get the index of the previous card
int previousIndex = this.cardLayout1.PreviousCardIndex;

// Example: disable Next button if at last card
if (nextIndex == -1)
{
    nextButton.Enabled = false;
}

// Example: disable Previous button if at first card
if (previousIndex == -1)
{
    previousButton.Enabled = false;
}
```

```vb
' Get the index of the next card
Dim nextIndex As Integer = Me.cardLayout1.NextCardIndex

' Get the index of the previous card
Dim previousIndex As Integer = Me.cardLayout1.PreviousCardIndex

' Example: disable Next button if at last card
If nextIndex = -1 Then
    nextButton.Enabled = False
End If

' Example: disable Previous button if at first card
If previousIndex = -1 Then
    previousButton.Enabled = False
End If
```

### Using Index Values

The index values return -1 when there is no next or previous card:

```csharp
private void NavigateNext()
{
    // Check if next card exists
    if (this.cardLayout1.NextCardIndex >= 0)
    {
        this.cardLayout1.Next();
    }
    else
    {
        MessageBox.Show("Already at the last card.");
    }
}

private void NavigatePrevious()
{
    // Check if previous card exists
    if (this.cardLayout1.PreviousCardIndex >= 0)
    {
        this.cardLayout1.Previous();
    }
    else
    {
        MessageBox.Show("Already at the first card.");
    }
}
```

```vb
Private Sub NavigateNext()
    ' Check if next card exists
    If Me.cardLayout1.NextCardIndex >= 0 Then
        Me.cardLayout1.Next()
    Else
        MessageBox.Show("Already at the last card.")
    End If
End Sub

Private Sub NavigatePrevious()
    ' Check if previous card exists
    If Me.cardLayout1.PreviousCardIndex >= 0 Then
        Me.cardLayout1.Previous()
    Else
        MessageBox.Show("Already at the first card.")
    End If
End Sub
```

## Aspect Ratio

The aspect ratio property controls whether a card maintains its original width-to-height ratio when resized by the layout manager.

### Enabling Aspect Ratio Maintenance

```csharp
// Enable aspect ratio maintenance for a card
this.cardLayout1.SetMaintainAspectRatio(this.panel1, true);

// Disable aspect ratio maintenance
this.cardLayout1.SetMaintainAspectRatio(this.panel1, false);
```

```vb
' Enable aspect ratio maintenance for a card
Me.cardLayout1.SetMaintainAspectRatio(Me.panel1, True)

' Disable aspect ratio maintenance
Me.cardLayout1.SetMaintainAspectRatio(Me.panel1, False)
```

### Checking Aspect Ratio Setting

```csharp
// Check if a card maintains aspect ratio
bool maintainsRatio = this.cardLayout1.GetMaintainAspectRatio(this.panel1);

if (maintainsRatio)
{
    // Card maintains aspect ratio
}
```

```vb
' Check if a card maintains aspect ratio
Dim maintainsRatio As Boolean = Me.cardLayout1.GetMaintainAspectRatio(Me.panel1)

If maintainsRatio Then
    ' Card maintains aspect ratio
End If
```

### Use Cases for Aspect Ratio

- **Images and graphics**: Maintain aspect ratio to prevent distortion
- **Content with specific proportions**: Keep 16:9, 4:3, or other ratios intact
- **Responsive layouts**: Ensure cards scale proportionally when container resizes

## Layout Modes

CardLayout provides two layout modes to control how child controls are sized and positioned within the container.

### Default Layout Mode

In `Default` mode:
- If the container is larger than the card's preferred size, the card is centered
- If the container is smaller than the card's preferred size, the card shrinks to fit
- Aspect ratio can be maintained during shrinking via `MaintainAspectRatio` property

```csharp
this.cardLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.CardLayoutMode.Default;
```

```vb
Me.cardLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.CardLayoutMode.Default
```

### Fill Layout Mode

In `Fill` mode:
- The card is resized to fill the entire container client area
- Useful for maximizing content display area
- Overrides preferred size settings

```csharp
this.cardLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.CardLayoutMode.Fill;
```

```vb
Me.cardLayout1.LayoutMode = Syncfusion.Windows.Forms.Tools.CardLayoutMode.Fill
```

### Comparing Layout Modes

| Aspect | Default Mode | Fill Mode |
|--------|--------------|-----------|
| Card sizing | Respects preferred size | Fills container |
| Positioning | Centered if container is larger | Fills entire area |
| Use case | Content-centric layouts | Full-screen cards |
| Aspect ratio | Can be maintained | May be distorted |

### Example: Dynamic Layout Mode Switching

```csharp
private void FillModeButton_Click(object sender, EventArgs e)
{
    this.cardLayout1.LayoutMode = CardLayoutMode.Fill;
}

private void DefaultModeButton_Click(object sender, EventArgs e)
{
    this.cardLayout1.LayoutMode = CardLayoutMode.Default;
}
```

```vb
Private Sub FillModeButton_Click(sender As Object, e As EventArgs)
    Me.cardLayout1.LayoutMode = CardLayoutMode.Fill
End Sub

Private Sub DefaultModeButton_Click(sender As Object, e As EventArgs)
    Me.cardLayout1.LayoutMode = CardLayoutMode.Default
End Sub
```

## Size Configuration

Control the sizing behavior of cards using extended properties for preferred and minimum sizes.

### Setting Preferred Size

The preferred size defines the ideal dimensions for a card:

```csharp
// Set preferred size for a card
this.cardLayout1.SetPreferredSize(this.panel1, new System.Drawing.Size(600, 400));
```

```vb
' Set preferred size for a card
Me.cardLayout1.SetPreferredSize(Me.panel1, New System.Drawing.Size(600, 400))
```

### Setting Minimum Size

The minimum size prevents cards from shrinking below a certain threshold:

```csharp
// Set minimum size for a card
this.cardLayout1.SetMinimumSize(this.panel1, new System.Drawing.Size(200, 150));
```

```vb
' Set minimum size for a card
Me.cardLayout1.SetMinimumSize(Me.panel1, New System.Drawing.Size(200, 150))
```

### Combined Size Configuration

```csharp
// Configure both preferred and minimum sizes
this.cardLayout1.SetPreferredSize(this.panel1, new System.Drawing.Size(600, 400));
this.cardLayout1.SetMinimumSize(this.panel1, new System.Drawing.Size(300, 250));

// Set layout mode to Default to respect these sizes
this.cardLayout1.LayoutMode = CardLayoutMode.Default;
```

```vb
' Configure both preferred and minimum sizes
Me.cardLayout1.SetPreferredSize(Me.panel1, New System.Drawing.Size(600, 400))
Me.cardLayout1.SetMinimumSize(Me.panel1, New System.Drawing.Size(300, 250))

' Set layout mode to Default to respect these sizes
Me.cardLayout1.LayoutMode = CardLayoutMode.Default
```

### Size Behavior with Layout Modes

**Default Mode:**
- If container > preferred size: Card centered at preferred size
- If container < preferred size: Card shrinks to minimum size
- Aspect ratio maintained if enabled

**Fill Mode:**
- Card always resizes to fill container
- Preferred and minimum sizes are overridden
