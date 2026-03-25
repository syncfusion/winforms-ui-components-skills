# Page Reordering and Navigation

## Design-Time Reordering

At design-time, you can reorder pages in several ways:

### Method 1: XPTaskPage Collection Editor

1. Select XPTaskPane in designer
2. Open Properties panel
3. Click "TaskPages" property button (...)
4. XPTaskPage Collection Editor opens
5. Select a page in the list
6. Use Up/Down arrows to reorder
7. Click OK to apply changes

### Method 2: Smart Tag Verbs

1. Select an XPTaskPage in designer
2. Right-click and choose verb options:
   - **Bring To Front** - Move page to beginning
   - **Send To Back** - Move page to end
   - **Previous Page** - View previous page in design view
   - **Next Page** - View next page in design view

```
Example verb menu:
├── Bring To Front (move to index 0)
├── Send To Back (move to end)
├── Previous Page (view)
└── Next Page (view)
```

3. Select verb to reorder

### Method 3: Property Grid Approach

1. In Form Designer, select multiple pages
2. Use context menu to cut/move
3. Restructure order via Collection Editor

## Runtime Page Navigation

### NextPage Property

Define which page displays after the current page:

```csharp
// Create sequential flow
xpTaskPage1.NextPage = xpTaskPage2;
xpTaskPage2.NextPage = xpTaskPage3;
xpTaskPage3.NextPage = null; // Last page, no next
```

```vb
' Create sequential flow
xpTaskPage1.NextPage = xpTaskPage2
xpTaskPage2.NextPage = xpTaskPage3
xpTaskPage3.NextPage = Nothing ' Last page, no next
```

**Use Case: Multi-Step Wizard**

```csharp
// Step 1: Welcome
xpTaskPage1.Title = "Welcome";
xpTaskPage1.NextPage = xpTaskPage2;

// Step 2: User Info
xpTaskPage2.Title = "Enter Details";
xpTaskPage2.NextPage = xpTaskPage3;

// Step 3: Confirmation
xpTaskPage3.Title = "Confirm";
xpTaskPage3.NextPage = null;

// User navigates: Welcome → Details → Confirm
```

### PreviousPage Property

Define which page displays before the current page:

```csharp
// Create reverse navigation
xpTaskPage3.PreviousPage = xpTaskPage2;
xpTaskPage2.PreviousPage = xpTaskPage1;
xpTaskPage1.PreviousPage = null; // First page, no previous
```

```vb
' Create reverse navigation
xpTaskPage3.PreviousPage = xpTaskPage2
xpTaskPage2.PreviousPage = xpTaskPage1
xpTaskPage1.PreviousPage = Nothing ' First page, no previous
```

### Bidirectional Navigation

Define both NextPage and PreviousPage for complete flow:

```csharp
// Create complete navigation chain
private void ConfigurePageNavigation()
{
    // Page 1
    xpTaskPage1.Title = "Step 1: Setup";
    xpTaskPage1.NextPage = xpTaskPage2;
    xpTaskPage1.PreviousPage = null;

    // Page 2
    xpTaskPage2.Title = "Step 2: Configure";
    xpTaskPage2.NextPage = xpTaskPage3;
    xpTaskPage2.PreviousPage = xpTaskPage1;

    // Page 3
    xpTaskPage3.Title = "Step 3: Review";
    xpTaskPage3.NextPage = null;
    xpTaskPage3.PreviousPage = xpTaskPage2;
}

// Call during initialization
ConfigurePageNavigation();
```

## Alternative Navigation Sequences

### Non-Linear Navigation

Create branching paths where different conditions lead to different pages:

```csharp
private bool userHasExistingAccount = false;

// Setup alternative paths
if (userHasExistingAccount)
{
    // Path for existing users: Welcome → Login → Dashboard
    welcomePage.NextPage = loginPage;
    loginPage.NextPage = dashboardPage;
}
else
{
    // Path for new users: Welcome → SignUp → Profile → Dashboard
    welcomePage.NextPage = signupPage;
    signupPage.NextPage = profilePage;
    profilePage.NextPage = dashboardPage;
}
```

### Skipping Pages Dynamically

```csharp
// User selects "Express Setup" - skip detailed config
if (cbExpressSetup.Checked)
{
    setupPage.NextPage = reviewPage; // Skip config page
}
else
{
    setupPage.NextPage = configPage; // Normal flow
}
```

## Page Order in Collections

The order in the TaskPages array determines:
- Dropdown menu order (pages listed top-to-bottom)
- Collection Editor order
- Page enumeration order

**Set TaskPages in Specific Order:**

```csharp
// Define pages in desired order
xpTaskPane1.TaskPages = new XPTaskPage[]
{
    welcomePage,      // Index 0, appears first in menu
    setupPage,        // Index 1
    advancedPage,     // Index 2
    reviewPage        // Index 3, appears last in menu
};
```

**Reorder at Runtime:**

```csharp
// Current order: [page1, page2, page3]
// Goal: Move page3 to front [page3, page1, page2]

List<XPTaskPage> pages = new List<XPTaskPage>(xpTaskPane1.TaskPages);
XPTaskPage page3 = pages[2];
pages.RemoveAt(2);
pages.Insert(0, page3);
xpTaskPane1.TaskPages = pages.ToArray();

// Dropdown menu order now reflects new array order
```

## Practical Reordering Scenarios

**Scenario 1: Conditional Page Hiding**

```csharp
// Hide advanced pages from new users
if (user.IsBeginnerLevel)
{
    var basicPages = new XPTaskPage[] { page1, page2 };
    xpTaskPane1.TaskPages = basicPages;
}
else
{
    var allPages = new XPTaskPage[] { page1, page2, advancedPage1, advancedPage2 };
    xpTaskPane1.TaskPages = allPages;
}
```

**Scenario 2: Dynamic Page Insertion**

```csharp
// Add additional page based on conditions
if (needsApprovalStep)
{
    List<XPTaskPage> pages = new List<XPTaskPage>(xpTaskPane1.TaskPages);
    pages.Insert(2, approvalPage); // Insert after page 2
    xpTaskPane1.TaskPages = pages.ToArray();
    
    // Update navigation
    page2.NextPage = approvalPage;
    approvalPage.NextPage = page3;
    approvalPage.PreviousPage = page2;
}
```

**Scenario 3: Custom Navigation Logic**

```csharp
// Handle next button click with custom logic
private void HandleNextPage()
{
    XPTaskPage current = xpTaskPane1.SelectedPage;

    if (current == page2)
    {
        // Validate page 2 before proceeding
        if (!ValidatePage2Data())
        {
            MessageBox.Show("Please complete all required fields");
            return;
        }
    }

    // Proceed to next page
    if (current.NextPage != null)
    {
        xpTaskPane1.SelectedPage = current.NextPage;
    }
}
```

## Important Navigation Behaviors

**Behavior 1: Navigation Buttons Respect NextPage/PreviousPage**

```csharp
// If NextPage is null, next button is disabled
xpTaskPage1.NextPage = null;
// Clicking next button on page1 does nothing

// If NextPage points to a page, next button is enabled
xpTaskPage1.NextPage = xpTaskPage2;
// Clicking next button navigates to page2
```

**Behavior 2: Menu Uses TaskPages Order**

```csharp
// TaskPages order determines dropdown menu order
// NextPage/PreviousPage determine arrow button navigation
// These can be independent!

xpTaskPane1.TaskPages = new[] { page1, page2, page3 }; // Menu order
page1.NextPage = page3; // Arrow navigates directly to page3
page3.PreviousPage = page1;
// User can: menu to page2, or arrows to page3 directly
```

## Troubleshooting Navigation

**Issue: Next button disabled unexpectedly**
- Solution: Check NextPage property is set correctly
- Solution: Verify NextPage is not null
- Solution: Check page is in TaskPages collection

**Issue: Pages in wrong menu order**
- Solution: Adjust TaskPages array order
- Solution: Re-assign TaskPages property to refresh

**Issue: Navigation doesn't match expected flow**
- Solution: Verify both NextPage AND PreviousPage are set
- Solution: Check for null references
- Solution: Debug SelectedPage property changes

**Next:** Learn styling and appearance customization in appearance-styling.md
