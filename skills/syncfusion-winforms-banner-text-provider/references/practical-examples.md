# Practical Examples

## Table of Contents
- [Example 1: User Registration Form](#example-1-user-registration-form)
- [Example 2: Search Interface](#example-2-search-interface)
- [Example 3: Financial Data Entry](#example-3-financial-data-entry)
- [Example 4: Dynamic Banner Text](#example-4-dynamic-banner-text)
- [Example 5: Ribbon Integration](#example-5-ribbon-integration)

## Example 1: User Registration Form

Complete registration form with multiple input controls and consistent banner text:

```csharp
public partial class RegistrationForm : Form
{
    private BannerTextProvider bannerProvider;
    private TextBoxExt nameTextBox;
    private TextBoxExt emailTextBox;
    private TextBoxExt phoneTextBox;
    private ComboBoxAdv countryCombo;
    private TextBoxExt passwordTextBox;

    public RegistrationForm()
    {
        InitializeComponent();
        InitializeBannerText();
    }

    private void InitializeBannerText()
    {
        // Create provider
        bannerProvider = new BannerTextProvider(this.components);

        // Define banner configuration
        var bannerStyle = new Font("Segoe UI", 9, FontStyle.Italic);
        var bannerColor = SystemColors.GrayText;

        // Full Name (required, EditMode)
        var nameBanner = new BannerTextInfo()
        {
            Text = "Enter your full name",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = bannerStyle,
            Color = bannerColor
        };
        bannerProvider.SetBannerText(nameTextBox, nameBanner);

        // Email (required, EditMode)
        var emailBanner = new BannerTextInfo()
        {
            Text = "you@example.com",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = bannerStyle,
            Color = bannerColor
        };
        bannerProvider.SetBannerText(emailTextBox, emailBanner);

        // Phone (optional, FocusMode)
        var phoneBanner = new BannerTextInfo()
        {
            Text = "+1 (555) 000-0000 (optional)",
            Visible = true,
            Mode = BannerTextMode.FocusMode,
            Font = bannerStyle,
            Color = bannerColor
        };
        bannerProvider.SetBannerText(phoneTextBox, phoneBanner);

        // Country (dropdown, FocusMode)
        var countryBanner = new BannerTextInfo()
        {
            Text = "Select country...",
            Visible = true,
            Mode = BannerTextMode.FocusMode,
            Font = new Font("Segoe UI", 9),
            Color = bannerColor
        };
        bannerProvider.SetBannerText(countryCombo, countryBanner);

        // Password (required, EditMode)
        var passwordBanner = new BannerTextInfo()
        {
            Text = "Minimum 8 characters",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = new Font("Segoe UI", 9, FontStyle.Italic),
            Color = Color.DarkRed  // Indicate importance
        };
        bannerProvider.SetBannerText(passwordTextBox, passwordBanner);
    }

    private void SubmitButton_Click(object sender, EventArgs e)
    {
        // Validation logic here
        if (ValidateForm())
        {
            MessageBox.Show("Registration successful!");
        }
    }

    private bool ValidateForm()
    {
        // Validation checks
        return !string.IsNullOrWhiteSpace(nameTextBox.Text) &&
               !string.IsNullOrWhiteSpace(emailTextBox.Text) &&
               passwordTextBox.Text.Length >= 8;
    }
}
```

**Key Points:**
- Required fields use EditMode (persistent guidance)
- Optional fields use FocusMode (cleaner interface)
- Colors indicate importance (red for password hint)
- Consistent font style across all fields

## Example 2: Search Interface

Search form with multiple filter options:

```csharp
public partial class SearchForm : Form
{
    private BannerTextProvider bannerProvider;
    private TextBoxExt searchTextBox;
    private ComboBoxAdv categoryCombo;
    private DoubleTextBox minPriceBox;
    private DoubleTextBox maxPriceBox;

    public SearchForm()
    {
        InitializeComponent();
        SetupSearchInterface();
    }

    private void SetupSearchInterface()
    {
        bannerProvider = new BannerTextProvider(this.components);

        var focusStyle = new Font("Arial", 9, FontStyle.Italic);
        var editStyle = new Font("Arial", 9, FontStyle.Italic);

        // Main search box (FocusMode - should be active immediately)
        var searchBanner = new BannerTextInfo()
        {
            Text = "Search by name, SKU, or ID...",
            Visible = true,
            Mode = BannerTextMode.FocusMode,
            Font = focusStyle,
            Color = Color.FromArgb(120, 120, 120)
        };
        bannerProvider.SetBannerText(searchTextBox, searchBanner);

        // Category filter (FocusMode - optional)
        var categoryBanner = new BannerTextInfo()
        {
            Text = "All categories",
            Visible = true,
            Mode = BannerTextMode.FocusMode,
            Font = focusStyle,
            Color = SystemColors.GrayText
        };
        bannerProvider.SetBannerText(categoryCombo, categoryBanner);

        // Price range (EditMode - important for filtering)
        var minBanner = new BannerTextInfo()
        {
            Text = "Min price",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = editStyle,
            Color = Color.DarkGreen
        };
        bannerProvider.SetBannerText(minPriceBox, minBanner);

        var maxBanner = new BannerTextInfo()
        {
            Text = "Max price",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = editStyle,
            Color = Color.DarkGreen
        };
        bannerProvider.SetBannerText(maxPriceBox, maxBanner);

        // Wire up real-time search
        searchTextBox.TextChanged += (s, e) => PerformSearch();
        categoryCombo.SelectedIndexChanged += (s, e) => PerformSearch();
        minPriceBox.TextChanged += (s, e) => PerformSearch();
        maxPriceBox.TextChanged += (s, e) => PerformSearch();
    }

    private void PerformSearch()
    {
        // Search implementation
        // Results update as user types
    }
}
```

**Key Points:**
- Search field uses FocusMode for immediate interaction
- Optional filters use FocusMode
- Price fields use EditMode with semantic colors

## Example 3: Financial Data Entry

Financial form with currency and numeric controls:

```csharp
public partial class InvoiceForm : Form
{
    private BannerTextProvider bannerProvider;
    private TextBoxExt companyNameBox;
    private CurrencyTextBox amountBox;
    private PercentTextBox discountBox;
    private CurrencyTextBox taxBox;

    public InvoiceForm()
    {
        InitializeComponent();
        InitializeFinancialForm();
    }

    private void InitializeFinancialForm()
    {
        bannerProvider = new BannerTextProvider(this.components);

        // Company name (text, EditMode)
        var companyBanner = new BannerTextInfo()
        {
            Text = "Enter customer company name",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = new Font("Arial", 9, FontStyle.Italic),
            Color = SystemColors.GrayText
        };
        bannerProvider.SetBannerText(companyNameBox, companyBanner);

        // Invoice amount (currency, EditMode)
        var amountBanner = new BannerTextInfo()
        {
            Text = "Total amount in USD",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = new Font("Arial", 9, FontStyle.Italic),
            Color = Color.DarkGreen
        };
        bannerProvider.SetBannerText(amountBox, amountBanner);

        // Discount (percent, EditMode)
        var discountBanner = new BannerTextInfo()
        {
            Text = "Discount percentage (0-100)",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = new Font("Arial", 9, FontStyle.Italic),
            Color = Color.FromArgb(70, 130, 180)  // Steel blue
        };
        bannerProvider.SetBannerText(discountBox, discountBanner);

        // Tax amount (currency, EditMode)
        var taxBanner = new BannerTextInfo()
        {
            Text = "Tax amount",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = new Font("Arial", 9, FontStyle.Italic),
            Color = Color.DarkRed  // Important
        };
        bannerProvider.SetBannerText(taxBox, taxBanner);

        // Wire up calculation
        amountBox.TextChanged += (s, e) => CalculateTotal();
        discountBox.TextChanged += (s, e) => CalculateTotal();
        taxBox.TextChanged += (s, e) => CalculateTotal();
    }

    private void CalculateTotal()
    {
        // Calculate invoice total with discount and tax
        if (decimal.TryParse(amountBox.Text, out decimal amount) &&
            decimal.TryParse(discountBox.Text, out decimal discount) &&
            decimal.TryParse(taxBox.Text, out decimal tax))
        {
            decimal discountAmount = amount * (discount / 100);
            decimal subtotal = amount - discountAmount;
            decimal total = subtotal + tax;
            
            // Update total display
        }
    }
}
```

**Key Points:**
- Numeric fields use semantic colors (green for amounts, red for tax)
- All use EditMode for persistent guidance
- Hints include units and ranges for clarity
- Real-time calculations update automatically

## Example 4: Dynamic Banner Text

Update banner text based on user actions:

```csharp
public partial class DynamicBannerForm : Form
{
    private BannerTextProvider bannerProvider;
    private TextBoxExt emailBox;
    private Button validateButton;

    public DynamicBannerForm()
    {
        InitializeComponent();
        InitializeDynamicBanners();
    }

    private void InitializeDynamicBanners()
    {
        bannerProvider = new BannerTextProvider(this.components);

        var initialBanner = new BannerTextInfo()
        {
            Text = "Enter email address",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = new Font("Arial", 9, FontStyle.Italic),
            Color = SystemColors.GrayText
        };

        bannerProvider.SetBannerText(emailBox, initialBanner);
        validateButton.Click += ValidateButton_Click;
    }

    private void ValidateButton_Click(object sender, EventArgs e)
    {
        string email = emailBox.Text;

        // Validate email format
        bool isValid = IsValidEmail(email);

        // Update banner based on validation
        var newBanner = new BannerTextInfo()
        {
            Text = isValid ? "Email is valid ✓" : "Invalid email format",
            Visible = true,
            Mode = BannerTextMode.EditMode,
            Font = new Font("Arial", 9, FontStyle.Bold),
            Color = isValid ? Color.Green : Color.Red
        };

        bannerProvider.SetBannerText(emailBox, newBanner);
    }

    private bool IsValidEmail(string email)
    {
        try
        {
            var addr = new System.Net.Mail.MailAddress(email);
            return addr.Address == email;
        }
        catch
        {
            return false;
        }
    }
}
```

**Key Points:**
- Banner updates reflect validation state
- Color changes provide visual feedback
- Text content communicates status to user

## Example 5: Ribbon Integration

Search field in a Ribbon/Toolbar:

```csharp
public partial class RibbonForm : Form
{
    private BannerTextProvider bannerProvider;
    private ToolStripEx ribbon;

    public RibbonForm()
    {
        InitializeComponent();
        InitializeRibbon();
    }

    private void InitializeRibbon()
    {
        bannerProvider = new BannerTextProvider(this.components);

        // Create ToolStripTextBox for ribbon search
        ToolStripTextBox searchBox = new ToolStripTextBox()
        {
            Name = "SearchBox",
            Size = new Size(150, 20)
        };

        // Add to ribbon
        ribbon.Items.Add(new ToolStripLabel("Search: "));
        ribbon.Items.Add(searchBox);

        // Configure banner for ribbon (compact text, small font)
        var searchBanner = new BannerTextInfo()
        {
            Text = "Search...",
            Visible = true,
            Mode = BannerTextMode.FocusMode,
            Font = new Font("Arial", 8, FontStyle.Italic),
            Color = Color.LightGray
        };

        bannerProvider.SetBannerText(searchBox, searchBanner);

        // Wire up search functionality
        searchBox.TextChanged += (s, e) => PerformRibbonSearch(searchBox.Text);
    }

    private void PerformRibbonSearch(string searchTerm)
    {
        // Ribbon search implementation
    }
}
```

**Key Points:**
- Smaller font size for space-constrained ribbon
- FocusMode for minimal visual clutter
- Brief, concise hint text

---

## Common Implementation Patterns

### Pattern: Clear Control Before Setting Banner

```csharp
// IMPORTANT: Always clear the control's Text first
textBox.Text = "";  // Clear default value

// Then set banner
var banner = new BannerTextInfo()
{
    Text = "Placeholder text",
    Visible = true
};

bannerProvider.SetBannerText(textBox, banner);
```

### Pattern: Initialize All Banners in Form_Load

```csharp
private void Form_Load(object sender, EventArgs e)
{
    bannerProvider = new BannerTextProvider(this.components);
    
    // Set all banners here
    SetupNameField();
    SetupEmailField();
    SetupPhoneField();
}

private void SetupNameField()
{
    // Field-specific banner setup
}
```

### Pattern: Helper Method for Consistency

```csharp
private void ApplyBanner(Control control, string text, 
    BannerTextMode mode = BannerTextMode.EditMode, 
    Color? color = null)
{
    var banner = new BannerTextInfo()
    {
        Text = text,
        Visible = true,
        Mode = mode,
        Color = color ?? SystemColors.GrayText,
        Font = new Font("Segoe UI", 9, FontStyle.Italic)
    };

    bannerProvider.SetBannerText(control, banner);
}

// Usage:
ApplyBanner(nameBox, "Full Name");
ApplyBanner(emailBox, "Email");
ApplyBanner(searchBox, "Search...", BannerTextMode.FocusMode);
```

---

**Next:** See [customization-and-tips.md](customization-and-tips.md) for advanced techniques and troubleshooting
