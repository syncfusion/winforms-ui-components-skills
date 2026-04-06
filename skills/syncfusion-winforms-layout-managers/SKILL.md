---
name: syncfusion-winforms-layout-managers
description: Guide for implementing Syncfusion layout managers in Windows Forms applications. Use when creating container layouts with BorderLayout (5-region docking), CardLayout (wizards/property pages), FlowLayout (horizontal/vertical flow), GridLayout (uniform grids), or GridBagLayout (flexible grids with spanning). Covers container controls, child control management, spacing configuration (HGap, VGap), positioning constraints, and designer integration for automatic control arrangement and structured layouts.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion WinForms LayoutManagers

The **LayoutManagers Package** provides five specialized layout managers that automatically arrange child controls within container controls (Panel, Form, etc.) based on specific layout strategies. These managers eliminate manual positioning and provide professional, maintainable layouts.

## When to Use This Skill

Use this skill when the user needs to:

- **Automatic Layout Management:** Let layout managers position controls instead of manual placement
- **BorderLayout (5-Region Docking):** Dock controls to North, South, East, West, or Center positions
- **CardLayout (Wizard/Property Pages):** Show one control at a time in a stack (wizards, tabbed content)
- **FlowLayout (Flow Arrangement):** Arrange controls horizontally or vertically with automatic wrapping
- **GridLayout (Uniform Grid):** Position controls in a uniform grid with equal-sized cells
- **GridBagLayout (Flexible Grid):** Create complex layouts with variable cell sizes and control spanning
- **Container-Based Layouts:** Apply layouts to Panel, Form, or any ContainerControl
- **Constraint-Based Positioning:** Define layout rules instead of absolute coordinates
- **Responsive Layouts:** Layouts that adapt when container resizes
- **Professional Forms:** Create consistent, maintainable form layouts

## Component Overview

The **LayoutManagers Package** (`Syncfusion.Windows.Forms.Tools`) provides five layout managers:

### 1. BorderLayout
**Purpose:** Dock controls along borders and center (like .NET docking)

**Key Features:**
- 5 regions: North, South, East, West, Center
- Position-based constraints
- Spacing control (HGap, VGap)
- Size customization per position

**When to use:** MDI-style interfaces, dock panels, application shells with header/footer/sidebar

### 2. CardLayout
**Purpose:** Show one child control at a time from a stack

**Key Features:**
- Stack of cards (only one visible)
- Navigation (First, Last, Next, Previous)
- Card naming/identification
- Layout modes (Default, Fill)
- Image support per card

**When to use:** Wizards, property pages, tabbed content without tabs, step-by-step forms

### 3. FlowLayout
**Purpose:** Arrange controls in horizontal or vertical flow (MOST COMMONLY USED)

**Key Features:**
- Horizontal or vertical layout modes
- Automatic wrapping
- Alignment options (Near, Far, Center, ChildConstraints)
- Per-child constraints (MinSize, PreferredSize)
- Flow direction (normal, reverse)
- AutoHeight feature

**When to use:** Toolbars, button panels, tag lists, responsive content, most general-purpose layouts

### 4. GridLayout
**Purpose:** Position controls in a uniform grid

**Key Features:**
- Rows and columns configuration
- Equal-sized cells
- Automatic child placement
- Spacing control (HGap, VGap)

**When to use:** Calculator layouts, button grids, uniform icon panels, simple tabular layouts

### 5. GridBagLayout
**Purpose:** Create complex layouts with flexible grid

**Key Features:**
- Variable row/column sizes
- Cell spanning (row and column)
- Anchor positions (9 options)
- Fill modes (None, Horizontal, Vertical, Both)
- Weight-based resizing (WeightX, WeightY)
- Per-cell padding (Insets)

**When to use:** Complex forms, dialog boxes, layouts with variable cell sizes, advanced positioning requirements

**Key Namespace:** `Syncfusion.Windows.Forms.Tools`

**Assembly:** `Syncfusion.Shared.Base.dll`

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** User needs to install, understand container concepts, or set up their first layout manager.

**Topics covered:**
- Assembly deployment and NuGet packages
- Container control concept (Panel, Form, ContainerControl)
- Child control management
- Adding layout managers via designer
- Adding layout managers via code
- Basic setup for all layout types
- Common settings (spacing, gaps)
- Layout events

### BorderLayout (5-Region Docking)
📄 **Read:** [references/border-layout.md](references/border-layout.md)

**When to read:** User wants to dock controls to borders (North, South, East, West, Center).

**Topics covered:**
- BorderLayout overview and position constraints
- Adding controls to each position
- Designer vs code approaches
- SetConstraints method
- Spacing configuration (HGap, VGap)
- Size customization per position
- MDI-style layout example
- Dock panel example
- Best practices for border layouts

### CardLayout (Wizard/Stack)
📄 **Read:** [references/card-layout.md](references/card-layout.md)

**When to read:** User needs wizards, property pages, or stack-based navigation.

**Topics covered:**
- CardLayout overview (one visible card at a time)
- Card naming and identification
- Navigation methods (First, Last, Next, Previous, ActivateCard)
- LayoutMode (Default, Fill)
- Image settings per card
- Designer usage (SelectedCard property)
- Programmatic card management
- Wizard control implementation
- Property page implementation
- Complete wizard example
- Best practices

### FlowLayout (Flow Arrangement)
📄 **Read:** [references/flow-layout.md](references/flow-layout.md)

**When to read:** User needs horizontal/vertical flow layouts (MOST COMMON use case).

**Topics covered:**
- FlowLayout overview (most versatile)
- LayoutMode (Horizontal, Vertical)
- Flow direction (normal, reverse)
- Alignment options (Near, Far, Center, ChildConstraints)
- AutoHeight feature for wrapping
- HGap and VGap spacing
- Per-child constraints (MinSize, PreferredSize)
- SetConstraints method
- Wrapping behavior
- Horizontal toolbar example
- Vertical sidebar example
- Tag list example
- Advanced constraint scenarios
- Best practices

### GridLayout (Uniform Grid)
📄 **Read:** [references/grid-layout.md](references/grid-layout.md)

**When to read:** User needs uniform grid with equal-sized cells.

**Topics covered:**
- GridLayout overview
- Rows and Columns configuration
- Automatic child placement order
- HGap and VGap spacing
- Designer vs code approaches
- Grid sizing behavior
- Calculator layout example
- Button grid example
- Best practices

### GridBagLayout (Flexible Grid)
📄 **Read:** [references/grid-bag-layout.md](references/grid-bag-layout.md)

**When to read:** User needs complex layouts with variable cell sizes and control spanning.

**Topics covered:**
- GridBagLayout overview (most powerful)
- Virtual grid concept (variable row/column sizes)
- GridBagConstraints configuration
- Anchor property (North, South, East, West, Center, NorthEast, NorthWest, SouthEast, SouthWest)
- Fill property (None, Horizontal, Vertical, Both)
- CellSpan (RowSpan, ColSpan)
- WeightX and WeightY (resize distribution)
- Insets (cell padding)
- Form layout example
- Calculator layout example
- Advanced spanning scenarios
- Best practices

---

## Layout Type Selection Guide

**Choose your layout based on requirements:**

| Requirement | Recommended Layout | Why |
|------------|-------------------|-----|
| Dock to borders (header/footer/sidebar) | **BorderLayout** | Simple 5-region positioning |
| Wizard or multi-step form | **CardLayout** | One visible card at a time with navigation |
| Horizontal button toolbar | **FlowLayout** (Horizontal) | Auto-wrapping, flexible spacing |
| Vertical sidebar or list | **FlowLayout** (Vertical) | Auto-wrapping, flexible spacing |
| Calculator or keypad | **GridLayout** or **GridBagLayout** | Uniform grid or flexible grid |
| Complex form with labels/fields | **GridBagLayout** | Variable cell sizes, spanning, alignment |
| Simple tabular layout (equal cells) | **GridLayout** | Uniform rows and columns |
| General-purpose responsive layout | **FlowLayout** | Most versatile, widely applicable |

---

## Quick Start Examples

### BorderLayout: Dock to Borders

```csharp
using Syncfusion.Windows.Forms.Tools;

// Create BorderLayout
BorderLayout borderLayout = new BorderLayout();
borderLayout.ContainerControl = panel1; // Panel as container

// Create controls for each position
Label northLabel = new Label { Text = "Header", BackColor = Color.LightBlue };
Label southLabel = new Label { Text = "Footer", BackColor = Color.LightGray };
Label eastLabel = new Label { Text = "Right", BackColor = Color.LightYellow };
Label westLabel = new Label { Text = "Left", BackColor = Color.LightGreen };
Panel centerPanel = new Panel { BackColor = Color.White };

// Add controls to container
panel1.Controls.Add(northLabel);
panel1.Controls.Add(southLabel);
panel1.Controls.Add(eastLabel);
panel1.Controls.Add(westLabel);
panel1.Controls.Add(centerPanel);

// Set constraints (position each control)
borderLayout.SetPosition(northLabel, BorderPosition.North)
borderLayout.SetPosition(southLabel, BorderPosition.South);
borderLayout.SetPosition(eastLabel, BorderPosition.East);
borderLayout.SetPosition(westLabel, BorderPosition.West);
borderLayout.SetPosition(centerPanel, BorderPosition.Center);

// Configure spacing
borderLayout.HGap = 5;
borderLayout.VGap = 5;
```

### CardLayout: Wizard Pages

```csharp
// Create CardLayout
CardLayout cardLayout = new CardLayout();
cardLayout.ContainerControl = panel1;

// Create wizard pages
Panel page1 = new Panel { BackColor = Color.AliceBlue };
Label label1 = new Label { Text = "Step 1: Welcome", Dock = DockStyle.Fill };
page1.Controls.Add(label1);

Panel page2 = new Panel { BackColor = Color.Honeydew };
Label label2 = new Label { Text = "Step 2: Configure", Dock = DockStyle.Fill };
page2.Controls.Add(label2);

Panel page3 = new Panel { BackColor = Color.LavenderBlush };
Label label3 = new Label { Text = "Step 3: Finish", Dock = DockStyle.Fill };
page3.Controls.Add(label3);

// Add pages to container
panel1.Controls.Add(page1);
panel1.Controls.Add(page2);
panel1.Controls.Add(page3);

// Name each card
cardLayout.SetCardName(page1, "Welcome");
cardLayout.SetCardName(page2, "Configure");
cardLayout.SetCardName(page3, "Finish");

// Navigation
cardLayout.LayoutMode = CardLayoutMode.Fill; // Fill container
cardLayout.First();  // Show first card

// Navigate programmatically
// cardLayout.Next();   // Next card
// cardLayout.Previous(); // Previous card
// cardLayout.ActivateCard("Configure"); // Jump to specific card
```

### FlowLayout: Horizontal Toolbar

```csharp
// Create FlowLayout
FlowLayout flowLayout = new FlowLayout();
flowLayout.ContainerControl = panel1;
flowLayout.LayoutMode = FlowLayoutMode.Horizontal;
flowLayout.Alignment = FlowAlignment.Near;
flowLayout.HGap = 10;
flowLayout.VGap = 5;

// Add buttons
for (int i = 1; i <= 8; i++)
{
    Button btn = new Button { Text = $"Button {i}", Size = new Size(80, 30) };
    panel1.Controls.Add(btn);
}

// Buttons will flow horizontally and wrap to next line automatically
```

### GridLayout: Button Grid

```csharp
// Create GridLayout (3x3 grid)
GridLayout gridLayout = new GridLayout();
gridLayout.ContainerControl = panel1;
gridLayout.Rows = 3;
gridLayout.Columns = 3;
gridLayout.HGap = 5;
gridLayout.VGap = 5;

// Add 9 buttons (automatic placement in grid)
for (int i = 1; i <= 9; i++)
{
    Button btn = new Button { Text = i.ToString() };
    panel1.Controls.Add(btn);
}
```

### GridBagLayout: Form Layout

```csharp
// Create GridBagLayout
GridBagLayout gridBagLayout = new GridBagLayout();
gridBagLayout.ContainerControl = panel1;

// Create form controls
Label nameLabel = new Label { Text = "Name:", AutoSize = true };
TextBox nameTextBox = new TextBox { Width = 200 };
Label emailLabel = new Label { Text = "Email:", AutoSize = true };
TextBox emailTextBox = new TextBox { Width = 200 };
Button submitButton = new Button { Text = "Submit" };

// Add to container
panel1.Controls.Add(nameLabel);
panel1.Controls.Add(nameTextBox);
panel1.Controls.Add(emailLabel);
panel1.Controls.Add(emailTextBox);
panel1.Controls.Add(submitButton);

// Create constraints
GridBagConstraints labelConstraints = new new GridBagConstraints(0, 0, 1, 1, 1.0, 0.0, AnchorTypes.East, FillType.Horizontal, new Insets(5,5,5,5), 0, 0, false);
labelConstraints.Anchor = GridBagAnchor.East; // Align right

GridBagConstraints fieldConstraints = new GridBagConstraints(1, 0, 1, 1);
fieldConstraints.Fill = GridBagFill.Horizontal;
fieldConstraints.WeightX = 1; // Take extra horizontal space

// Apply constraints
gridBagLayout.SetConstraints(nameLabel, new new GridBagConstraints(0, 0, 1, 1, 1.0, 0.0, AnchorTypes.East, FillType.Horizontal, new Insets(5,5,5,5), 0, 0, false));
gridBagLayout.SetConstraints(nameTextBox, new new GridBagConstraints(0, 0, 1, 1, 1.0, 0.0, AnchorTypes.East, FillType.Horizontal, new Insets(5,5,5,5), 0, 0, false) {WeightX = 1 });
gridBagLayout.SetConstraints(emailLabel, new new GridBagConstraints(0, 0, 1, 1, 1.0, 0.0, AnchorTypes.East, FillType.Horizontal, new Insets(5,5,5,5), 0, 0, false));
gridBagLayout.SetConstraints(emailTextBox, new new GridBagConstraints(0, 0, 1, 1, 1.0, 0.0, AnchorTypes.East, FillType.Horizontal, new Insets(5,5,5,5), 0, 0, false){ WeightX = 1 });
gridBagLayout.SetConstraints(submitButton, new new GridBagConstraints(0, 0, 1, 1, 1.0, 0.0, AnchorTypes.East, FillType.Horizontal, new Insets(5,5,5,5), 0, 0, false));
```

---

## Common Patterns

### Application Shell (BorderLayout)

```csharp
// Header + Footer + Sidebar + Content area
BorderLayout borderLayout = new BorderLayout();
borderLayout.ContainerControl = this; // Form as container
borderLayout.HGap = 0;
borderLayout.VGap = 0;

Panel header = new Panel { Height = 60, BackColor = Color.Navy };
Panel footer = new Panel { Height = 30, BackColor = Color.Gray };
Panel sidebar = new Panel { Width = 200, BackColor = Color.LightGray };
Panel content = new Panel { BackColor = Color.White };

this.Controls.AddRange(new Control[] { header, footer, sidebar, content });

borderLayout.SetPosition(header, BorderPosition.North);
borderLayout.SetPosition(footer, BorderPosition.South);
borderLayout.SetPosition(sidebar, BorderPosition.West);
borderLayout.SetPosition(content, BorderPosition.Center);
```

**When to use:** Main application window with fixed header/footer and sidebar.

### Multi-Step Wizard (CardLayout)

See Quick Start example above. Add Next/Previous buttons to navigate between wizard steps.

### Responsive Toolbar (FlowLayout)

See Quick Start example above. Buttons automatically wrap when window resizes.

### Login Form (GridBagLayout)

```csharp
// Labels in column 0, fields in column 1
// Submit button spans both columns
// All elements properly aligned and spaced
```

See GridBagLayout reference for complete example.

---

## Key Properties

### Common to All Layouts

| Property | Type | Description |
|----------|------|-------------|
| **ContainerControl** | `Control` | Container (Panel, Form) where layout is applied |

### BorderLayout

| Property | Type | Description |
|----------|------|-------------|
| **HGap** | `int` | Horizontal gap between regions |
| **VGap** | `int` | Vertical gap between regions |

### CardLayout

| Property | Type | Description |
|----------|------|-------------|
| **LayoutMode** | `CardLayoutMode` | Default or Fill |
| **SelectedCard** | `Control` | Currently visible card (designer) |

### FlowLayout

| Property | Type | Description |
|----------|------|-------------|
| **LayoutMode** | `FlowLayoutMode` | Horizontal or Vertical |
| **Alignment** | `FlowAlignment` | Near, Far, Center, ChildConstraints |
| **ReverseRows** | `bool` | Reverse flow direction |
| **AutoHeight** | `bool` | Auto-adjust height for wrapping |
| **HGap** | `int` | Horizontal gap between controls |
| **VGap** | `int` | Vertical gap between controls |

### GridLayout

| Property | Type | Description |
|----------|------|-------------|
| **Rows** | `int` | Number of rows |
| **Columns** | `int` | Number of columns |
| **HGap** | `int` | Horizontal gap between cells |
| **VGap** | `int` | Vertical gap between cells |

### GridBagLayout

| Property | Type | Description |
|----------|------|-------------|
| *(Constraints set per child)* | `GridBagConstraints` | See GridBagLayout reference |

---

## Common Use Cases

### 1. Application Main Window
**Scenario:** Header, footer, sidebar, content area.
- Use **BorderLayout**
- North: Header, South: Footer, West: Sidebar, Center: Content
- Read: [border-layout.md](references/border-layout.md)

### 2. Setup Wizard
**Scenario:** Multi-step configuration wizard.
- Use **CardLayout**
- Each card is one step, navigate with Next/Previous buttons
- Read: [card-layout.md](references/card-layout.md)

### 3. Toolbar with Auto-Wrapping
**Scenario:** Horizontal toolbar that wraps to next line.
- Use **FlowLayout** (Horizontal mode)
- Set AutoHeight = true
- Read: [flow-layout.md](references/flow-layout.md)

### 4. Calculator Keypad
**Scenario:** 4x4 button grid.
- Use **GridLayout** (4 rows, 4 columns)
- Or **GridBagLayout** for variable button sizes
- Read: [grid-layout.md](references/grid-layout.md) or [grid-bag-layout.md](references/grid-bag-layout.md)

### 5. Data Entry Form
**Scenario:** Labels on left, fields on right.
- Use **GridBagLayout**
- Column 0: Labels (right-aligned), Column 1: Fields (fill horizontal)
- Read: [grid-bag-layout.md](references/grid-bag-layout.md)

### 6. Tag Cloud
**Scenario:** Tags that flow and wrap.
- Use **FlowLayout** (Horizontal mode)
- Add Label controls dynamically
- Read: [flow-layout.md](references/flow-layout.md)

---

## Events

**Common layout event:**
- **LayoutComplete** - Raised when layout recalculation is complete

```csharp
flowLayout.LayoutComplete += (sender, e) => {
    // Layout has been recalculated
};
```

---

## Best Practices

1. **Layout Selection:** Use the selection guide above to choose the right layout type
2. **Container Requirements:** Always set ContainerControl property (Panel, Form)
3. **Child Addition Order:** Add children to container BEFORE setting constraints
4. **FlowLayout for Most Cases:** When in doubt, start with FlowLayout (most versatile)
5. **GridBagLayout for Complex Forms:** Use for professional data entry forms
6. **Spacing Consistency:** Use HGap/VGap for consistent spacing
7. **Designer Support:** Use designer for rapid prototyping, code for dynamic layouts
8. **Constraint Objects:** Reuse GridBagConstraints objects for similar controls
9. **Testing Resize:** Always test layouts with different container sizes
10. **Documentation:** Comment constraint configurations for maintainability

---

## See Also

- [Syncfusion LayoutManagers Documentation](https://help.syncfusion.com/windowsforms/layoutmanagers/overview)
- [BorderLayout API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.BorderLayout.html)
- [CardLayout API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.CardLayout.html)
- [FlowLayout API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.FlowLayout.html)
- [GridLayout API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.GridLayout.html)
- [GridBagLayout API Reference](https://help.syncfusion.com/cr/windowsforms/Syncfusion.Windows.Forms.Tools.GridBagLayout.html)
