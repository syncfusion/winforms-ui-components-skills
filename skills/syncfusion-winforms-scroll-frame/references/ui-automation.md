# UI Automation and Testing

## Table of Contents
- [Overview](#overview)
- [Coded UI Test (CUIT)](#coded-ui-test-cuit)
  - [Automation Levels](#automation-levels)
  - [Requirements and Configuration](#requirements-and-configuration)
  - [Getting Started with Coded UI](#getting-started-with-coded-ui)
  - [Recording Actions](#recording-actions)
  - [Adding Assertions](#adding-assertions)
  - [Running Tests](#running-tests)
- [Quick Test Professional (QTP/UFT)](#quick-test-professional-qtpuft)
- [Complete Testing Example](#complete-testing-example)

## Overview

Microsoft UI Automation is an accessibility framework for Windows that enables automated testing and accessibility tools. SfScrollFrame provides full support for UI automation, allowing:

- **Automated testing** through Coded UI Tests (CUIT)
- **QTP/UFT testing** for enterprise test automation
- **Accessibility tools** to interact with scrollbar elements
- **Property validation** for test assertions

UI Automation exposes information about UI elements to assistive technologies and automated test scripts, enabling interaction with the application without manual user input.

**Learn More:** [Microsoft UI Automation Overview](https://learn.microsoft.com/en-us/dotnet/framework/ui-automation/ui-automation-overview)

## Coded UI Test (CUIT)

Coded UI Tests (CUITs) are automated tests that drive your application through its user interface. They enable functional testing of UI controls, record user actions, and verify expected behavior through assertions.

### Automation Levels

SfScrollFrame supports two levels of Coded UI automation:

| Level | Description |
|-------|-------------|
| **Level 1** | **Record and Playback**<br>Recorder identifies elements involved in actions (clicks, scrolls, etc.)<br>Playback processes actions based on generated code via Microsoft Active Accessibility (MSAA) |
| **Level 2** | **Property Validation**<br>Defines default properties based on MSAA control type<br>Users can add assertions to verify property values<br>Enables validation of scrollbar state, position, and appearance |

### Requirements and Configuration

#### Supported Visual Studio Versions

Coded UI Test support is available in:
- Visual Studio Ultimate
- Visual Studio Premium

**Not available in:** Visual Studio Community, Professional, Express editions

#### Platform and Configuration Support

For detailed information about supported platforms and configurations, see:
[Supported Configurations and Platforms for Coded UI Tests](https://learn.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2015/test/supported-configurations-and-platforms-for-coded-ui-tests-and-action-recordings?view=vs-2015)

#### Prerequisites

- Windows Forms application with SfScrollFrame
- Visual Studio Ultimate or Premium
- Application must be running to record actions

### Getting Started with Coded UI

#### Step 1: Create or Open Application

Open an existing application that uses SfScrollFrame, or create a new one:

```csharp
public partial class MainForm : Form
{
    private SfScrollFrame sfScrollFrame1;
    private ListView listView1;

    public MainForm()
    {
        InitializeComponent();
        
        // Setup ListView
        listView1 = new ListView { View = View.Details, Size = new Size(400, 300) };
        listView1.Columns.Add("Item", 200);
        listView1.Columns.Add("Value", 180);
        
        for (int i = 0; i < 100; i++)
        {
            listView1.Items.Add(new ListViewItem(new[] { $"Item {i}", $"Value {i}" }));
        }
        
        // Attach SfScrollFrame
        sfScrollFrame1 = new SfScrollFrame();
        sfScrollFrame1.Control = listView1;
        
        this.Controls.Add(listView1);
    }
}
```

#### Step 2: Create Coded UI Test Project

1. **In Visual Studio:**
   - Right-click solution in Solution Explorer
   - Select Add → New Project
   - Choose "Test" category
   - Select "Coded UI Test Project"
   - Name: `SfScrollFrameUITests`
   - Click OK

2. **Generate Code Dialog Appears:**
   - Choose "Record actions, edit UI map or add assertions"
   - Click OK

3. **CodedUITestBuilder appears** in the bottom-right corner of the screen

#### Step 3: Alternative - Add to Existing Test Project

If you already have a Coded UI Test project:

1. Right-click the test method (`CodedUITestMethod1`)
2. Select "Generate Code for Coded UI Test"
3. CodedUITestBuilder window appears

### Recording Actions

The CodedUITestBuilder allows you to record user interactions with your application.

#### Start Recording

1. **Launch CodedUITestBuilder** (appears bottom-right of screen)
2. **Start your application** (the one containing SfScrollFrame)
3. **Click the Record button** (red circle) in CodedUITestBuilder
4. **Perform actions** on your application:
   - Click scrollbar arrow buttons
   - Drag scrollbar thumb
   - Click on scrollbar track
   - Right-click for context menu
5. **Stop recording** when finished

#### CodedUITestBuilder Interface

**Buttons:**
- **Record** (Red circle) - Start/stop recording actions
- **Generate Code** (Diskette icon) - Save recorded actions as code
- **Show Recorded Steps** - View list of recorded actions
- **Add Assertions** (Crosshair icon) - Drag to element to add validations

#### Recording Example Actions

**Action 1: Scroll down using arrow button**
1. Start recording
2. Click the down arrow on vertical scrollbar 5 times
3. Stop recording
4. Click "Generate Code"
5. Enter method name: `ScrollDownFiveTimes`
6. Click OK

**Action 2: Scroll to middle by dragging thumb**
1. Start recording
2. Click and drag vertical scrollbar thumb to middle position
3. Release mouse
4. Stop recording
5. Generate code: `ScrollToMiddle`

**Action 3: Use context menu**
1. Start recording
2. Right-click on scrollbar
3. Click "Bottom" in context menu
4. Stop recording
5. Generate code: `ScrollToBottomViaMenu`

### Adding Assertions

Assertions verify that the application is in the expected state after actions are performed.

#### Step 1: Drag Crosshair to Element

1. Click the **crosshair icon** in CodedUITestBuilder
2. Drag the crosshair to the scrollbar element
3. Release over the scrollbar thumb or track

#### Step 2: View Properties

The **UI Control Map** window appears showing:
- Available properties of the scrollbar
- Current values
- Property types

**Example Properties:**
- `Value` - Current scroll position
- `Maximum` - Maximum scroll value
- `Minimum` - Minimum scroll value
- `Enabled` - Whether scrollbar is enabled
- `Visible` - Whether scrollbar is visible

#### Step 3: Add Assertion

1. In UI Control Map, select the property to verify
2. Click "Add Assertion"
3. Choose comparison method:
   - `AreEqual` - Exact match
   - `Contains` - Partial match
   - `Greater than`, `Less than` - Numeric comparison
4. Enter expected value
5. Click OK
6. Generate code for the assertion

#### Assertion Example

```csharp
// Generated assertion code
public void AssertScrollPositionAtBottom()
{
    // Get scrollbar
    WinControl verticalScrollBar = this.UIMainFormWindow.UIVerticalScrollBar;
    
    // Assert Value equals Maximum
    Assert.AreEqual(
        this.AssertScrollPositionAtBottomExpectedValues.UIVerticalScrollBarValue,
        verticalScrollBar.Value,
        "Scrollbar should be at maximum position"
    );
}
```

#### Adding Assertion During Recording

1. **Record actions** that change scroll position
2. **Pause recording**
3. **Drag crosshair** to scrollbar element
4. **Verify property value** in UI Control Map
5. **Add assertion** for expected value
6. **Continue or stop recording**
7. **Generate code**

### Running Tests

#### Run Test from Test Explorer

1. **Open Test Explorer:**
   - Menu: Test → Windows → Test Explorer
   - Or: Ctrl+E, T

2. **Find your test:**
   - Tests appear under test project name
   - Example: `CodedUITestMethod1`

3. **Run test:**
   - Right-click test name
   - Select "Run Tests"
   - Or click "Run All" button

#### Run Test from Code

1. **Right-click test method** in code editor
2. Select "Run Tests"
3. Test executes automatically

#### Test Execution

When running:
1. Application launches automatically
2. Recorded actions replay
3. Assertions validate
4. Results appear in Test Explorer

**Test Results:**
- ✅ **Passed** - All actions and assertions succeeded
- ❌ **Failed** - Action failed or assertion didn't match
- ⚠️ **Inconclusive** - Test didn't complete

#### View Test Results

In Test Explorer:
- **Green checkmark** - Test passed
- **Red X** - Test failed
- Click test to see **output and error messages**
- **Duration** shown for each test

### Complete Coded UI Example

Here's a complete test class with multiple test methods:

```csharp
using System;
using Microsoft.VisualStudio.TestTools.UITesting;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Microsoft.VisualStudio.TestTools.UITesting.WinControls;

namespace SfScrollFrameUITests
{
    [CodedUITest]
    public class ScrollFrameTests
    {
        [TestMethod]
        public void TestScrollToBottom()
        {
            // Launch application
            ApplicationUnderTest app = ApplicationUnderTest.Launch(@"C:\Path\To\MyApp.exe");
            
            // Get scrollbar control
            WinWindow mainWindow = new WinWindow();
            mainWindow.SearchProperties[WinWindow.PropertyNames.Name] = "MainForm";
            
            WinControl scrollBar = new WinControl(mainWindow);
            scrollBar.SearchProperties[WinControl.PropertyNames.ControlType] = "ScrollBar";
            scrollBar.SearchProperties[WinControl.PropertyNames.Name] = "VerticalScrollBar";
            
            // Get initial position
            int initialValue = Convert.ToInt32(scrollBar.GetProperty("Value"));
            
            // Click down arrow 10 times
            WinButton downButton = new WinButton(scrollBar);
            downButton.SearchProperties[WinButton.PropertyNames.Name] = "Down";
            for (int i = 0; i < 10; i++)
            {
                Mouse.Click(downButton);
                Playback.Wait(100);
            }
            
            // Verify scrollbar moved
            int finalValue = Convert.ToInt32(scrollBar.GetProperty("Value"));
            Assert.IsTrue(finalValue > initialValue, "Scrollbar should have scrolled down");
            
            // Close application
            app.Close();
        }
        
        [TestMethod]
        public void TestScrollToTopUsingContextMenu()
        {
            // Launch application
            ApplicationUnderTest app = ApplicationUnderTest.Launch(@"C:\Path\To\MyApp.exe");
            
            // Get scrollbar
            WinWindow mainWindow = new WinWindow();
            mainWindow.SearchProperties[WinWindow.PropertyNames.Name] = "MainForm";
            
            WinControl scrollBar = new WinControl(mainWindow);
            scrollBar.SearchProperties[WinControl.PropertyNames.ControlType] = "ScrollBar";
            
            // Scroll to middle first
            int maxValue = Convert.ToInt32(scrollBar.GetProperty("Maximum"));
            scrollBar.SetProperty("Value", maxValue / 2);
            
            // Right-click to show context menu
            Mouse.Click(scrollBar, MouseButtons.Right);
            Playback.Wait(500);
            
            // Click "Top" menu item
            WinMenuItem topItem = new WinMenuItem(mainWindow);
            topItem.SearchProperties[WinMenuItem.PropertyNames.Name] = "Top";
            Mouse.Click(topItem);
            Playback.Wait(500);
            
            // Verify scrollbar is at top
            int currentValue = Convert.ToInt32(scrollBar.GetProperty("Value"));
            int minValue = Convert.ToInt32(scrollBar.GetProperty("Minimum"));
            Assert.AreEqual(minValue, currentValue, "Scrollbar should be at minimum position");
            
            // Close application
            app.Close();
        }
        
        [TestMethod]
        public void TestThumbDrag()
        {
            // Launch application
            ApplicationUnderTest app = ApplicationUnderTest.Launch(@"C:\Path\To\MyApp.exe");
            
            // Get scrollbar thumb
            WinWindow mainWindow = new WinWindow();
            mainWindow.SearchProperties[WinWindow.PropertyNames.Name] = "MainForm";
            
            WinControl thumb = new WinControl(mainWindow);
            thumb.SearchProperties[WinControl.PropertyNames.ControlType] = "Thumb";
            
            // Get initial position
            Point initialPosition = new Point(thumb.BoundingRectangle.X, thumb.BoundingRectangle.Y);
            
            // Drag thumb down 100 pixels
            Mouse.StartDragging(thumb);
            Mouse.StopDragging(initialPosition.X, initialPosition.Y + 100);
            Playback.Wait(500);
            
            // Verify thumb moved
            Point finalPosition = new Point(thumb.BoundingRectangle.X, thumb.BoundingRectangle.Y);
            Assert.IsTrue(finalPosition.Y > initialPosition.Y, "Thumb should have moved down");
            
            // Close application
            app.Close();
        }
        
        #region Additional test support
        
        public TestContext TestContext { get; set; }
        
        [TestInitialize()]
        public void TestInitialize()
        {
            // Runs before each test
            Playback.PlaybackSettings.WaitForReadyLevel = WaitForReadyLevel.AllThreads;
        }
        
        [TestCleanup()]
        public void TestCleanup()
        {
            // Runs after each test
        }
        
        #endregion
    }
}
```

## Quick Test Professional (QTP/UFT)

SfScrollFrame also supports testing with HP's Quick Test Professional (QTP) and Unified Functional Testing (UFT).

### QTP/UFT Support

For detailed information on testing SfScrollFrame with QTP/UFT, refer to:

**Documentation:** [UFT/QTP Testing - SfScrollFrame](https://help.syncfusion.com/windowsforms/testing/uft/supported-controls-and-methods#sfscrollframe)

### Supported Operations

- Identify SfScrollFrame controls
- Get and set scrollbar properties
- Perform scroll actions
- Verify scrollbar state

### Basic QTP Example

```vbscript
' Get SfScrollFrame control
Set scrollFrame = Window("MainForm").WinObject("sfScrollFrame1")

' Get vertical scrollbar value
currentValue = scrollFrame.GetProperty("VerticalScrollBar.Value")

' Set scroll position
scrollFrame.SetProperty "VerticalScrollBar.Value", 100

' Verify scroll position
expectedValue = 100
actualValue = scrollFrame.GetProperty("VerticalScrollBar.Value")
If actualValue = expectedValue Then
    Reporter.ReportEvent micPass, "Scroll Position", "Value is correct"
Else
    Reporter.ReportEvent micFail, "Scroll Position", "Expected " & expectedValue & " but got " & actualValue
End If
```

## Complete Testing Example

Here's a comprehensive testing strategy for SfScrollFrame:

### Test Plan

**Test Suite: SfScrollFrame Functionality**

1. **Test Case 1: Arrow Button Clicks**
   - Verify down arrow scrolls down
   - Verify up arrow scrolls up
   - Verify scroll amount matches SmallChange

2. **Test Case 2: Thumb Dragging**
   - Verify thumb can be dragged to any position
   - Verify scroll position updates during drag
   - Verify thumb returns to correct position

3. **Test Case 3: Context Menu**
   - Verify right-click shows menu
   - Verify "Top" menu item scrolls to minimum
   - Verify "Bottom" menu item scrolls to maximum

4. **Test Case 4: Programmatic Scrolling**
   - Set Value property and verify scroll position
   - Verify Value cannot exceed Maximum
   - Verify Value cannot go below Minimum

5. **Test Case 5: Theme Changes**
   - Verify scrollbar appearance updates with theme
   - Verify functionality maintained after theme change

### Automated Test Implementation

```csharp
[TestClass]
public class SfScrollFrameAutomatedTests
{
    private const string AppPath = @"C:\MyApp\MyApp.exe";
    
    [TestMethod]
    public void VerifyArrowButtonScrolling()
    {
        using (var app = ApplicationUnderTest.Launch(AppPath))
        {
            var scrollBar = GetVerticalScrollBar();
            int initial = GetScrollValue(scrollBar);
            
            // Click down arrow
            ClickDownArrow(scrollBar);
            Playback.Wait(200);
            
            int after = GetScrollValue(scrollBar);
            Assert.IsTrue(after > initial, "Scroll value should increase after clicking down arrow");
        }
    }
    
    [TestMethod]
    public void VerifyThumbDragging()
    {
        using (var app = ApplicationUnderTest.Launch(AppPath))
        {
            var thumb = GetScrollBarThumb();
            Point start = GetThumbPosition(thumb);
            
            // Drag thumb down
            Mouse.StartDragging(thumb);
            Mouse.StopDragging(start.X, start.Y + 50);
            Playback.Wait(500);
            
            Point end = GetThumbPosition(thumb);
            Assert.IsTrue(end.Y > start.Y, "Thumb should move down when dragged down");
        }
    }
    
    [TestMethod]
    public void VerifyContextMenuScrollToTop()
    {
        using (var app = ApplicationUnderTest.Launch(AppPath))
        {
            var scrollBar = GetVerticalScrollBar();
            
            // Scroll to middle first
            SetScrollValue(scrollBar, 50);
            Playback.Wait(200);
            
            // Use context menu
            Mouse.Click(scrollBar, MouseButtons.Right);
            Playback.Wait(300);
            
            WinMenuItem topItem = new WinMenuItem();
            topItem.SearchProperties["Name"] = "Top";
            Mouse.Click(topItem);
            Playback.Wait(500);
            
            // Verify at top
            int value = GetScrollValue(scrollBar);
            Assert.AreEqual(0, value, "Scrollbar should be at position 0 after clicking 'Top'");
        }
    }
    
    // Helper methods
    private WinControl GetVerticalScrollBar()
    {
        WinWindow window = new WinWindow();
        window.SearchProperties["Name"] = "MainForm";
        
        WinControl scrollBar = new WinControl(window);
        scrollBar.SearchProperties["ControlType"] = "ScrollBar";
        scrollBar.SearchProperties["Orientation"] = "Vertical";
        return scrollBar;
    }
    
    private int GetScrollValue(WinControl scrollBar)
    {
        return Convert.ToInt32(scrollBar.GetProperty("Value"));
    }
    
    private void SetScrollValue(WinControl scrollBar, int value)
    {
        scrollBar.SetProperty("Value", value);
    }
    
    private void ClickDownArrow(WinControl scrollBar)
    {
        WinButton button = new WinButton(scrollBar);
        button.SearchProperties["Name"] = "Down";
        Mouse.Click(button);
    }
    
    private WinControl GetScrollBarThumb()
    {
        WinWindow window = new WinWindow();
        window.SearchProperties["Name"] = "MainForm";
        
        WinControl thumb = new WinControl(window);
        thumb.SearchProperties["ControlType"] = "Thumb";
        return thumb;
    }
    
    private Point GetThumbPosition(WinControl thumb)
    {
        return new Point(thumb.BoundingRectangle.X, thumb.BoundingRectangle.Y);
    }
}
```

## Best Practices

1. **Wait for UI:** Use `Playback.Wait()` after actions to ensure UI updates
2. **Specific searches:** Use detailed search properties to find correct controls
3. **Clean state:** Reset application to known state before each test
4. **Independent tests:** Each test should be runnable independently
5. **Clear assertions:** Use descriptive assertion messages
6. **Handle failures:** Include appropriate error handling and cleanup
7. **Test data:** Use consistent, predictable test data
8. **Documentation:** Document test purpose, expected results, and prerequisites
