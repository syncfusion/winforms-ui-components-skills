# Getting Started with DomainUpdownExt

## Table of Contents
- [Assembly Dependencies](#assembly-dependencies)
- [Create a Simple Application](#create-a-simple-application)
- [Add Control Through Designer](#add-control-through-designer)
- [Add Control Manually](#add-control-manually)
- [Adding Items](#adding-items)

## Assembly Dependencies

Required assemblies to reference in your project:
- Syncfusion.Grid.Base
- Syncfusion.Grid.Windows
- Syncfusion.Shared.Base
- Syncfusion.Shared.Windows
- Syncfusion.Tools.Base
- Syncfusion.Tools.Windows

Install via NuGet Package Manager with:
```
Install-Package Syncfusion.Tools.Windows
```

Or add the assembly references directly to your project through Visual Studio's "Add Reference" dialog.

## Create a Simple Application

### Add Control Through Designer

1. Create a new Windows Forms project in Visual Studio
2. Open the form designer
3. Drag **DomainUpDownExt** from the toolbox to your form
4. Required assembly references will be added automatically

### Add Control Manually

Include the namespace in your code file:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

Create and add the control to your form:

```csharp
public class Form1 : Form
{
    public Form1()
    {
        DomainUpDownExt domainUpDownExt1 = new DomainUpDownExt();
        this.Controls.Add(domainUpDownExt1);
    }
}
```

```vb
Public Class Form1
    Inherits Form
    
    Public Sub New()
        Dim domainUpDownExt1 As New DomainUpDownExt()
        Me.Controls.Add(domainUpDownExt1)
    End Sub
End Class
```

## Adding Items

Add items to the DomainUpDownExt control using the Items collection:

```csharp
// Add items individually
this.domainUpDownExt1.Items.Add("One");
this.domainUpDownExt1.Items.Add("Two");
this.domainUpDownExt1.Items.Add("Three");
this.domainUpDownExt1.Items.Add("Four");
this.domainUpDownExt1.Items.Add("Five");
```

```vb
' Add items individually
Me.domainUpDownExt1.Items.Add("One")
Me.domainUpDownExt1.Items.Add("Two")
Me.domainUpDownExt1.Items.Add("Three")
Me.domainUpDownExt1.Items.Add("Four")
Me.domainUpDownExt1.Items.Add("Five")
```

### Adding Items from a Collection

```csharp
string[] options = { "Option 1", "Option 2", "Option 3" };
foreach (string option in options)
{
    domainUpDownExt1.Items.Add(option);
}
```

### Clearing Items

```csharp
domainUpDownExt1.Items.Clear();
```

### Removing Specific Items

```csharp
// Remove by value
domainUpDownExt1.Items.Remove("One");

// Remove by index
domainUpDownExt1.Items.RemoveAt(0);
```

## Next Steps

- Configure the spin button orientation and alignment using [references/spinbutton-control.md](spinbutton-control.md)
- Manage text and items using [references/text-and-items.md](text-and-items.md)
- Customize appearance using [references/appearance-customization.md](appearance-customization.md)
- Enable keyboard navigation using [references/keyboard-and-interaction.md](keyboard-and-interaction.md)
