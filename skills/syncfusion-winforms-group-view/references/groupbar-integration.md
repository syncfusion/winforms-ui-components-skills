# GroupBar Integration

This guide covers how to integrate GroupView controls with GroupBar to create VS.NET toolbox-style interfaces and Microsoft OutlookBar clones.

## Overview

GroupView controls can be added to GroupBar items as client controls, creating collapsible, categorized navigation panels. This pattern is commonly used to replicate:
- Visual Studio .NET Toolbox interface
- Microsoft Outlook navigation bars
- Adobe Photoshop tool panels
- Multi-category navigation menus

## GroupBar Basics

The **GroupBar** control provides collapsible groups, each capable of hosting a client control. GroupView is the ideal client control for displaying categorized item lists within each group.

### Key Concepts

- **GroupBar**: Container control with collapsible groups
- **GroupBarItem**: Individual group within GroupBar
- **Client Control**: The control displayed within a GroupBarItem (GroupView in this case)

## Basic Integration

### Step 1: Create GroupBar

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create GroupBar instance
GroupBar groupBar1 = new GroupBar();
groupBar1.Location = new Point(10, 10);
groupBar1.Size = new Size(250, 500);
groupBar1.BorderStyle = BorderStyle.FixedSingle;
```

### Step 2: Create GroupBarItems

```csharp
// Create GroupBar items (groups)
GroupBarItem groupBarItem1 = new GroupBarItem();
groupBarItem1.Text = "Windows Forms";

GroupBarItem groupBarItem2 = new GroupBarItem();
groupBarItem2.Text = "Components";

GroupBarItem groupBarItem3 = new GroupBarItem();
groupBarItem3.Text = "Data";

// Add GroupBarItems to GroupBar
this.groupBar1.GroupBarItems.AddRange(new GroupBarItem[] {
    groupBarItem1,
    groupBarItem2,
    groupBarItem3
});
```

### Step 3: Create GroupView for Each Item

```csharp
// Create GroupView for first GroupBarItem
GroupView groupView1 = new GroupView();
groupView1.Dock = DockStyle.Fill; // Fill the GroupBarItem area
groupView1.FlatLook = true;
groupView1.BorderStyle = BorderStyle.None;
groupView1.IntegratedScrolling = true;
groupView1.SmallImageView = true;

// Add items to GroupView
groupView1.GroupViewItems.AddRange(new GroupViewItem[] {
    new GroupViewItem("Button", 0, true, "Add button control", "item1"),
    new GroupViewItem("TextBox", 1, true, "Add textbox control", "item2"),
    new GroupViewItem("Label", 2, true, "Add label control", "item3"),
    new GroupViewItem("CheckBox", 3, true, "Add checkbox control", "item4")
});
```

### Step 4: Attach GroupView to GroupBarItem

```csharp
// Set GroupView as the client control of GroupBarItem
groupBarItem1.Client = groupView1;
```

### Step 5: Add GroupBar to Form

```csharp
// Add GroupBar to form
this.Controls.Add(this.groupBar1);
```

## Complete Integration Example

Full implementation with three groups containing different GroupView controls:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace GroupBarDemo
{
    public partial class ToolboxClone : Form
    {
        private GroupBar groupBar1;
        private ImageList imageList1;
        
        public ToolboxClone()
        {
            InitializeComponent();
            CreateToolboxInterface();
        }
        
        private void CreateToolboxInterface()
        {
            // Create ImageList
            this.imageList1 = new ImageList();
            this.imageList1.ImageSize = new Size(16, 16);
            // Load images (ensure these files exist)
            LoadImages();
            
            // Create GroupBar
            this.groupBar1 = new GroupBar();
            this.groupBar1.Location = new Point(10, 10);
            this.groupBar1.Size = new Size(250, 500);
            this.groupBar1.BorderStyle = BorderStyle.FixedSingle;
            
            // Create and configure groups
            CreateWindowsFormsGroup();
            CreateComponentsGroup();
            CreateDataGroup();
            
            // Add GroupBar to form
            this.Controls.Add(this.groupBar1);
        }
        
        private void CreateWindowsFormsGroup()
        {
            // Create GroupBarItem
            GroupBarItem groupItem = new GroupBarItem();
            groupItem.Text = "Windows Forms";
            
            // Create GroupView
            GroupView groupView = new GroupView();
            ConfigureGroupView(groupView);
            
            // Add items
            groupView.GroupViewItems.AddRange(new GroupViewItem[] {
                new GroupViewItem("Pointer", 0, true, "Selection tool", "pointer"),
                new GroupViewItem("Button", 1, true, "Add button", "button"),
                new GroupViewItem("CheckBox", 2, true, "Add checkbox", "checkbox"),
                new GroupViewItem("RadioButton", 3, true, "Add radio button", "radio"),
                new GroupViewItem("TextBox", 4, true, "Add textbox", "textbox"),
                new GroupViewItem("Label", 5, true, "Add label", "label"),
                new GroupViewItem("Panel", 6, true, "Add panel", "panel")
            });
            
            // Attach to GroupBarItem
            groupItem.Client = groupView;
            
            // Handle item selection
            groupView.GroupViewItemSelected += (sender, e) =>
            {
                int index = groupView.SelectedItem;
                if (index >= 0)
                {
                    string tool = groupView.GroupViewItems[index].Text;
                    this.Text = $"Toolbox - Selected: {tool}";
                }
            };
            
            // Add to GroupBar
            this.groupBar1.GroupBarItems.Add(groupItem);
        }
        
        private void CreateComponentsGroup()
        {
            // Create GroupBarItem
            GroupBarItem groupItem = new GroupBarItem();
            groupItem.Text = "Components";
            
            // Create GroupView
            GroupView groupView = new GroupView();
            ConfigureGroupView(groupView);
            
            // Add items
            groupView.GroupViewItems.AddRange(new GroupViewItem[] {
                new GroupViewItem("Timer", 7, true, "Add timer component", "timer"),
                new GroupViewItem("ImageList", 8, true, "Add image list", "imagelist"),
                new GroupViewItem("ToolTip", 9, true, "Add tooltip", "tooltip"),
                new GroupViewItem("ContextMenu", 10, true, "Add context menu", "contextmenu")
            });
            
            // Attach to GroupBarItem
            groupItem.Client = groupView;
            
            // Add to GroupBar
            this.groupBar1.GroupBarItems.Add(groupItem);
        }
        
        private void CreateDataGroup()
        {
            // Create GroupBarItem
            GroupBarItem groupItem = new GroupBarItem();
            groupItem.Text = "Data";
            
            // Create GroupView
            GroupView groupView = new GroupView();
            ConfigureGroupView(groupView);
            
            // Add items
            groupView.GroupViewItems.AddRange(new GroupViewItem[] {
                new GroupViewItem("DataSet", 11, true, "Add dataset", "dataset"),
                new GroupViewItem("DataGridView", 12, true, "Add datagrid", "datagrid"),
                new GroupViewItem("BindingSource", 13, true, "Add binding source", "binding")
            });
            
            // Attach to GroupBarItem
            groupItem.Client = groupView;
            
            // Add to GroupBar
            this.groupBar1.GroupBarItems.Add(groupItem);
        }
        
        private void ConfigureGroupView(GroupView groupView)
        {
            // Standard configuration for toolbox-style GroupView
            groupView.Dock = DockStyle.Fill;
            groupView.FlatLook = true;
            groupView.BorderStyle = BorderStyle.None;
            groupView.IntegratedScrolling = true;
            groupView.FlowView = true;
            groupView.ShowFlowViewItemText = true;
            groupView.FlowViewItemTextLength = 30;
            groupView.SmallImageView = true;
            groupView.SmallImageList = this.imageList1;
            
            // Enable highlighting
            groupView.HighlightText = true;
            groupView.HighlightImage = true;
            groupView.HighlightItemColor = Color.FromArgb(229, 243, 255);
            groupView.HighlightTextColor = Color.Black;
            
            // Selection colors
            groupView.SelectedItemColor = Color.FromArgb(0, 120, 215);
            groupView.SelectedTextColor = Color.White;
            
            // Spacing
            groupView.ItemXSpacing = 4;
            groupView.ItemYSpacing = 4;
        }
        
        private void LoadImages()
        {
            // Load images for toolbox items
            // This is a placeholder - add actual image loading logic
            try
            {
                for (int i = 0; i < 14; i++)
                {
                    this.imageList1.Images.Add(CreatePlaceholderImage());
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Error loading images: {ex.Message}");
            }
        }
        
        private Image CreatePlaceholderImage()
        {
            // Create a simple placeholder image
            Bitmap bmp = new Bitmap(16, 16);
            using (Graphics g = Graphics.FromImage(bmp))
            {
                g.Clear(Color.LightGray);
                g.DrawRectangle(Pens.DarkGray, 0, 0, 15, 15);
            }
            return bmp;
        }
    }
}
```

## VS.NET Toolbox Configuration

Specific settings to replicate the Visual Studio toolbox appearance and behavior:

```csharp
public void ConfigureVSToolboxStyle(GroupView groupView)
{
    // Visual appearance
    groupView.Dock = DockStyle.Fill;
    groupView.FlatLook = true;
    groupView.BorderStyle = BorderStyle.None;
    groupView.BackColor = Color.FromArgb(245, 245, 245);
    
    // Layout
    groupView.FlowView = true;
    groupView.ShowFlowViewItemText = true;
    groupView.FlowViewItemTextLength = 40;
    groupView.Orientation = GroupViewOrientation.Vertical;
    
    // Scrolling
    groupView.IntegratedScrolling = true;
    
    // Images
    groupView.SmallImageView = true;
    // groupView.SmallImageList = your16x16ImageList;
    
    // Highlighting (subtle, like VS.NET)
    groupView.HighlightText = true;
    groupView.HighlightImage = true;
    groupView.HighlightItemColor = Color.FromArgb(229, 243, 255);
    groupView.HighlightTextColor = Color.Black;
    
    // Selection (blue highlight like VS.NET)
    groupView.SelectedItemColor = Color.FromArgb(51, 153, 255);
    groupView.SelectedTextColor = Color.White;
    
    // Spacing (tight, like VS.NET)
    groupView.ItemXSpacing = 2;
    groupView.ItemYSpacing = 2;
    
    // Text formatting
    groupView.TextWrap = false;
    groupView.TextUnderline = false;
}
```

## Outlook-Style Navigation Bar

Configuration for Microsoft Outlook-style navigation:

```csharp
public void ConfigureOutlookStyle()
{
    this.groupBar1 = new GroupBar();
    this.groupBar1.VisualStyle = VisualStyle.Office2007Blue;
    
    // Create Mail group
    GroupBarItem mailGroup = new GroupBarItem { Text = "Mail" };
    GroupView mailView = new GroupView
    {
        Dock = DockStyle.Fill,
        FlatLook = false,
        FlowView = true,
        SmallImageView = false
    };
    mailView.GroupViewItems.AddRange(new GroupViewItem[] {
        new GroupViewItem("Inbox", 0, true, "View inbox", "inbox"),
        new GroupViewItem("Sent Items", 1, true, "View sent", "sent"),
        new GroupViewItem("Drafts", 2, true, "View drafts", "drafts")
    });
    mailGroup.Client = mailView;
    this.groupBar1.GroupBarItems.Add(mailGroup);
    
    // Create Calendar group
    GroupBarItem calendarGroup = new GroupBarItem { Text = "Calendar" };
    GroupView calendarView = new GroupView { Dock = DockStyle.Fill };
    calendarView.GroupViewItems.AddRange(new GroupViewItem[] {
        new GroupViewItem("Today", 0), new GroupViewItem("Week", 1)
    });
    calendarGroup.Client = calendarView;
    this.groupBar1.GroupBarItems.Add(calendarGroup);
}
```

## Multi-Group Coordination

Coordinate multiple GroupView controls within a single GroupBar:

```csharp
public class CoordinatedGroupBar : Form
{
    private GroupBar groupBar1;
    private string selectedTool = null;
    
    public CoordinatedGroupBar()
    {
        InitializeComponent();
        SetupCoordinatedGroups();
    }
    
    private void SetupCoordinatedGroups()
    {
        this.groupBar1 = new GroupBar();
        this.groupBar1.Location = new Point(10, 10);
        this.groupBar1.Size = new Size(250, 500);
        
        // Create multiple groups
        AddGroup("Controls", new string[] { "Button", "TextBox", "Label" });
        AddGroup("Containers", new string[] { "Panel", "GroupBox", "TabControl" });
        AddGroup("Menus", new string[] { "MenuStrip", "ToolStrip", "StatusStrip" });
        
        this.Controls.Add(this.groupBar1);
    }
    
    private void AddGroup(string groupName, string[] items)
    {
        GroupBarItem groupItem = new GroupBarItem();
        groupItem.Text = groupName;
        
        GroupView groupView = new GroupView();
        groupView.Dock = DockStyle.Fill;
        groupView.FlatLook = true;
        groupView.BorderStyle = BorderStyle.None;
        groupView.IntegratedScrolling = true;
        
        // Add items
        for (int i = 0; i < items.Length; i++)
        {
            groupView.GroupViewItems.Add(
                new GroupViewItem(items[i], -1, true, $"Add {items[i]}", $"item{i}")
            );
        }
        
        // Coordinate selection across all groups
        groupView.GroupViewItemSelected += (sender, e) =>
        {
            int index = groupView.SelectedItem;
            if (index >= 0)
            {
                selectedTool = groupView.GroupViewItems[index].Text;
                UpdateStatusBar($"Selected: {selectedTool} from {groupName}");
                
                // Deselect items in other groups
                DeselectOtherGroups(groupView);
            }
        };
        
        groupItem.Client = groupView;
        this.groupBar1.GroupBarItems.Add(groupItem);
    }
    
    private void DeselectOtherGroups(GroupView currentGroupView)
    {
        foreach (GroupBarItem groupItem in this.groupBar1.GroupBarItems)
        {
            if (groupItem.Client is GroupView gv && gv != currentGroupView)
            {
                gv.SelectedItem = -1; // Deselect
            }
        }
    }
    
    private void UpdateStatusBar(string message)
    {
        this.Text = message;
    }
}
```

## Best Practices

### GroupBar Configuration
- **Set GroupView.Dock = DockStyle.Fill** to fill the GroupBarItem area
- **Use GroupView.BorderStyle = BorderStyle.None** to avoid double borders
- **Enable IntegratedScrolling** for groups with many items
- **Configure consistent styling** across all GroupView controls in the GroupBar

### Performance Optimization
- **Create GroupView controls on demand** rather than all at initialization
- **Dispose unused GroupView controls** when groups are collapsed or removed
- **Use FlowView mode** for large item counts to improve rendering performance
- **Limit the number of visible groups** to 5-7 for optimal user experience

### User Experience
- **Provide visual feedback** for selected items across all groups
- **Maintain selection state** when switching between groups
- **Use consistent icons** and terminology across groups
- **Enable tooltips** for all items to provide helpful descriptions
- **Handle double-click** to execute default actions on items

### Integration Patterns
- **One GroupView per GroupBarItem** - Standard pattern
- **Coordinate selections** across multiple groups if needed
- **Share ImageLists** across GroupView controls for consistency
- **Implement drag-and-drop** from GroupView to design surface for toolbox functionality
- **Persist group expand/collapse state** for better user experience
