# Getting Started with GridLayout

## Table of Contents
- [Assembly Deployment](#assembly-deployment)
- [Creating the Project](#creating-the-project)
- [Adding GridLayout Through Designer](#adding-gridlayout-through-designer)
- [Adding GridLayout Through Code](#adding-gridlayout-through-code)
- [Adding Child Controls](#adding-child-controls)

## Assembly Deployment

To use GridLayout in your Windows Forms application, you need to add the required assembly reference:

**Required Assembly:**
- `Syncfusion.Shared.Base.dll`

**NuGet Package:**
Install the NuGet package via Package Manager Console:
```
Install-Package Syncfusion.Shared.Base
```

Or through the NuGet Package Manager UI:
1. Right-click on project → Manage NuGet Packages
2. Search for "Syncfusion.Shared.Windows"
3. Click Install

The required assembly will be added automatically when you install the NuGet package.

## Creating the Project

Create a new Windows Forms project in Visual Studio:
1. Open Visual Studio
2. Create a new **Windows Forms App (.NET Framework)** project
3. Name your project (e.g., "GridLayoutExample")
4. Add the Syncfusion.Shared.Base.dll reference

## Adding GridLayout Through Designer

The easiest way to add GridLayout to your form is through the Visual Studio designer:

**Steps:**
1. Open your form in the designer view
2. Go to **Toolbox** → **Syncfusion Components**
3. Drag **GridLayout** onto your form
4. A dialog will appear asking to set a container control
5. Select the form as the container control

When added through the designer:
- `Syncfusion.Shared.Base.dll` is automatically referenced
- An instance of GridLayout is created in your form
- The component is ready for configuration

### Adding Layout Components Through Designer

Once GridLayout is added:
1. Drag child controls (buttons, labels, text boxes, etc.) from the toolbox onto the form
2. The controls will automatically be included in the grid layout
3. Resize and position controls in the designer as needed
4. GridLayout will arrange them in the grid at runtime

## Adding GridLayout Through Code

To create and configure GridLayout programmatically:

**Step 1: Add the namespace**

C#:
```csharp
using Syncfusion.Windows.Forms.Tools;
```

VB.NET:
```vb
Imports Syncfusion.Windows.Forms.Tools
```

**Step 2: Create the GridLayout instance**

C#:
```csharp
GridLayout gridLayout1 = new GridLayout();
this.gridLayout1.ContainerControl = this;
```

VB.NET:
```vb
Dim gridLayout1 As GridLayout = New GridLayout()
Me.gridLayout1.ContainerControl = Me
```

The `ContainerControl` property specifies the form or container that the layout manager will manage. Setting it to `this` makes the form the container.

**Step 3: Configure basic settings (optional)**

C#:
```csharp
gridLayout1.Rows = 2;
gridLayout1.Columns = 2;
gridLayout1.HGap = 10;
gridLayout1.VGap = 10;
```

VB.NET:
```vb
gridLayout1.Rows = 2
gridLayout1.Columns = 2
gridLayout1.HGap = 10
gridLayout1.VGap = 10
```

## Adding Child Controls

Child controls can be added to participate in the GridLayout either through the designer or programmatically.

### Through Designer

Simply drag controls onto the form after adding GridLayout. They will automatically be included in the layout.

### Through Code

Use the `SetParticipateInLayout` method to explicitly include controls:

C#:
```csharp
// Create controls
ButtonAdv buttonAdv1 = new ButtonAdv();
ButtonAdv buttonAdv2 = new ButtonAdv();
ButtonAdv buttonAdv3 = new ButtonAdv();
ButtonAdv buttonAdv4 = new ButtonAdv();

// Set properties
this.buttonAdv1.Text = "Button 1";
this.buttonAdv2.Text = "Button 2";
this.buttonAdv3.Text = "Button 3";
this.buttonAdv4.Text = "Button 4";

// Add to form
this.Controls.Add(this.buttonAdv1);
this.Controls.Add(this.buttonAdv2);
this.Controls.Add(this.buttonAdv3);
this.Controls.Add(this.buttonAdv4);

// Include in layout
this.gridLayout1.SetParticipateInLayout(this.buttonAdv1, true);
this.gridLayout1.SetParticipateInLayout(this.buttonAdv2, true);
this.gridLayout1.SetParticipateInLayout(this.buttonAdv3, true);
this.gridLayout1.SetParticipateInLayout(this.buttonAdv4, true);
```

VB.NET:
```vb
' Create controls
Dim buttonAdv1 As ButtonAdv = New ButtonAdv()
Dim buttonAdv2 As ButtonAdv = New ButtonAdv()
Dim buttonAdv3 As ButtonAdv = New ButtonAdv()
Dim buttonAdv4 As ButtonAdv = New ButtonAdv()

' Set properties
Me.buttonAdv1.Text = "Button 1"
Me.buttonAdv2.Text = "Button 2"
Me.buttonAdv3.Text = "Button 3"
Me.buttonAdv4.Text = "Button 4"

' Add to form
Me.Controls.Add(Me.buttonAdv1)
Me.Controls.Add(Me.buttonAdv2)
Me.Controls.Add(Me.buttonAdv3)
Me.Controls.Add(Me.buttonAdv4)

' Include in layout
Me.gridLayout1.SetParticipateInLayout(Me.buttonAdv1, True)
Me.gridLayout1.SetParticipateInLayout(Me.buttonAdv2, True)
Me.gridLayout1.SetParticipateInLayout(Me.buttonAdv3, True)
Me.gridLayout1.SetParticipateInLayout(Me.buttonAdv4, True)
```

**Key Points:**
- Controls must be added to the form's `Controls` collection first
- Then use `SetParticipateInLayout` to include them in the grid layout
- The order of addition determines the layout arrangement (left to right, top to bottom)
