# Getting Started with SplitContainerAdv

## Assembly Dependencies

To use SplitContainerAdv in your Windows Forms application, add the following assembly references:

- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`
- `Syncfusion.Tools.Windows.dll`

For detailed information on installing NuGet packages, refer to the [Syncfusion Windows Forms installation guide](https://help.syncfusion.com/windowsforms/installation/install-nuget-packages).

## Adding Control via Designer

The easiest way to add SplitContainerAdv is through the Visual Studio designer:

1. Open your Windows Forms project
2. Locate the toolbox and search for "SplitContainer"
3. Drag and drop the control onto your form
4. The required assembly references will be added automatically

Child controls can be added by dragging from the toolbox and dropping into either panel of the SplitContainerAdv.

## Adding Control Manually via Code

### Step 1: Include Required Namespace

```csharp
using Syncfusion.Windows.Forms.Tools;
```

```vb
Imports Syncfusion.Windows.Forms.Tools
```

### Step 2: Create and Configure SplitContainerAdv

```csharp
// Create instance
SplitContainerAdv splitContainerAdv1 = new SplitContainerAdv();

// Configure basic properties
this.splitContainerAdv1.Dock = System.Windows.Forms.DockStyle.Fill;

// Add to form
this.Controls.Add(splitContainerAdv1);
```

```vb
' Create instance
Dim splitContainerAdv1 As SplitContainerAdv = New SplitContainerAdv()

' Configure basic properties
Me.splitContainerAdv1.Dock = System.Windows.Forms.DockStyle.Fill

' Add to form
Me.Controls.Add(splitContainerAdv1)
```

### Step 3: Add Child Controls to Panels

After creating the SplitContainerAdv, add controls to Panel1 and Panel2:

```csharp
// Create child controls
Label label1 = new Label();
Label label2 = new Label();

label1.Text = "Panel 1";
label1.Font = new System.Drawing.Font("Microsoft Sans Serif", 14.25F);
label1.Dock = System.Windows.Forms.DockStyle.Fill;

label2.Text = "Panel 2";
label2.Font = new System.Drawing.Font("Microsoft Sans Serif", 14.25F);
label2.Dock = System.Windows.Forms.DockStyle.Fill;

// Add controls to respective panels
this.splitContainerAdv1.Panel1.Controls.Add(label1);
this.splitContainerAdv1.Panel2.Controls.Add(label2);
```

```vb
' Create child controls
Dim label1 As New Label()
Dim label2 As New Label()

label1.Text = "Panel 1"
label1.Font = New System.Drawing.Font("Microsoft Sans Serif", 14.25F)
label1.Dock = System.Windows.Forms.DockStyle.Fill

label2.Text = "Panel 2"
label2.Font = New System.Drawing.Font("Microsoft Sans Serif", 14.25F)
label2.Dock = System.Windows.Forms.DockStyle.Fill

' Add controls to respective panels
Me.splitContainerAdv1.Panel1.Controls.Add(label1)
Me.splitContainerAdv1.Panel2.Controls.Add(label2)
```

## Splitter Orientation

The SplitContainerAdv supports two orientations for splitting panels:

### Horizontal Orientation (Default)

Splits the container vertically with Panel1 on the left and Panel2 on the right:

```csharp
this.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Horizontal;
```

```vb
Me.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Horizontal
```

### Vertical Orientation

Splits the container horizontally with Panel1 on the top and Panel2 on the bottom:

```csharp
this.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Vertical;
```

```vb
Me.splitContainerAdv1.Orientation = System.Windows.Forms.Orientation.Vertical
```

## Complete Initialization Example

```csharp
// Complete setup in Form Load event
private void Form1_Load(object sender, EventArgs e)
{
    // Create and configure SplitContainerAdv
    SplitContainerAdv splitContainer = new SplitContainerAdv();
    splitContainer.Dock = DockStyle.Fill;
    splitContainer.Orientation = Orientation.Horizontal;
    splitContainer.SplitterDistance = 200;
    
    // Create and add controls to Panel1
    TreeView treeView = new TreeView();
    treeView.Dock = DockStyle.Fill;
    splitContainer.Panel1.Controls.Add(treeView);
    
    // Create and add controls to Panel2
    RichTextBox textBox = new RichTextBox();
    textBox.Dock = DockStyle.Fill;
    splitContainer.Panel2.Controls.Add(textBox);
    
    // Add SplitContainerAdv to form
    this.Controls.Add(splitContainer);
}
```

```vb
' Complete setup in Form Load event
Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
    ' Create and configure SplitContainerAdv
    Dim splitContainer As New SplitContainerAdv()
    splitContainer.Dock = DockStyle.Fill
    splitContainer.Orientation = Orientation.Horizontal
    splitContainer.SplitterDistance = 200
    
    ' Create and add controls to Panel1
    Dim treeView As New TreeView()
    treeView.Dock = DockStyle.Fill
    splitContainer.Panel1.Controls.Add(treeView)
    
    ' Create and add controls to Panel2
    Dim textBox As New RichTextBox()
    textBox.Dock = DockStyle.Fill
    splitContainer.Panel2.Controls.Add(textBox)
    
    ' Add SplitContainerAdv to form
    Me.Controls.Add(splitContainer)
End Sub
```

## Best Practices

- **Use DockStyle.Fill** on child controls to make them fill their panel completely
- **Set SplitterDistance** to position the initial split point
- **Specify minimum sizes** using Panel1MinSize and Panel2MinSize to prevent panels from becoming too small
- **Add controls in Form_Load or designer** to ensure proper initialization
