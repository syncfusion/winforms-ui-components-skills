# Wizard Pages Configuration

This guide covers creating, managing, and configuring WizardControlPage instances in the WizardControl.

## When to Read This

Read this reference when:
- Creating and adding wizard pages
- Setting page titles and descriptions
- Configuring full-page mode without banner
- Reordering pages or implementing non-linear navigation
- Accessing and navigating pages at runtime

## Creating Wizard Pages

### Designer Method

1. Select WizardControl in designer.
2. Click the smart tag (►) → **Add Page** (or right-click → **Add Page**).
3. Alternatively, open **WizardPages** property → click ellipsis → use Collection Editor.

### Programmatic Method

```csharp
using Syncfusion.Windows.Forms.Tools;

WizardControlPage page1 = new WizardControlPage();
WizardControlPage page2 = new WizardControlPage();
WizardControlPage page3 = new WizardControlPage();

page1.Title = "Step 1";
page1.Description = "Enter basic information";

page2.Title = "Step 2";
page2.Description = "Configure settings";

page3.Title = "Step 3";
page3.Description = "Review and finish";

wizardControl1.WizardPages = new WizardControlPage[] { page1, page2, page3 };
```

## Setting Page Title and Description

```csharp
WizardControlPage userInfoPage = new WizardControlPage
{
    Title = "User Information",
    Description = "Please enter your personal details"
};
```

**Dynamic title at runtime:**

```csharp
private void UpdatePageTitle(WizardControlPage page, string userName)
{
    page.Title = $"Welcome, {userName}";
    page.Description = "Let's complete your profile setup";
}
```

## Full Page Mode

The `FullPage` property hides the banner panel, allowing the page to occupy the entire wizard area.

```csharp
WizardControlPage termsPage = new WizardControlPage
{
    Title = "Terms and Conditions",
    FullPage = true
};

RichTextBox rtbTerms = new RichTextBox
{
    Dock = DockStyle.Fill,
    Text = "TERMS AND CONDITIONS\n\nLong legal text...",
    ReadOnly = true
};

termsPage.Controls.Add(rtbTerms);
```

**Use when:** displaying long documents, complex diagrams, data grids needing maximum space.

## Reordering Wizard Pages

### Collection Editor (Design Time)

1. Open **WizardPages** property → click ellipsis.
2. Select a page and use the **Up/Down** arrow buttons.

### NextPage and PreviousPage Properties

```csharp
// Linear: page1 -> page2 -> page3
page1.NextPage = page2;
page2.PreviousPage = page1;
page2.NextPage = page3;
page3.PreviousPage = page2;

// Non-linear: skip page2
page1.NextPage = page3;
page3.PreviousPage = page1;
```

### Conditional Page Flow

```csharp
selectionPage.NextClick += (sender, e) =>
{
    selectionPage.NextPage = rbBasic.Checked ? basicConfigPage : advancedConfigPage;
};
```

## Accessing Pages at Runtime

```csharp
// Get current page
WizardControlPage currentPage = wizardControl1.SelectedWizardPage;

// Navigate to a specific page
wizardControl1.SelectedWizardPage = settingsPage;

// Navigate by index
wizardControl1.SelectedWizardPage = wizardControl1.WizardPages[2];

// Iterate
foreach (WizardControlPage page in wizardControl1.WizardPages)
{
    Console.WriteLine(page.Title);
}

// Count
int totalPages = wizardControl1.WizardPages.Length;

// Find by index
int currentIndex = Array.IndexOf(wizardControl1.WizardPages, wizardControl1.SelectedWizardPage);
```

## LayoutName Property

Assign a string identifier to each page for programmatic lookup:

```csharp
welcomePage.LayoutName = "WelcomePage";
settingsPage.LayoutName = "SettingsPage";

WizardControlPage FindPageByName(string layoutName)
{
    foreach (WizardControlPage page in wizardControl1.WizardPages)
    {
        if (page.LayoutName == layoutName) return page;
    }
    return null;
}
```

## Complete Example: Looping / Edit Navigation

Allow users to jump back to earlier pages from a review page:

```csharp
Button btnEditPersonal = new Button { Text = "Edit Personal Info", Location = new Point(20, 60) };
btnEditPersonal.Click += (sender, e) =>
{
    wizardControl1.SelectedWizardPage = personalInfoPage;
};

Button btnEditContact = new Button { Text = "Edit Contact Details", Location = new Point(20, 100) };
btnEditContact.Click += (sender, e) =>
{
    wizardControl1.SelectedWizardPage = contactPage;
};

reviewPage.Controls.Add(btnEditPersonal);
reviewPage.Controls.Add(btnEditContact);
```

## Best Practices

- Always set `BackVisible = false` on the first page.
- Always set `NextVisible = false` and `FinishVisible = true` on the last page.
- Use `LayoutName` on every page for easier programmatic access.
- Use `NextPage`/`PreviousPage` for conditional flows; leave unset for sequential navigation.

## Next Steps

- [navigation-buttons.md](navigation-buttons.md) — button visibility and customization
- [page-validation-events.md](page-validation-events.md) — validation and events