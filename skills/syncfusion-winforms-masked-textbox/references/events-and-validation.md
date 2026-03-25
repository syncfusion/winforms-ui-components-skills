# Events and Validation

Handle user interaction and validate input using the MaskedEditBox control's comprehensive event model.

## Available Events

The MaskedEditBox control exposes standard WinForms events plus specialized input events:

### Standard TextBox Events

| Event | Triggers | Use Case |
|-------|----------|----------|
| `TextChanged` | Any text modification | Track input changes, real-time validation |
| `Leave` | Focus leaves control | Field-level validation, cleanup |
| `Enter` | Control receives focus | Setup, clear previous state |
| `KeyDown` | Key is pressed | Intercept special keys, prevent invalid input |
| `KeyPress` | Character is entered | Filter allowed characters |
| `KeyUp` | Key is released | Post-key processing |
| `Validating` | Validation is triggered | Complex validation logic |
| `Validated` | Validation passes | Success handling |

### Focus and Input Events

```csharp
// When user enters field
maskedEditBox.Enter += (sender, e) => 
{
    // Select all text, show hint
    maskedEditBox.SelectAll();
};

// When user leaves field
maskedEditBox.Leave += (sender, e) =>
{
    // Validate, format, save
    ValidateAndSave();
};

// When text changes
maskedEditBox.TextChanged += (sender, e) =>
{
    // Real-time feedback
    UpdateValidationStatus();
};
```

## Real-Time Input Validation

### Track Input as User Types

```csharp
private void ConfigureRealTimeValidation()
{
    maskedEditBox.Mask = "(###) ###-####";
    maskedEditBox.TextChanged += MaskedEditBox_TextChanged;
}

private void MaskedEditBox_TextChanged(object sender, EventArgs e)
{
    // Provide real-time feedback
    if (string.IsNullOrEmpty(maskedEditBox.Value))
    {
        statusLabel.Text = "Enter phone number";
        statusLabel.ForeColor = Color.Gray;
    }
    else if (maskedEditBox.Value.Length < 10)
    {
        statusLabel.Text = "Incomplete (" + maskedEditBox.Value.Length + "/10)";
        statusLabel.ForeColor = Color.Orange;
    }
    else if (maskedEditBox.Value.Length == 10)
    {
        statusLabel.Text = "✓ Valid";
        statusLabel.ForeColor = Color.Green;
    }
}
```

### Progress Indicator

```csharp
private void ShowInputProgress()
{
    maskedEditBox.TextChanged += (sender, e) =>
    {
        int filled = maskedEditBox.Value.Length;
        int total = maskedEditBox.Mask.Count(c => c == '#');
        
        progressBar.Maximum = total;
        progressBar.Value = filled;
        
        double percent = (filled / (double)total) * 100;
        percentLabel.Text = percent.ToString("F0") + "%";
    };
}
```

## Field-Level Validation

### Validate on Leave Event

```csharp
private void ValidatePhoneOnLeave()
{
    maskedEditBox.Mask = "(###) ###-####";
    maskedEditBox.Leave += (sender, e) =>
    {
        if (string.IsNullOrEmpty(maskedEditBox.Value))
        {
            errorLabel.Text = "Phone number is required";
            errorLabel.Visible = true;
            maskedEditBox.BackColor = Color.LightPink;
        }
        else if (maskedEditBox.Value.Length != 10)
        {
            errorLabel.Text = "Phone must be 10 digits";
            errorLabel.Visible = true;
            maskedEditBox.BackColor = Color.LightPink;
        }
        else if (!IsValidPhoneNumber(maskedEditBox.Value))
        {
            errorLabel.Text = "Invalid phone number";
            errorLabel.Visible = true;
            maskedEditBox.BackColor = Color.LightPink;
        }
        else
        {
            errorLabel.Visible = false;
            maskedEditBox.BackColor = Color.White;
        }
    };
}

private bool IsValidPhoneNumber(string phone)
{
    // Example: prevent 555-0100 series
    return !phone.StartsWith("555010");
}
```

## Specialized Input Events

### Prevent Invalid Sequences

```csharp
private void PreventInvalidAreaCodes()
{
    maskedEditBox.Mask = "(###) ###-####";
    maskedEditBox.TextChanged += (sender, e) =>
    {
        // After first 3 digits entered
        if (maskedEditBox.Value.Length >= 3)
        {
            int areaCode = int.Parse(maskedEditBox.Value.Substring(0, 3));
            
            // Reject 000, 001, 555
            if (areaCode == 0 || areaCode == 1 || areaCode == 555)
            {
                MessageBox.Show("Invalid area code");
                maskedEditBox.Value = "";
            }
        }
    };
}
```

### Block Repeated Patterns

```csharp
private void PreventRepeatedDigits()
{
    maskedEditBox.TextChanged += (sender, e) =>
    {
        string value = maskedEditBox.Value;
        
        // Prevent all same digit (e.g., "1111111111")
        if (value.Length > 0 && value.All(c => c == value[0]))
        {
            MessageBox.Show("All digits cannot be the same");
            maskedEditBox.Value = "";
        }
    };
}
```

## KeyPress and KeyDown Events

### Intercept Key Input

```csharp
private void ConfigureKeyHandling()
{
    maskedEditBox.KeyPress += (sender, e) =>
    {
        // Allow digits and special keys
        if (!char.IsDigit(e.KeyChar) && e.KeyChar != 8) // 8 = backspace
        {
            e.Handled = true;  // Block character
        }
    };
}

private void PreventSpecialKeys()
{
    maskedEditBox.KeyDown += (sender, e) =>
    {
        // Allow: Tab, Backspace, Delete, Ctrl+A/C/V/X, Arrow keys
        bool allowed = e.KeyCode == Keys.Tab ||
                       e.KeyCode == Keys.Back ||
                       e.KeyCode == Keys.Delete ||
                       (e.Control && (e.KeyCode == Keys.A || 
                                      e.KeyCode == Keys.C || 
                                      e.KeyCode == Keys.V || 
                                      e.KeyCode == Keys.X)) ||
                       e.KeyCode >= Keys.Left && e.KeyCode <= Keys.Down;
        
        if (!allowed)
        {
            e.Handled = true;
        }
    };
}
```

## Conditional Validation

### Validate Based on Business Rules

```csharp
private void ValidateWithBusinessLogic()
{
    maskedEditBox.Leave += async (sender, e) =>
    {
        string phoneValue = maskedEditBox.Value;
        
        if (phoneValue.Length != 10)
            return;
        
        // Check if phone already exists
        bool exists = await DatabaseService.PhoneExistsAsync(phoneValue);
        
        if (exists)
        {
            errorLabel.Text = "Phone number already registered";
            maskedEditBox.BackColor = Color.LightPink;
        }
        else
        {
            maskedEditBox.BackColor = Color.White;
            errorLabel.Visible = false;
        }
    };
}
```

## Error Display and Messaging

### Error Label with Icon

```csharp
private void SetupErrorDisplay()
{
    // Error label
    Label errorLabel = new Label();
    errorLabel.ForeColor = Color.Red;
    errorLabel.Font = new Font("Arial", 9);
    errorLabel.Visible = false;
    
    // Error icon
    PictureBox errorIcon = new PictureBox();
    errorIcon.Image = SystemIcons.Error.ToBitmap();
    errorIcon.SizeMode = PictureBoxSizeMode.AutoSize;
    errorIcon.Visible = false;
    
    maskedEditBox.TextChanged += (sender, e) =>
    {
        if (maskedEditBox.Value.Length == 0)
        {
            errorLabel.Visible = false;
            errorIcon.Visible = false;
        }
        else if (maskedEditBox.Value.Length < 10)
        {
            errorLabel.Text = "Incomplete";
            errorLabel.ForeColor = Color.Orange;
            errorLabel.Visible = true;
            errorIcon.Visible = true;
        }
        else
        {
            errorLabel.Text = "✓ Valid";
            errorLabel.ForeColor = Color.Green;
            errorIcon.Visible = false;
        }
    };
}
```

## Complete Validation Example

```csharp
private class PhoneInputForm : Form
{
    private MaskedEditBox phoneField;
    private Label statusLabel;
    private Button submitButton;

    public PhoneInputForm()
    {
        // Setup controls
        phoneField = new MaskedEditBox();
        phoneField.Mask = "(###) ###-####";
        phoneField.Location = new Point(10, 10);
        phoneField.Size = new Size(200, 25);
        
        statusLabel = new Label();
        statusLabel.Location = new Point(10, 40);
        statusLabel.Size = new Size(200, 20);
        
        submitButton = new Button();
        submitButton.Text = "Submit";
        submitButton.Location = new Point(10, 70);
        submitButton.Click += SubmitButton_Click;
        
        // Add to form
        this.Controls.Add(phoneField);
        this.Controls.Add(statusLabel);
        this.Controls.Add(submitButton);
        
        // Setup events
        phoneField.TextChanged += PhoneField_TextChanged;
        phoneField.Leave += PhoneField_Leave;
        
        this.Text = "Phone Input";
        this.Size = new Size(300, 150);
    }

    private void PhoneField_TextChanged(object sender, EventArgs e)
    {
        // Real-time validation
        string value = phoneField.Value;
        
        if (string.IsNullOrEmpty(value))
        {
            statusLabel.Text = "";
        }
        else if (value.Length < 10)
        {
            statusLabel.Text = $"Incomplete: {value.Length}/10";
            statusLabel.ForeColor = Color.Orange;
        }
        else
        {
            statusLabel.Text = "✓ Valid phone";
            statusLabel.ForeColor = Color.Green;
        }
    }

    private void PhoneField_Leave(object sender, EventArgs e)
    {
        // Field-level validation
        if (!ValidatePhoneField())
        {
            phoneField.Focus();
            phoneField.SelectAll();
        }
    }

    private bool ValidatePhoneField()
    {
        string phone = phoneField.Value;
        
        // Require complete input
        if (phone.Length != 10)
        {
            statusLabel.Text = "Required field";
            statusLabel.ForeColor = Color.Red;
            phoneField.BackColor = Color.LightPink;
            return false;
        }
        
        // Reject invalid patterns
        if (phone.StartsWith("000") || phone.StartsWith("555"))
        {
            statusLabel.Text = "Invalid phone number";
            statusLabel.ForeColor = Color.Red;
            phoneField.BackColor = Color.LightPink;
            return false;
        }
        
        // Valid
        phoneField.BackColor = Color.White;
        return true;
    }

    private void SubmitButton_Click(object sender, EventArgs e)
    {
        if (ValidatePhoneField())
        {
            MessageBox.Show("Phone saved: " + phoneField.Value);
        }
    }
}
```

## Best Practices

1. **Real-time feedback** - Show validation status as user types for better UX
2. **Clear error messages** - Use specific messages (e.g., "Area code invalid" not just "Invalid")
3. **Visual indicators** - Use colors, icons to indicate valid/invalid states
4. **Prevent data loss** - Save or recover incomplete input before losing focus
5. **Allow corrections** - Let users fix errors without clearing entire field
6. **Performance** - Keep event handlers lightweight to avoid lag
7. **Test edge cases** - Test with boundary values (000, 555, etc.)
8. **Accessibility** - Announce errors to screen readers via labels
