---
name: syncfusion-winforms-xptaskpane
description: Guide to implementing Syncfusion XPTaskPane component for Windows Forms. Use when user needs to create Microsoft Office XP-style task pane layouts, manage multiple task pages, configure navigation headers, reorder pages, or customize appearance. Covers container setup, page management, header controls, scrolling, and styling.
metadata:
  author: Syncfusion Inc
  version: "33.1.44"
---

# Implementing Syncfusion XPTaskPane

The XPTaskPane component is a Microsoft Office XP-inspired container control that hosts multiple task pages with built-in navigation and design-time support. Use it when you need a professional-looking multi-page layout with page browsing via dropdown menu and navigation buttons.

## When to Use XPTaskPane

Use XPTaskPane when you need to:
- Create multi-page layouts inspired by Office XP UI
- Provide navigation between multiple content panels
- Allow users to browse pages via dropdown menu or arrow buttons
- Build collapsible task pane interfaces
- Organize related functionality into distinct pages
- Provide design-time drag-and-drop support for pages

## Component Overview

XPTaskPane consists of:
- **XPTaskPane** - Main container control
- **WizardContainer** - Page container (automatically added)
- **XPTaskPage** - Individual pages displayed in the pane
- **Header** - Navigation toolbar with buttons, menu, and close icon

```
XPTaskPane (main control)
├── HeaderLeftToolbar (previous/next buttons)
├── HeaderRightToolbar (menu dropdown + close button)
└── TaskPanePageContainer (WizardContainer)
    ├── XPTaskPage 1
    ├── XPTaskPage 2
    └── XPTaskPage N
```

## Documentation Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Assembly dependencies and NuGet installation
- Adding XPTaskPane via Form Designer
- Adding XPTaskPane programmatically with code
- Creating WizardContainer and XPTaskPage instances
- Basic initialization steps

### Page Management
📄 **Read:** [references/page-management.md](references/page-management.md)
- Creating and adding XPTaskPage instances
- Configuring page title and layout name
- Adding controls to task pages
- Accessing the TaskPages collection
- SelectedPage property usage

### Navigation & Header Controls
📄 **Read:** [references/navigation-header.md](references/navigation-header.md)
- Header toolbar components and items
- Navigation button visibility and functionality
- Dropdown menu for page selection
- Header menu customization
- Close button configuration
- Custom images for toolbar buttons

### Page Reordering & Navigation
📄 **Read:** [references/page-reordering.md](references/page-reordering.md)
- Reordering pages at design-time
- NextPage and PreviousPage properties
- Sequential page navigation at runtime
- Bring to Front and Send to Back operations
- Page ordering in collection editor

### Appearance & Styling
📄 **Read:** [references/appearance-styling.md](references/appearance-styling.md)
- Visual styles (OfficeXP and Office2007)
- Font and foreground color customization
- Border styles and border properties
- Header label styling
- Page-level appearance configuration
- Custom colors and color schemes

### Content Scrolling
📄 **Read:** [references/scrolling-content.md](references/scrolling-content.md)
- Enabling vertical scrolling on pages
- ScrollSpeed property configuration
- Scroll behavior on mouse hover
- Content overflow handling

---

## Quick Start Example

```csharp
using Syncfusion.Windows.Forms.Tools;

// Step 1: Create XPTaskPane
XPTaskPane xpTaskPane1 = new XPTaskPane();
this.Controls.Add(xpTaskPane1);

// Step 2: Create and add WizardContainer
WizardContainer wizardContainer1 = new WizardContainer();
xpTaskPane1.Controls.Add(wizardContainer1);
xpTaskPane1.TaskPanePageContainer = wizardContainer1;

// Step 3: Create task pages
XPTaskPage page1 = new XPTaskPage();
page1.Title = "Page 1";
wizardContainer1.Controls.Add(page1);

XPTaskPage page2 = new XPTaskPage();
page2.Title = "Page 2";
wizardContainer1.Controls.Add(page2);

// Step 4: Set pages in TaskPages collection
xpTaskPane1.TaskPages = new XPTaskPage[] { page1, page2 };

// Step 5: Add controls to pages
Button btn = new Button() { Text = "Click Me" };
page1.Controls.Add(btn);
```

## Common Patterns

### Pattern 1: Designer-Based Setup
1. Drag XPTaskPane from toolbox to form
2. Click "Add Page" in Smart Tags
3. Design each page visually
4. No code required for basic setup

### Pattern 2: Runtime Page Creation
- Create pages dynamically in response to data or conditions
- Use TaskPages property to update collection
- Useful for dynamic layouts

### Pattern 3: Navigation Between Pages
- Set NextPage and PreviousPage properties
- Define page sequence programmatically
- Control order independent of collection

### Pattern 4: Custom Styling
- Set VisualStyle to Office2007 or OfficeXP
- Customize Font and ForeColor globally
- Override individual page appearance

## Key Properties at a Glance

| Property | Type | Purpose |
|----------|------|---------|
| `TaskPages` | XPTaskPage[] | Array of all task pages |
| `SelectedPage` | XPTaskPage | Currently active page |
| `TaskPanePageContainer` | Control | Container for pages (WizardContainer) |
| `VisualStyle` | VisualStyle | Office theme (OfficeXP or Office2007) |
| `ScrollSpeed` | int | Scroll speed for page content |
| `VerticalScroll` | bool | Enable vertical scrolling |
| `Font` | Font | Global font for task pane |
| `ForeColor` | Color | Global text color |
| `HeaderLabel` | Label | Header text control |
| `HeaderLeftToolbar` | ToolBar | Previous/next navigation buttons |
| `HeaderRightToolbar` | ToolBar | Menu and close button |

---

## Typical Use Cases

1. **Multi-Step Wizard** - Create step-by-step workflows
2. **Settings Dashboard** - Organize settings into logical sections
3. **Document Viewer** - Browse multiple document sections
4. **Feature Explorer** - Group related features into pages
5. **Task Organization** - Display related tasks in collapsible pages
