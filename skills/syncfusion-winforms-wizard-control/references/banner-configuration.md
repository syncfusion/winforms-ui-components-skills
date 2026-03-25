# Banner Configuration

This guide covers configuring and customizing the banner panel in WizardControl, including title, description, images, and layout.

## When to Read This

Read this reference when:
- Customizing the wizard header/banner area
- Adding banner images or logos
- Styling title and description text
- Configuring auto-layout for banner elements
- Adding custom controls to the banner panel
- Creating branded wizard interfaces

## Banner Panel Overview

The **BannerPanel** is a gradient panel at the top of the wizard that displays:
- **Title label** - Main heading for the current page
- **Description label** - Descriptive text for the current page
- **Banner image** - Optional PictureBox for logo or graphic
- **Custom controls** - Any additional controls you want in the header

## Banner Property

The `Banner` property sets the image/logo displayed in the banner area.

### Adding Banner Image

**C#:**
```csharp
// Create picture box for banner
PictureBox pictureBox = new PictureBox
{
    Image = Image.FromFile("logo.png"),
    SizeMode = PictureBoxSizeMode.StretchImage,
    Size = new Size(100, 60),
    Location = new Point(450, 5)
};

// Set as wizard banner
wizardControl1.Banner = pictureBox;
```

**VB.NET:**
```vbnet
' Create picture box for banner
Dim pictureBox As New PictureBox With {
    .Image = Image.FromFile("logo.png"),
    .SizeMode = PictureBoxSizeMode.StretchImage,
    .Size = New Size(100, 60),
    .Location = New Point(450, 5)
}

' Set as wizard banner
wizardControl1.Banner = pictureBox
```

### Banner Image from Resources

**C#:**
```csharp
// Load from embedded resources
PictureBox pictureBox = new PictureBox
{
    Image = Properties.Resources.CompanyLogo,
    SizeMode = PictureBoxSizeMode.Zoom,
    Size = new Size(120, 70)
};

wizardControl1.Banner = pictureBox;
```

**VB.NET:**
```vbnet
' Load from embedded resources
Dim pictureBox As New PictureBox With {
    .Image = My.Resources.CompanyLogo,
    .SizeMode = PictureBoxSizeMode.Zoom,
    .Size = New Size(120, 70)
}

wizardControl1.Banner = pictureBox
```

## Title and Description Labels

The wizard displays a title and description for each page in the banner area.

### Configuring Title Label

The title label displays the page's Title property:

**C#:**
```csharp
// Title is set per page
welcomePage.Title = "Welcome to Setup";

// Access and customize title label directly
Label titleLabel = wizardControl1.Title;
titleLabel.Font = new Font("Segoe UI", 12, FontStyle.Bold);
titleLabel.ForeColor = Color.DarkBlue;
```

**VB.NET:**
```vbnet
' Title is set per page
welcomePage.Title = "Welcome to Setup"

' Access and customize title label directly
Dim titleLabel As Label = wizardControl1.Title
titleLabel.Font = New Font("Segoe UI", 12, FontStyle.Bold)
titleLabel.ForeColor = Color.DarkBlue
```

### Configuring Description Label

The description label displays the page's Description property:

**C#:**
```csharp
// Description is set per page
welcomePage.Description = "This wizard will guide you through installation";

// Access and customize description label directly
Label descLabel = wizardControl1.Description;
descLabel.Font = new Font("Segoe UI", 9, FontStyle.Regular);
descLabel.ForeColor = Color.Gray;
```

**VB.NET:**
```vbnet
' Description is set per page
welcomePage.Description = "This wizard will guide you through installation"

' Access and customize description label directly
Dim descLabel As Label = wizardControl1.Description
descLabel.Font = New Font("Segoe UI", 9, FontStyle.Regular)
descLabel.ForeColor = Color.Gray
```

**Important:** Title and description appearance (Font, ForeColor) is shared across all pages. Only the Text changes per page.

## Auto-Layout Properties

The wizard can automatically position banner elements for you.

### AutoLayoutBanner

Controls automatic positioning of the banner image:

**C#:**
```csharp
// Enable auto-layout for banner image
wizardControl1.AutoLayoutBanner = true;
```

**VB.NET:**
```vbnet
' Enable auto-layout for banner image
wizardControl1.AutoLayoutBanner = True
```

### AutoLayoutTitle

Controls automatic positioning of the title label:

**C#:**
```csharp
// Enable auto-layout for title
wizardControl1.AutoLayoutTitle = true;
```

**VB.NET:**
```vbnet
' Enable auto-layout for title
wizardControl1.AutoLayoutTitle = True
```

### AutoLayoutDescription

Controls automatic positioning of the description label:

**C#:**
```csharp
// Enable auto-layout for description
wizardControl1.AutoLayoutDescription = true;
```

**VB.NET:**
```vbnet
' Enable auto-layout for description
wizardControl1.AutoLayoutDescription = True
```

### Disabling Auto-Layout

For complete control over positioning, disable auto-layout:

**C#:**
```csharp
// Disable auto-layout for custom positioning
wizardControl1.AutoLayoutBanner = false;
wizardControl1.AutoLayoutTitle = false;
wizardControl1.AutoLayoutDescription = false;

// Manually position elements
wizardControl1.Title.Location = new Point(20, 15);
wizardControl1.Description.Location = new Point(20, 40);
wizardControl1.Banner.Location = new Point(500, 10);
```

## BannerPanel Customization

Access and customize the gradient panel that contains the banner elements.

### Accessing BannerPanel

**C#:**
```csharp
// Create gradient panel for banner
GradientPanel bannerPanel = new GradientPanel();

// Create label controls
Label lblTitle = new Label
{
    Text = "Page Title",
    Location = new Point(20, 15),
    AutoSize = true,
    Font = new Font("Segoe UI", 12, FontStyle.Bold),
    ForeColor = Color.DarkSlateGray
};

Label lblDescription = new Label
{
    Text = "This is the description of the wizard page",
    Location = new Point(20, 42),
    AutoSize = true,
    Font = new Font("Segoe UI", 9),
    ForeColor = Color.Gray
};

// Add labels to banner panel
bannerPanel.Controls.Add(lblTitle);
bannerPanel.Controls.Add(lblDescription);

// Add banner panel to wizard
wizardControl1.Controls.Add(bannerPanel);

// Set references
wizardControl1.Title = lblTitle;
wizardControl1.Description = lblDescription;
wizardControl1.BannerPanel = bannerPanel;
```

**VB.NET:**
```vbnet
' Create gradient panel for banner
Dim bannerPanel As New GradientPanel()

' Create label controls
Dim lblTitle As New Label With {
    .Text = "Page Title",
    .Location = New Point(20, 15),
    .AutoSize = True,
    .Font = New Font("Segoe UI", 12, FontStyle.Bold),
    .ForeColor = Color.DarkSlateGray
}

Dim lblDescription As New Label With {
    .Text = "This is the description of the wizard page",
    .Location = New Point(20, 42),
    .AutoSize = True,
    .Font = New Font("Segoe UI", 9),
    .ForeColor = Color.Gray
}

' Add labels to banner panel
bannerPanel.Controls.Add(lblTitle)
bannerPanel.Controls.Add(lblDescription)

' Add banner panel to wizard
wizardControl1.Controls.Add(bannerPanel)

' Set references
wizardControl1.Title = lblTitle
wizardControl1.Description = lblDescription
wizardControl1.BannerPanel = bannerPanel
```

### Styling BannerPanel Background

The banner panel is a GradientPanel, supporting gradient backgrounds:

**C#:**
```csharp
using Syncfusion.Drawing;

// Get or create banner panel
GradientPanel bannerPanel = wizardControl1.BannerPanel as GradientPanel;

if (bannerPanel != null)
{
    // Set gradient background
    bannerPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.AliceBlue,
        Color.LightSteelBlue
    );
    
    // Alternative: solid color
    // bannerPanel.BackColor = Color.LightBlue;
}
```

**VB.NET:**
```vbnet
Imports Syncfusion.Drawing

' Get or create banner panel
Dim bannerPanel As GradientPanel = TryCast(wizardControl1.BannerPanel, GradientPanel)

If bannerPanel IsNot Nothing Then
    ' Set gradient background
    bannerPanel.BackgroundColor = New BrushInfo(
        GradientStyle.Horizontal,
        Color.AliceBlue,
        Color.LightSteelBlue
    )
    
    ' Alternative: solid color
    ' bannerPanel.BackColor = Color.LightBlue
End If
```

### Adding Custom Controls to Banner

**C#:**
```csharp
private void AddProgressToBanner()
{
    // Get banner panel
    Panel bannerPanel = wizardControl1.BannerPanel;
    
    // Create progress label
    Label lblProgress = new Label
    {
        Text = "Step 1 of 5",
        Location = new Point(bannerPanel.Width - 100, 15),
        AutoSize = true,
        Font = new Font("Segoe UI", 9, FontStyle.Bold),
        ForeColor = Color.Gray
    };
    
    // Add to banner
    bannerPanel.Controls.Add(lblProgress);
    
    // Update on page change
    wizardControl1.BeforePageSelect += (sender, e) =>
    {
        int currentIndex = Array.IndexOf(
            wizardControl1.WizardPages,
            wizardControl1.SelectedWizardPage
        );
        lblProgress.Text = $"Step {currentIndex + 1} of {wizardControl1.WizardPages.Length}";
    };
}
```

## Complete Banner Configuration Example

**C#:**
```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;
using Syncfusion.Drawing;

public partial class BrandedWizard : Form
{
    private WizardControl wizardControl1;
    private GradientPanel bannerPanel;
    private PictureBox bannerImage;
    private Label titleLabel;
    private Label descriptionLabel;
    private Label progressLabel;
    
    public BrandedWizard()
    {
        InitializeComponent();
        SetupBrandedBanner();
        CreateWizardPages();
    }
    
    private void SetupBrandedBanner()
    {
        // Create wizard
        wizardControl1 = new WizardControl
        {
            Dock = DockStyle.Fill
        };
        
        // Create gradient banner panel
        bannerPanel = new GradientPanel
        {
            Height = 80,
            Dock = DockStyle.Top,
            BackgroundColor = new BrushInfo(
                GradientStyle.Vertical,
                Color.FromArgb(245, 250, 255),
                Color.FromArgb(225, 235, 245)
            )
        };
        
        // Company logo
        bannerImage = new PictureBox
        {
            Image = Properties.Resources.CompanyLogo,
            SizeMode = PictureBoxSizeMode.Zoom,
            Size = new Size(100, 60),
            Location = new Point(15, 10)
        };
        
        // Title label
        titleLabel = new Label
        {
            Text = "Wizard Title",
            Location = new Point(125, 15),
            AutoSize = true,
            Font = new Font("Segoe UI", 14, FontStyle.Bold),
            ForeColor = Color.FromArgb(0, 51, 102)
        };
        
        // Description label
        descriptionLabel = new Label
        {
            Text = "Wizard description text",
            Location = new Point(125, 45),
            AutoSize = true,
            Font = new Font("Segoe UI", 9),
            ForeColor = Color.FromArgb(80, 80, 80)
        };
        
        // Progress indicator
        progressLabel = new Label
        {
            Text = "Step 1 of 4",
            Location = new Point(bannerPanel.Width - 120, 30),
            AutoSize = true,
            Font = new Font("Segoe UI", 9, FontStyle.Bold),
            ForeColor = Color.FromArgb(0, 120, 215),
            BackColor = Color.Transparent
        };
        
        // Add controls to banner
        bannerPanel.Controls.Add(bannerImage);
        bannerPanel.Controls.Add(titleLabel);
        bannerPanel.Controls.Add(descriptionLabel);
        bannerPanel.Controls.Add(progressLabel);
        
        // Add banner to wizard
        wizardControl1.Controls.Add(bannerPanel);
        
        // Configure wizard banner properties
        wizardControl1.BannerPanel = bannerPanel;
        wizardControl1.Title = titleLabel;
        wizardControl1.Description = descriptionLabel;
        wizardControl1.Banner = bannerImage;
        
        // Disable auto-layout for custom positioning
        wizardControl1.AutoLayoutBanner = false;
        wizardControl1.AutoLayoutTitle = false;
        wizardControl1.AutoLayoutDescription = false;
        
        // Update progress on page change
        wizardControl1.BeforePageSelect += UpdateProgress;
        
        // Add wizard to form
        this.Controls.Add(wizardControl1);
        this.Text = "Branded Setup Wizard";
        this.Size = new Size(700, 500);
    }
    
    private void CreateWizardPages()
    {
        // Create 4 wizard pages
        WizardControlPage page1 = new WizardControlPage
        {
            Title = "Welcome",
            Description = "Welcome to the setup wizard",
            BackVisible = false
        };
        
        WizardControlPage page2 = new WizardControlPage
        {
            Title = "Configuration",
            Description = "Configure your settings"
        };
        
        WizardControlPage page3 = new WizardControlPage
        {
            Title = "Installation",
            Description = "Installing components"
        };
        
        WizardControlPage page4 = new WizardControlPage
        {
            Title = "Complete",
            Description = "Setup completed successfully",
            NextVisible = false,
            CancelVisible = false,
            FinishVisible = true
        };
        
        // Add pages
        wizardControl1.WizardPages = new WizardControlPage[]
        {
            page1, page2, page3, page4
        };
    }
    
    private void UpdateProgress(object sender, EventArgs e)
    {
        int currentIndex = Array.IndexOf(
            wizardControl1.WizardPages,
            wizardControl1.SelectedWizardPage
        );
        int totalPages = wizardControl1.WizardPages.Length;
        
        progressLabel.Text = $"Step {currentIndex + 1} of {totalPages}";
    }
}
```

## Banner Styling Examples

### Modern Flat Banner

**C#:**
```csharp
private void CreateModernBanner()
{
    GradientPanel banner = wizardControl1.BannerPanel as GradientPanel;
    
    if (banner != null)
    {
        // Flat white background
        banner.BackColor = Color.White;
        
        // Bottom border for separation
        banner.BorderStyle = BorderStyle.None;
        banner.Paint += (sender, e) =>
        {
            // Draw bottom line
            e.Graphics.DrawLine(
                new Pen(Color.FromArgb(220, 220, 220), 1),
                0, banner.Height - 1,
                banner.Width, banner.Height - 1
            );
        };
    }
    
    // Modern typography
    wizardControl1.Title.Font = new Font("Segoe UI Light", 18);
    wizardControl1.Title.ForeColor = Color.FromArgb(60, 60, 60);
    
    wizardControl1.Description.Font = new Font("Segoe UI", 9);
    wizardControl1.Description.ForeColor = Color.FromArgb(120, 120, 120);
}
```

### Dark Theme Banner

**C#:**
```csharp
private void CreateDarkBanner()
{
    GradientPanel banner = wizardControl1.BannerPanel as GradientPanel;
    
    if (banner != null)
    {
        // Dark gradient
        banner.BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(40, 40, 40),
            Color.FromArgb(20, 20, 20)
        );
    }
    
    // Light text for dark background
    wizardControl1.Title.ForeColor = Color.White;
    wizardControl1.Description.ForeColor = Color.FromArgb(180, 180, 180);
}
```

### Branded Gradient Banner

**C#:**
```csharp
private void CreateBrandedBanner()
{
    GradientPanel banner = wizardControl1.BannerPanel as GradientPanel;
    
    if (banner != null)
    {
        // Corporate colors gradient
        banner.BackgroundColor = new BrushInfo(
            GradientStyle.Horizontal,
            Color.FromArgb(0, 100, 200),    // Brand primary
            Color.FromArgb(0, 150, 255)     // Brand secondary
        );
    }
    
    // White text on colored background
    wizardControl1.Title.ForeColor = Color.White;
    wizardControl1.Title.Font = new Font("Arial", 14, FontStyle.Bold);
    
    wizardControl1.Description.ForeColor = Color.FromArgb(230, 240, 255);
    wizardControl1.Description.Font = new Font("Arial", 9);
}
```

## Canceling Auto-Layout

Handle the `BannerControlLocationChanging` event to prevent auto-layout:

**C#:**
```csharp
private void SetupBanner()
{
    // Subscribe to event
    wizardControl1.BannerControlLocationChanging += 
        WizardControl1_BannerControlLocationChanging;
    
    // Position controls manually
    wizardControl1.Title.Location = new Point(30, 20);
    wizardControl1.Description.Location = new Point(30, 50);
}

private void WizardControl1_BannerControlLocationChanging(
    object sender, EventArgs e)
{
    // Cancel auto-layout event to keep custom positions
    if (e is System.ComponentModel.CancelEventArgs cancelArgs)
    {
        cancelArgs.Cancel = true;
    }
}
```

## Next Steps

After configuring the banner:

1. **Implement Validation** → Read: [page-validation-events.md](page-validation-events.md)
   - Validate page data
   - Handle navigation events
   - Control wizard flow

2. **Customize Appearance** → Read: [appearance-customization.md](appearance-customization.md)
   - Style wizard pages
   - Set background colors
   - Configure border styles
