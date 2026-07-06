---
name: syncfusion-winforms-wizard-control
description: Guide for implementing Syncfusion WizardControl component in Windows Forms applications. Use when creating multi-step forms, installation wizards, setup dialogs, or step-by-step workflow interfaces with navigation buttons. Covers assembly deployment, page management, banner configuration, validation, navigation button handling, events, and appearance customization for professional wizard interfaces.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Implementing WizardControl in Windows Forms

This skill guides the implementation of the Syncfusion WizardControl component in Windows Forms applications. The WizardControl provides a rich, interactive interface for breaking complex tasks into simple sequential steps with guided navigation.

## When to Use This Skill

Use this skill when users need to:
- Create multi-step installation or setup wizards
- Implement sequential forms with Back/Next navigation
- Build guided workflows for complex data entry
- Design step-by-step configuration interfaces
- Create onboarding experiences with multiple pages
- Implement wizard-style dialogs for task completion
- Guide users through categorized data collection
- Build registration or checkout processes with progress tracking

## Component Overview

The **WizardControl** is a multi-page container that guides users through sequential tasks with automatic navigation button management.

**Key Capabilities:**
- **Multi-page management** - WizardControlPage collection with flexible ordering
- **Navigation buttons** - Back, Next, Cancel, Finish, Help with per-page visibility control
- **Banner panel** - Header with title, description, and image for context
- **Page validation** - ValidatePage event for data validation before navigation
- **Design-time support** - Smart tags, collection editor, visual page navigation
- **Flexible layout** - FullPage mode, custom button placement, BannerPanel customization
- **Event system** - Page lifecycle events, button click events, navigation events
- **Theming support** - Full appearance customization with Style property
- **RTL support** - Right-to-left layout for international applications

**Assembly:** `Syncfusion.Tools.Windows.dll`  
**Namespace:** `Syncfusion.Windows.Forms.Tools`  
**Dependencies:** 6 assemblies (Grid.Base, Grid.Windows, Shared.Base, Shared.Windows, Tools.Base, Tools.Windows)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:**
- Setting up WizardControl for the first time
- Understanding assembly requirements and installation
- Adding control via designer or code
- Creating basic wizard with multiple pages
- Configuring WizardContainer and WizardControlPages
- Troubleshooting setup issues

### Wizard Pages
📄 **Read:** [references/wizard-pages.md](references/wizard-pages.md)

**When to read:**
- Creating and managing WizardControlPage collection
- Setting page titles and descriptions
- Configuring FullPage property to hide banner
- Reordering pages using collection editor or properties
- Setting NextPage and PreviousPage for custom sequences
- Accessing pages at design time and runtime
- Building multi-page wizards with dynamic content

### Navigation Buttons
📄 **Read:** [references/navigation-buttons.md](references/navigation-buttons.md)

**When to read:**
- Controlling button visibility (BackVisible, NextVisible, etc.)
- Enabling/disabling buttons per page
- Adding Finish button to final page
- Creating custom buttons in wizard
- Reordering button sequence with GridBagLayout
- Customizing button appearance (FlatStyle, colors, images)
- Accessing BackButton, NextButton, CancelButton properties

### Banner Configuration
📄 **Read:** [references/banner-configuration.md](references/banner-configuration.md)

**When to read:**
- Configuring BannerPanel with gradient background
- Setting Banner property for header image
- Customizing Title and Description labels
- Using AutoLayoutBanner/Title/Description properties
- Adding custom controls to banner area
- Styling title and description text
- Creating branded wizard headers

### Page Validation and Events
📄 **Read:** [references/page-validation-events.md](references/page-validation-events.md)

**When to read:**
- Implementing page validation with ValidatePage event
- Handling navigation events (BeforeNext, BeforeBack, BeforeFinish)
- Responding to button clicks (BackClick, NextClick, etc.)
- Using PageLoad event for page initialization
- Implementing custom page sequences
- Canceling navigation based on validation
- Using BackButtonCausesValidation property

### Appearance Customization
📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

**When to read:**
- Customizing wizard and page backgrounds
- Setting fonts and colors for text
- Configuring border styles (Fixed3D, FixedSingle)
- Styling BannerPanel with gradients
- Applying themes with Style property
- Creating light/dark mode wizards
- Matching application branding

### Design-Time Features
📄 **Read:** [references/design-time-features.md](references/design-time-features.md)

**When to read:**
- Using smart tags for wizard operations
- Adding/removing pages at design time
- Navigating between pages in designer
- Using collection editor for page management
- Configuring properties in Property Grid
- Understanding SelectedWizardPage property
- Working with CardLayout property

## Quick Start Example

### Basic Installation Wizard

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

public partial class InstallationWizard : Form
{
    private WizardControl wizardControl1;
    private WizardControlPage welcomePage;
    private WizardControlPage licensePage;
    private WizardControlPage installPage;
    private WizardControlPage finishPage;
    
    public InstallationWizard()
    {
        InitializeComponent();
        CreateWizard();
    }
    
    private void CreateWizard()
    {
        // Create wizard control
        wizardControl1 = new WizardControl
        {
            Dock = DockStyle.Fill,
            Style = Syncfusion.Windows.Forms.Tools.Wizard.Style.Metro
        };
        
        // Create pages
        welcomePage = new WizardControlPage
        {
            Title = "Welcome",
            Description = "Welcome to the Installation Wizard",
            BackVisible = false  // Hide Back on first page
        };
        
        licensePage = new WizardControlPage
        {
            Title = "License Agreement",
            Description = "Please read and accept the license terms"
        };
        
        installPage = new WizardControlPage
        {
            Title = "Installing",
            Description = "Please wait while files are being installed"
        };
        
        finishPage = new WizardControlPage
        {
            Title = "Completed",
            Description = "Installation completed successfully",
            NextVisible = false,
            CancelVisible = false,
            FinishVisible = true
        };
        
        // Add pages to wizard
        wizardControl1.WizardPages = new WizardControlPage[]
        {
            welcomePage,
            licensePage,
            installPage,
            finishPage
        };
        
        // Add wizard to form
        this.Controls.Add(wizardControl1);
        this.Text = "Setup Wizard";
        this.Size = new Size(600, 450);
    }
}
```

**VB.NET:**
```vbnet
Imports System
Imports System.Drawing
Imports System.Windows.Forms
Imports Syncfusion.Windows.Forms.Tools

Public Partial Class InstallationWizard
    Inherits Form
    
    Private wizardControl1 As WizardControl
    Private welcomePage As WizardControlPage
    Private licensePage As WizardControlPage
    Private installPage As WizardControlPage
    Private finishPage As WizardControlPage
    
    Public Sub New()
        InitializeComponent()
        CreateWizard()
    End Sub
    
    Private Sub CreateWizard()
        ' Create wizard control
        wizardControl1 = New WizardControl With {
            .Dock = DockStyle.Fill,
            .Style = Syncfusion.Windows.Forms.Tools.Wizard.Style.Metro
        }
        
        ' Create pages
        welcomePage = New WizardControlPage With {
            .Title = "Welcome",
            .Description = "Welcome to the Installation Wizard",
            .BackVisible = False
        }
        
        licensePage = New WizardControlPage With {
            .Title = "License Agreement",
            .Description = "Please read and accept the license terms"
        }
        
        installPage = New WizardControlPage With {
            .Title = "Installing",
            .Description = "Please wait while files are being installed"
        }
        
        finishPage = New WizardControlPage With {
            .Title = "Completed",
            .Description = "Installation completed successfully",
            .NextVisible = False,
            .CancelVisible = False,
            .FinishVisible = True
        }
        
        ' Add pages to wizard
        wizardControl1.WizardPages = New WizardControlPage() {
            welcomePage,
            licensePage,
            installPage,
            finishPage
        }
        
        ' Add wizard to form
        Me.Controls.Add(wizardControl1)
        Me.Text = "Setup Wizard"
        Me.Size = New Size(600, 450)
    End Sub
End Class
```

## Common Patterns

### Pattern 1: Wizard with Validation

Validate data before allowing navigation to next page:

**C#:**
```csharp
public partial class RegistrationWizard : Form
{
    private WizardControlPage personalInfoPage;
    private TextBox txtName;
    private TextBox txtEmail;
    
    private void SetupWizard()
    {
        personalInfoPage = new WizardControlPage
        {
            Title = "Personal Information",
            Description = "Please enter your details"
        };
        
        // Add controls to page
        txtName = new TextBox { Location = new Point(20, 20), Width = 300 };
        txtEmail = new TextBox { Location = new Point(20, 60), Width = 300 };
        
        personalInfoPage.Controls.Add(new Label 
        { 
            Text = "Name:", 
            Location = new Point(20, 0) 
        });
        personalInfoPage.Controls.Add(txtName);
        personalInfoPage.Controls.Add(new Label 
        { 
            Text = "Email:", 
            Location = new Point(20, 40) 
        });
        personalInfoPage.Controls.Add(txtEmail);
        
        // Subscribe to validation event
        personalInfoPage.ValidatePage += PersonalInfoPage_ValidatePage;
    }
    
    private void PersonalInfoPage_ValidatePage(object sender, 
        System.ComponentModel.CancelEventArgs e)
    {
        // Validate name
        if (string.IsNullOrWhiteSpace(txtName.Text))
        {
            MessageBox.Show("Please enter your name.", "Validation Error",
                MessageBoxButtons.OK, MessageBoxIcon.Warning);
            e.Cancel = true;
            txtName.Focus();
            return;
        }
        
        // Validate email
        if (string.IsNullOrWhiteSpace(txtEmail.Text) || 
            !txtEmail.Text.Contains("@"))
        {
            MessageBox.Show("Please enter a valid email address.", 
                "Validation Error",
                MessageBoxButtons.OK, MessageBoxIcon.Warning);
            e.Cancel = true;
            txtEmail.Focus();
            return;
        }
    }
}
```

### Pattern 2: Dynamic Page Sequence

Control page flow based on user selections:

**C#:**
```csharp
public partial class ConfigurationWizard : Form
{
    private WizardControlPage typePage;
    private WizardControlPage basicSettingsPage;
    private WizardControlPage advancedSettingsPage;
    private RadioButton rbBasic;
    private RadioButton rbAdvanced;
    
    private void SetupDynamicWizard()
    {
        typePage = new WizardControlPage
        {
            Title = "Configuration Type",
            Description = "Choose your configuration level"
        };
        
        rbBasic = new RadioButton 
        { 
            Text = "Basic Configuration", 
            Location = new Point(20, 20),
            Checked = true
        };
        rbAdvanced = new RadioButton 
        { 
            Text = "Advanced Configuration", 
            Location = new Point(20, 50) 
        };
        
        typePage.Controls.Add(rbBasic);
        typePage.Controls.Add(rbAdvanced);
        
        basicSettingsPage = new WizardControlPage
        {
            Title = "Basic Settings",
            Description = "Configure basic options"
        };
        
        advancedSettingsPage = new WizardControlPage
        {
            Title = "Advanced Settings",
            Description = "Configure advanced options"
        };
        
        // Set dynamic page sequence
        typePage.NextClick += TypePage_NextClick;
    }
    
    private void TypePage_NextClick(object sender, EventArgs e)
    {
        // Dynamically set next page based on selection
        if (rbBasic.Checked)
        {
            typePage.NextPage = basicSettingsPage;
        }
        else
        {
            typePage.NextPage = advancedSettingsPage;
        }
    }
}
```

### Pattern 3: Wizard with Progress Tracking

Show current step progress in banner:

**C#:**
```csharp
public partial class DataImportWizard : Form
{
    private Label lblProgress;
    
    private void SetupProgressWizard()
    {
        // Create progress label in banner
        lblProgress = new Label
        {
            Text = "Step 1 of 4",
            Location = new Point(400, 10),
            AutoSize = true,
            Font = new Font("Arial", 9, FontStyle.Bold)
        };
        
        // Add to wizard's banner panel (assuming gradientPanel1 is banner)
        wizardControl1.BannerPanel.Controls.Add(lblProgress);
        
        // Subscribe to page changes
        wizardControl1.BeforePageSelect += WizardControl1_BeforePageSelect;
    }
    
    private void WizardControl1_BeforePageSelect(object sender, EventArgs e)
    {
        // Update progress based on current page
        int currentIndex = wizardControl1.WizardPages.IndexOf(
            wizardControl1.SelectedWizardPage);
        int totalPages = wizardControl1.WizardPages.Count;
        
        lblProgress.Text = $"Step {currentIndex + 1} of {totalPages}";
    }
}
```

## Key Properties

### WizardControl Properties

| Property | Type | Description |
|----------|------|-------------|
| `WizardPages` | `WizardControlPage[]` | Collection of wizard pages |
| `SelectedWizardPage` | `WizardControlPage` | Currently displayed page |
| `BannerPanel` | `Panel` | Panel containing banner controls |
| `Banner` | `Control` | Picture box for banner image |
| `Title` | `Label` | Label displaying page title |
| `Description` | `Label` | Label displaying page description |
| `BackButton` | `Button` | Back navigation button |
| `NextButton` | `Button` | Next navigation button |
| `CancelButton` | `Button` | Cancel button |
| `FinishButton` | `Button` | Finish button |
| `HelpButton` | `Button` | Help button |
| `Style` | `Wizard.Style` | Visual theme (Default, Metro, Office) |
| `BackButtonCausesValidation` | `bool` | Whether Back button triggers validation |
| `AutoLayoutBanner` | `bool` | Automatically layout banner controls |
| `AutoLayoutTitle` | `bool` | Automatically layout title label |
| `AutoLayoutDescription` | `bool` | Automatically layout description label |

### WizardControlPage Properties

| Property | Type | Description |
|----------|------|-------------|
| `Title` | `string` | Page title text |
| `Description` | `string` | Page description text |
| `BackVisible` | `bool` | Back button visibility |
| `NextVisible` | `bool` | Next button visibility |
| `CancelVisible` | `bool` | Cancel button visibility |
| `FinishVisible` | `bool` | Finish button visibility |
| `HelpVisible` | `bool` | Help button visibility |
| `BackEnabled` | `bool` | Back button enabled state |
| `NextEnabled` | `bool` | Next button enabled state |
| `CancelEnabled` | `bool` | Cancel button enabled state |
| `FinishEnabled` | `bool` | Finish button enabled state |
| `HelpEnabled` | `bool` | Help button enabled state |
| `FullPage` | `bool` | Hide banner for full-page content |
| `NextPage` | `WizardPage` | Custom next page override |
| `PreviousPage` | `WizardPage` | Custom previous page override |
| `CancelOverFinish` | `bool` | Position cancel over finish button |
| `LayoutName` | `string` | Unique identifier for page |

## Common Use Cases

1. **Installation Wizards** - Multi-step software installation with welcome, license, options, install, and finish pages

2. **Setup Assistants** - Application configuration wizards for first-run setup or preferences

3. **Registration Forms** - Step-by-step user registration with personal info, account details, preferences, and confirmation

4. **Data Import Wizards** - Guided data import with source selection, mapping, validation, and import execution

5. **Report Generators** - Report creation wizards with parameter selection, formatting, preview, and export

6. **Configuration Managers** - Complex configuration interfaces broken into logical categories with navigation

7. **Checkout Processes** - E-commerce checkout flows with cart, shipping, payment, and confirmation pages

8. **Onboarding Experiences** - New user onboarding with tutorial steps, feature introduction, and setup completion

## Additional Resources

- [WizardControl API Documentation](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.WizardControl.html)
- [Control Dependencies](https://help.syncfusion.com/windowsforms/control-dependencies#wizardcontrol)
- **Assembly:** `Syncfusion.Tools.Windows.dll`
- **Namespace:** `Syncfusion.Windows.Forms.Tools`
