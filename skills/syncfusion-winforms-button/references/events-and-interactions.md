# Events & Interactions in SfButton

## Click Events

### Basic Click Event

Handle button clicks with the Click event:

```csharp
// Attach click event
sfButton1.Click += SfButton1_Click;

// Event handler
private void SfButton1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Button was clicked!");
}
```

### Lambda Expression (Modern Approach)

Inline event handling with lambda:

```csharp
sfButton1.Click += (sender, e) => 
{
    MessageBox.Show("Button clicked!");
};
```

### Multiple Event Handlers

Attach multiple handlers to one button:

```csharp
// First handler
sfButton1.Click += (sender, e) => 
{
    Console.WriteLine("Handler 1");
};

// Second handler
sfButton1.Click += (sender, e) => 
{
    Console.WriteLine("Handler 2");
};

// Both execute when button is clicked
```

### Event Handler with Button Reference

Access the sender to identify which button:

```csharp
private void Button_Click(object sender, EventArgs e)
{
    SfButton button = sender as SfButton;
    if (button != null)
    {
        MessageBox.Show($"Button '{button.Name}' was clicked");
    }
}
```

---

## Accept Button (Form Submission)

### Setting Accept Button

Set SfButton as the form's Accept button (triggered by ENTER key):

```csharp
// In Form1_Load or constructor
this.AcceptButton = sfButton1;

// When user presses ENTER, this button's Click event fires
```

### Complete Example

```csharp
public partial class LoginForm : Form
{
    private SfButton btnLogin;
    private TextBox txtUsername;
    private TextBox txtPassword;

    public LoginForm()
    {
        InitializeComponent();
        
        txtUsername = new TextBox { Location = new Point(50, 50), Size = new Size(200, 30) };
        txtPassword = new TextBox { PasswordChar = '*', Location = new Point(50, 100), Size = new Size(200, 30) };
        btnLogin = new SfButton { Text = "Login", Location = new Point(50, 150), Size = new Size(200, 40) };
        btnLogin.Click += BtnLogin_Click;
        
        this.Controls.AddRange(new Control[] { txtUsername, txtPassword, btnLogin });
        this.AcceptButton = btnLogin;  // ENTER key triggers login
    }

    private void BtnLogin_Click(object sender, EventArgs e)
    {
        string username = txtUsername.Text;
        string password = txtPassword.Text;
        
        if (ValidateCredentials(username, password))
        {
            MessageBox.Show("Login successful!");
            this.DialogResult = DialogResult.OK;
            this.Close();
        }
        else
        {
            MessageBox.Show("Invalid credentials");
        }
    }

    private bool ValidateCredentials(string username, string password)
    {
        // Validation logic
        return !string.IsNullOrEmpty(username) && password.Length > 0;
    }
}
```

**Note:** The Accept button may not activate if another control intercepts the ENTER key.

---

## Cancel Button (Form Cancellation)

Set SfButton as the form's Cancel button (triggered by ESC key):

```csharp
public partial class DialogForm : Form
{
    public DialogForm()
    {
        InitializeComponent();
        
        var btnOK = new SfButton { Text = "OK", Location = new Point(100, 150), Size = new Size(100, 40) };
        btnOK.Click += (s, e) => { this.DialogResult = DialogResult.OK; this.Close(); };
        
        var btnCancel = new SfButton { Text = "Cancel", Location = new Point(210, 150), Size = new Size(100, 40) };
        btnCancel.Click += (s, e) => { this.DialogResult = DialogResult.Cancel; this.Close(); };
        
        this.Controls.AddRange(new Control[] { btnOK, btnCancel });
        this.CancelButton = btnCancel;  // ESC key triggers cancel
    }
}
```

**Note:** The Cancel button may not work if another control intercepts the ESC key.

---

## Tooltip Support

### Using SfToolTip

Display tooltips when user hovers over button:

```csharp
using Syncfusion.WinForms.Controls;

// Create SfToolTip instance
SfToolTip sfToolTip1 = new SfToolTip();

// Attach tooltip to button
sfToolTip1.SetToolTip(this.sfButton1, "Click to save changes");
```

### Multiple Tooltips

```csharp
// Attach tooltips to multiple buttons
var toolTip = new SfToolTip();
toolTip.SetToolTip(btnSave, "Save the current document (Ctrl+S)");
toolTip.SetToolTip(btnDelete, "Delete selected item (this cannot be undone)");
toolTip.SetToolTip(btnExit, "Close application");

// Tooltips appear on hover and auto-hide after 5-10 seconds
```

---

## Focus Handling

### Setting Focus

Programmatically set focus to a button:

```csharp
// Set focus to the button
sfButton1.Focus();

// Check if button has focus
if (sfButton1.Focused)
{
    Console.WriteLine("Button has focus");
}
```

### Tab Order

Control the order buttons receive focus via TAB key:

```csharp
// Set TabIndex (0-based, lower = earlier in tab order)
btnFirst.TabIndex = 0;
btnSecond.TabIndex = 1;
btnThird.TabIndex = 2;

// User presses TAB to move between buttons in order
```

### Enabling/Disabling Focus

```csharp
// Allow focus (default)
sfButton1.TabStop = true;

// Skip button in tab order
sfButton1.TabStop = false;
```

### Focus Rectangle

Show visual indicator when button has focus:

```csharp
// Show focus rectangle when button is focused
sfButton1.FocusRectangleVisible = true;
```

---

## Enabled/Disabled State

### Disabling Buttons

Disable buttons for unavailable actions:

```csharp
// Disable the button
sfButton1.Enabled = false;

// Enable the button
sfButton1.Enabled = true;

// Check if button is enabled
if (sfButton1.Enabled)
{
    Console.WriteLine("Button is clickable");
}
```

### Visual Feedback

When disabled, button shows:
- Grayed-out appearance
- Disabled state colors (if configured)
- No response to clicks
- No tooltip display

### Conditional Disabling Example

```csharp
// Enable button only when input is not empty
btnSubmit.Enabled = false;  // Initially disabled
txtInput.TextChanged += (s, e) => 
{
    btnSubmit.Enabled = !string.IsNullOrWhiteSpace(txtInput.Text);
};
```

---

## Complete Interaction Example

```csharp
public partial class InteractionDemo : Form
{
    private TextBox txtLog;
    private SfButton btnAction, btnCancel;

    public InteractionDemo()
    {
        InitializeComponent();
        this.Text = "Button Interaction Demo";
        this.Size = new Size(400, 300);

        txtLog = new TextBox 
        { 
            Location = new Point(20, 20), Size = new Size(360, 150), 
            Multiline = true, ReadOnly = true,
            Text = "Ready. Press Tab to navigate, Enter to execute, Esc to close."
        };

        btnAction = new SfButton { Text = "Perform Action", Location = new Point(80, 180), Size = new Size(120, 40) };
        btnAction.Click += (s, e) => txtLog.AppendText($"\r\n[{DateTime.Now:HH:mm:ss}] Action performed!");
        btnAction.Focus();

        btnCancel = new SfButton { Text = "Close", Location = new Point(210, 180), Size = new Size(120, 40) };
        btnCancel.Click += (s, e) => { txtLog.AppendText("\r\nClosing form..."); this.Close(); };

        var toolTip = new SfToolTip();
        toolTip.SetToolTip(btnAction, "Click to perform the action (Enter key works)");
        toolTip.SetToolTip(btnCancel, "Close this form (Escape key works)");
        
        this.Controls.AddRange(new Control[] { txtLog, btnAction, btnCancel });
        this.AcceptButton = btnAction;
        this.CancelButton = btnCancel;
    }
}
```

---

## Best Practices for Events

### 1. Clear Event Handlers Before Removal

```csharp
// Detach event handler
sfButton1.Click -= SfButton1_Click;
```

### 2. Use Null Coalescing for Safety

```csharp
private void BtnClick_Click(object sender, EventArgs e)
{
    var button = sender as SfButton ?? throw new InvalidOperationException();
    Console.WriteLine($"Button {button.Name} clicked");
}
```

### 3. Handle Exceptions in Click Handlers

```csharp
sfButton1.Click += (sender, e) =>
{
    try
    {
        // Button action logic
        PerformAction();
    }
    catch (Exception ex)
    {
        MessageBox.Show($"Error: {ex.Message}", "Error");
    }
};
```

### 4. Separate Complex Logic into Methods

```csharp
private void BtnSave_Click(object sender, EventArgs e)
{
    if (ValidateForm())
    {
        SaveData();
        ShowSuccessMessage();
    }
    else
    {
        ShowErrorMessage();
    }
}
```

### 5. Respect Enabled State

```csharp
// Check enabled before processing in event handler
private void BtnAction_Click(object sender, EventArgs e)
{
    if (!btnAction.Enabled)
        return;  // Ignore click if disabled
    
    // Process action
}
```
