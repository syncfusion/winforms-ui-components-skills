# Getting Started with TabBarSplitterControl

This guide covers installation, setup, and basic understanding of the TabBarSplitterControl component for Windows Forms applications.

## Overview

TabBarSplitterControl enables you to create tabbed pages with dynamic splitters in Windows Forms applications. When used with GridControl, it provides a workbook-like appearance where each tab can contain separate control instances.

**Primary Use Case:** Building multi-sheet interfaces similar to Excel workbooks, where each tab (TabBarPage) contains its own GridControl or other controls.

## When to Use TabBarSplitterControl

Use TabBarSplitterControl when you need:

- **Workbook-like interfaces:** Multiple sheets with tabs for navigation
- **GridControl with tabs:** Separate data grids for different data sets
- **Formula and cross-reference support:** GridControls with formulas referencing other sheets
- **Multi-page data organization:** Organize related data views in separate tabs
- **Spreadsheet applications:** Build Excel-like interfaces in Windows Forms
- **Tabbed data views:** Any scenario requiring tabbed navigation with controls per tab

## Installation

### NuGet Package

TabBarSplitterControl is part of the `Syncfusion.Shared.Base` assembly. Install via NuGet:

**PowerShell (Package Manager Console):**
```powershell
Install-Package Syncfusion.Shared.Base -Version *
```

**NET CLI:**
```bash
dotnet add package Syncfusion.Shared.Base --version *
```

**Best Practice:** Using `*` ensures the latest version is installed during package restore, providing bug fixes and new features automatically.

### Package Details

- **Assembly:** Syncfusion.Shared.Base
- **Namespace:** Syncfusion.Windows.Forms
- **Dependencies:** Automatically included by NuGet package

The NuGet package automatically includes all required dependencies, so no additional manual references are needed.

## Namespace Imports

Add the appropriate namespace import to your form or class:

**C#:**
```csharp
using Syncfusion.Windows.Forms;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms
```

If integrating with GridControl, also import:

**C#:**
```csharp
using Syncfusion.Windows.Forms.Grid;
```

**VB.NET:**
```vb
Imports Syncfusion.Windows.Forms.Grid
```

## Core Components

### TabBarSplitterControl

The main container control that hosts multiple TabBarPage objects.

**Key Characteristics:**
- Acts as a container for TabBarPage instances
- Provides tabbed navigation
- Supports dynamic splitters
- Standard WinForms control (can be docked, positioned, sized)

### TabBarPage

Individual page within the TabBarSplitterControl.

**Key Characteristics:**
- Each page has its own Controls collection
- Page tab displays the `Text` property value
- Can host any WinForms control (commonly GridControl)
- Multiple pages can be added to one TabBarSplitterControl

## Basic Architecture

```
TabBarSplitterControl
├── TabBarPage (Tab: "Sheet1")
│   └── GridControl (or other controls)
├── TabBarPage (Tab: "Sheet2")
│   └── GridControl (or other controls)
└── TabBarPage (Tab: "Sheet3")
    └── GridControl (or other controls)
```

## GridControl Integration Basics

TabBarSplitterControl is particularly useful with GridControl for creating workbook-like applications:

**Common Pattern:**
1. Create TabBarSplitterControl instance
2. Create multiple TabBarPage instances (one per sheet)
3. Add GridControl to each TabBarPage's Controls collection
4. Add TabBarPages to TabBarSplitterControl's Controls collection
5. Add TabBarSplitterControl to form

**Benefits:**
- Separate data grids per tab
- Formula cells can reference other sheets
- Cross-reference support between sheets
- Organized data presentation
- Familiar spreadsheet-like user experience

## Designer Availability

TabBarSplitterControl supports Visual Studio Designer integration:

- Can be dragged and dropped from toolbox
- TabBarPage management via TabBarPageCollectionEditor
- Property window configuration
- Visual design-time experience

**Note for AI:** Designer operations are manual user tasks. For programmatic implementation (which AI can help with), refer to the programmatic-implementation.md reference file.

## Implementation Approaches

### 1. Programmatic Creation (AI-Assisted)
Create and configure controls entirely in code. This is what AI assistants can help implement.

### 2. Designer-Based Creation (Manual)
Use Visual Studio Designer for drag-and-drop and visual configuration. Users perform these steps manually.

### 3. Hybrid Approach
Use Designer for initial layout, then configure programmatically. AI can assist with the code portion.

## Next Steps

For complete programmatic implementation details including:
- Creating TabBarSplitterControl instances
- Adding and configuring TabBarPages
- Adding controls to pages
- Multiple page scenarios
- C# and VB.NET examples

**Read:** [programmatic-implementation.md](programmatic-implementation.md)
