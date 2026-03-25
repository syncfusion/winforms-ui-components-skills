# Getting Started with CardLayout

## Table of Contents
- [Assembly Installation](#assembly-installation)
- [Adding via Designer](#adding-via-designer)
- [Adding via Code](#adding-via-code)
- [Basic Navigation](#basic-navigation)

## Assembly Installation

### NuGet Package Installation

To use CardLayout in your Windows Forms application, you need to add the Syncfusion NuGet package:

```
Install-Package Syncfusion.Shared.Base
```

Alternatively, add the required assembly reference manually:
- `Syncfusion.Shared.Base.dll`

For detailed instructions on installing NuGet packages in Windows Forms applications, refer to the Syncfusion installation documentation.

### Required Namespace

Include the following namespace in your code:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

## Adding via Designer

### Step-by-Step Process

1. **Create a new Windows Forms project** in Visual Studio

2. **Drag CardLayout from the toolbox** to the designer view
   - The required assembly `Syncfusion.Shared.Base` will be automatically added

3. **Confirm container setup**
   - A popup dialog will appear asking to add CardLayout to the form
   - Click **Yes** to set the form as the container control

4. **Add child controls (cards)**
   - Drag Panel or other controls from the toolbox to the designer
   - Each control added becomes a card in the layout
   - Only one card will be visible at design time and runtime

### Example Layout Structure
```
Form
└── CardLayout (ContainerControl = Form)
    ├── Panel1 (Card1)
    ├── Panel2 (Card2)
    └── Panel3 (Card3)
```

## Adding via Code

### Basic Setup

```csharp
// Create CardLayout instance
private Syncfusion.Windows.Forms.Tools.CardLayout cardLayout1;

// In your form constructor or Load event
this.cardLayout1 = new Syncfusion.Windows.Forms.Tools.CardLayout(this.components);
this.components = new System.ComponentModel.Container();

// Set the form as the container
this.cardLayout1.ContainerControl = this;
```

```vb
' Create CardLayout instance
Private cardLayout1 As Syncfusion.Windows.Forms.Tools.CardLayout

' In your form constructor or Load event
Me.cardLayout1 = New Syncfusion.Windows.Forms.Tools.CardLayout(Me.components)
Me.components = New System.ComponentModel.Container()

' Set the form as the container
Me.cardLayout1.ContainerControl = Me
```

### Creating Cards (Panels)

```csharp
// Create main container panel
private System.Windows.Forms.Panel cardLayoutPanel;
this.cardLayoutPanel = new System.Windows.Forms.Panel();
this.cardLayoutPanel.BackColor = Color.White;
this.cardLayoutPanel.Dock = DockStyle.Fill;

// Set the panel as container
this.cardLayout1.ContainerControl = cardLayoutPanel;

// Create individual cards
private System.Windows.Forms.Panel panel1;
private System.Windows.Forms.Panel panel2;
private System.Windows.Forms.Panel panel3;

this.panel1 = new System.Windows.Forms.Panel();
this.panel2 = new System.Windows.Forms.Panel();
this.panel3 = new System.Windows.Forms.Panel();

// Set properties for each card
this.cardLayout1.SetPreferredSize(this.panel1, new System.Drawing.Size(400, 300));
this.cardLayout1.SetPreferredSize(this.panel2, new System.Drawing.Size(400, 300));
this.cardLayout1.SetPreferredSize(this.panel3, new System.Drawing.Size(400, 300));

// Add cards to container
this.cardLayoutPanel.Controls.Add(this.panel1);
this.cardLayoutPanel.Controls.Add(this.panel2);
this.cardLayoutPanel.Controls.Add(this.panel3);

// Add container to form
this.Controls.Add(this.cardLayoutPanel);
```

```vb
' Create main container panel
Private cardLayoutPanel As System.Windows.Forms.Panel
Me.cardLayoutPanel = New System.Windows.Forms.Panel()
Me.cardLayoutPanel.BackColor = Color.White
Me.cardLayoutPanel.Dock = DockStyle.Fill

' Set the panel as container
Me.cardLayout1.ContainerControl = cardLayoutPanel

' Create individual cards
Private panel1 As System.Windows.Forms.Panel
Private panel2 As System.Windows.Forms.Panel
Private panel3 As System.Windows.Forms.Panel

Me.panel1 = New System.Windows.Forms.Panel()
Me.panel2 = New System.Windows.Forms.Panel()
Me.panel3 = New System.Windows.Forms.Panel()

' Set properties for each card
Me.cardLayout1.SetPreferredSize(Me.panel1, New System.Drawing.Size(400, 300))
Me.cardLayout1.SetPreferredSize(Me.panel2, New System.Drawing.Size(400, 300))
Me.cardLayout1.SetPreferredSize(Me.panel3, New System.Drawing.Size(400, 300))

' Add cards to container
Me.cardLayoutPanel.Controls.Add(Me.panel1)
Me.cardLayoutPanel.Controls.Add(Me.panel2)
Me.cardLayoutPanel.Controls.Add(Me.panel3)

' Add container to form
Me.Controls.Add(Me.cardLayoutPanel)
```

### Adding Controls to Cards

You can add controls to individual cards just like any other container:

```csharp
// Add label to panel1
Label label1 = new Label();
label1.Text = "Welcome to Card 1";
label1.Location = new Point(10, 10);
panel1.Controls.Add(label1);

// Add button to panel2
Button button1 = new Button();
button1.Text = "Click Me";
button1.Location = new Point(10, 10);
panel2.Controls.Add(button1);
```

```vb
' Add label to panel1
Dim label1 As New Label()
label1.Text = "Welcome to Card 1"
label1.Location = New Point(10, 10)
panel1.Controls.Add(label1)

' Add button to panel2
Dim button1 As New Button()
button1.Text = "Click Me"
button1.Location = New Point(10, 10)
panel2.Controls.Add(button1)
```

## Basic Navigation

### Using Next() and Previous() Methods

```csharp
// Move to next card
private void NextButton_Click(object sender, EventArgs e)
{
    this.cardLayout1.Next();
}

// Move to previous card
private void PreviousButton_Click(object sender, EventArgs e)
{
    this.cardLayout1.Previous();
}
```

```vb
' Move to next card
Private Sub NextButton_Click(sender As Object, e As EventArgs)
    Me.cardLayout1.Next()
End Sub

' Move to previous card
Private Sub PreviousButton_Click(sender As Object, e As EventArgs)
    Me.cardLayout1.Previous()
End Sub
```

### Selecting a Card by Name

```csharp
// Display a specific card
this.cardLayout1.SelectedCard = "Card1";
```

```vb
' Display a specific card
Me.cardLayout1.SelectedCard = "Card1"
```

### First and Last Navigation

```csharp
// Jump to first card
this.cardLayout1.First();

// Jump to last card
this.cardLayout1.Last();
```

```vb
' Jump to first card
Me.cardLayout1.First()

' Jump to last card
Me.cardLayout1.Last()
```
