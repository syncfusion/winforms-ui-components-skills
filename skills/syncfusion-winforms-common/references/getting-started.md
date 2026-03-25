# Getting Started with Syncfusion Windows Forms Controls

## Table of Contents
- [Adding Controls via Designer](#adding-controls-via-designer)
- [Adding Controls via Code-Behind](#adding-controls-via-code-behind)
- [Creating a Project via Project Template](#creating-a-project-via-project-template)
- [Adding Forms via Item Template](#adding-forms-via-item-template)

---

## Adding Controls via Designer

Syncfusion WinForms controls are automatically added to the Visual Studio **Toolbox** during installation.

1. Create a Windows Forms project in Visual Studio.
2. In the Toolbox search box, type the control name (e.g., `TextBoxExt`).
3. Drag the control and drop it onto the form designer.

The required assembly references are added automatically when dropping a control from the Toolbox.

> After adding controls via the Toolbox, a licensing registration message box may appear if using a trial or NuGet setup — register the license key to dismiss it permanently.

---

## Adding Controls via Code-Behind

Add Syncfusion controls programmatically at runtime using C# or VB.

### Steps

1. Create a Windows Forms project and add the required assembly references (see [Control Dependencies](#control-dependencies-reference)).
2. Instantiate the control using its full namespace.
3. Set `Location` and `Size` properties.
4. Add to the form's `Controls` collection.

### C# Example (TextBoxExt)

```csharp
// Required assemblies: Syncfusion.Tools.Base.dll, Syncfusion.Tools.Windows.dll,
//                      Syncfusion.Shared.Base.dll, Syncfusion.Shared.Windows.dll
using Syncfusion.Windows.Forms.Tools;

Syncfusion.Windows.Forms.Tools.TextBoxExt textBoxExt1 =
    new Syncfusion.Windows.Forms.Tools.TextBoxExt();

textBoxExt1.Location = new System.Drawing.Point(100, 100);
textBoxExt1.Size = new System.Drawing.Size(200, 25);

// 'this' refers to the parent Form
this.Controls.Add(textBoxExt1);
```

### VB Example (TextBoxExt)

```vb
Dim textBoxExt1 As New Syncfusion.Windows.Forms.Tools.TextBoxExt()

Me.textBoxExt1.Location = New System.Drawing.Point(100, 100)
Me.textBoxExt1.Size = New System.Drawing.Size(200, 25)

Me.Controls.Add(Me.textBoxExt1)
```

---

## Creating a Project via Project Template

Syncfusion provides a Visual Studio **Project Template** to scaffold a WinForms application with Syncfusion references pre-configured.

> Available from Syncfusion Essential Studio v14.3.0.49 onward.

### Steps

1. In Visual Studio, go to **File → New → Project**.
2. Search for **Syncfusion** or navigate to **Syncfusion → Windows → Syncfusion Windows Forms Application**.
3. Name the project, set the location and target framework (minimum .NET Framework 3.5), click **OK**.
4. Configure the project in the **Project Configuration Wizard**:
   - **Language:** C# or VB
   - **Assemblies From:** Choose the assembly source (NuGet / Installed location)
   - **Select Control:** Choose the control(s) to include
5. Click **Create** — the project is scaffolded with required references and a sample form.
6. If using a trial/NuGet setup, a licensing registration message box appears — register the license key.

---

## Adding Forms via Item Template

Syncfusion **Item Templates** let you add pre-built Syncfusion form/item files to an existing project.

> Available from Syncfusion Essential Studio v13.1.0.21 onward.

### Using the Syncfusion Item Template Gallery

1. Right-click the WinForms project in Solution Explorer.
2. Select **Add Syncfusion Item → New Item...**
3. In the **Syncfusion Item Template Gallery**, select the required **Version** and **Theme**.
4. Choose the item template from the gallery.
5. Click **Add** — the selected template is added to the project with Syncfusion references.

### Using Visual Studio Add New Item

1. Right-click the project → **Add → New Item**.
2. Under the **Syncfusion** tab, find templates for both C# and VB items.
3. Select and add the template — references are added automatically.
