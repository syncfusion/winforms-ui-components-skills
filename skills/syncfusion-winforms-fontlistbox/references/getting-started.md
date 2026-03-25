# Getting Started with FontListBox

## Assembly References

Add the following assemblies to your WinForms project:

| Assembly | Purpose |
|---|---|
| `Syncfusion.Grid.Base` | Core grid infrastructure |
| `Syncfusion.Grid.Windows` | WinForms grid rendering |
| `Syncfusion.Shared.Base` | Shared base components |
| `Syncfusion.Shared.Windows` | Shared WinForms components |
| `Syncfusion.Tools.Base` | Tools base (includes FontListBox) |
| `Syncfusion.Tools.Windows` | Tools WinForms rendering |

**NuGet (installs all required assemblies):**
```powershell
Install-Package Syncfusion.Tools.Windows
```

**Namespace import:**

C#:
```csharp
using Syncfusion.Windows.Forms.Tools;
```

VB.NET:
```vb
Imports Syncfusion.Windows.Forms.Tools
```

---

## Adding FontListBox via Designer

1. Create a new Windows Forms application in Visual Studio.
2. Drag the **FontListBox** control from the Toolbox onto the form.
3. All required assemblies are added automatically.

The control immediately populates itself with all fonts installed on the system — no additional code needed.

---

## Adding FontListBox Programmatically

**C#:**
```csharp
FontListBox fontListBox1 = new FontListBox();
fontListBox1.Size = new System.Drawing.Size(160, 94);
this.Controls.Add(fontListBox1);
```

**VB.NET:**
```vb
Dim fontListBox1 As New FontListBox()
fontListBox1.Size = New System.Drawing.Size(160, 94)
Me.Controls.Add(fontListBox1)
```

> The list is auto-populated with installed system fonts as soon as the control is created. No `DataSource` or `Items.Add` calls are needed.

---

## Selection Mode

Use the `SelectionMode` property (inherited from `ListBox`) to control how many items can be selected at once:

| `SelectionMode` Value | Description |
|---|---|
| `None` | No items can be selected |
| `One` | Only one item at a time (default) |
| `MultiSimple` | Multiple items via click only |
| `MultiExtended` | Multiple items via SHIFT, CTRL, and arrow keys |

**C#:**
```csharp
fontListBox1.SelectionMode = System.Windows.Forms.SelectionMode.MultiExtended;
```

**VB.NET:**
```vb
fontListBox1.SelectionMode = System.Windows.Forms.SelectionMode.MultiExtended
```

---

## Handling Font Selection — SelectedIndexChanged

The most common use case: apply the user's selected font to another control (such as a `Label` or `TextBox`).

**C#:**
```csharp
// Wire up the event
fontListBox1.SelectedIndexChanged += new System.EventHandler(FontListBox1_SelectedIndexChanged);

// Handler — apply selected font to a label
private void FontListBox1_SelectedIndexChanged(object sender, System.EventArgs e)
{
    this.label1.Font = new System.Drawing.Font(
        this.fontListBox1.SelectedItem.ToString(),
        11,
        System.Drawing.FontStyle.Regular);
}
```

**VB.NET:**
```vb
' Wire up the event
AddHandler fontListBox1.SelectedIndexChanged, AddressOf FontListBox1_SelectedIndexChanged

' Handler
Private Sub FontListBox1_SelectedIndexChanged(ByVal sender As Object, ByVal e As System.EventArgs)
    Me.label1.Font = New System.Drawing.Font(
        Me.fontListBox1.SelectedItem.ToString(),
        11,
        System.Drawing.FontStyle.Regular)
End Sub
```

> `SelectedItem.ToString()` returns the font family name string (e.g., `"Arial"`, `"Segoe UI"`), which can be passed directly to the `Font` constructor.

---

## Reading Multiple Selected Items

When `SelectionMode` is `MultiSimple` or `MultiExtended`, use `SelectedItems`:

**C#:**
```csharp
foreach (var item in fontListBox1.SelectedItems)
{
    Console.WriteLine(item.ToString()); // font name
}
```

**VB.NET:**
```vb
For Each item In fontListBox1.SelectedItems
    Console.WriteLine(item.ToString())
Next
```
