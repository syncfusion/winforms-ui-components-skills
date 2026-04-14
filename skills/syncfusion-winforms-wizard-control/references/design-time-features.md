# Design-Time Features

This guide covers using WizardControl's design-time features in Visual Studio, including smart tags, collection editor, context menus, and property grid commands.

## When to Read This

Read this reference when:
- Working with WizardControl in Visual Studio Designer
- Adding or removing pages visually
- Reordering pages without code
- Navigating between pages at design time
- Configuring wizard properties in Property Grid
- Using smart tags for quick configuration

## Smart Tag Operations

Smart tags provide quick access to common wizard operations.

### Accessing Smart Tags

1. Click the WizardControl in the designer
2. Click the small arrow icon (►) in the top-right corner
3. The smart tag panel appears with available actions

### Available Smart Tag Actions

**Add Page:**
- Adds a new WizardControlPage to the end of the collection
- New page is automatically selected
- Page properties can be set immediately in Property Grid

**Remove Page:**
- Removes the currently selected page
- If the last page is removed, the previous page becomes selected
- Cannot remove page if only one page exists

**Previous Page:**
- Navigates to the previous page in the collection
- Visual designer updates to show that page
- Disabled if currently on the first page

**Next Page:**
- Navigates to the next page in the collection
- Visual designer updates to show that page
- Disabled if currently on the last page

**Edit Pages:**
- Opens the WizardControlPage Collection Editor
- Allows bulk page management
- Provides detailed configuration options

### Smart Tag Workflow

**Quick Page Setup:**

1. Drop WizardControl onto form
2. Click smart tag arrow (►)
3. Click "Add Page" multiple times to create pages
4. Use "Next Page"/"Previous Page" to navigate
5. Configure each page in Property Grid:
   - Set `Title` property
   - Set `Description` property
   - Configure button visibility

**Example:** Creating a 4-page wizard in seconds:
1. Smart tag → "Add Page" (page 1 created)
2. Smart tag → "Add Page" (page 2 created)
3. Smart tag → "Add Page" (page 3 created)
4. Smart tag → "Add Page" (page 4 created)
5. Smart tag → "Previous Page" three times to go back to page 1
6. Set Title in Property Grid
7. Smart tag → "Next Page" and repeat

## Context Menu Operations

Right-click on WizardControl or WizardControlPage for additional options.

### Context Menu on WizardControl

- **Add Page** - Adds new page at end
- **Edit Pages** - Opens collection editor
- **Properties** - Opens Property Grid focused on wizard

### Context Menu on WizardControlPage

- **Bring To Front** - Moves page earlier in sequence (decreases index)
- **Send To Back** - Moves page later in sequence (increases index)
- **Delete** - Removes the page
- **Properties** - Opens Property Grid focused on page

**Note:** "Bring To Front" and "Send To Back" only move one position at a time. For significant reordering, use the collection editor.

## Collection Editor

The WizardControlPage Collection Editor provides comprehensive page management.

### Opening Collection Editor

**Method 1:** Smart tag → "Edit Pages"

**Method 2:** Property Grid → Find `WizardPages` property → Click ellipsis button (...)

**Method 3:** Context menu → "Edit Pages"

### Collection Editor Interface

**Left Panel (Members):**
- Lists all WizardControlPage instances
- Shows page index numbers
- Select a page to edit properties

**Right Panel (Properties):**
- Shows all properties of selected page
- Configure Title, Description, button visibility, etc.
- Changes apply immediately

**Buttons:**

**Add:**
- Creates new WizardControlPage
- Added at end of collection
- Automatically selected for configuration

**Remove:**
- Deletes selected page
- Cannot remove if only one page remains

**Up Arrow (↑):**
- Moves selected page earlier in sequence
- Decreases page index by 1
- Disabled if page is first

**Down Arrow (↓):**
- Moves selected page later in sequence
- Increases page index by 1
- Disabled if page is last

### Collection Editor Workflow

**Creating Multiple Pages:**

1. Open collection editor
2. Click "Add" button repeatedly to create pages
3. Select first page in list
4. Set properties in right panel:
   ```
   Title: Welcome
   Description: Welcome to the setup wizard
   BackVisible: False
   ```
5. Select next page
6. Configure properties
7. Repeat for all pages
8. Click "OK" to save

**Reordering Pages:**

1. Open collection editor
2. Select page to move
3. Click Up (↑) or Down (↓) arrows
4. Repeat until desired order achieved
5. Click "OK"

**Bulk Configuration:**

The collection editor is ideal for:
- Creating many pages at once
- Significant reordering (moving page from position 1 to 7)
- Reviewing all pages and their settings
- Configuring properties that require precise values

## Property Grid Commands

Configure wizard and pages using the Property Grid.

### WizardControl Properties

Select the WizardControl to see these key properties:

**WizardPages (Collection):**
- Click ellipsis (...) to open collection editor
- Shows number of pages in parentheses: `(Collection) [4]`

**SelectedWizardPage:**
- Dropdown showing all pages
- Select page to navigate to it in designer
- Changes which page is visible

**AutoLayoutBanner, AutoLayoutTitle, AutoLayoutDescription:**
- Boolean properties controlling automatic positioning
- Set to `False` for manual positioning

**BackButtonCausesValidation:**
- Set to `False` to allow back navigation without validation

**Style:**
- Dropdown with theme options
- Choose: Default, Office2016Colorful, Office2016White, Metro, etc.

**Font, ForeColor, BackColor:**
- Standard appearance properties

### WizardControlPage Properties

Select a WizardControlPage (click inside page area) to see:

**Title:**
- Text displayed in banner title label
- Changes per page

**Description:**
- Text displayed in banner description label
- Changes per page

**BackVisible, NextVisible, CancelVisible, FinishVisible, HelpVisible:**
- Boolean properties controlling button visibility
- Configure appropriate visibility for first, middle, and last pages

**BackEnabled, NextEnabled, CancelEnabled, FinishEnabled, HelpEnabled:**
- Boolean properties controlling button enabled state
- Use to disable navigation until conditions are met

**NextPage, PreviousPage:**
- Reference to other WizardControlPage instances
- Set for custom page sequences (non-linear navigation)
- Leave empty for automatic sequential navigation

**FullPage:**
- Boolean: `True` hides banner panel
- Use for pages requiring full space (terms and conditions, etc.)

**LayoutName:**
- String identifier for page
- Useful for finding pages programmatically

## Page Navigation in Designer

### Navigating Between Pages

**Method 1: Smart Tag**
- Click smart tag → "Next Page" or "Previous Page"

**Method 2: SelectedWizardPage Property**
- Select WizardControl
- Find `SelectedWizardPage` in Property Grid
- Choose page from dropdown

**Method 3: Clicking on Page Outline**
- In Document Outline window (View → Other Windows → Document Outline)
- Expand WizardControl node
- Click on desired WizardControlPage

**Method 4: Navigation Buttons**
- The wizard's Back/Next buttons are clickable in designer
- Click to navigate between pages visually

### Selecting Specific Page

To work on a specific page:

1. Select WizardControl
2. Property Grid → `SelectedWizardPage` → Choose page
3. Page displays in designer
4. Add controls by dragging from Toolbox
5. Configure controls in Property Grid

## CardLayout Property

The `CardLayout` property manages page visibility and ordering.

**Purpose:** Internal property that controls which page is displayed

**Usage:** Typically managed automatically by WizardControl

**Access:**
```csharp
// Get the CardLayout
CardLayout layout = wizardControl1.CardLayout;

// Manually select a card (page)
layout.SelectedCardIndex = 2;  // Show third page
```

**Note:** In most scenarios, use `SelectedWizardPage` instead of manipulating `CardLayout` directly.

## Design-Time Best Practices

- **Simple wizards (3–5 pages):** Use smart tags to add pages one at a time, configure each immediately.
- **Complex wizards (6+ pages):** Open collection editor, add all pages at once, configure in batch.
- **First page:** Set `BackVisible = False` in Property Grid.
- **Last page:** Set `NextVisible = False`, `FinishVisible = True` (and optionally `CancelVisible = False`).
- **Adding controls:** Navigate to the target page first (smart tag or `SelectedWizardPage` drop-down), then drag from Toolbox.
- **Layout:** Use `TableLayoutPanel` or `Panel` for complex page layouts — `Dock = DockStyle.Fill`.
- **Naming:** Set `LayoutName` on every page (e.g., `"WelcomePage"`) for programmatic lookup.
- **Testing:** Click Back/Next in designer for visual transition preview; run (F5) to test validation and events.

### Common Mistakes

| Mistake | Solution |
|---------|----------|
| Back button visible on first page | First page → `BackVisible = False` |
| Next shown on last page instead of Finish | Last page → `NextVisible = False`, `FinishVisible = True` |
| Controls added to WizardControl (not page) | Verify correct page is selected before dragging controls |
| Pages show default "WizardControlPage1" title | Set `Title` and `Description` for each page |
| Reordering pages in code breaks sequence | Use collection editor Up/Down arrows, or set `NextPage`/`PreviousPage` |

## Troubleshooting Design-Time Issues

| Issue | Solution |
|-------|----------|
| Smart tag not appearing | Click control to select it; rebuild solution; reopen designer |
| Collection editor shows generic title | Known .NET Core/5+ issue (GitHub #14049) — functionality unaffected |
| Cannot add controls to page | Verify page is selected (Property Grid shows WizardControlPage); navigate to page via smart tag |
| Page changes not saving | Close collection editor with OK (not X); rebuild; check `.Designer.cs` |
| SelectedWizardPage dropdown empty | Add pages to WizardPages first; reopen designer |

## Next Steps

- [getting-started.md](getting-started.md) — assembly requirements and programmatic setup
- [page-validation-events.md](page-validation-events.md) — implement page validation and events
