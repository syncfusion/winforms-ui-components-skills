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

### Page Creation Strategy

**For Simple Wizards (3-5 pages):**
- Use smart tags to add pages quickly
- Configure each page immediately after creation

**For Complex Wizards (6+ pages):**
- Open collection editor
- Add all pages at once
- Configure page properties in batch
- Close editor and add controls to pages

### Configuring Button Visibility

**At Design Time:**

1. Navigate to first page
2. Select page in designer
3. Property Grid → `BackVisible` = `False`
4. Navigate to last page
5. Property Grid → `NextVisible` = `False`
6. Property Grid → `FinishVisible` = `True`
7. Property Grid → `CancelVisible` = `False` (optional)

**Result:** First page has no Back button, last page has Finish instead of Next.

### Adding Controls to Pages

**Workflow:**

1. Navigate to desired page (smart tag or Property Grid)
2. Drag controls from Toolbox onto page
3. Position and size controls
4. Configure control properties
5. Repeat for each page

**Tip:** Use Layout containers (Panel, TableLayoutPanel) for complex layouts:
```csharp
// Add TableLayoutPanel to page for organized layout
TableLayoutPanel layout = new TableLayoutPanel
{
    Dock = DockStyle.Fill,
    ColumnCount = 2,
    RowCount = 3
};
wizardControlPage1.Controls.Add(layout);
```

### Testing Navigation in Designer

**Limited Testing Available:**
- Click Back/Next buttons in designer to see transitions
- Pages update visually
- Cannot test validation or events at design time
- Run application (F5) to test full functionality

### Naming Pages

**Best Practice:** Set `LayoutName` property for all pages:

1. Select page in designer
2. Property Grid → `LayoutName` = `"WelcomePage"`
3. Easier to identify pages in code
4. Enables finding pages by name: `FindPageByName("WelcomePage")`

### Common Design-Time Mistakes

**Mistake 1: Not Setting BackVisible on First Page**
- **Problem:** Back button shows on welcome page
- **Solution:** First page → `BackVisible` = `False`

**Mistake 2: Last Page Shows Next Instead of Finish**
- **Problem:** User clicks Next on final page
- **Solution:** Last page → `NextVisible` = `False`, `FinishVisible` = `True`

**Mistake 3: Controls Added to WizardControl Instead of WizardControlPage**
- **Problem:** Controls appear on all pages or behind navigation
- **Solution:** Ensure correct page is selected before adding controls

**Mistake 4: Pages Created But Not Configured**
- **Problem:** Pages have default "WizardControlPage1" titles
- **Solution:** Configure Title and Description for each page

**Mistake 5: Reordering Pages in Code After Design-Time Setup**
- **Problem:** Page order doesn't match expected sequence
- **Solution:** Use collection editor for reordering, or set NextPage/PreviousPage properties

## Troubleshooting Design-Time Issues

### Issue: Smart Tag Not Appearing

**Solutions:**
- Ensure WizardControl is selected (click on it)
- Check that control has focus (border with resize handles visible)
- Close and reopen designer
- Rebuild solution

### Issue: Collection Editor Shows Wrong Type

**Problem:** In .NET Core/5+, collection editor may show generic title

**Solution:**
- This is a known Visual Studio issue (GitHub #14049)
- Functionality is not affected
- Use editor normally despite title display

### Issue: Cannot Add Controls to Page

**Solutions:**
- Verify correct page is selected (check Property Grid shows WizardControlPage)
- Ensure page is visible in designer
- Try navigating to page using smart tag
- Close and reopen form designer

### Issue: Page Changes Not Saving

**Solutions:**
- Click "Save" after modifying properties
- Close collection editor with "OK" button (not X)
- Rebuild solution to ensure changes persist
- Check .Designer.cs file to verify changes

### Issue: SelectedWizardPage Dropdown Empty

**Solutions:**
- Ensure pages have been added to WizardPages collection
- Close and reopen designer
- Rebuild solution

## Next Steps

After mastering design-time features:

1. **Getting Started** → Read: [getting-started.md](getting-started.md)
   - Review programmatic page creation
   - Understand assembly requirements

2. **Validation and Events** → Read: [page-validation-events.md](page-validation-events.md)
   - Implement page validation
   - Handle navigation events

3. **Return to Main Guide** → Read: [../SKILL.md](../SKILL.md)
   - Review all wizard capabilities
   - Access additional references
