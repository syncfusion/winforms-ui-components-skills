# Card Navigation in CardLayout

## Table of Contents
- [Selecting Cards by Name](#selecting-cards-by-name)
- [Navigation Methods](#navigation-methods)
- [Designer-Based Navigation](#designer-based-navigation)
- [Code-Based Navigation](#code-based-navigation)
- [ComboBox Integration](#combobox-integration)

## Selecting Cards by Name

The `SelectedCard` property allows you to display a specific card by its name. This is the most direct way to navigate to a known card.

### Basic Card Selection

```csharp
// Display a card by name
this.cardLayout1.SelectedCard = "WelcomePage";
```

```vb
' Display a card by name
Me.cardLayout1.SelectedCard = "WelcomePage"
```

### Getting the Currently Selected Card

```csharp
// Get the name of the currently displayed card
string currentCard = this.cardLayout1.SelectedCard;
```

```vb
' Get the name of the currently displayed card
Dim currentCard As String = Me.cardLayout1.SelectedCard
```

### Example: Card Selection Validation

```csharp
private void DisplayCard(string cardName)
{
    try
    {
        // Get all available card names
        string[] availableCards = this.cardLayout1.GetCardNames();
        
        // Check if requested card exists
        if (System.Array.IndexOf(availableCards, cardName) >= 0)
        {
            this.cardLayout1.SelectedCard = cardName;
        }
        else
        {
            MessageBox.Show("Card not found: " + cardName);
        }
    }
    catch (Exception ex)
    {
        MessageBox.Show("Error: " + ex.Message);
    }
}
```

```vb
Private Sub DisplayCard(cardName As String)
    Try
        ' Get all available card names
        Dim availableCards As String() = Me.cardLayout1.GetCardNames()
        
        ' Check if requested card exists
        If System.Array.IndexOf(availableCards, cardName) >= 0 Then
            Me.cardLayout1.SelectedCard = cardName
        Else
            MessageBox.Show("Card not found: " & cardName)
        End If
    Catch ex As Exception
        MessageBox.Show("Error: " & ex.Message)
    End Try
End Sub
```

## Navigation Methods

CardLayout provides four methods for sequential navigation through cards: `First()`, `Next()`, `Previous()`, and `Last()`.

### First() Method

Display the first card in the layout:

```csharp
this.cardLayout1.First();
```

```vb
Me.cardLayout1.First()
```

### Next() Method

Display the next card in sequence:

```csharp
// Move to next card
this.cardLayout1.Next();
```

```vb
' Move to next card
Me.cardLayout1.Next()
```

### Previous() Method

Display the previous card in sequence:

```csharp
// Move to previous card
this.cardLayout1.Previous();
```

```vb
' Move to previous card
Me.cardLayout1.Previous()
```

### Last() Method

Display the last card in the layout:

```csharp
this.cardLayout1.Last();
```

```vb
Me.cardLayout1.Last()
```

### Navigation Boundaries

When you call `Next()` or `Previous()` at the boundaries, the layout remains at the current card:

```csharp
// At the last card - Next() has no effect
this.cardLayout1.Last();
this.cardLayout1.Next(); // Still at last card

// At the first card - Previous() has no effect
this.cardLayout1.First();
this.cardLayout1.Previous(); // Still at first card
```

```vb
' At the last card - Next() has no effect
Me.cardLayout1.Last()
Me.cardLayout1.Next() ' Still at last card

' At the first card - Previous() has no effect
Me.cardLayout1.First()
Me.cardLayout1.Previous() ' Still at first card
```

## Designer-Based Navigation

### Using SmartTag

In Visual Studio designer, you can navigate through cards using the SmartTag feature (Visual Studio 2005 and later):

1. Select the CardLayout control in the designer
2. Click the smart tag (small arrow) that appears
3. Use the dropdown to select a card
4. The selected card will display in the designer

### Setting SelectedCard in Properties Panel

You can also set the `SelectedCard` property directly in the Properties panel:

1. Select the CardLayout control in the designer
2. In the Properties panel, find the `SelectedCard` property
3. Enter the card name or select from the dropdown
4. The designer will display the selected card

## Code-Based Navigation

### Button-Driven Navigation

Create Previous and Next buttons to navigate through cards:

```csharp
private void Form_Load(object sender, EventArgs e)
{
    // Set up button click handlers
    nextButton.Click += NextButton_Click;
    previousButton.Click += PreviousButton_Click;
    firstButton.Click += FirstButton_Click;
    lastButton.Click += LastButton_Click;
}

private void NextButton_Click(object sender, EventArgs e)
{
    if (this.cardLayout1.NextCardIndex >= 0)
    {
        this.cardLayout1.Next();
    }
}

private void PreviousButton_Click(object sender, EventArgs e)
{
    if (this.cardLayout1.PreviousCardIndex >= 0)
    {
        this.cardLayout1.Previous();
    }
}

private void FirstButton_Click(object sender, EventArgs e)
{
    this.cardLayout1.First();
}

private void LastButton_Click(object sender, EventArgs e)
{
    this.cardLayout1.Last();
}
```

```vb
Private Sub Form_Load(sender As Object, e As EventArgs)
    ' Set up button click handlers
    AddHandler nextButton.Click, AddressOf NextButton_Click
    AddHandler previousButton.Click, AddressOf PreviousButton_Click
    AddHandler firstButton.Click, AddressOf FirstButton_Click
    AddHandler lastButton.Click, AddressOf LastButton_Click
End Sub

Private Sub NextButton_Click(sender As Object, e As EventArgs)
    If Me.cardLayout1.NextCardIndex >= 0 Then
        Me.cardLayout1.Next()
    End If
End Sub

Private Sub PreviousButton_Click(sender As Object, e As EventArgs)
    If Me.cardLayout1.PreviousCardIndex >= 0 Then
        Me.cardLayout1.Previous()
    End If
End Sub

Private Sub FirstButton_Click(sender As Object, e As EventArgs)
    Me.cardLayout1.First()
End Sub

Private Sub LastButton_Click(sender As Object, e As EventArgs)
    Me.cardLayout1.Last()
End Sub
```

### Wizard Pattern Implementation

```csharp
public class WizardForm : Form
{
    private CardLayout cardLayout1;
    private Button nextButton;
    private Button previousButton;
    private Label stepLabel;

    private void UpdateUI()
    {
        // Update button states
        previousButton.Enabled = (this.cardLayout1.PreviousCardIndex >= 0);
        nextButton.Enabled = (this.cardLayout1.NextCardIndex >= 0);

        // Update step indicator
        string[] cardNames = this.cardLayout1.GetCardNames();
        int currentIndex = System.Array.IndexOf(cardNames, this.cardLayout1.SelectedCard);
        stepLabel.Text = string.Format("Step {0} of {1}", currentIndex + 1, cardNames.Length);
    }

    private void NextButton_Click(object sender, EventArgs e)
    {
        if (this.cardLayout1.NextCardIndex >= 0)
        {
            this.cardLayout1.Next();
            UpdateUI();
        }
    }

    private void PreviousButton_Click(object sender, EventArgs e)
    {
        if (this.cardLayout1.PreviousCardIndex >= 0)
        {
            this.cardLayout1.Previous();
            UpdateUI();
        }
    }
}
```

## ComboBox Integration

### Populating ComboBox with Card Names

```csharp
private void PopulateCardComboBox()
{
    // Get all card names from the layout
    string[] cardNames = this.cardLayout1.GetCardNames();
    
    // Clear existing items
    cardSelectionComboBox.Items.Clear();
    
    // Add all card names
    foreach (string cardName in cardNames)
    {
        cardSelectionComboBox.Items.Add(cardName);
    }
    
    // Set current selection
    cardSelectionComboBox.SelectedItem = this.cardLayout1.SelectedCard;
}
```

```vb
Private Sub PopulateCardComboBox()
    ' Get all card names from the layout
    Dim cardNames As String() = Me.cardLayout1.GetCardNames()
    
    ' Clear existing items
    cardSelectionComboBox.Items.Clear()
    
    ' Add all card names
    For Each cardName In cardNames
        cardSelectionComboBox.Items.Add(cardName)
    Next
    
    ' Set current selection
    cardSelectionComboBox.SelectedItem = Me.cardLayout1.SelectedCard
End Sub
```

### Handling ComboBox Selection

```csharp
private void CardSelectionComboBox_SelectedIndexChanged(object sender, EventArgs e)
{
    if (cardSelectionComboBox.SelectedItem != null)
    {
        string selectedCard = cardSelectionComboBox.SelectedItem.ToString();
        this.cardLayout1.SelectedCard = selectedCard;
    }
}
```

```vb
Private Sub CardSelectionComboBox_SelectedIndexChanged(sender As Object, e As EventArgs)
    If cardSelectionComboBox.SelectedItem IsNot Nothing Then
        Dim selectedCard As String = cardSelectionComboBox.SelectedItem.ToString()
        Me.cardLayout1.SelectedCard = selectedCard
    End If
End Sub
```

### Complete ComboBox Example

```csharp
public partial class CardLayoutForm : Form
{
    private CardLayout cardLayout1;
    private ComboBox cardSelectionComboBox;

    public CardLayoutForm()
    {
        InitializeComponent();
        this.Load += Form_Load;
    }

    private void Form_Load(object sender, EventArgs e)
    {
        // Populate ComboBox when form loads
        PopulateCardComboBox();
        cardSelectionComboBox.SelectedIndexChanged += CardSelectionComboBox_SelectedIndexChanged;
    }

    private void PopulateCardComboBox()
    {
        string[] cardNames = this.cardLayout1.GetCardNames();
        foreach (string cardName in cardNames)
        {
            cardSelectionComboBox.Items.Add(cardName);
        }
        cardSelectionComboBox.SelectedItem = this.cardLayout1.SelectedCard;
    }

    private void CardSelectionComboBox_SelectedIndexChanged(object sender, EventArgs e)
    {
        if (cardSelectionComboBox.SelectedItem != null)
        {
            this.cardLayout1.SelectedCard = cardSelectionComboBox.SelectedItem.ToString();
        }
    }
}
```

```vb
Public Partial Class CardLayoutForm
    Inherits Form
    
    Private cardLayout1 As CardLayout
    Private cardSelectionComboBox As ComboBox

    Public Sub New()
        InitializeComponent()
        AddHandler Me.Load, AddressOf Form_Load
    End Sub

    Private Sub Form_Load(sender As Object, e As EventArgs)
        ' Populate ComboBox when form loads
        PopulateCardComboBox()
        AddHandler cardSelectionComboBox.SelectedIndexChanged, AddressOf CardSelectionComboBox_SelectedIndexChanged
    End Sub

    Private Sub PopulateCardComboBox()
        Dim cardNames As String() = Me.cardLayout1.GetCardNames()
        For Each cardName In cardNames
            cardSelectionComboBox.Items.Add(cardName)
        Next
        cardSelectionComboBox.SelectedItem = Me.cardLayout1.SelectedCard
    End Sub

    Private Sub CardSelectionComboBox_SelectedIndexChanged(sender As Object, e As EventArgs)
        If cardSelectionComboBox.SelectedItem IsNot Nothing Then
            Me.cardLayout1.SelectedCard = cardSelectionComboBox.SelectedItem.ToString()
        End If
    End Sub
End Class
```
