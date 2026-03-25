# Configuring Child Controls in CardLayout

## Table of Contents
- [Adding Child Controls](#adding-child-controls)
- [Container Setup](#container-setup)
- [Image Settings](#image-settings)
- [Size Configuration](#size-configuration)
- [Styling Child Controls](#styling-child-controls)

## Adding Child Controls

Child controls added to a CardLayout automatically become "cards" in the layout stack. Each card is managed independently, and only one card is visible at a time.

### Adding Controls via Designer

1. Create the CardLayout and set its container
2. Drag controls (Panel, Label, etc.) from the toolbox to the designer
3. Each control automatically becomes a card
4. Drop controls on the main form or container panel

### Adding Controls Programmatically

```csharp
// Create a panel to act as a card
Panel card1 = new Panel();
card1.BackColor = Color.LightBlue;

// Add the panel to the container
cardLayoutPanel.Controls.Add(card1);

// The card is now managed by CardLayout
```

```vb
' Create a panel to act as a card
Dim card1 As New Panel()
card1.BackColor = Color.LightBlue

' Add the panel to the container
cardLayoutPanel.Controls.Add(card1)

' The card is now managed by CardLayout
```

### Adding Child Controls to Cards

Once a card exists, you can add controls to it like any other container:

```csharp
// Create a label and add to card1
Label label1 = new Label();
label1.Text = "Welcome to Card 1";
label1.AutoSize = true;
label1.Location = new Point(10, 10);
card1.Controls.Add(label1);

// Create a button and add to card1
Button button1 = new Button();
button1.Text = "Submit";
button1.Location = new Point(10, 40);
card1.Controls.Add(button1);
```

```vb
' Create a label and add to card1
Dim label1 As New Label()
label1.Text = "Welcome to Card 1"
label1.AutoSize = True
label1.Location = New Point(10, 10)
card1.Controls.Add(label1)

' Create a button and add to card1
Dim button1 As New Button()
button1.Text = "Submit"
button1.Location = New Point(10, 40)
card1.Controls.Add(button1)
```

### Example: Multiple Cards with Controls

```csharp
private void CreateMultipleCards()
{
    // Card 1 - Welcome
    Panel card1 = new Panel();
    card1.BackColor = Color.LightBlue;
    Label welcomeLabel = new Label { Text = "Step 1: Welcome", AutoSize = true };
    card1.Controls.Add(welcomeLabel);
    cardLayoutPanel.Controls.Add(card1);
    cardLayout1.SetCardName(card1, "WelcomeCard");

    // Card 2 - Form
    Panel card2 = new Panel();
    card2.BackColor = Color.LightGreen;
    Label formLabel = new Label { Text = "Step 2: Enter Details", AutoSize = true };
    TextBox textBox = new TextBox { Location = new Point(10, 30) };
    card2.Controls.Add(formLabel);
    card2.Controls.Add(textBox);
    cardLayoutPanel.Controls.Add(card2);
    cardLayout1.SetCardName(card2, "FormCard");

    // Card 3 - Confirmation
    Panel card3 = new Panel();
    card3.BackColor = Color.LightYellow;
    Label confirmLabel = new Label { Text = "Step 3: Confirmation", AutoSize = true };
    card3.Controls.Add(confirmLabel);
    cardLayoutPanel.Controls.Add(card3);
    cardLayout1.SetCardName(card3, "ConfirmCard");
}
```

## Container Setup

The CardLayout requires a container control to manage cards. The container acts as the parent for all cards.

### Setting the Container

```csharp
// Method 1: Set the form as container
cardLayout1.ContainerControl = this; // Where 'this' is the Form

// Method 2: Set a panel as container
Panel mainPanel = new Panel();
mainPanel.Dock = DockStyle.Fill;
cardLayout1.ContainerControl = mainPanel;
this.Controls.Add(mainPanel);
```

```vb
' Method 1: Set the form as container
cardLayout1.ContainerControl = Me ' Where 'Me' is the Form

' Method 2: Set a panel as container
Dim mainPanel As New Panel()
mainPanel.Dock = DockStyle.Fill
cardLayout1.ContainerControl = mainPanel
Me.Controls.Add(mainPanel)
```

### Container Best Practices

1. **Set container before adding cards**
   ```csharp
   // Correct order
   cardLayout1.ContainerControl = cardLayoutPanel;
   cardLayoutPanel.Controls.Add(card1);
   ```

2. **Use Panel for better control**
   ```csharp
   // Panels provide better sizing control
   Panel containerPanel = new Panel();
   containerPanel.Dock = DockStyle.Fill;
   cardLayout1.ContainerControl = containerPanel;
   ```

3. **Ensure container is properly sized**
   ```csharp
   containerPanel.Width = 600;
   containerPanel.Height = 400;
   ```

## Image Settings

You can display background images on cards (child controls). This is especially useful for visual card identification or decorative purposes.

### Setting Background Images

```csharp
// Load and set background image for a card
System.Drawing.Image backgroundImage = System.Drawing.Image.FromFile("path/to/image.jpg");
panel1.BackgroundImage = backgroundImage;
```

```vb
' Load and set background image for a card
Dim backgroundImage As System.Drawing.Image = System.Drawing.Image.FromFile("path/to/image.jpg")
panel1.BackgroundImage = backgroundImage
```

### Image Properties

```csharp
// Set image layout
panel1.BackgroundImageLayout = ImageLayout.Stretch; // Fill entire control
panel1.BackgroundImageLayout = ImageLayout.Center;  // Center the image
panel1.BackgroundImageLayout = ImageLayout.Tile;    // Tile the image
panel1.BackgroundImageLayout = ImageLayout.Zoom;    // Maintain aspect ratio while filling
```

```vb
' Set image layout
panel1.BackgroundImageLayout = ImageLayout.Stretch ' Fill entire control
panel1.BackgroundImageLayout = ImageLayout.Center  ' Center the image
panel1.BackgroundImageLayout = ImageLayout.Tile    ' Tile the image
panel1.BackgroundImageLayout = ImageLayout.Zoom    ' Maintain aspect ratio while filling
```

### Example: Cards with Background Images

```csharp
private void SetupCardsWithImages()
{
    // Load images
    Image image1 = Image.FromFile("card1.jpg");
    Image image2 = Image.FromFile("card2.jpg");

    // Set images for cards
    panel1.BackgroundImage = image1;
    panel1.BackgroundImageLayout = ImageLayout.Stretch;

    panel2.BackgroundImage = image2;
    panel2.BackgroundImageLayout = ImageLayout.Zoom;
}
```

### Image Disposal

Remember to dispose of images when done:

```csharp
private void Form_FormClosing(object sender, FormClosingEventArgs e)
{
    if (panel1.BackgroundImage != null)
    {
        panel1.BackgroundImage.Dispose();
    }

    if (panel2.BackgroundImage != null)
    {
        panel2.BackgroundImage.Dispose();
    }
}
```

```vb
Private Sub Form_FormClosing(sender As Object, e As FormClosingEventArgs)
    If panel1.BackgroundImage IsNot Nothing Then
        panel1.BackgroundImage.Dispose()
    End If

    If panel2.BackgroundImage IsNot Nothing Then
        panel2.BackgroundImage.Dispose()
    End If
End Sub
```

## Size Configuration

Control how cards are sized within the container using extended properties.

### Setting Preferred Size

The preferred size defines the ideal dimensions for a card:

```csharp
// Set preferred size for a card
cardLayout1.SetPreferredSize(panel1, new Size(600, 400));
cardLayout1.SetPreferredSize(panel2, new Size(600, 400));
```

```vb
' Set preferred size for a card
cardLayout1.SetPreferredSize(panel1, New Size(600, 400))
cardLayout1.SetPreferredSize(panel2, New Size(600, 400))
```

### Setting Minimum Size

The minimum size prevents cards from becoming too small:

```csharp
// Set minimum size for a card
cardLayout1.SetMinimumSize(panel1, new Size(300, 200));
cardLayout1.SetMinimumSize(panel2, new Size(300, 200));
```

```vb
' Set minimum size for a card
cardLayout1.SetMinimumSize(panel1, New Size(300, 200))
cardLayout1.SetMinimumSize(panel2, New Size(300, 200))
```

### Getting Size Values

```csharp
// Get the preferred size
Size prefSize = cardLayout1.GetPreferredSize(panel1);

// Get the minimum size
Size minSize = cardLayout1.GetMinimumSize(panel1);
```

```vb
' Get the preferred size
Dim prefSize As Size = cardLayout1.GetPreferredSize(panel1)

' Get the minimum size
Dim minSize As Size = cardLayout1.GetMinimumSize(panel1)
```

### Size Configuration Example

```csharp
private void ConfigureCardSizes()
{
    // Different sizes for different cards
    Size largeCardSize = new Size(800, 600);
    Size smallCardSize = new Size(400, 300);
    Size minSize = new Size(200, 150);

    // Set sizes
    cardLayout1.SetPreferredSize(card1, largeCardSize);
    cardLayout1.SetMinimumSize(card1, minSize);

    cardLayout1.SetPreferredSize(card2, smallCardSize);
    cardLayout1.SetMinimumSize(card2, minSize);

    // Set layout mode
    cardLayout1.LayoutMode = CardLayoutMode.Default;
}
```

## Styling Child Controls

Apply visual styles to cards and their child controls to create cohesive designs.

### Card Background and Border

```csharp
// Set card background color
panel1.BackColor = Color.White;

// Add border by using a different control
Panel borderPanel = new Panel();
borderPanel.BorderStyle = BorderStyle.FixedSingle;
borderPanel.BackColor = Color.White;
```

```vb
' Set card background color
panel1.BackColor = Color.White

' Add border by using a different control
Dim borderPanel As New Panel()
borderPanel.BorderStyle = BorderStyle.FixedSingle
borderPanel.BackColor = Color.White
```

### Styling Child Controls Within Cards

```csharp
// Style a label
Label titleLabel = new Label();
titleLabel.Text = "Card Title";
titleLabel.Font = new Font("Arial", 16, FontStyle.Bold);
titleLabel.ForeColor = Color.DarkBlue;
titleLabel.AutoSize = true;
panel1.Controls.Add(titleLabel);

// Style a button
Button actionButton = new Button();
actionButton.Text = "Next";
actionButton.BackColor = Color.LightBlue;
actionButton.ForeColor = Color.White;
actionButton.Location = new Point(10, 50);
panel1.Controls.Add(actionButton);
```

```vb
' Style a label
Dim titleLabel As New Label()
titleLabel.Text = "Card Title"
titleLabel.Font = New Font("Arial", 16, FontStyle.Bold)
titleLabel.ForeColor = Color.DarkBlue
titleLabel.AutoSize = True
panel1.Controls.Add(titleLabel)

' Style a button
Dim actionButton As New Button()
actionButton.Text = "Next"
actionButton.BackColor = Color.LightBlue
actionButton.ForeColor = Color.White
actionButton.Location = New Point(10, 50)
panel1.Controls.Add(actionButton)
```

### Example: Styled Wizard Card

```csharp
private Panel CreateStyledCard(string title, string description)
{
    Panel card = new Panel();
    card.BackColor = Color.WhiteSmoke;
    card.Padding = new Padding(20);

    // Title
    Label titleLabel = new Label();
    titleLabel.Text = title;
    titleLabel.Font = new Font("Arial", 14, FontStyle.Bold);
    titleLabel.Dock = DockStyle.Top;
    titleLabel.Height = 30;
    card.Controls.Add(titleLabel);

    // Description
    Label descLabel = new Label();
    descLabel.Text = description;
    descLabel.Font = new Font("Arial", 10);
    descLabel.Dock = DockStyle.Top;
    descLabel.Height = 60;
    descLabel.AutoSize = false;
    descLabel.WordWrap = true;
    card.Controls.Add(descLabel);

    return card;
}
```
