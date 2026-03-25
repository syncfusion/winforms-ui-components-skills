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
        
        // Create username textbox
        txtUsername = new TextBox();
        txtUsername.Location = new Point(50, 50);
        txtUsername.Size = new Size(200, 30);
        this.Controls.Add(txtUsername);

        // Create password textbox
        txtPassword = new TextBox();
        txtPassword.PasswordChar = '*';
        txtPassword.Location = new Point(50, 100);
        txtPassword.Size = new Size(200, 30);
        this.Controls.Add(txtPassword);

        // Create login button
        btnLogin = new SfButton();
        btnLogin.Text = "Login";
        btnLogin.Location = new Point(50, 150);
        btnLogin.Size = new Size(200, 40);
        btnLogin.Click += BtnLogin_Click;
        this.Controls.Add(btnLogin);

        // Set as Accept button (ENTER key triggers login)
        this.AcceptButton = btnLogin;
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

### Accept Button Note

**Important:** The Accept button may not activate if another control on the form intercepts the ENTER key (like TextBox).

---

## Cancel Button (Form Cancellation)

### Setting Cancel Button

Set SfButton as the form's Cancel button (triggered by ESC key):

```csharp
// In Form1_Load or constructor
this.CancelButton = sfButton2;

// When user presses ESC, this button's Click event fires
```

### Complete Example

```csharp
public partial class DialogForm : Form
{
    private SfButton btnOK;
    private SfButton btnCancel;

    public DialogForm()
    {
        InitializeComponent();
        
        // OK button
        btnOK = new SfButton();
        btnOK.Text = "OK";
        btnOK.Location = new Point(100, 150);
        btnOK.Size = new Size(100, 40);
        btnOK.Click += BtnOK_Click;
        this.Controls.Add(btnOK);

        // Cancel button
        btnCancel = new SfButton();
        btnCancel.Text = "Cancel";
        btnCancel.Location = new Point(210, 150);
        btnCancel.Size = new Size(100, 40);
        btnCancel.Click += BtnCancel_Click;
        this.Controls.Add(btnCancel);

        // Set Cancel button for ESC key
        this.CancelButton = btnCancel;
    }

    private void BtnOK_Click(object sender, EventArgs e)
    {
        this.DialogResult = DialogResult.OK;
        this.Close();
    }

    private void BtnCancel_Click(object sender, EventArgs e)
    {
        this.DialogResult = DialogResult.Cancel;
        this.Close();
    }
}
```

### Cancel Button Note

**Important:** The Cancel button may not work if another control on the form intercepts the ESC key.

---

## Tooltip Support

### Using SfToolTip

Display tooltips when user hovers over button:

```csharp
using Syncfusion.WinForms.Buttons;

// Create SfToolTip instance
SfToolTip sfToolTip1 = new SfToolTip();

// Attach tooltip to button
sfToolTip1.SetToolTip(this.sfButton1, "Click to save changes");
```

### Complete Tooltip Example

```csharp
public partial class Form1 : Form
{
    private SfButton btnSave;
    private SfButton btnDelete;
    private SfToolTip toolTip;

    public Form1()
    {
        InitializeComponent();
        
        // Create tooltip
        toolTip = new SfToolTip();
        
        // Save button with tooltip
        btnSave = new SfButton();
        btnSave.Text = "Save";
        btnSave.Location = new Point(50, 50);
        btnSave.Size = new Size(100, 40);
        this.Controls.Add(btnSave);
        toolTip.SetToolTip(btnSave, "Save the current document (Ctrl+S)");

        // Delete button with tooltip
        btnDelete = new SfButton();
        btnDelete.Text = "Delete";
        btnDelete.Location = new Point(160, 50);
        btnDelete.Size = new Size(100, 40);
        this.Controls.Add(btnDelete);
        toolTip.SetToolTip(btnDelete, "Delete selected item (this cannot be undone)");
    }
}
```

### Tooltip Features

```csharp
// Set multiple tooltips
sfToolTip.SetToolTip(btnSave, "Save document");
sfToolTip.SetToolTip(btnDelete, "Delete item");
sfToolTip.SetToolTip(btnExit, "Close application");

// Tooltip appears on hover
// Disappears when mouse leaves button
// Auto-hides after 5-10 seconds
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
public partial class Form1 : Form
{
    private TextBox txtInput;
    private SfButton btnSubmit;

    public Form1()
    {
        InitializeComponent();
        
        txtInput = new TextBox();
        txtInput.Location = new Point(50, 50);
        txtInput.Size = new Size(200, 30);
        txtInput.TextChanged += TxtInput_TextChanged;
        this.Controls.Add(txtInput);

        btnSubmit = new SfButton();
        btnSubmit.Text = "Submit";
        btnSubmit.Location = new Point(50, 100);
        btnSubmit.Size = new Size(200, 40);
        btnSubmit.Enabled = false;  // Initially disabled
        this.Controls.Add(btnSubmit);
    }

    private void TxtInput_TextChanged(object sender, EventArgs e)
    {
        // Enable button only if input is not empty
        btnSubmit.Enabled = !string.IsNullOrWhiteSpace(txtInput.Text);
    }
}
```

---

## Complete Interaction Example

```csharp
public partial class InteractionDemo : Form
{
    private SfButton btnAction;
    private SfButton btnCancel;
    private TextBox txtLog;
    private SfToolTip toolTip;

    public InteractionDemo()
    {
        InitializeComponent();
        
        // Configure form
        this.Text = "Button Interaction Demo";
        this.Size = new Size(400, 300);

        // Log textbox
        txtLog = new TextBox();
        txtLog.Location = new Point(20, 20);
        txtLog.Size = new Size(360, 150);
        txtLog.Multiline = true;
        txtLog.ReadOnly = true;
        this.Controls.Add(txtLog);

        // Action button
        btnAction = new SfButton();
        btnAction.Text = "Perform Action";
        btnAction.Location = new Point(80, 180);
        btnAction.Size = new Size(120, 40);
        btnAction.Click += BtnAction_Click;
        btnAction.Focus();  // Initial focus
        this.Controls.Add(btnAction);

        // Cancel button
        btnCancel = new SfButton();
        btnCancel.Text = "Close";
        btnCancel.Location = new Point(210, 180);
        btnCancel.Size = new Size(120, 40);
        btnCancel.Click += BtnCancel_Click;
        this.Controls.Add(btnCancel);

        // Setup tooltips
        toolTip = new SfToolTip();
        toolTip.SetToolTip(btnAction, "Click to perform the action (Enter key works)");
        toolTip.SetToolTip(btnCancel, "Close this form (Escape key works)");

        // Set Accept and Cancel buttons
        this.AcceptButton = btnAction;
        this.CancelButton = btnCancel;

        // Initial message
        txtLog.Text = "Ready. Press Tab to navigate, Enter to execute, Esc to close.";
    }

    private void BtnAction_Click(object sender, EventArgs e)
    {
        txtLog.AppendText($"\r\n[{DateTime.Now:HH:mm:ss}] Action performed!");
    }

    private void BtnCancel_Click(object sender, EventArgs e)
    {
        txtLog.AppendText("\r\nClosing form...");
        this.Close();
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
