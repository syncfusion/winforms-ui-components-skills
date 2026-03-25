# Page Validation and Events

This guide covers implementing page validation, handling wizard events, and controlling navigation flow in WizardControl.

## When to Read This

Read this reference when:
- Validating user input before allowing page navigation
- Handling wizard navigation events (Back, Next, Finish, etc.)
- Implementing custom page sequences based on data
- Responding to page lifecycle events
- Canceling navigation based on conditions
- Creating conditional workflows

## ValidatePage Event

The `ValidatePage` event fires when a user attempts to navigate away from a page, allowing you to validate data and cancel navigation if needed.

### Basic Validation

**C#:**
```csharp
private void SetupValidation()
{
    // Subscribe to ValidatePage event
    personalInfoPage.ValidatePage += PersonalInfoPage_ValidatePage;
}

private void PersonalInfoPage_ValidatePage(
    object sender,
    System.ComponentModel.CancelEventArgs e)
{
    // Validate name field
    if (string.IsNullOrWhiteSpace(txtName.Text))
    {
        MessageBox.Show(
            "Please enter your name.",
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        );
        
        e.Cancel = true;  // Cancel navigation
        txtName.Focus();
        return;
    }
    
    // Validate email field
    if (string.IsNullOrWhiteSpace(txtEmail.Text) || 
        !txtEmail.Text.Contains("@"))
    {
        MessageBox.Show(
            "Please enter a valid email address.",
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        );
        
        e.Cancel = true;
        txtEmail.Focus();
        return;
    }
    
    // Validation passed - navigation continues
}
```

**VB.NET:**
```vbnet
Private Sub SetupValidation()
    ' Subscribe to ValidatePage event
    AddHandler personalInfoPage.ValidatePage, AddressOf PersonalInfoPage_ValidatePage
End Sub

Private Sub PersonalInfoPage_ValidatePage(
    sender As Object,
    e As System.ComponentModel.CancelEventArgs)
    
    ' Validate name field
    If String.IsNullOrWhiteSpace(txtName.Text) Then
        MessageBox.Show(
            "Please enter your name.",
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        )
        
        e.Cancel = True
        txtName.Focus()
        Return
    End If
    
    ' Validate email field
    If String.IsNullOrWhiteSpace(txtEmail.Text) OrElse 
       Not txtEmail.Text.Contains("@") Then
        MessageBox.Show(
            "Please enter a valid email address.",
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        )
        
        e.Cancel = True
        txtEmail.Focus()
        Return
    End If
End Sub
```

### Complex Validation Example

**C#:**
```csharp
public partial class RegistrationWizard : Form
{
    private WizardControlPage accountPage;
    private TextBox txtUsername;
    private TextBox txtPassword;
    private TextBox txtConfirmPassword;
    
    private void SetupAccountPage()
    {
        accountPage = new WizardControlPage
        {
            Title = "Create Account",
            Description = "Choose your username and password"
        };
        
        // Add controls
        Label lblUsername = new Label 
        { 
            Text = "Username:", 
            Location = new Point(20, 20), 
            AutoSize = true 
        };
        txtUsername = new TextBox 
        { 
            Location = new Point(20, 45), 
            Size = new Size(300, 20) 
        };
        
        Label lblPassword = new Label 
        { 
            Text = "Password:", 
            Location = new Point(20, 80), 
            AutoSize = true 
        };
        txtPassword = new TextBox 
        { 
            Location = new Point(20, 105), 
            Size = new Size(300, 20),
            UseSystemPasswordChar = true 
        };
        
        Label lblConfirm = new Label 
        { 
            Text = "Confirm Password:", 
            Location = new Point(20, 140), 
            AutoSize = true 
        };
        txtConfirmPassword = new TextBox 
        { 
            Location = new Point(20, 165), 
            Size = new Size(300, 20),
            UseSystemPasswordChar = true 
        };
        
        accountPage.Controls.AddRange(new Control[] 
        {
            lblUsername, txtUsername,
            lblPassword, txtPassword,
            lblConfirm, txtConfirmPassword
        });
        
        // Subscribe to validation
        accountPage.ValidatePage += AccountPage_ValidatePage;
    }
    
    private void AccountPage_ValidatePage(
        object sender,
        System.ComponentModel.CancelEventArgs e)
    {
        // Validate username
        if (txtUsername.Text.Length < 3)
        {
            ShowValidationError(
                "Username must be at least 3 characters long.",
                txtUsername
            );
            e.Cancel = true;
            return;
        }
        
        if (!System.Text.RegularExpressions.Regex.IsMatch(
            txtUsername.Text, @"^[a-zA-Z0-9_]+$"))
        {
            ShowValidationError(
                "Username can only contain letters, numbers, and underscores.",
                txtUsername
            );
            e.Cancel = true;
            return;
        }
        
        // Validate password strength
        if (txtPassword.Text.Length < 8)
        {
            ShowValidationError(
                "Password must be at least 8 characters long.",
                txtPassword
            );
            e.Cancel = true;
            return;
        }
        
        // Check password complexity
        bool hasUpper = txtPassword.Text.Any(char.IsUpper);
        bool hasLower = txtPassword.Text.Any(char.IsLower);
        bool hasDigit = txtPassword.Text.Any(char.IsDigit);
        
        if (!hasUpper || !hasLower || !hasDigit)
        {
            ShowValidationError(
                "Password must contain uppercase, lowercase, and numbers.",
                txtPassword
            );
            e.Cancel = true;
            return;
        }
        
        // Verify passwords match
        if (txtPassword.Text != txtConfirmPassword.Text)
        {
            ShowValidationError(
                "Passwords do not match.",
                txtConfirmPassword
            );
            e.Cancel = true;
            return;
        }
    }
    
    private void ShowValidationError(string message, Control focusControl)
    {
        MessageBox.Show(
            message,
            "Validation Error",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        );
        focusControl.Focus();
    }
}
```

## BackButtonCausesValidation Property

Control whether the Back button triggers page validation:

**C#:**
```csharp
// Disable validation when going back
wizardControl1.BackButtonCausesValidation = false;

// Enable validation for both Back and Next
wizardControl1.BackButtonCausesValidation = true;
```

**VB.NET:**
```vbnet
' Disable validation when going back
wizardControl1.BackButtonCausesValidation = False

' Enable validation for both Back and Next
wizardControl1.BackButtonCausesValidation = True
```

**Use Case:** Allow users to go back without fixing validation errors on the current page.

## Wizard Control Events

Events fired at the WizardControl level for overall wizard navigation.

### Back Event

Fires when the Back button is clicked:

**C#:**
```csharp
wizardControl1.Back += (sender, e) =>
{
    Console.WriteLine("Back button clicked");
    // Perform any back navigation logic
};
```

### Next Event

Fires when the Next button is clicked:

**C#:**
```csharp
wizardControl1.Next += (sender, e) =>
{
    Console.WriteLine("Next button clicked");
    // Perform any next navigation logic
};
```

### Cancel Event

Fires when the Cancel button is clicked:

**C#:**
```csharp
wizardControl1.Cancel += (sender, e) =>
{
    DialogResult result = MessageBox.Show(
        "Are you sure you want to cancel?",
        "Confirm Cancel",
        MessageBoxButtons.YesNo,
        MessageBoxIcon.Question
    );
    
    if (result == DialogResult.Yes)
    {
        this.Close();
    }
};
```

### Finish Event

Fires when the Finish button is clicked:

**C#:**
```csharp
wizardControl1.Finish += (sender, e) =>
{
    // Save data or perform final actions
    SaveWizardData();
    
    MessageBox.Show(
        "Setup completed successfully!",
        "Success",
        MessageBoxButtons.OK,
        MessageBoxIcon.Information
    );
    
    this.Close();
};
```

### Help Event

Fires when the Help button is clicked:

**C#:**
```csharp
wizardControl1.Help += (sender, e) =>
{
    // Show help for current page
    WizardControlPage currentPage = wizardControl1.SelectedWizardPage;
    ShowHelp(currentPage.Title);
};

private void ShowHelp(string pageName)
{
    // Open help documentation or display help dialog
    MessageBox.Show(
        $"Help for: {pageName}\n\n[Help content here]",
        "Help",
        MessageBoxButtons.OK,
        MessageBoxIcon.Information
    );
}
```

### BeforePageSelect Event

Fires before navigating to a new page:

**C#:**
```csharp
wizardControl1.BeforePageSelect += (sender, e) =>
{
    WizardControlPage currentPage = wizardControl1.SelectedWizardPage;
    
    // Log page transitions
    Console.WriteLine($"Leaving page: {currentPage.Title}");
    
    // Can cancel navigation if needed
    // e.Cancel = true;
};
```

### BeforeBack Event

Fires before navigating to the previous page:

**C#:**
```csharp
wizardControl1.BeforeBack += (sender, e) =>
{
    Console.WriteLine("About to go back");
    
    // Optionally cancel back navigation
    // if (someCondition)
    // {
    //     e.Cancel = true;
    // }
};
```

### BeforeNext Event

Fires before navigating to the next page:

**C#:**
```csharp
wizardControl1.BeforeNext += (sender, e) =>
{
    Console.WriteLine("About to go next");
    
    // Can perform pre-navigation checks
    WizardControlPage currentPage = wizardControl1.SelectedWizardPage;
    
    if (currentPage == configPage && !IsConfigValid())
    {
        MessageBox.Show("Please complete all configuration options.");
        e.Cancel = true;
    }
};
```

### BeforeFinish Event

Fires before the Finish action completes:

**C#:**
```csharp
wizardControl1.BeforeFinish += (sender, e) =>
{
    // Final validation before completing wizard
    if (!AllPagesValid())
    {
        MessageBox.Show(
            "Please complete all required information.",
            "Cannot Finish",
            MessageBoxButtons.OK,
            MessageBoxIcon.Warning
        );
        e.Cancel = true;
    }
};
```

## Wizard Page Events

Events fired at the WizardControlPage level for page-specific logic.

### PageLoad Event

Fires when a page is displayed:

**C#:**
```csharp
welcomePage.PageLoad += (sender, e) =>
{
    Console.WriteLine("Welcome page loaded");
    // Initialize page data
};

summaryPage.PageLoad += (sender, e) =>
{
    // Update summary with data from previous pages
    UpdateSummaryDisplay();
};
```

### Button Click Events

Each page can handle individual button clicks:

**C#:**
```csharp
// BackClick event
page2.BackClick += (sender, e) =>
{
    Console.WriteLine("Back clicked on page 2");
};

// NextClick event
page2.NextClick += (sender, e) =>
{
    Console.WriteLine("Next clicked on page 2");
    // Can modify navigation flow here
};

// CancelClick event
page2.CancelClick += (sender, e) =>
{
    Console.WriteLine("Cancel clicked on page 2");
};

// FinishClick event
finalPage.FinishClick += (sender, e) =>
{
    Console.WriteLine("Finish clicked");
};

// HelpClick event
page2.HelpClick += (sender, e) =>
{
    Console.WriteLine("Help clicked on page 2");
};
```

## Complete Validation Example

**C#:**
```csharp
public partial class DataImportWizard : Form
{
    private WizardControl wizardControl1;
    private WizardControlPage sourcePage;
    private WizardControlPage mappingPage;
    private WizardControlPage validationPage;
    private WizardControlPage importPage;
    
    private ComboBox cboDataSource;
    private ListBox lstAvailableFields;
    private ListBox lstMappedFields;
    private ProgressBar progressBar;
    
    public DataImportWizard()
    {
        InitializeComponent();
        SetupWizard();
        SetupEvents();
    }
    
    private void SetupWizard()
    {
        wizardControl1 = new WizardControl { Dock = DockStyle.Fill };
        
        // Page 1: Source selection
        sourcePage = new WizardControlPage
        {
            Title = "Select Data Source",
            Description = "Choose where to import data from",
            BackVisible = false
        };
        
        cboDataSource = new ComboBox
        {
            Location = new Point(20, 40),
            Size = new Size(300, 20),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        cboDataSource.Items.AddRange(new object[] 
        { 
            "SQL Server", 
            "Excel File", 
            "CSV File", 
            "JSON File" 
        });
        sourcePage.Controls.Add(new Label 
        { 
            Text = "Data Source:", 
            Location = new Point(20, 20), 
            AutoSize = true 
        });
        sourcePage.Controls.Add(cboDataSource);
        
        // Page 2: Field mapping
        mappingPage = new WizardControlPage
        {
            Title = "Map Fields",
            Description = "Map source fields to destination"
        };
        
        lstAvailableFields = new ListBox
        {
            Location = new Point(20, 40),
            Size = new Size(200, 200)
        };
        lstMappedFields = new ListBox
        {
            Location = new Point(300, 40),
            Size = new Size(200, 200)
        };
        mappingPage.Controls.AddRange(new Control[]
        {
            new Label { Text = "Available Fields:", Location = new Point(20, 20), AutoSize = true },
            lstAvailableFields,
            new Label { Text = "Mapped Fields:", Location = new Point(300, 20), AutoSize = true },
            lstMappedFields
        });
        
        // Page 3: Validation
        validationPage = new WizardControlPage
        {
            Title = "Validate Data",
            Description = "Review validation results"
        };
        
        // Page 4: Import
        importPage = new WizardControlPage
        {
            Title = "Importing Data",
            Description = "Please wait...",
            BackEnabled = false,
            NextVisible = false,
            CancelEnabled = false,
            FinishVisible = true,
            FinishEnabled = false
        };
        
        progressBar = new ProgressBar
        {
            Location = new Point(20, 60),
            Size = new Size(500, 25),
            Style = ProgressBarStyle.Marquee
        };
        importPage.Controls.Add(new Label 
        { 
            Text = "Importing records...", 
            Location = new Point(20, 30), 
            AutoSize = true 
        });
        importPage.Controls.Add(progressBar);
        
        wizardControl1.WizardPages = new WizardControlPage[]
        {
            sourcePage,
            mappingPage,
            validationPage,
            importPage
        };
        
        this.Controls.Add(wizardControl1);
        this.Size = new Size(700, 500);
        this.Text = "Data Import Wizard";
    }
    
    private void SetupEvents()
    {
        // Validation events
        sourcePage.ValidatePage += SourcePage_ValidatePage;
        mappingPage.ValidatePage += MappingPage_ValidatePage;
        validationPage.ValidatePage += ValidationPage_ValidatePage;
        
        // Page load events
        mappingPage.PageLoad += MappingPage_PageLoad;
        validationPage.PageLoad += ValidationPage_PageLoad;
        importPage.PageLoad += ImportPage_PageLoad;
        
        // Disable back button validation
        wizardControl1.BackButtonCausesValidation = false;
        
        // Finish event
        wizardControl1.Finish += WizardControl1_Finish;
    }
    
    private void SourcePage_ValidatePage(
        object sender, 
        System.ComponentModel.CancelEventArgs e)
    {
        if (cboDataSource.SelectedIndex == -1)
        {
            MessageBox.Show(
                "Please select a data source.",
                "Validation Error",
                MessageBoxButtons.OK,
                MessageBoxIcon.Warning
            );
            e.Cancel = true;
            cboDataSource.Focus();
        }
    }
    
    private void MappingPage_PageLoad(object sender, EventArgs e)
    {
        // Load available fields based on selected source
        LoadAvailableFields();
    }
    
    private void MappingPage_ValidatePage(
        object sender, 
        System.ComponentModel.CancelEventArgs e)
    {
        if (lstMappedFields.Items.Count == 0)
        {
            MessageBox.Show(
                "Please map at least one field.",
                "Validation Error",
                MessageBoxButtons.OK,
                MessageBoxIcon.Warning
            );
            e.Cancel = true;
        }
    }
    
    private void ValidationPage_PageLoad(object sender, EventArgs e)
    {
        // Run validation checks
        RunDataValidation();
    }
    
    private void ValidationPage_ValidatePage(
        object sender, 
        System.ComponentModel.CancelEventArgs e)
    {
        // Check if validation passed
        if (!IsDataValid())
        {
            DialogResult result = MessageBox.Show(
                "Data validation found errors. Continue anyway?",
                "Validation Errors",
                MessageBoxButtons.YesNo,
                MessageBoxIcon.Warning
            );
            
            if (result == DialogResult.No)
            {
                e.Cancel = true;
            }
        }
    }
    
    private async void ImportPage_PageLoad(object sender, EventArgs e)
    {
        // Start import process
        await ImportDataAsync();
        
        // Enable finish when done
        importPage.FinishEnabled = true;
        progressBar.Style = ProgressBarStyle.Continuous;
        progressBar.Value = 100;
        
        MessageBox.Show(
            "Import completed successfully!",
            "Success",
            MessageBoxButtons.OK,
            MessageBoxIcon.Information
        );
    }
    
    private void WizardControl1_Finish(object sender, EventArgs e)
    {
        // Close wizard
        this.DialogResult = DialogResult.OK;
        this.Close();
    }
    
    // Helper methods
    private void LoadAvailableFields()
    {
        lstAvailableFields.Items.Clear();
        // Load based on cboDataSource.SelectedItem
        lstAvailableFields.Items.AddRange(new object[] 
        { 
            "Field1", "Field2", "Field3", "Field4" 
        });
    }
    
    private void RunDataValidation()
    {
        // Perform validation logic
    }
    
    private bool IsDataValid()
    {
        // Check validation results
        return true;
    }
    
    private async System.Threading.Tasks.Task ImportDataAsync()
    {
        // Simulate import
        await System.Threading.Tasks.Task.Delay(3000);
    }
}
```

## Custom Page Sequences

Use events to create dynamic navigation flows:

**C#:**
```csharp
private void SetupDynamicFlow()
{
    // Intercept next button to control flow
    selectionPage.NextClick += (sender, e) =>
    {
        if (rbQuickSetup.Checked)
        {
            // Skip detailed pages, go straight to summary
            selectionPage.NextPage = summaryPage;
        }
        else
        {
            // Go to first detail page
            selectionPage.NextPage = detailPage1;
        }
    };
}
```

## Next Steps

After implementing validation and events:

1. **Customize Appearance** → Read: [appearance-customization.md](appearance-customization.md)
   - Style wizard pages
   - Configure colors and fonts
   - Apply themes

2. **Design-Time Features** → Read: [design-time-features.md](design-time-features.md)
   - Use smart tags
   - Work with collection editor
   - Navigate pages in designer
