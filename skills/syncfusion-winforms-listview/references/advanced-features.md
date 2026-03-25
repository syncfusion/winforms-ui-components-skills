# Advanced Features: Localization, UI Automation, and Scrollbar Customization

## Table of Contents
- [Localization](#localization)
  - [Localize at Sample Level](#localize-at-sample-level)
  - [Editing Default Resource File](#editing-default-resource-file)
  - [Localize with Different Assembly](#localize-with-different-assembly)
  - [Right-to-Left Support](#right-to-left-support)
- [UI Automation](#ui-automation)
  - [Coded UI Test Support](#coded-ui-test-support)
  - [Enabling Accessibility](#enabling-accessibility)
  - [Creating Coded UI Tests](#creating-coded-ui-tests)
  - [QTP Testing](#qtp-testing)
- [Scrollbar Customization](#scrollbar-customization)
  - [Auto-Hide Scrollbars](#auto-hide-scrollbars)
  - [Controlling Scrollbar Visibility](#controlling-scrollbar-visibility)
  - [Scroll Increments](#scroll-increments)

---

## Localization

Localization translates application resources into different languages for specific cultures. Customize the ListView to support multiple languages using resource files.

### Localize at Sample Level

To localize SfListView based on `CurrentUICulture`:

**Step 1:** Create a `Resources` folder in your application.

**Step 2:** Add the default resource file `Syncfusion.SfListView.WinForms.resx` to the Resources folder.

Download: [Syncfusion.SfListView.WinForms.resx](https://www.syncfusion.com/downloads/support/directtrac/general/ze/ResourceFile1283641291)

**Step 3:** Add a culture-specific resource file:
- Right-click Resources folder → Add → New Item
- Select Resource File
- Name it: `Syncfusion.SfListView.WinForms.<culture-name>.resx`
- Example: `Syncfusion.SfListView.WinForms.de-DE.resx` for German

**Step 4:** Add Name/Value pairs in the Resource Designer of the culture-specific file. Change values to the corresponding language.

**Step 5:** Set the application culture before `InitializeComponent()`:

```csharp
public Form1()
{
    System.Threading.Thread.CurrentThread.CurrentCulture = 
        new System.Globalization.CultureInfo("de-DE");
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("de-DE");
    InitializeComponent();
}
```

**Supported cultures:**
- `de-DE`: German (Germany)
- `fr-FR`: French (France)
- `es-ES`: Spanish (Spain)
- `ja-JP`: Japanese (Japan)
- `zh-CN`: Chinese (Simplified)
- And more...

### Editing Default Resource File

Edit the default resource file to change static texts without creating culture-specific files:

**Step 1:** Add `Syncfusion.SfListView.WinForms.resx` to the Resources folder.

**Step 2:** Open the Resource Designer and modify Name/Value pairs.

**Step 3:** Run the application. The ListView will use the modified strings.

**Use case:** Customize default English text without localization (e.g., branding changes).

### Localize with Different Assembly

When the resource file is in a different assembly or namespace, use `SR.SetResources()`:

```csharp
public Form1()
{
    System.Threading.Thread.CurrentThread.CurrentCulture = 
        new System.Globalization.CultureInfo("de-DE");
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("de-DE");
    
    // Set resource assembly and namespace
    SR.SetResources(typeof(CustomListView).Assembly, "SfListViewExt");
    
    InitializeComponent();
}
```

**Parameters:**
- First argument: Assembly containing the resource file
- Second argument: Namespace where resource file is located

### Right-to-Left Support

Align items from right to left for RTL languages (Arabic, Hebrew):

```csharp
sfListView1.RightToLeft = RightToLeft.Yes;
```

**RightToLeft Options:**
- `RightToLeft.Yes`: Enable RTL layout
- `RightToLeft.No`: Standard LTR layout (default)
- `RightToLeft.Inherit`: Inherit from parent control

---

## UI Automation

UI Automation provides accessibility to UI elements, enabling automated testing and screen reader support.

### Coded UI Test Support

Coded UI Tests (CUITs) drive your application through automated UI interactions. SfListView supports two levels of CUIT automation:

**Level 1: Record and Playback**
- Recorder identifies elements involved in actions
- Playback processes based on generated code via Microsoft Active Accessibility

**Level 2: Property Validation**
- Default properties defined for each control
- Users can add assertions on properties

**Requirements:**
- Visual Studio Ultimate or Premium
- Supported platforms and configurations: [MSDN Reference](https://learn.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2015/test/supported-configurations-and-platforms-for-coded-ui-tests-and-action-recordings?view=vs-2015)

### Enabling Accessibility

Enable Coded UI support with the `AccessibilityEnabled` property:

```csharp
sfListView1.AccessibilityEnabled = true;
```

This property exposes UI elements to accessibility frameworks and test automation tools.

### Creating Coded UI Tests

**Step 1:** Create or open an application with SfListView.

**Step 2:** Create a Coded UI Test Project. A CUIT file is added automatically.

**Step 3:** In the Generate Code dialog, choose "Record actions, edit UI map or add assertions".

**Step 4:** The CodedUITestBuilder appears in the bottom right corner. Click Start Recording to record actions.

**Step 5:** Perform actions on the ListView application (click items, scroll, filter, etc.).

**Step 6:** Drag the crosshair from CodedUITestBuilder onto UI elements to inspect properties.

**Step 7:** Click GenerateCode to create a test method from recorded actions.

**Step 8:** Create assertions:
- Drag crosshair to an element
- The Assertion window shows available properties
- Select properties and expected values
- Click Generate Code

**Step 9:** Run the test:
- Right-click the test method
- Click "Run Tests"

**Example generated code:**

```csharp
[TestMethod]
public void CodedUITestMethod1()
{
    // To generate code for this test, select "Generate Code for Coded UI Test" 
    // from the shortcut menu and select one of the menu items.
    this.UIMap.ClickListViewItem();
    this.UIMap.AssertItemSelected();
}
```

### QTP Testing

SfListView also supports Quick Test Professional (QTP/UFT) testing. Refer to the [UFT/QTP documentation](https://help.syncfusion.com/windowsforms/testing/uft/supported-controls-and-methods#sflistview) for supported controls and methods.

---

## Scrollbar Customization

Control scrollbar visibility and behavior to customize the scrolling experience.

### Auto-Hide Scrollbars

Automatically show or hide scrollbars based on content overflow:

```csharp
sfListView1.AutoHideScrollBars = true;
```

**Default:** `true`

**Behavior:**
- `true`: Scrollbars appear only when content overflows
- `false`: Scrollbar visibility controlled by `HorizontalScrollBarVisible` and `VerticalScrollBarVisible` properties

### Controlling Scrollbar Visibility

When `AutoHideScrollBars` is `false`, manually control scrollbar visibility:

**Horizontal scrollbar:**
```csharp
sfListView1.AutoHideScrollBars = false;
sfListView1.HorizontalScrollBarVisible = true;
```

**Vertical scrollbar:**
```csharp
sfListView1.AutoHideScrollBars = false;
sfListView1.VerticalScrollBarVisible = true;
```

**Both scrollbars:**
```csharp
sfListView1.AutoHideScrollBars = false;
sfListView1.HorizontalScrollBarVisible = true;
sfListView1.VerticalScrollBarVisible = true;
```

**Use cases:**
- Always show scrollbars for visual consistency
- Hide scrollbars for clean, minimal designs
- Force horizontal scrolling for wide content

### Scroll Increments

Customize mouse wheel scroll sensitivity:

**Horizontal scroll increment:**
```csharp
sfListView1.HorizontalScrollIncrement = 5;
```

**Vertical scroll increment:**
```csharp
sfListView1.VerticalScrollIncrement = 5;
```

**How it works:**
- Each mouse wheel notch scrolls by `ItemHeight * ScrollIncrement` pixels
- Default is typically 1 (one item per scroll)
- Increase for faster scrolling through large lists
- Decrease for finer scroll control

**Example scenarios:**
- Large datasets: Set increment to 5-10 for faster navigation
- Detailed content: Set increment to 1-2 for precise scrolling
- Accessibility: Adjust based on user preferences

---

## Common Scenarios

### Scenario 1: Multilingual Application with German Support
```csharp
public Form1()
{
    // Set German culture
    System.Threading.Thread.CurrentThread.CurrentCulture = 
        new System.Globalization.CultureInfo("de-DE");
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("de-DE");
    
    InitializeComponent();
    
    // Enable RTL if needed (not typically for German)
    // sfListView1.RightToLeft = RightToLeft.No;
}
```

Ensure `Syncfusion.SfListView.WinForms.de-DE.resx` exists in Resources folder with German translations.

### Scenario 2: Automated Testing with Coded UI
```csharp
// Enable accessibility for testing
sfListView1.AccessibilityEnabled = true;

// In your Coded UI test project
[TestMethod]
public void TestListViewSelection()
{
    // Generated code from CodedUITestBuilder
    this.UIMap.SelectFirstItem();
    this.UIMap.AssertItemIsSelected();
}
```

### Scenario 3: Clean UI with Auto-Hide Scrollbars and Fast Scrolling
```csharp
// Auto-hide scrollbars for clean design
sfListView1.AutoHideScrollBars = true;

// Faster scrolling for large dataset
sfListView1.VerticalScrollIncrement = 8;
sfListView1.HorizontalScrollIncrement = 8;
```

### Scenario 4: Arabic Application with RTL Support
```csharp
public Form1()
{
    // Set Arabic culture
    System.Threading.Thread.CurrentThread.CurrentCulture = 
        new System.Globalization.CultureInfo("ar-SA");
    System.Threading.Thread.CurrentThread.CurrentUICulture = 
        new System.Globalization.CultureInfo("ar-SA");
    
    InitializeComponent();
    
    // Enable RTL for Arabic
    sfListView1.RightToLeft = RightToLeft.Yes;
}
```

### Scenario 5: Always-Visible Scrollbars with Custom Increments
```csharp
// Always show both scrollbars
sfListView1.AutoHideScrollBars = false;
sfListView1.HorizontalScrollBarVisible = true;
sfListView1.VerticalScrollBarVisible = true;

// Custom scroll speed
sfListView1.VerticalScrollIncrement = 3;
```

---

## Troubleshooting

**Issue: Localization not working**
- Verify culture-specific resource file is named correctly: `Syncfusion.SfListView.WinForms.<culture>.resx`
- Ensure `CurrentCulture` and `CurrentUICulture` are set BEFORE `InitializeComponent()`
- Check that resource file Build Action is set to "Embedded Resource"
- Confirm Name/Value pairs in resource file match the default resource keys

**Issue: Resource file in different assembly not loading**
- Use `SR.SetResources()` method with correct assembly and namespace
- Verify the assembly reference is correct
- Ensure resource file is marked as "Embedded Resource"

**Issue: RTL layout not applying**
- Confirm `RightToLeft = RightToLeft.Yes` is set
- Check parent form's RightToLeft setting (inherited if set to `Inherit`)
- Verify culture is set to an RTL language (ar-*, he-*, etc.)

**Issue: Coded UI tests not recognizing controls**
- Ensure `AccessibilityEnabled = true` before tests run
- Verify Visual Studio version supports Coded UI (Ultimate/Premium)
- Check that control is visible and loaded before test executes
- Regenerate UI map if control structure changed

**Issue: Scrollbar visibility not changing**
- Confirm `AutoHideScrollBars = false` when manually controlling visibility
- Verify content actually requires scrolling (overflow exists)
- Check that `HorizontalScrollBarVisible` and `VerticalScrollBarVisible` are set correctly

**Issue: Scroll increment not working as expected**
- Ensure `ScrollIncrement` is set to a positive integer
- Check if content is large enough to scroll
- Verify mouse wheel is generating scroll events (test with trackpad vs. mouse)
- Try different increment values (1-10 range typically works best)

**Issue: Accessibility properties not showing in test tools**
- Enable accessibility before launching test tools
- Restart application after setting `AccessibilityEnabled`
- Verify control is in the visual tree and not hidden
- Check that proper .NET framework version is targeted (4.5+)
