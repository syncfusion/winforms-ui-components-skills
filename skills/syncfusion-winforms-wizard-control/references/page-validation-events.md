# Page Validation and Events

This guide covers implementing page validation, handling wizard events, and controlling navigation flow in WizardControl.

## When to Read This

Read this reference when:
- Validating user input before allowing page navigation
- Handling wizard navigation events (Back, Next, Finish, etc.)
- Canceling navigation based on conditions
- Responding to page lifecycle events
- Creating conditional workflows

## ValidatePage Event

The `ValidatePage` event fires when a user attempts to navigate away from a page. Set `e.Cancel = true` to block navigation.

### Basic Validation

```csharp
personalInfoPage.ValidatePage += PersonalInfoPage_ValidatePage;

private void PersonalInfoPage_ValidatePage(object sender, System.ComponentModel.CancelEventArgs e)
{
    if (string.IsNullOrWhiteSpace(txtName.Text))
    {
        MessageBox.Show("Please enter your name.", "Validation Error",
            MessageBoxButtons.OK, MessageBoxIcon.Warning);
        e.Cancel = true;
        txtName.Focus();
        return;
    }

    if (string.IsNullOrWhiteSpace(txtEmail.Text) || !txtEmail.Text.Contains("@"))
    {
        MessageBox.Show("Please enter a valid email address.", "Validation Error",
            MessageBoxButtons.OK, MessageBoxIcon.Warning);
        e.Cancel = true;
        txtEmail.Focus();
    }
}
```

### Password Strength Validation

```csharp
private void AccountPage_ValidatePage(object sender, System.ComponentModel.CancelEventArgs e)
{
    if (txtUsername.Text.Length < 3)
    {
        ShowError("Username must be at least 3 characters.", txtUsername);
        e.Cancel = true; return;
    }

    if (!System.Text.RegularExpressions.Regex.IsMatch(txtUsername.Text, @"^[a-zA-Z0-9_]+$"))
    {
        ShowError("Username can only contain letters, numbers, and underscores.", txtUsername);
        e.Cancel = true; return;
    }

    if (txtPassword.Text.Length < 8 ||
        !txtPassword.Text.Any(char.IsUpper) ||
        !txtPassword.Text.Any(char.IsLower) ||
        !txtPassword.Text.Any(char.IsDigit))
    {
        ShowError("Password must be 8+ chars with uppercase, lowercase, and a digit.", txtPassword);
        e.Cancel = true; return;
    }

    if (txtPassword.Text != txtConfirmPassword.Text)
    {
        ShowError("Passwords do not match.", txtConfirmPassword);
        e.Cancel = true;
    }
}

private void ShowError(string message, Control focusControl)
{
    MessageBox.Show(message, "Validation Error", MessageBoxButtons.OK, MessageBoxIcon.Warning);
    focusControl.Focus();
}
```

## BackButtonCausesValidation

```csharp
// Disable validation when going back (recommended)
wizardControl1.BackButtonCausesValidation = false;

// Enable validation for both Back and Next
wizardControl1.BackButtonCausesValidation = true;
```

## WizardControl-Level Events

```csharp
// Back
wizardControl1.Back += (sender, e) => Console.WriteLine("Back clicked");

// Next
wizardControl1.Next += (sender, e) => Console.WriteLine("Next clicked");

// Cancel — prompt confirmation
wizardControl1.Cancel += (sender, e) =>
{
    if (MessageBox.Show("Are you sure you want to cancel?", "Confirm",
        MessageBoxButtons.YesNo, MessageBoxIcon.Question) == DialogResult.Yes)
        this.Close();
};

// Finish — save data and close
wizardControl1.Finish += (sender, e) =>
{
    SaveWizardData();
    MessageBox.Show("Setup completed successfully!", "Success",
        MessageBoxButtons.OK, MessageBoxIcon.Information);
    this.Close();
};

// Help — show page-specific help
wizardControl1.Help += (sender, e) =>
{
    WizardControlPage cur = wizardControl1.SelectedWizardPage;
    MessageBox.Show($"Help for: {cur.Title}", "Help",
        MessageBoxButtons.OK, MessageBoxIcon.Information);
};

// BeforePageSelect — fires before navigating to a new page
wizardControl1.BeforePageSelect += (sender, e) =>
{
    Console.WriteLine($"Leaving: {wizardControl1.SelectedWizardPage.Title}");
    // e.Cancel = true; to block navigation
};

// BeforeNext — pre-navigation check
wizardControl1.BeforeNext += (sender, e) =>
{
    if (wizardControl1.SelectedWizardPage == configPage && !IsConfigValid())
    {
        MessageBox.Show("Please complete all configuration options.");
        e.Cancel = true;
    }
};

// BeforeFinish — final validation before completing
wizardControl1.BeforeFinish += (sender, e) =>
{
    if (!AllPagesValid())
    {
        MessageBox.Show("Please complete all required information.", "Cannot Finish",
            MessageBoxButtons.OK, MessageBoxIcon.Warning);
        e.Cancel = true;
    }
};
```

## WizardControlPage-Level Events

```csharp
// PageLoad — fires when a page is displayed
summaryPage.PageLoad += (sender, e) => UpdateSummaryDisplay();

// Button click events
page2.BackClick   += (sender, e) => Console.WriteLine("Back on page 2");
page2.NextClick   += (sender, e) => Console.WriteLine("Next on page 2");
page2.CancelClick += (sender, e) => Console.WriteLine("Cancel on page 2");
finalPage.FinishClick += (sender, e) => Console.WriteLine("Finish clicked");
page2.HelpClick   += (sender, e) => Console.WriteLine("Help on page 2");
```

## Complete Example: Data Import Wizard

```csharp
public partial class DataImportWizard : Form
{
    private WizardControl wizardControl1;
    private WizardControlPage sourcePage, mappingPage, importPage;
    private ComboBox cboDataSource;
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

        sourcePage = new WizardControlPage
        {
            Title = "Select Data Source",
            Description = "Choose where to import data from",
            BackVisible = false
        };
        cboDataSource = new ComboBox
        {
            Location = new Point(20, 40), Size = new Size(300, 20),
            DropDownStyle = ComboBoxStyle.DropDownList
        };
        cboDataSource.Items.AddRange(new object[] { "SQL Server", "Excel File", "CSV File" });
        sourcePage.Controls.Add(new Label { Text = "Data Source:", Location = new Point(20, 20), AutoSize = true });
        sourcePage.Controls.Add(cboDataSource);

        mappingPage = new WizardControlPage
        {
            Title = "Map Fields",
            Description = "Map source fields to destination"
        };
        lstMappedFields = new ListBox { Location = new Point(300, 40), Size = new Size(200, 200) };
        mappingPage.Controls.Add(lstMappedFields);

        importPage = new WizardControlPage
        {
            Title = "Importing Data", Description = "Please wait...",
            BackEnabled = false, NextVisible = false, CancelEnabled = false,
            FinishVisible = true, FinishEnabled = false
        };
        progressBar = new ProgressBar
        {
            Location = new Point(20, 60), Size = new Size(500, 25),
            Style = ProgressBarStyle.Marquee
        };
        importPage.Controls.Add(progressBar);

        wizardControl1.WizardPages = new WizardControlPage[] { sourcePage, mappingPage, importPage };
        this.Controls.Add(wizardControl1);
        this.Size = new Size(700, 500);
        this.Text = "Data Import Wizard";
    }

    private void SetupEvents()
    {
        wizardControl1.BackButtonCausesValidation = false;
        sourcePage.ValidatePage += (s, e) =>
        {
            if (cboDataSource.SelectedIndex == -1)
            {
                MessageBox.Show("Please select a data source.", "Validation Error",
                    MessageBoxButtons.OK, MessageBoxIcon.Warning);
                e.Cancel = true;
            }
        };
        mappingPage.ValidatePage += (s, e) =>
        {
            if (lstMappedFields.Items.Count == 0)
            {
                MessageBox.Show("Please map at least one field.", "Validation Error",
                    MessageBoxButtons.OK, MessageBoxIcon.Warning);
                e.Cancel = true;
            }
        };
        importPage.PageLoad += async (s, e) =>
        {
            await ImportDataAsync();
            importPage.FinishEnabled = true;
            progressBar.Style = ProgressBarStyle.Continuous;
            progressBar.Value = 100;
        };
        wizardControl1.Finish += (s, e) => { this.DialogResult = DialogResult.OK; this.Close(); };
    }

    private async System.Threading.Tasks.Task ImportDataAsync()
    {
        await System.Threading.Tasks.Task.Delay(3000); // Simulate import
    }
}
```

## Custom Page Sequences via Events

```csharp
selectionPage.NextClick += (sender, e) =>
{
    selectionPage.NextPage = rbQuickSetup.Checked ? summaryPage : detailPage1;
};
```

## Next Steps

- [appearance-customization.md](appearance-customization.md) — styling and themes
- [design-time-features.md](design-time-features.md) — designer workflow