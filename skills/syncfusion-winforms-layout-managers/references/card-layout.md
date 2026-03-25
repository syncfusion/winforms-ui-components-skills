# CardLayout

## Table of Contents
- [Overview](#overview)
- [What is CardLayout](#what-is-cardlayout)
- [Key Features](#key-features)
- [Card Naming](#card-naming)
- [Adding Cards via Designer](#adding-cards-via-designer)
- [Adding Cards via Code](#adding-cards-via-code)
- [Navigation Methods](#navigation-methods)
- [LayoutMode Property](#layoutmode-property)
- [Card Index Properties](#card-index-properties)
- [Aspect Ratio](#aspect-ratio)
- [Image Settings](#image-settings)
- [Wizard Implementation](#wizard-implementation)
- [Property Pages Implementation](#property-pages-implementation)
- [Dynamic Card Management](#dynamic-card-management)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

**CardLayout** is a layout manager that treats each child control in a container as a card in a stack. Only one card is visible at a time, and you can navigate between cards using methods like First(), Last(), Next(), Previous(), or jump to a specific card by name.

This layout is perfect for wizards, property pages, multi-step forms, or any scenario where you want to show one panel at a time without using tab controls.

## What is CardLayout

**Purpose:** Show one child control at a time from a stack of cards

**Behavior:**
- Stack metaphor: Like a deck of cards, only the top card is visible
- Navigation: Move between cards sequentially or jump to specific card
- Single visibility: All other cards are hidden when one is shown
- Container-based: Works with Panel, Form, or any ContainerControl

**Common Uses:**
- Setup wizards (Welcome → Configure → Finish)
- Property pages without tabs
- Multi-step data entry forms
- Slideshow presentations
- Tutorial walkthroughs

> **Note:** Syncfusion's WizardControl uses CardLayout internally in its implementation.

## Key Features

- **Card Naming:** Each card has a unique string identifier
- **Navigation:** First, Last, Next, Previous, ActivateCard(name)
- **Layout Modes:** Default (centered) or Fill (full container)
- **Card Index:** Get previous/next card indices
- **Aspect Ratio:** Maintain aspect ratio when resizing
- **Image Support:** Add images to card panels
- **Designer Support:** SelectedCard property for design-time viewing

## Card Naming

### Setting Card Names

Each card must have a unique name (string) for identification and navigation:

```csharp
// Set card names using SetCardName method
cardLayout1.SetCardName(panel1, "Welcome");
cardLayout1.SetCardName(panel2, "Configure");
cardLayout1.SetCardName(panel3, "Finish");
```

```vbnet
' Set card names
cardLayout1.SetCardName(panel1, "Welcome")
cardLayout1.SetCardName(panel2, "Configure")
cardLayout1.SetCardName(panel3, "Finish")
```

**Designer:** Card names appear as an extended property in the Properties window for each child control.

### Card Name Methods

| Method | Description |
|--------|-------------|
| `SetCardName(Control, string)` | Sets the card name for a child control |
| `GetCardName(Control)` | Returns the card name of a child control |
| `GetCardNames()` | Returns an array of all card names |
| `GetComponentFromName(string)` | Returns the control associated with a card name |
| `GetNewCardName()` | Generates a new unique card name |

```csharp
// Get card name
string cardName = cardLayout1.GetCardName(panel1);

// Get all card names
string[] allCards = cardLayout1.GetCardNames();

// Get control from name
Control card = cardLayout1.GetComponentFromName("Welcome");
```

## Adding Cards via Designer

### Step-by-Step Designer Usage

1. **Add CardLayout to Form:**
   - Drag `CardLayout` from Toolbox to form
   - CardLayout appears in component tray
   - Popup asks if form should be container → Click **Yes**

2. **Set ContainerControl (if not using Form):**
   - Select CardLayout in component tray
   - In Properties, set `ContainerControl` to your Panel

3. **Add Child Controls (Cards):**
   - Drag `Panel` controls onto container (one panel per card)
   - Each panel becomes a card
   - Add controls (labels, textboxes, etc.) to each panel

4. **Set Card Names:**
   - Select each panel
   - In Properties, find "CardName on cardLayout1" extended property
   - Set descriptive names: "Welcome", "Configure", "Finish", etc.

5. **View Different Cards at Design Time:**
   - Select CardLayout in component tray
   - In Properties, use `SelectedCard` property
   - Select different cards from dropdown to view/edit them

6. **Configure LayoutMode:**
   - Select CardLayout
   - Set `LayoutMode` property (Default or Fill)

## Adding Cards via Code

### Complete Code Example (3-Card Wizard)

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public class WizardForm : Form
{
    private Panel containerPanel;
    private CardLayout cardLayout1;
    private Panel welcomePanel, configurePanel, finishPanel;
    private Button btnNext, btnPrevious, btnFinish;

    public WizardForm()
    {
        // Create container panel
        containerPanel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White
        };
        this.Controls.Add(containerPanel);

        // Create CardLayout
        cardLayout1 = new CardLayout();
        cardLayout1.ContainerControl = containerPanel;
        cardLayout1.LayoutMode = CardLayoutMode.Fill; // Fill entire container

        // Create cards (panels)
        welcomePanel = CreateWelcomeCard();
        configurePanel = CreateConfigureCard();
        finishPanel = CreateFinishCard();

        // Add cards to container
        containerPanel.Controls.Add(welcomePanel);
        containerPanel.Controls.Add(configurePanel);
        containerPanel.Controls.Add(finishPanel);

        // Set card names
        cardLayout1.SetCardName(welcomePanel, "Welcome");
        cardLayout1.SetCardName(configurePanel, "Configure");
        cardLayout1.SetCardName(finishPanel, "Finish");

        // Show first card
        cardLayout1.First();

        // Create navigation buttons
        CreateNavigationButtons();
    }

    private Panel CreateWelcomeCard()
    {
        Panel panel = new Panel { BackColor = Color.AliceBlue };
        Label label = new Label
        {
            Text = "Welcome to Setup Wizard\n\nClick Next to continue.",
            Font = new Font("Arial", 12),
            TextAlign = ContentAlignment.MiddleCenter,
            Dock = DockStyle.Fill
        };
        panel.Controls.Add(label);
        return panel;
    }

    private Panel CreateConfigureCard()
    {
        Panel panel = new Panel { BackColor = Color.Honeydew };
        Label label = new Label
        {
            Text = "Configure Settings\n\nEnter your preferences.",
            Font = new Font("Arial", 12),
            TextAlign = ContentAlignment.MiddleCenter,
            Dock = DockStyle.Fill
        };
        panel.Controls.Add(label);
        return panel;
    }

    private Panel CreateFinishCard()
    {
        Panel panel = new Panel { BackColor = Color.LavenderBlush };
        Label label = new Label
        {
            Text = "Setup Complete!\n\nClick Finish to exit.",
            Font = new Font("Arial", 12),
            TextAlign = ContentAlignment.MiddleCenter,
            Dock = DockStyle.Fill
        };
        panel.Controls.Add(label);
        return panel;
    }

    private void CreateNavigationButtons()
    {
        Panel buttonPanel = new Panel
        {
            Dock = DockStyle.Bottom,
            Height = 50,
            BackColor = Color.LightGray
        };

        btnPrevious = new Button { Text = "< Previous", Width = 100, Top = 10, Left = 10 };
        btnNext = new Button { Text = "Next >", Width = 100, Top = 10, Left = 120 };
        btnFinish = new Button { Text = "Finish", Width = 100, Top = 10, Left = 230, Enabled = false };

        btnPrevious.Click += (s, e) => { cardLayout1.Previous(); UpdateButtons(); };
        btnNext.Click += (s, e) => { cardLayout1.Next(); UpdateButtons(); };
        btnFinish.Click += (s, e) => { MessageBox.Show("Wizard completed!"); this.Close(); };

        buttonPanel.Controls.AddRange(new Control[] { btnPrevious, btnNext, btnFinish });
        this.Controls.Add(buttonPanel);

        UpdateButtons();
    }

    private void UpdateButtons()
    {
        // Enable/disable buttons based on current card
        int currentIndex = Array.IndexOf(cardLayout1.GetCardNames(), 
            cardLayout1.GetCardName(containerPanel.Controls[0]));
        
        btnPrevious.Enabled = cardLayout1.PreviousCardIndex >= 0;
        btnNext.Enabled = cardLayout1.NextCardIndex >= 0;
        btnFinish.Enabled = cardLayout1.NextCardIndex < 0; // Last card
    }

    [STAThread]
    static void Main()
    {
        Application.Run(new WizardForm());
    }
}
```

```vbnet
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Class WizardForm
    Inherits Form

    Private containerPanel As Panel
    Private cardLayout1 As CardLayout
    Private welcomePanel, configurePanel, finishPanel As Panel
    Private btnNext, btnPrevious, btnFinish As Button

    Public Sub New()
        ' Create container panel
        containerPanel = New Panel With {
            .Dock = DockStyle.Fill,
            .BackColor = Color.White
        }
        Me.Controls.Add(containerPanel)

        ' Create CardLayout
        cardLayout1 = New CardLayout()
        cardLayout1.ContainerControl = containerPanel
        cardLayout1.LayoutMode = CardLayoutMode.Fill

        ' Create cards
        welcomePanel = CreateWelcomeCard()
        configurePanel = CreateConfigureCard()
        finishPanel = CreateFinishCard()

        ' Add cards to container
        containerPanel.Controls.Add(welcomePanel)
        containerPanel.Controls.Add(configurePanel)
        containerPanel.Controls.Add(finishPanel)

        ' Set card names
        cardLayout1.SetCardName(welcomePanel, "Welcome")
        cardLayout1.SetCardName(configurePanel, "Configure")
        cardLayout1.SetCardName(finishPanel, "Finish")

        ' Show first card
        cardLayout1.First()

        ' Create navigation buttons
        CreateNavigationButtons()
    End Sub

    ' Additional methods similar to C# version...

    <STAThread>
    Shared Sub Main()
        Application.Run(New WizardForm())
    End Sub
End Class
```

## Navigation Methods

CardLayout provides several methods to navigate between cards:

### First() - Show First Card

Shows the first card in the stack:

```csharp
cardLayout1.First();
```

### Last() - Show Last Card

Shows the last card in the stack:

```csharp
cardLayout1.Last();
```

### Next() - Show Next Card

Shows the next card in sequence:

```csharp
cardLayout1.Next();
```

**Behavior:** If already on the last card, this method has no effect.

### Previous() - Show Previous Card

Shows the previous card in sequence:

```csharp
cardLayout1.Previous();
```

**Behavior:** If already on the first card, this method has no effect.

### ActivateCard(string name) - Jump to Specific Card

Jumps directly to a named card:

```csharp
// Jump to "Configure" card
cardLayout1.ActivateCard("Configure");
```

**Use case:** Skip wizard steps, jump to error pages, or implement non-linear navigation.

## LayoutMode Property

CardLayout supports two layout modes:

### CardLayoutMode.Default

**Behavior:**
- Child control is centered within container
- If container is larger than child's preferred size, child is centered
- If container is smaller, child shrinks to its minimum size
- Aspect ratio can be maintained (see MaintainAspectRatio)

```csharp
cardLayout1.LayoutMode = CardLayoutMode.Default;
```

**When to use:** Cards have fixed sizes and you want them centered.

### CardLayoutMode.Fill

**Behavior:**
- Child control resizes to fill entire container client area
- Ignores preferred/minimum sizes
- Card always matches container dimensions

```csharp
cardLayout1.LayoutMode = CardLayoutMode.Fill;
```

**When to use:** Cards should adapt to container size (most common for wizards/property pages).

## Card Index Properties

### NextCardIndex Property

Returns the index of the next card (or -1 if on last card):

```csharp
int nextIndex = cardLayout1.NextCardIndex;

if (nextIndex >= 0)
{
    // There is a next card
    btnNext.Enabled = true;
}
else
{
    // On last card
    btnNext.Enabled = false;
    btnFinish.Enabled = true;
}
```

### PreviousCardIndex Property

Returns the index of the previous card (or -1 if on first card):

```csharp
int previousIndex = cardLayout1.PreviousCardIndex;

if (previousIndex >= 0)
{
    // There is a previous card
    btnPrevious.Enabled = true;
}
else
{
    // On first card
    btnPrevious.Enabled = false;
}
```

**Use case:** Enable/disable Next/Previous buttons based on position in card stack.

## Aspect Ratio

### MaintainAspectRatio Property

When LayoutMode is Default and the child control needs to shrink, you can maintain its aspect ratio:

```csharp
// Maintain aspect ratio when card shrinks
cardLayout1.SetMaintainAspectRatio(panel1, true);
```

**Methods:**

| Method | Description |
|--------|-------------|
| `SetMaintainAspectRatio(Control, bool)` | Sets whether to maintain aspect ratio |
| `GetMaintainAspectRatio(Control)` | Gets the aspect ratio setting |

**When to use:** Cards contain images or content where proportions must be preserved.

## Image Settings

You can add background images to card panels using standard control properties:

```csharp
// Add background image to a card
panel1.BackgroundImage = Properties.Resources.WelcomeBackground;
panel1.BackgroundImageLayout = ImageLayout.Stretch;

// Or use a Label with Image property
Label imageLabel = new Label
{
    Image = Properties.Resources.Logo,
    ImageAlign = ContentAlignment.MiddleCenter,
    Dock = DockStyle.Fill
};
panel1.Controls.Add(imageLabel);
```

## Wizard Implementation

See the complete 3-card wizard example in the "Adding Cards via Code" section above.

**Key elements:**
1. Multiple panels (one per wizard step)
2. CardLayout with LayoutMode.Fill
3. Named cards ("Welcome", "Configure", "Finish")
4. Navigation buttons (Previous, Next, Finish)
5. Button enable/disable logic based on NextCardIndex/PreviousCardIndex

## Property Pages Implementation

### ListBox-Based Navigation

```csharp
// Create property pages with ListBox navigation
public class PropertyPagesForm : Form
{
    private Panel containerPanel;
    private ListBox pageList;
    private CardLayout cardLayout1;

    public PropertyPagesForm()
    {
        this.Size = new Size(600, 400);

        // Create ListBox for page selection
        pageList = new ListBox
        {
            Dock = DockStyle.Left,
            Width = 150
        };
        pageList.Items.AddRange(new string[] { "General", "Display", "Advanced" });
        pageList.SelectedIndexChanged += PageList_SelectedIndexChanged;
        this.Controls.Add(pageList);

        // Create container for cards
        containerPanel = new Panel
        {
            Dock = DockStyle.Fill,
            BackColor = Color.White
        };
        this.Controls.Add(containerPanel);

        // Create CardLayout
        cardLayout1 = new CardLayout();
        cardLayout1.ContainerControl = containerPanel;
        cardLayout1.LayoutMode = CardLayoutMode.Fill;

        // Create property pages
        Panel generalPage = CreateGeneralPage();
        Panel displayPage = CreateDisplayPage();
        Panel advancedPage = CreateAdvancedPage();

        containerPanel.Controls.AddRange(new Control[] { generalPage, displayPage, advancedPage });

        cardLayout1.SetCardName(generalPage, "General");
        cardLayout1.SetCardName(displayPage, "Display");
        cardLayout1.SetCardName(advancedPage, "Advanced");

        // Show first page
        pageList.SelectedIndex = 0;
    }

    private void PageList_SelectedIndexChanged(object sender, EventArgs e)
    {
        string selectedPage = pageList.SelectedItem.ToString();
        cardLayout1.ActivateCard(selectedPage);
    }

    private Panel CreateGeneralPage()
    {
        Panel panel = new Panel { BackColor = Color.WhiteSmoke };
        Label label = new Label { Text = "General Settings", Font = new Font("Arial", 14, FontStyle.Bold), Top = 20, Left = 20 };
        panel.Controls.Add(label);
        return panel;
    }

    private Panel CreateDisplayPage()
    {
        Panel panel = new Panel { BackColor = Color.WhiteSmoke };
        Label label = new Label { Text = "Display Settings", Font = new Font("Arial", 14, FontStyle.Bold), Top = 20, Left = 20 };
        panel.Controls.Add(label);
        return panel;
    }

    private Panel CreateAdvancedPage()
    {
        Panel panel = new Panel { BackColor = Color.WhiteSmoke };
        Label label = new Label { Text = "Advanced Settings", Font = new Font("Arial", 14, FontStyle.Bold), Top = 20, Left = 20 };
        panel.Controls.Add(label);
        return panel;
    }
}
```

## Dynamic Card Management

### Adding Cards at Runtime

```csharp
// Create new card dynamically
Panel newCard = new Panel { BackColor = Color.LightYellow };
Label label = new Label { Text = "New Card", Dock = DockStyle.Fill };
newCard.Controls.Add(label);

// Add to container
containerPanel.Controls.Add(newCard);

// Set card name
cardLayout1.SetCardName(newCard, "NewCard");

// Activate the new card
cardLayout1.ActivateCard("NewCard");
```

### Removing Cards

```csharp
// Get card control by name
Control cardToRemove = cardLayout1.GetComponentFromName("OldCard");

if (cardToRemove != null)
{
    // Remove from container
    containerPanel.Controls.Remove(cardToRemove);
    cardToRemove.Dispose();
}
```

### Reordering Cards

Cards are shown in the order they were added. To reorder:

```csharp
// Remove and re-add in new order
Control card1 = cardLayout1.GetComponentFromName("Card1");
Control card2 = cardLayout1.GetComponentFromName("Card2");

containerPanel.Controls.Remove(card1);
containerPanel.Controls.Remove(card2);

// Re-add in new order
containerPanel.Controls.Add(card2); // Card2 now first
containerPanel.Controls.Add(card1); // Card1 now second
```

## Common Patterns

### Multi-Step Wizard
**Scenario:** Setup wizard with Welcome → Configure → Finish steps
- Use CardLayout with Fill mode
- Add Next/Previous/Finish buttons
- Enable/disable buttons based on card index
- See complete example in "Wizard Implementation" section

### Property Pages Without Tabs
**Scenario:** Settings dialog with category selection
- Use ListBox or TreeView for navigation
- Use CardLayout to show selected category
- ActivateCard() when selection changes
- See example in "Property Pages Implementation" section

### Multi-Step Data Entry Form
**Scenario:** Complex form split into multiple steps
- One card per data entry section
- Validate before allowing Next
- Show summary on final card

### Tutorial/Walkthrough
**Scenario:** Application tutorial with multiple slides
- One card per tutorial step
- Previous/Next navigation
- Skip button to ActivateCard("End")

### Slideshow Presentation
**Scenario:** Image slideshow or presentation
- One card per slide
- Timer for automatic advancement
- Previous/Next for manual control

## Best Practices

1. **Use Fill Mode for Adaptive Cards**
   - Set `LayoutMode = CardLayoutMode.Fill` for wizards and property pages
   - Cards will resize to match container

2. **Name Cards Descriptively**
   - Use clear names: "Welcome", "Configure", "Finish"
   - Not: "Card1", "Card2", "Card3"

3. **Track Current Card**
   - Use `NextCardIndex` and `PreviousCardIndex` to enable/disable navigation buttons
   - Check for -1 to detect first/last card

4. **Validate Before Navigating**
   - Validate current card before allowing Next()
   - Show error messages if validation fails

```csharp
btnNext.Click += (s, e) =>
{
    if (ValidateCurrentCard())
    {
        cardLayout1.Next();
        UpdateButtons();
    }
    else
    {
        MessageBox.Show("Please complete all required fields.");
    }
};
```

5. **Use Panels for Cards**
   - Add Panel controls as cards, not individual controls
   - Add other controls to the panels

6. **Disable Navigation at Boundaries**
   - Disable Previous button on first card
   - Disable Next button on last card
   - Enable Finish button only on last card

7. **Consider ActivateCard for Non-Linear Navigation**
   - Allow skipping steps
   - Jump to error pages
   - Implement "Back to Start" button

8. **Test Card Transitions**
   - Test all navigation paths
   - Ensure no orphaned cards
   - Verify button states at each card

9. **Handle Form Closing**
   - Prompt if wizard incomplete
   - Save progress if needed

10. **Use Images for Visual Appeal**
    - Add background images to cards
    - Use icons for visual cues

## Troubleshooting

### Card Not Showing

**Problem:** Card is added but not visible

**Solutions:**
- Check `LayoutMode` property (try Fill mode)
- Verify card was added to correct container
- Ensure ContainerControl is set correctly
- Call `cardLayout1.LayoutContainer()` to force layout

### Navigation Not Working

**Problem:** Next/Previous methods don't change cards

**Solutions:**
- Verify card names are set correctly
- Check `NextCardIndex` and `PreviousCardIndex` values
- Ensure cards were added in correct order
- Check if already on first/last card

### Multiple Cards Visible

**Problem:** More than one card is visible at once

**Solutions:**
- Verify only one CardLayout is applied to container
- Check that all children are properly added as cards
- Don't mix CardLayout with other layout managers on same container

### Buttons Not Enabling/Disabling

**Problem:** Navigation buttons don't update state

**Solutions:**
- Check `NextCardIndex` and `PreviousCardIndex` after navigation
- Call `UpdateButtons()` method after every navigation
- Verify logic for first/last card detection

### Card Size Issues

**Problem:** Card doesn't fill container or is too small

**Solutions:**
- Set `LayoutMode = CardLayoutMode.Fill`
- Check panel `Dock` property (should not be set)
- Set `PreferredSize` for Default mode
- Verify container size is appropriate

### Card Content Not Updating

**Problem:** Card content doesn't refresh when shown

**Solutions:**
- Handle `LayoutComplete` event to refresh content
- Call `card.Refresh()` or `card.Invalidate()` when activated
- Ensure data binding is updated when card becomes visible

---

## See Also

- [CardLayout API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.CardLayout.html)
- [WizardControl](https://help.syncfusion.com/windowsforms/wizard/overview) (uses CardLayout internally)
- [LayoutManagers Overview](https://help.syncfusion.com/windowsforms/layoutmanagers/overview)
