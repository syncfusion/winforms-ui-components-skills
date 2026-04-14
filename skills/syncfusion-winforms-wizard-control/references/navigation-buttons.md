# Navigation Buttons Configuration

This guide covers configuring and customizing the navigation buttons in WizardControl.

## When to Read This

Read this reference when:
- Controlling button visibility on specific pages
- Enabling or disabling buttons based on conditions
- Adding Finish button to the final page
- Creating custom buttons in the wizard
- Customizing button appearance

## Default Navigation Buttons

| Button | Purpose | Default Visibility |
|--------|---------|-------------------|
| **Back** | Navigate to previous page | Visible (except first page) |
| **Next** | Navigate to next page | Visible (except last page) |
| **Cancel** | Cancel and close wizard | Visible on all pages |
| **Finish** | Complete wizard | Hidden by default |
| **Help** | Show help information | Visible on all pages |

## Button Visibility

### Per-Page Visibility Properties

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
lastPage.CancelVisible = false;
lastPage.FinishVisible = true;
```

### Dynamic Visibility

```csharp
wizardControl1.BeforePageSelect += (sender, e) =>
{
    WizardControlPage cur = wizardControl1.SelectedWizardPage;
    bool isLast = wizardControl1.WizardPages[wizardControl1.WizardPages.Length - 1] == cur;
    cur.NextVisible = !isLast;
    cur.FinishVisible = isLast;
};
```

## Button Enabled State

```csharp
// Disable Next until validation passes
licensePage.NextEnabled = false;
chkAcceptTerms.CheckedChanged += (sender, e) =>
{
    licensePage.NextEnabled = chkAcceptTerms.Checked;
};

// Disable all navigation during processing
processingPage.BackEnabled = false;
processingPage.NextEnabled = false;
processingPage.CancelEnabled = false;
```

## Adding Finish Button

```csharp
WizardControlPage finishPage = new WizardControlPage
{
    Title = "Setup Complete",
    Description = "Click Finish to exit the wizard",
    NextVisible = false,
    CancelVisible = false,
    FinishVisible = true
};
```

### CancelOverFinish Property

```csharp
// true: Cancel positioned over Finish — set CancelVisible = false to show Finish
finishPage.CancelOverFinish = true;
finishPage.CancelVisible = false;
finishPage.FinishVisible = true;
```

**Important:** If `CancelOverFinish = true` and `CancelVisible = true`, the Cancel button overrides `FinishVisible`, hiding the Finish button.

## Adding Custom Buttons

```csharp
Button btnExport = new Button { Text = "Export...", Size = new Size(80, 25) };
btnExport.Click += (sender, e) => MessageBox.Show("Exporting data...");

wizardControl1.Controls.Add(btnExport);
wizardControl1.GridBagLayout.GetConstraintsRef(btnExport).GridPosX = 1;
wizardControl1.GridBagLayout.GetConstraintsRef(btnExport).GridPosY = 5;
```

## Reordering Button Sequence

Default GridPosX values: 0–1 = custom area, 2 = Help, 3 = spacer, 4 = Back, 5 = Next, 6 = Cancel/Finish.

```csharp
wizardControl1.GridBagLayout.GetConstraintsRef(wizardControl1.BackButton).GridPosX = 4;
wizardControl1.GridBagLayout.GetConstraintsRef(wizardControl1.NextButton).GridPosX = 5;
wizardControl1.GridBagLayout.GetConstraintsRef(wizardControl1.FinishButton).GridPosX = 6;
```

**Note:** WizardControl may reset built-in button positions on page change. Manual positioning is most reliable for custom buttons.

## Button Appearance Customization

### Accessing Built-in Buttons

```csharp
Button backBtn   = wizardControl1.BackButton;
Button nextBtn   = wizardControl1.NextButton;
Button cancelBtn = wizardControl1.CancelButton;
Button finishBtn = wizardControl1.FinishButton;
Button helpBtn   = wizardControl1.HelpButton;
```

### Complete Styling Example

```csharp
private void ApplyModernButtonStyle()
{
    StylePrimaryButton(wizardControl1.NextButton);
    StylePrimaryButton(wizardControl1.FinishButton);
    StyleSecondaryButton(wizardControl1.BackButton);
    StyleSecondaryButton(wizardControl1.CancelButton);
}

private void StylePrimaryButton(Button btn)
{
    btn.FlatStyle = FlatStyle.Flat;
    btn.BackColor = Color.FromArgb(0, 120, 215);
    btn.ForeColor = Color.White;
    btn.Font = new Font("Segoe UI", 9);
    btn.FlatAppearance.BorderSize = 0;
    btn.FlatAppearance.MouseOverBackColor = Color.FromArgb(0, 99, 177);
    btn.FlatAppearance.MouseDownBackColor = Color.FromArgb(0, 78, 138);
    btn.Cursor = Cursors.Hand;
}

private void StyleSecondaryButton(Button btn)
{
    btn.FlatStyle = FlatStyle.Flat;
    btn.BackColor = Color.FromArgb(240, 240, 240);
    btn.ForeColor = Color.Black;
    btn.Font = new Font("Segoe UI", 9);
    btn.FlatAppearance.BorderSize = 1;
    btn.FlatAppearance.BorderColor = Color.FromArgb(200, 200, 200);
    btn.FlatAppearance.MouseOverBackColor = Color.FromArgb(229, 229, 229);
    btn.FlatAppearance.MouseDownBackColor = Color.FromArgb(204, 204, 204);
    btn.Cursor = Cursors.Hand;
}
```

### Adding Button Images

```csharp
wizardControl1.BackButton.Image = Image.FromFile("back.png");
wizardControl1.BackButton.ImageAlign = ContentAlignment.MiddleLeft;
wizardControl1.BackButton.TextImageRelation = TextImageRelation.ImageBeforeText;

wizardControl1.NextButton.Image = Image.FromFile("next.png");
wizardControl1.NextButton.ImageAlign = ContentAlignment.MiddleRight;
wizardControl1.NextButton.TextImageRelation = TextImageRelation.TextBeforeImage;
```

## Button Property Reference

| Property | Type | Description |
|----------|------|-------------|
| `FlatStyle` | `FlatStyle` | Flat, Popup, Standard, System |
| `FlatAppearance` | `FlatButtonAppearance` | Border and mouse-state colors |
| `Font` | `Font` | Button text font |
| `ForeColor` | `Color` | Text color |
| `BackColor` | `Color` | Background color |
| `Image` | `Image` | Button icon/image |
| `ImageAlign` | `ContentAlignment` | Image alignment |
| `ImageIndex` | `int` | ImageList index |
| `ImageList` | `ImageList` | Image collection |
| `Text` | `string` | Button label |
| `TextAlign` | `ContentAlignment` | Text alignment |
| `TextImageRelation` | `TextImageRelation` | Image position relative to text |
| `Size` | `Size` | Button dimensions |
| `Cursor` | `Cursor` | Mouse cursor over button |

## Next Steps

- [banner-configuration.md](banner-configuration.md) — banner customization
- [page-validation-events.md](page-validation-events.md) — event handling and validation