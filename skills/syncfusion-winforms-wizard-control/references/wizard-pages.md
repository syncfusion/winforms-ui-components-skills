# Wizard Pages Configuration

This guide covers creating, managing, and configuring WizardControlPage instances in the WizardControl.

## When to Read This

Read this reference when:
- Creating and adding wizard pages to a wizard
- Managing the WizardPages collection
- Setting page titles and descriptions
- Configuring full-page mode without banner
- Reordering pages using different methods
- Implementing custom page sequences with NextPage/PreviousPage
- Accessing and manipulating pages at runtime
- Understanding page layout and selection

## WizardControlPage Overview

A **WizardControlPage** is a container that holds controls for one step of the wizard. Each page can have:
- Title and description displayed in the banner
- Custom controls for user interaction
- Individual button visibility settings
- Custom navigation flow using NextPage/PreviousPage properties

## Creating Wizard Pages

### Designer Method

**Using Collection Editor:**
1. Select WizardControl in designer
2. Find `WizardPages` property in Property Grid
3. Click the ellipsis button (...)
4. Click "Add" to create new pages
5. Configure each page's properties
6. Click OK

**Using Smart Tag:**
1. Click the smart tag icon on WizardControl
2. Select "Add Page"
3. A new page is added to the collection

**Using Context Menu:**
1. Right-click the WizardControl in designer
2. Select "Add Page" from context menu

### Programmatic Method

**C#:**
```csharp
using Syncfusion.Windows.Forms.Tools;

// Create page instances
WizardControlPage page1 = new WizardControlPage();
WizardControlPage page2 = new WizardControlPage();
WizardControlPage page3 = new WizardControlPage();

// Configure pages
page1.Title = "Step 1";
page1.Description = "Enter basic information";

page2.Title = "Step 2";
page2.Description = "Configure settings";

page3.Title = "Step 3";
page3.Description = "Review and finish";

// Add to wizard
wizardControl1.WizardPages = new WizardControlPage[]
{
    page1,
    page2,
    page3
};
```

**VB.NET:**
```vbnet
Imports Syncfusion.Windows.Forms.Tools

' Create page instances
Dim page1 As New WizardControlPage()
Dim page2 As New WizardControlPage()
Dim page3 As New WizardControlPage()

' Configure pages
page1.Title = "Step 1"
page1.Description = "Enter basic information"

page2.Title = "Step 2"
page2.Description = "Configure settings"

page3.Title = "Step 3"
page3.Description = "Review and finish"

' Add to wizard
wizardControl1.WizardPages = New WizardControlPage() {
    page1,
    page2,
    page3
}
```

## Setting Page Title and Description

Each page displays a title and description in the wizard's banner area.

### Basic Title and Description

**C#:**
```csharp
WizardControlPage userInfoPage = new WizardControlPage
{
    Title = "User Information",
    Description = "Please enter your personal details"
};
```

**VB.NET:**
```vbnet
Dim userInfoPage As New WizardControlPage With {
    .Title = "User Information",
    .Description = "Please enter your personal details"
}
```

### Dynamic Title and Description

Change title and description at runtime based on context:

**C#:**
```csharp
private void UpdatePageTitle(WizardControlPage page, string userName)
{
    page.Title = $"Welcome, {userName}";
    page.Description = "Let's complete your profile setup";
}

// Usage
private void txtName_TextChanged(object sender, EventArgs e)
{
    if (!string.IsNullOrEmpty(txtName.Text))
    {
        UpdatePageTitle(profilePage, txtName.Text);
    }
}
```

**VB.NET:**
```vbnet
Private Sub UpdatePageTitle(page As WizardControlPage, userName As String)
    page.Title = $"Welcome, {userName}"
    page.Description = "Let's complete your profile setup"
End Sub

' Usage
Private Sub txtName_TextChanged(sender As Object, e As EventArgs)
    If Not String.IsNullOrEmpty(txtName.Text) Then
        UpdatePageTitle(profilePage, txtName.Text)
    End If
End Sub
```

## Full Page Mode

The `FullPage` property hides the banner panel, allowing the page to occupy the entire wizard area.

**C#:**
```csharp
// Create a full-page wizard page (no banner)
WizardControlPage termsPage = new WizardControlPage
{
    Title = "Terms and Conditions",  // Not displayed in full page mode
    Description = "Read carefully",   // Not displayed in full page mode
    FullPage = true
};

// Add a rich text box that fills the entire page
RichTextBox rtbTerms = new RichTextBox
{
    Dock = DockStyle.Fill,
    Text = "TERMS AND CONDITIONS\n\nLong legal text...",
    ReadOnly = true
};

termsPage.Controls.Add(rtbTerms);
```

**VB.NET:**
```vbnet
' Create a full-page wizard page (no banner)
Dim termsPage As New WizardControlPage With {
    .Title = "Terms and Conditions",
    .Description = "Read carefully",
    .FullPage = True
}

' Add a rich text box that fills the entire page
Dim rtbTerms As New RichTextBox With {
    .Dock = DockStyle.Fill,
    .Text = "TERMS AND CONDITIONS" & vbCrLf & vbCrLf & "Long legal text...",
    .ReadOnly = True
}

termsPage.Controls.Add(rtbTerms)
```

**When to use FullPage:**
- Displaying long documents (license agreements, terms)
- Showing complex diagrams or images
- Presenting data grids that need maximum space
- Creating splash or welcome screens

## Reordering Wizard Pages

WizardControl provides multiple methods to reorder pages, affecting the navigation sequence.

### Method 1: Collection Editor

**Steps:**
1. Select WizardControl
2. Click `WizardPages` property in Property Grid
3. In Collection Editor, select a page
4. Use Up/Down arrow buttons to reorder
5. Click OK

This is the easiest method at design time.

### Method 2: Context Menu (Designer)

**Steps:**
1. Right-click a page in the designer
2. Select "Bring To Front" to move to beginning
3. Or select "Send To Back" to move to end

**Limitation:** Only moves to first or last position.

### Method 3: NextPage and PreviousPage Properties

Set explicit navigation using properties on each page:

**C#:**
```csharp
// Linear sequence: page1 → page2 → page3
page1.NextPage = page2;
page2.PreviousPage = page1;
page2.NextPage = page3;
page3.PreviousPage = page2;

// Non-linear: page1 → page3 (skip page2)
page1.NextPage = page3;
page3.PreviousPage = page1;
```

**VB.NET:**
```vbnet
' Linear sequence: page1 → page2 → page3
page1.NextPage = page2
page2.PreviousPage = page1
page2.NextPage = page3
page3.PreviousPage = page2

' Non-linear: page1 → page3 (skip page2)
page1.NextPage = page3
page3.PreviousPage = page1
```

**Advantages:**
- Create non-linear flows
- Skip pages based on conditions
- Implement conditional navigation

## Custom Page Sequences

Create dynamic navigation based on user input or application state.

### Conditional Page Flow

**C#:**
```csharp
public partial class ConfigWizard : Form
{
    private WizardControlPage selectionPage;
    private WizardControlPage basicConfigPage;
    private WizardControlPage advancedConfigPage;
    private WizardControlPage summaryPage;
    private RadioButton rbBasic;
    private RadioButton rbAdvanced;
    
    private void SetupConditionalFlow()
    {
        // Selection page with radio buttons
        selectionPage = new WizardControlPage
        {
            Title = "Configuration Type",
            Description = "Choose your configuration level"
        };
        
        rbBasic = new RadioButton
        {
            Text = "Basic Configuration (Recommended)",
            Location = new Point(20, 20),
            Checked = true
        };
        
        rbAdvanced = new RadioButton
        {
            Text = "Advanced Configuration",
            Location = new Point(20, 50)
        };
        
        selectionPage.Controls.Add(rbBasic);
        selectionPage.Controls.Add(rbAdvanced);
        
        // Hook into Next button click to set dynamic flow
        selectionPage.NextClick += (sender, e) =>
        {
            if (rbBasic.Checked)
            {
                selectionPage.NextPage = basicConfigPage;
                basicConfigPage.NextPage = summaryPage;
            }
            else
            {
                selectionPage.NextPage = advancedConfigPage;
                advancedConfigPage.NextPage = summaryPage;
            }
        };
        
        // Basic config page
        basicConfigPage = new WizardControlPage
        {
            Title = "Basic Settings",
            Description = "Configure common options"
        };
        
        // Advanced config page
        advancedConfigPage = new WizardControlPage
        {
            Title = "Advanced Settings",
            Description = "Configure all options"
        };
        
        // Summary page
        summaryPage = new WizardControlPage
        {
            Title = "Summary",
            Description = "Review your settings"
        };
        
        // Add all pages (even if not all are visited)
        wizardControl1.WizardPages = new WizardControlPage[]
        {
            selectionPage,
            basicConfigPage,
            advancedConfigPage,
            summaryPage
        };
    }
}
```

### Looping Back to Previous Pages

Allow users to return to earlier steps for corrections:

**C#:**
```csharp
private void CreateLoopingWizard()
{
    WizardControlPage reviewPage = new WizardControlPage
    {
        Title = "Review Your Information",
        Description = "Check your entries before submitting"
    };
    
    // Add "Edit" buttons that navigate back
    Button btnEditPersonal = new Button
    {
        Text = "Edit Personal Info",
        Location = new Point(20, 60)
    };
    btnEditPersonal.Click += (sender, e) =>
    {
        // Navigate back to personalInfoPage
        wizardControl1.SelectedWizardPage = personalInfoPage;
    };
    
    Button btnEditContact = new Button
    {
        Text = "Edit Contact Details",
        Location = new Point(20, 100)
    };
    btnEditContact.Click += (sender, e) =>
    {
        // Navigate back to contactPage
        wizardControl1.SelectedWizardPage = contactPage;
    };
    
    reviewPage.Controls.Add(btnEditPersonal);
    reviewPage.Controls.Add(btnEditContact);
}
```

## Accessing Wizard Pages

### Getting Current Page

**C#:**
```csharp
// Get currently displayed page
WizardControlPage currentPage = wizardControl1.SelectedWizardPage;

// Check which page is active
if (currentPage == finalPage)
{
    MessageBox.Show("You're on the last page!");
}
```

**VB.NET:**
```vbnet
' Get currently displayed page
Dim currentPage As WizardControlPage = wizardControl1.SelectedWizardPage

' Check which page is active
If currentPage Is finalPage Then
    MessageBox.Show("You're on the last page!")
End If
```

### Navigating to Specific Page

**C#:**
```csharp
// Navigate to a specific page directly
wizardControl1.SelectedWizardPage = settingsPage;

// Navigate by index
wizardControl1.SelectedWizardPage = wizardControl1.WizardPages[2];
```

**VB.NET:**
```vbnet
' Navigate to a specific page directly
wizardControl1.SelectedWizardPage = settingsPage

' Navigate by index
wizardControl1.SelectedWizardPage = wizardControl1.WizardPages(2)
```

### Iterating Through Pages

**C#:**
```csharp
// Loop through all pages
foreach (WizardControlPage page in wizardControl1.WizardPages)
{
    Console.WriteLine($"Page: {page.Title}");
}

// Get page count
int totalPages = wizardControl1.WizardPages.Length;

// Find page index
int currentIndex = Array.IndexOf(wizardControl1.WizardPages, 
                                  wizardControl1.SelectedWizardPage);
```

**VB.NET:**
```vbnet
' Loop through all pages
For Each page As WizardControlPage In wizardControl1.WizardPages
    Console.WriteLine($"Page: {page.Title}")
Next

' Get page count
Dim totalPages As Integer = wizardControl1.WizardPages.Length

' Find page index
Dim currentIndex As Integer = Array.IndexOf(wizardControl1.WizardPages, 
                                             wizardControl1.SelectedWizardPage)
```

## LayoutName Property

The `LayoutName` property provides a unique identifier for pages, useful for tracking and navigation:

**C#:**
```csharp
// Set layout names
welcomePage.LayoutName = "WelcomePage";
settingsPage.LayoutName = "SettingsPage";
summaryPage.LayoutName = "SummaryPage";

// Find page by layout name
WizardControlPage FindPageByName(string layoutName)
{
    foreach (WizardControlPage page in wizardControl1.WizardPages)
    {
        if (page.LayoutName == layoutName)
            return page;
    }
    return null;
}

// Usage
WizardControlPage settingsPage = FindPageByName("SettingsPage");
if (settingsPage != null)
{
    wizardControl1.SelectedWizardPage = settingsPage;
}
```

## Complete Example: Multi-Step Registration Wizard

**C#:**
```csharp
public partial class RegistrationWizard : Form
{
    private WizardControl wizardControl1;
    private WizardControlPage welcomePage;
    private WizardControlPage personalInfoPage;
    private WizardControlPage contactPage;
    private WizardControlPage preferencesPage;
    private WizardControlPage reviewPage;
    private WizardControlPage completePage;
    
    public RegistrationWizard()
    {
        InitializeComponent();
        SetupRegistrationWizard();
    }
    
    private void SetupRegistrationWizard()
    {
        wizardControl1 = new WizardControl
        {
            Dock = DockStyle.Fill
        };
        
        // Page 1: Welcome
        welcomePage = new WizardControlPage
        {
            Title = "Welcome to Registration",
            Description = "Let's get you started",
            BackVisible = false,
            LayoutName = "Welcome"
        };
        Label lblWelcome = new Label
        {
            Text = "Thank you for choosing our service!\n\n" +
                   "This wizard will guide you through the registration process.",
            Location = new Point(20, 20),
            Size = new Size(500, 100),
            Font = new Font("Arial", 10)
        };
        welcomePage.Controls.Add(lblWelcome);
        
        // Page 2: Personal Information
        personalInfoPage = new WizardControlPage
        {
            Title = "Personal Information",
            Description = "Tell us about yourself",
            LayoutName = "PersonalInfo"
        };
        
        // Add form controls
        Label lblName = new Label { Text = "Full Name:", Location = new Point(20, 20), AutoSize = true };
        TextBox txtName = new TextBox { Location = new Point(20, 45), Size = new Size(300, 20) };
        
        Label lblEmail = new Label { Text = "Email:", Location = new Point(20, 80), AutoSize = true };
        TextBox txtEmail = new TextBox { Location = new Point(20, 105), Size = new Size(300, 20) };
        
        Label lblPhone = new Label { Text = "Phone:", Location = new Point(20, 140), AutoSize = true };
        TextBox txtPhone = new TextBox { Location = new Point(20, 165), Size = new Size(300, 20) };
        
        personalInfoPage.Controls.AddRange(new Control[] 
        { 
            lblName, txtName, 
            lblEmail, txtEmail, 
            lblPhone, txtPhone 
        });
        
        // Page 3: Contact Preferences
        contactPage = new WizardControlPage
        {
            Title = "Contact Preferences",
            Description = "How would you like us to reach you?",
            LayoutName = "Contact"
        };
        
        CheckBox chkEmailNotify = new CheckBox 
        { 
            Text = "Email notifications", 
            Location = new Point(20, 20),
            Checked = true
        };
        CheckBox chkSMSNotify = new CheckBox 
        { 
            Text = "SMS notifications", 
            Location = new Point(20, 50) 
        };
        CheckBox chkNewsLetter = new CheckBox 
        { 
            Text = "Subscribe to newsletter", 
            Location = new Point(20, 80) 
        };
        
        contactPage.Controls.AddRange(new Control[] 
        { 
            chkEmailNotify, 
            chkSMSNotify, 
            chkNewsLetter 
        });
        
        // Page 4: Preferences
        preferencesPage = new WizardControlPage
        {
            Title = "Preferences",
            Description = "Customize your experience",
            LayoutName = "Preferences"
        };
        
        Label lblTheme = new Label { Text = "Theme:", Location = new Point(20, 20), AutoSize = true };
        ComboBox cboTheme = new ComboBox 
        { 
            Location = new Point(20, 45), 
            Size = new Size(200, 20),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        cboTheme.Items.AddRange(new object[] { "Light", "Dark", "Auto" });
        cboTheme.SelectedIndex = 0;
        
        Label lblLanguage = new Label { Text = "Language:", Location = new Point(20, 80), AutoSize = true };
        ComboBox cboLanguage = new ComboBox 
        { 
            Location = new Point(20, 105), 
            Size = new Size(200, 20),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        cboLanguage.Items.AddRange(new object[] { "English", "Spanish", "French", "German" });
        cboLanguage.SelectedIndex = 0;
        
        preferencesPage.Controls.AddRange(new Control[] 
        { 
            lblTheme, cboTheme, 
            lblLanguage, cboLanguage 
        });
        
        // Page 5: Review
        reviewPage = new WizardControlPage
        {
            Title = "Review Your Information",
            Description = "Please verify your details before submitting",
            LayoutName = "Review"
        };
        
        Label lblReview = new Label
        {
            Text = "Please review your information:\n\n" +
                   "Click Back to make changes or Next to continue.",
            Location = new Point(20, 20),
            Size = new Size(500, 100),
            Font = new Font("Arial", 10)
        };
        reviewPage.Controls.Add(lblReview);
        
        // Page 6: Complete
        completePage = new WizardControlPage
        {
            Title = "Registration Complete!",
            Description = "Your account has been created",
            NextVisible = false,
            CancelVisible = false,
            FinishVisible = true,
            LayoutName = "Complete"
        };
        
        Label lblComplete = new Label
        {
            Text = "Congratulations!\n\n" +
                   "Your registration is complete.\n\n" +
                   "Click Finish to start using the application.",
            Location = new Point(20, 20),
            Size = new Size(500, 120),
            Font = new Font("Arial", 11, FontStyle.Bold)
        };
        completePage.Controls.Add(lblComplete);
        
        // Add all pages
        wizardControl1.WizardPages = new WizardControlPage[]
        {
            welcomePage,
            personalInfoPage,
            contactPage,
            preferencesPage,
            reviewPage,
            completePage
        };
        
        // Add wizard to form
        this.Controls.Add(wizardControl1);
        this.Text = "User Registration";
        this.Size = new Size(700, 500);
        this.StartPosition = FormStartPosition.CenterScreen;
    }
}
```

## Next Steps

After configuring wizard pages:

1. **Customize Navigation Buttons** → Read: [navigation-buttons.md](navigation-buttons.md)
   - Control button visibility per page
   - Add custom buttons
   - Style button appearance

2. **Implement Validation** → Read: [page-validation-events.md](page-validation-events.md)
   - Validate data before navigation
   - Handle page events
   - Create conditional workflows
