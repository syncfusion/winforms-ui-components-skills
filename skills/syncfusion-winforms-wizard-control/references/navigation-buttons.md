# Navigation Buttons Configuration

This guide covers configuring and customizing the navigation buttons in WizardControl, including visibility, state, custom buttons, and appearance.

## When to Read This

Read this reference when:
- Controlling button visibility on specific pages
- Enabling or disabling buttons based on conditions
- Adding Finish button to the final page
- Creating custom buttons in the wizard
- Reordering button positions
- Customizing button appearance (colors, styles, images)
- Accessing button properties programmatically

## Default Navigation Buttons

The WizardControl provides five built-in navigation buttons:

| Button | Purpose | Default Visibility |
|--------|---------|-------------------|
| **Back** | Navigate to previous page | Visible (except first page) |
| **Next** | Navigate to next page | Visible (except last page) |
| **Cancel** | Cancel and close wizard | Visible on all pages |
| **Finish** | Complete wizard | Hidden by default |
| **Help** | Show help information | Visible on all pages |

## Button Visibility

Control which buttons appear on each page using per-page visibility properties.

### Per-Page Visibility Properties

**C#:**
```csharp
// First page: hide Back button
firstPage.BackVisible = false;
firstPage.NextVisible = true;
firstPage.CancelVisible = true;
firstPage.HelpVisible = true;
firstPage.FinishVisible = false;

// Last page: hide Next, show Finish
lastPage.BackVisible = true;
lastPage.NextVisible = false;
lastPage.CancelVisible = false;  // Often hidden on last page
lastPage.HelpVisible = true;
lastPage.FinishVisible = true;

// Middle page: standard visibility
middlePage.BackVisible = true;
middlePage.NextVisible = true;
middlePage.CancelVisible = true;
middlePage.HelpVisible = true;
middlePage.FinishVisible = false;
```

**VB.NET:**
```vbnet
' First page: hide Back button
firstPage.BackVisible = False
firstPage.NextVisible = True
firstPage.CancelVisible = True
firstPage.HelpVisible = True
firstPage.FinishVisible = False

' Last page: hide Next, show Finish
lastPage.BackVisible = True
lastPage.NextVisible = False
lastPage.CancelVisible = False
lastPage.HelpVisible = True
lastPage.FinishVisible = True

' Middle page: standard visibility
middlePage.BackVisible = True
middlePage.NextVisible = True
middlePage.CancelVisible = True
middlePage.HelpVisible = True
middlePage.FinishVisible = False
```

### Dynamic Visibility

Change button visibility at runtime based on conditions:

**C#:**
```csharp
private void UpdateButtonVisibility()
{
    WizardControlPage currentPage = wizardControl1.SelectedWizardPage;
    
    // Hide Next button if current page is the last
    bool isLastPage = (wizardControl1.WizardPages[
        wizardControl1.WizardPages.Length - 1] == currentPage);
    
    currentPage.NextVisible = !isLastPage;
    currentPage.FinishVisible = isLastPage;
}

// Call when page changes
wizardControl1.BeforePageSelect += (sender, e) =>
{
    UpdateButtonVisibility();
};
```

**VB.NET:**
```vbnet
Private Sub UpdateButtonVisibility()
    Dim currentPage As WizardControlPage = wizardControl1.SelectedWizardPage
    
    ' Hide Next button if current page is the last
    Dim isLastPage As Boolean = (wizardControl1.WizardPages(
        wizardControl1.WizardPages.Length - 1) Is currentPage)
    
    currentPage.NextVisible = Not isLastPage
    currentPage.FinishVisible = isLastPage
End Sub

' Call when page changes
AddHandler wizardControl1.BeforePageSelect, Sub(sender, e)
    UpdateButtonVisibility()
End Sub
```

## Button Enabled State

Control whether buttons can be clicked using enabled properties.

### Per-Page Enabled Properties

**C#:**
```csharp
// Disable Next until validation passes
licensePage.NextEnabled = false;

// Re-enable when checkbox is checked
chkAcceptTerms.CheckedChanged += (sender, e) =>
{
    licensePage.NextEnabled = chkAcceptTerms.Checked;
};

// Disable all navigation during processing
processingPage.BackEnabled = false;
processingPage.NextEnabled = false;
processingPage.CancelEnabled = false;
```

**VB.NET:**
```vbnet
' Disable Next until validation passes
licensePage.NextEnabled = False

' Re-enable when checkbox is checked
AddHandler chkAcceptTerms.CheckedChanged, Sub(sender, e)
    licensePage.NextEnabled = chkAcceptTerms.Checked
End Sub

' Disable all navigation during processing
processingPage.BackEnabled = False
processingPage.NextEnabled = False
processingPage.CancelEnabled = False
```

### Complete Example: Conditional Next Button

**C#:**
```csharp
public partial class LicenseWizard : Form
{
    private WizardControlPage licensePage;
    private RichTextBox txtLicense;
    private CheckBox chkAgree;
    
    private void SetupLicensePage()
    {
        licensePage = new WizardControlPage
        {
            Title = "License Agreement",
            Description = "Please read and accept the license terms",
            NextEnabled = false  // Initially disabled
        };
        
        // License text
        txtLicense = new RichTextBox
        {
            Text = "END USER LICENSE AGREEMENT\n\n" +
                   "PLEASE READ CAREFULLY...",
            Location = new Point(20, 20),
            Size = new Size(500, 250),
            ReadOnly = true
        };
        
        // Accept checkbox
        chkAgree = new CheckBox
        {
            Text = "I have read and accept the license agreement",
            Location = new Point(20, 280),
            AutoSize = true
        };
        
        // Enable Next when checked
        chkAgree.CheckedChanged += (sender, e) =>
        {
            licensePage.NextEnabled = chkAgree.Checked;
        };
        
        licensePage.Controls.Add(txtLicense);
        licensePage.Controls.Add(chkAgree);
    }
}
```

## Adding Finish Button

The Finish button typically appears on the last page to complete the wizard.

### Basic Finish Button Configuration

**C#:**
```csharp
WizardControlPage finishPage = new WizardControlPage
{
    Title = "Setup Complete",
    Description = "Click Finish to exit the wizard",
    NextVisible = false,      // Hide Next on last page
    CancelVisible = false,    // Optional: hide Cancel
    FinishVisible = true      // Show Finish button
};
```

**VB.NET:**
```vbnet
Dim finishPage As New WizardControlPage With {
    .Title = "Setup Complete",
    .Description = "Click Finish to exit the wizard",
    .NextVisible = False,
    .CancelVisible = False,
    .FinishVisible = True
}
```

### CancelOverFinish Property

The `CancelOverFinish` property determines button positioning:

**C#:**
```csharp
// false: Cancel and Finish both visible
finishPage.CancelOverFinish = false;
finishPage.CancelVisible = true;
finishPage.FinishVisible = true;

// true: Cancel positioned over Finish (hides Finish if Cancel visible)
finishPage.CancelOverFinish = true;
finishPage.CancelVisible = false;  // Set to false to show Finish
finishPage.FinishVisible = true;
```

**VB.NET:**
```vbnet
' false: Cancel and Finish both visible
finishPage.CancelOverFinish = False
finishPage.CancelVisible = True
finishPage.FinishVisible = True

' true: Cancel positioned over Finish
finishPage.CancelOverFinish = True
finishPage.CancelVisible = False
finishPage.FinishVisible = True
```

**Important:** If `CancelOverFinish` is `true` and `CancelVisible` is `true`, the Cancel button will override the `FinishVisible` property, hiding the Finish button.

## Adding Custom Buttons

Add your own buttons to the wizard's button area.

### Creating a Custom Button

**C#:**
```csharp
private void AddCustomButton()
{
    // Create custom button
    Button btnExport = new Button
    {
        Text = "Export...",
        Size = new Size(80, 25)
    };
    
    btnExport.Click += (sender, e) =>
    {
        // Handle export functionality
        MessageBox.Show("Exporting data...");
    };
    
    // Add button to wizard control
    wizardControl1.Controls.Add(btnExport);
    
    // Position using GridBagLayout
    // X=0 is leftmost position, higher values move right
    wizardControl1.GridBagLayout.GetConstraintsRef(btnExport).GridPosX = 1;
    wizardControl1.GridBagLayout.GetConstraintsRef(btnExport).GridPosY = 5;
}
```

**VB.NET:**
```vbnet
Private Sub AddCustomButton()
    ' Create custom button
    Dim btnExport As New Button With {
        .Text = "Export...",
        .Size = New Size(80, 25)
    }
    
    AddHandler btnExport.Click, Sub(sender, e)
        ' Handle export functionality
        MessageBox.Show("Exporting data...")
    End Sub
    
    ' Add button to wizard control
    wizardControl1.Controls.Add(btnExport)
    
    ' Position using GridBagLayout
    wizardControl1.GridBagLayout.GetConstraintsRef(btnExport).GridPosX = 1
    wizardControl1.GridBagLayout.GetConstraintsRef(btnExport).GridPosY = 5
End Sub
```

### Complete Custom Button Example

**C#:**
```csharp
public partial class CustomButtonWizard : Form
{
    private Button btnPrint;
    private Button btnSave;
    
    private void SetupCustomButtons()
    {
        // Print button (left side)
        btnPrint = new Button
        {
            Text = "Print",
            Size = new Size(75, 25)
        };
        btnPrint.Click += BtnPrint_Click;
        
        wizardControl1.Controls.Add(btnPrint);
        wizardControl1.GridBagLayout.GetConstraintsRef(btnPrint).GridPosX = 0;
        wizardControl1.GridBagLayout.GetConstraintsRef(btnPrint).GridPosY = 5;
        
        // Save button (left side, next to Print)
        btnSave = new Button
        {
            Text = "Save",
            Size = new Size(75, 25)
        };
        btnSave.Click += BtnSave_Click;
        
        wizardControl1.Controls.Add(btnSave);
        wizardControl1.GridBagLayout.GetConstraintsRef(btnSave).GridPosX = 1;
        wizardControl1.GridBagLayout.GetConstraintsRef(btnSave).GridPosY = 5;
    }
    
    private void BtnPrint_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Printing current page...");
    }
    
    private void BtnSave_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Saving progress...");
    }
}
```

## Reordering Button Sequence

Change the default button order using GridBagLayout positioning.

### Default Button Positions

Default GridPosX values (left to right):
- Position 0-1: Custom buttons area (left side)
- Position 2: Help button
- Position 3: Spacer/filler
- Position 4: Back button
- Position 5: Next button
- Position 6: Cancel/Finish button

### Custom Button Order

**C#:**
```csharp
private void ReorderButtons()
{
    // Move Cancel to left side (before Back)
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.CancelButton).GridPosX = 3;
    
    // Move Help to right side (after Next)
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.HelpButton).GridPosX = 6;
    
    // Reorder: Back → Next → Finish (left to right)
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.BackButton).GridPosX = 4;
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.NextButton).GridPosX = 5;
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.FinishButton).GridPosX = 6;
}
```

**VB.NET:**
```vbnet
Private Sub ReorderButtons()
    ' Move Cancel to left side
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.CancelButton).GridPosX = 3
    
    ' Move Help to right side
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.HelpButton).GridPosX = 6
    
    ' Reorder buttons
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.BackButton).GridPosX = 4
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.NextButton).GridPosX = 5
    wizardControl1.GridBagLayout.GetConstraintsRef(
        wizardControl1.FinishButton).GridPosX = 6
End Sub
```

**Note:** WizardControl automatically resets some button positions after page changes. Manual positioning is best done for custom buttons that don't change with pages.

## Button Appearance Customization

Customize the visual appearance of navigation buttons.

### Accessing Button Properties

**C#:**
```csharp
// Access built-in buttons
Button backBtn = wizardControl1.BackButton;
Button nextBtn = wizardControl1.NextButton;
Button cancelBtn = wizardControl1.CancelButton;
Button finishBtn = wizardControl1.FinishButton;
Button helpBtn = wizardControl1.HelpButton;
```

**VB.NET:**
```vbnet
' Access built-in buttons
Dim backBtn As Button = wizardControl1.BackButton
Dim nextBtn As Button = wizardControl1.NextButton
Dim cancelBtn As Button = wizardControl1.CancelButton
Dim finishBtn As Button = wizardControl1.FinishButton
Dim helpBtn As Button = wizardControl1.HelpButton
```

### FlatStyle Customization

**C#:**
```csharp
private void StyleButtons()
{
    // Set flat style
    wizardControl1.NextButton.FlatStyle = FlatStyle.Flat;
    wizardControl1.NextButton.ForeColor = Color.White;
    wizardControl1.NextButton.BackColor = Color.DodgerBlue;
    
    // Flat appearance settings
    wizardControl1.NextButton.FlatAppearance.BorderSize = 1;
    wizardControl1.NextButton.FlatAppearance.BorderColor = Color.DarkBlue;
    wizardControl1.NextButton.FlatAppearance.MouseOverBackColor = Color.RoyalBlue;
    wizardControl1.NextButton.FlatAppearance.MouseDownBackColor = Color.MediumBlue;
}
```

**VB.NET:**
```vbnet
Private Sub StyleButtons()
    ' Set flat style
    wizardControl1.NextButton.FlatStyle = FlatStyle.Flat
    wizardControl1.NextButton.ForeColor = Color.White
    wizardControl1.NextButton.BackColor = Color.DodgerBlue
    
    ' Flat appearance settings
    wizardControl1.NextButton.FlatAppearance.BorderSize = 1
    wizardControl1.NextButton.FlatAppearance.BorderColor = Color.DarkBlue
    wizardControl1.NextButton.FlatAppearance.MouseOverBackColor = Color.RoyalBlue
    wizardControl1.NextButton.FlatAppearance.MouseDownBackColor = Color.MediumBlue
End Sub
```

### Adding Button Images

**C#:**
```csharp
private void AddButtonImages()
{
    // Load images
    Image backIcon = Image.FromFile("back.png");
    Image nextIcon = Image.FromFile("next.png");
    
    // Set images
    wizardControl1.BackButton.Image = backIcon;
    wizardControl1.BackButton.ImageAlign = ContentAlignment.MiddleLeft;
    wizardControl1.BackButton.TextImageRelation = TextImageRelation.ImageBeforeText;
    
    wizardControl1.NextButton.Image = nextIcon;
    wizardControl1.NextButton.ImageAlign = ContentAlignment.MiddleRight;
    wizardControl1.NextButton.TextImageRelation = TextImageRelation.TextBeforeImage;
}
```

**VB.NET:**
```vbnet
Private Sub AddButtonImages()
    ' Load images
    Dim backIcon As Image = Image.FromFile("back.png")
    Dim nextIcon As Image = Image.FromFile("next.png")
    
    ' Set images
    wizardControl1.BackButton.Image = backIcon
    wizardControl1.BackButton.ImageAlign = ContentAlignment.MiddleLeft
    wizardControl1.BackButton.TextImageRelation = TextImageRelation.ImageBeforeText
    
    wizardControl1.NextButton.Image = nextIcon
    wizardControl1.NextButton.ImageAlign = ContentAlignment.MiddleRight
    wizardControl1.NextButton.TextImageRelation = TextImageRelation.TextBeforeImage
End Sub
```

### Using ImageList

**C#:**
```csharp
private void SetupImageList()
{
    // Create image list
    ImageList buttonImages = new ImageList
    {
        ImageSize = new Size(16, 16)
    };
    
    buttonImages.Images.Add("back", Image.FromFile("back.png"));
    buttonImages.Images.Add("next", Image.FromFile("next.png"));
    buttonImages.Images.Add("finish", Image.FromFile("finish.png"));
    
    // Assign to buttons
    wizardControl1.BackButton.ImageList = buttonImages;
    wizardControl1.BackButton.ImageIndex = 0;  // "back"
    
    wizardControl1.NextButton.ImageList = buttonImages;
    wizardControl1.NextButton.ImageIndex = 1;  // "next"
    
    wizardControl1.FinishButton.ImageList = buttonImages;
    wizardControl1.FinishButton.ImageIndex = 2;  // "finish"
}
```

### Complete Styling Example

**C#:**
```csharp
public partial class StyledWizard : Form
{
    private void ApplyModernButtonStyle()
    {
        // Primary action button (Next/Finish) - Blue
        StylePrimaryButton(wizardControl1.NextButton);
        StylePrimaryButton(wizardControl1.FinishButton);
        
        // Secondary action button (Back) - Gray
        StyleSecondaryButton(wizardControl1.BackButton);
        
        // Cancel button - Red
        StyleCancelButton(wizardControl1.CancelButton);
        
        // Help button - Default style
        StyleHelpButton(wizardControl1.HelpButton);
    }
    
    private void StylePrimaryButton(Button btn)
    {
        btn.FlatStyle = FlatStyle.Flat;
        btn.BackColor = Color.FromArgb(0, 120, 215);  // Blue
        btn.ForeColor = Color.White;
        btn.Font = new Font("Segoe UI", 9, FontStyle.Regular);
        btn.FlatAppearance.BorderSize = 0;
        btn.FlatAppearance.MouseOverBackColor = Color.FromArgb(0, 99, 177);
        btn.FlatAppearance.MouseDownBackColor = Color.FromArgb(0, 78, 138);
        btn.Cursor = Cursors.Hand;
    }
    
    private void StyleSecondaryButton(Button btn)
    {
        btn.FlatStyle = FlatStyle.Flat;
        btn.BackColor = Color.FromArgb(240, 240, 240);  // Light gray
        btn.ForeColor = Color.Black;
        btn.Font = new Font("Segoe UI", 9, FontStyle.Regular);
        btn.FlatAppearance.BorderSize = 1;
        btn.FlatAppearance.BorderColor = Color.FromArgb(200, 200, 200);
        btn.FlatAppearance.MouseOverBackColor = Color.FromArgb(229, 229, 229);
        btn.FlatAppearance.MouseDownBackColor = Color.FromArgb(204, 204, 204);
        btn.Cursor = Cursors.Hand;
    }
    
    private void StyleCancelButton(Button btn)
    {
        btn.FlatStyle = FlatStyle.Flat;
        btn.BackColor = Color.Transparent;
        btn.ForeColor = Color.FromArgb(200, 50, 50);  // Dark red
        btn.Font = new Font("Segoe UI", 9, FontStyle.Regular);
        btn.FlatAppearance.BorderSize = 1;
        btn.FlatAppearance.BorderColor = Color.FromArgb(200, 50, 50);
        btn.FlatAppearance.MouseOverBackColor = Color.FromArgb(255, 230, 230);
        btn.FlatAppearance.MouseDownBackColor = Color.FromArgb(255, 200, 200);
        btn.Cursor = Cursors.Hand;
    }
    
    private void StyleHelpButton(Button btn)
    {
        btn.FlatStyle = FlatStyle.Flat;
        btn.BackColor = Color.Transparent;
        btn.ForeColor = Color.FromArgb(0, 120, 215);
        btn.Font = new Font("Segoe UI", 9, FontStyle.Underline);
        btn.FlatAppearance.BorderSize = 0;
        btn.FlatAppearance.MouseOverBackColor = Color.FromArgb(240, 248, 255);
        btn.Cursor = Cursors.Hand;
    }
}
```

## Button Property Reference

### Common Button Properties

| Property | Type | Description |
|----------|------|-------------|
| `FlatStyle` | `FlatStyle` | Flat, Popup, Standard, System |
| `FlatAppearance` | `FlatButtonAppearance` | Border, colors for mouse states |
| `Font` | `Font` | Button text font |
| `ForeColor` | `Color` | Text color |
| `BackColor` | `Color` | Background color |
| `Image` | `Image` | Button icon/image |
| `ImageAlign` | `ContentAlignment` | Image alignment |
| `ImageIndex` | `int` | ImageList index |
| `ImageList` | `ImageList` | Image collection |
| `Text` | `string` | Button text |
| `TextAlign` | `ContentAlignment` | Text alignment |
| `TextImageRelation` | `TextImageRelation` | Image position relative to text |
| `Size` | `Size` | Button dimensions |
| `Enabled` | `bool` | Button enabled state |
| `Visible` | `bool` | Button visibility |
| `Cursor` | `Cursor` | Mouse cursor over button |

## Next Steps

After configuring navigation buttons:

1. **Configure Banner** → Read: [banner-configuration.md](banner-configuration.md)
   - Customize banner panel appearance
   - Add banner images
   - Style title and description

2. **Implement Events** → Read: [page-validation-events.md](page-validation-events.md)
   - Handle button click events
   - Implement page validation
   - Control navigation flow
