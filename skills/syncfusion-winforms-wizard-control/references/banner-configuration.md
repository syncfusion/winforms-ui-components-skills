# Banner Configuration

This guide covers configuring and customizing the banner panel in WizardControl.

## When to Read This

Read this reference when:
- Customizing the wizard header/banner area
- Adding banner images or logos
- Styling title and description text
- Configuring auto-layout for banner elements
- Adding custom controls to the banner panel

## Banner Panel Overview

The **BannerPanel** (top of the wizard) contains:
- **Title label** — main heading for the current page
- **Description label** — descriptive text for the current page
- **Banner image** — optional PictureBox for logo or graphic
- **Custom controls** — any additional controls in the header

## Adding Banner Image

```csharp
PictureBox pictureBox = new PictureBox
{
    Image = Image.FromFile("logo.png"),
    SizeMode = PictureBoxSizeMode.StretchImage,
    Size = new Size(100, 60),
    Location = new Point(450, 5)
};

wizardControl1.Banner = pictureBox;
```

## Title and Description Labels

```csharp
// Title text is set per page
welcomePage.Title = "Welcome to Setup";

// Customize title label appearance (shared across all pages)
wizardControl1.Title.Font = new Font("Segoe UI", 12, FontStyle.Bold);
wizardControl1.Title.ForeColor = Color.DarkBlue;

// Description text is set per page
welcomePage.Description = "This wizard will guide you through installation";

// Customize description label appearance
wizardControl1.Description.Font = new Font("Segoe UI", 9, FontStyle.Regular);
wizardControl1.Description.ForeColor = Color.Gray;
```

**Note:** Font and ForeColor are shared across all pages. Only the Text changes per page.

## Auto-Layout Properties

```csharp
// Enable automatic positioning of banner elements
wizardControl1.AutoLayoutBanner = true;
wizardControl1.AutoLayoutTitle = true;
wizardControl1.AutoLayoutDescription = true;

// Disable auto-layout for full manual control
wizardControl1.AutoLayoutBanner = false;
wizardControl1.AutoLayoutTitle = false;
wizardControl1.AutoLayoutDescription = false;

wizardControl1.Title.Location = new Point(20, 15);
wizardControl1.Description.Location = new Point(20, 40);
wizardControl1.Banner.Location = new Point(500, 10);
```

## BannerPanel Styling

```csharp
using Syncfusion.Drawing;

GradientPanel bannerPanel = wizardControl1.BannerPanel as GradientPanel;
if (bannerPanel != null)
{
    bannerPanel.BackgroundColor = new BrushInfo(
        GradientStyle.Horizontal,
        Color.AliceBlue,
        Color.LightSteelBlue
    );
}
```

## Adding Custom Controls to Banner

```csharp
private void AddProgressToBanner()
{
    Panel bannerPanel = wizardControl1.BannerPanel;

    Label lblProgress = new Label
    {
        Text = "Step 1 of 5",
        Location = new Point(bannerPanel.Width - 100, 15),
        AutoSize = true,
        Font = new Font("Segoe UI", 9, FontStyle.Bold),
        ForeColor = Color.Gray
    };
    bannerPanel.Controls.Add(lblProgress);

    wizardControl1.BeforePageSelect += (sender, e) =>
    {
        int idx = Array.IndexOf(wizardControl1.WizardPages, wizardControl1.SelectedWizardPage);
        lblProgress.Text = $"Step {idx + 1} of {wizardControl1.WizardPages.Length}";
    };
}
```

## Complete Banner Configuration Example

```csharp
using Syncfusion.Drawing;

private void SetupBrandedBanner()
{
    wizardControl1 = new WizardControl { Dock = DockStyle.Fill };

    GradientPanel bannerPanel = new GradientPanel
    {
        Height = 80,
        Dock = DockStyle.Top,
        BackgroundColor = new BrushInfo(
            GradientStyle.Vertical,
            Color.FromArgb(245, 250, 255),
            Color.FromArgb(225, 235, 245))
    };

    PictureBox logo = new PictureBox
    {
        Image = Image.FromFile("logo.png"),
        SizeMode = PictureBoxSizeMode.Zoom,
        Size = new Size(100, 60),
        Location = new Point(15, 10)
    };

    Label titleLabel = new Label
    {
        Text = "Wizard Title",
        Location = new Point(125, 15),
        AutoSize = true,
        Font = new Font("Segoe UI", 14, FontStyle.Bold),
        ForeColor = Color.FromArgb(0, 51, 102)
    };

    Label descLabel = new Label
    {
        Text = "Wizard description text",
        Location = new Point(125, 45),
        AutoSize = true,
        Font = new Font("Segoe UI", 9),
        ForeColor = Color.FromArgb(80, 80, 80)
    };

    Label progressLabel = new Label
    {
        Text = "Step 1 of 4",
        Location = new Point(bannerPanel.Width - 120, 30),
        AutoSize = true,
        Font = new Font("Segoe UI", 9, FontStyle.Bold),
        ForeColor = Color.FromArgb(0, 120, 215),
        BackColor = Color.Transparent
    };

    bannerPanel.Controls.AddRange(new Control[] { logo, titleLabel, descLabel, progressLabel });
    wizardControl1.Controls.Add(bannerPanel);

    wizardControl1.BannerPanel = bannerPanel;
    wizardControl1.Title = titleLabel;
    wizardControl1.Description = descLabel;
    wizardControl1.Banner = logo;

    wizardControl1.AutoLayoutBanner = false;
    wizardControl1.AutoLayoutTitle = false;
    wizardControl1.AutoLayoutDescription = false;

    wizardControl1.BeforePageSelect += (s, e) =>
    {
        int idx = Array.IndexOf(wizardControl1.WizardPages, wizardControl1.SelectedWizardPage);
        progressLabel.Text = $"Step {idx + 1} of {wizardControl1.WizardPages.Length}";
    };

    this.Controls.Add(wizardControl1);
    this.Size = new Size(700, 500);
}
```

## Canceling Auto-Layout

```csharp
wizardControl1.BannerControlLocationChanging += (sender, e) =>
{
    if (e is System.ComponentModel.CancelEventArgs cancelArgs)
        cancelArgs.Cancel = true;
};
```

## Next Steps

- [page-validation-events.md](page-validation-events.md) — validation and events
- [appearance-customization.md](appearance-customization.md) — full wizard styling