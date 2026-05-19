# Getting Started with Syncfusion WinForms Rating Control

This guide covers the initial setup and basic implementation of the Syncfusion WinForms Rating control, including assembly references, control creation, and fundamental configuration.

## Assembly Deployment and NuGet Packages

### Required Assemblies

The Rating control requires the following assemblies to be referenced in your WinForms project:

**Primary Assembly:**
- `Syncfusion.Tools.Windows.dll` - Contains the RatingControl class

**Dependent Assemblies:**
- `Syncfusion.Grid.Base.dll`
- `Syncfusion.Grid.Windows.dll`
- `Syncfusion.Shared.Base.dll`
- `Syncfusion.Shared.Windows.dll`
- `Syncfusion.Tools.Base.dll`

### NuGet Installation

Install the Rating control via NuGet Package Manager:

```powershell
Install-Package Syncfusion.Tools.Windows
```

This command installs the primary package along with all required dependencies automatically.

### Manual Assembly Reference

If not using NuGet, manually add references to the assemblies from your Syncfusion installation directory:
```
C:\Program Files (x86)\Syncfusion\Essential Studio\<version>\precompiledassemblies\<framework-version>\
```

## Required Namespace

Add the following namespace to your form class:

```csharp
using Syncfusion.Windows.Forms.Tools;
```

## Adding Rating Control via Designer

### Using the Toolbox

1. Open your Windows Form in the designer
2. Locate "RatingControl" in the Syncfusion toolbox category
3. Drag and drop the RatingControl onto your form
4. Configure properties in the Properties window

The designer automatically adds the necessary namespace and initializes the control in the form's `InitializeComponent()` method.

## Adding Rating Control via Code

### Basic Code Implementation

Create and configure a RatingControl programmatically:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace RatingExample
{
    public partial class Form1 : Form
    {
        private RatingControl ratingControl1;

        public Form1()
        {
            InitializeComponent();
            InitializeRatingControl();
        }

        private void InitializeRatingControl()
        {
            // Create the RatingControl instance
            ratingControl1 = new RatingControl();
            
            // Set basic properties
            ratingControl1.Location = new System.Drawing.Point(50, 50);
            ratingControl1.Name = "ratingControl1";
            ratingControl1.Size = new System.Drawing.Size(200, 40);
            ratingControl1.TabIndex = 0;
            
            // Add to form's controls
            this.Controls.Add(ratingControl1);
        }
    }
}
```

## Setting Rating Values

### The Value Property

The `Value` property represents the current rating value. It accepts a float value ranging from 0 to the number of rating items (default is 5).

```csharp
// Set a rating of 3 stars
ratingControl1.Value = 3;

// Set a rating of 4.5 stars (requires Half precision mode)
ratingControl1.Value = 4.5f;

// Clear the rating
ratingControl1.Value = 0;
```

### Reading the Current Value

```csharp
float currentRating = ratingControl1.Value;
Console.WriteLine($"Current rating: {currentRating}");
```

## Built-in Shapes Quick Overview

The Rating control provides 6 predefined shapes accessible through the `Shape` property:

### Available Shapes

1. **Star** (Default) - Classic 5-pointed star
2. **Circle** - Circular shape
3. **Triangle** - Triangle shape
4. **Heart** - Heart symbol
5. **Diamond** - Diamond/rhombus shape
6. **Kite** - Kite-shaped symbol

### Setting a Shape

```csharp
// Use heart shapes
ratingControl1.Shape = Syncfusion.Windows.Forms.Tools.Shapes.Heart;

// Use diamond shapes
ratingControl1.Shape = Syncfusion.Windows.Forms.Tools.Shapes.Diamond;

// Use default star shapes
ratingControl1.Shape = Syncfusion.Windows.Forms.Tools.Shapes.Star;
```

## Complete Basic Implementation Example

Here's a complete example demonstrating basic setup with multiple rating controls:

```csharp
using System;
using System.Drawing;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Tools;

namespace RatingBasicExample
{
    public partial class MainForm : Form
    {
        private RatingControl productRating;
        private RatingControl serviceRating;
        private Label lblProduct;
        private Label lblService;

        public MainForm()
        {
            InitializeComponent();
            InitializeRatings();
        }

        private void InitializeRatings()
        {
            // Product rating label
            lblProduct = new Label
            {
                Text = "Product Quality:",
                Location = new Point(20, 20),
                AutoSize = true
            };

            // Product rating control with stars
            productRating = new RatingControl
            {
                Location = new Point(150, 15),
                Size = new Size(200, 40),
                Shape = Shapes.Star,
                Value = 4
            };

            // Service rating label
            lblService = new Label
            {
                Text = "Service Rating:",
                Location = new Point(20, 70),
                AutoSize = true
            };

            // Service rating control with hearts
            serviceRating = new RatingControl
            {
                Location = new Point(150, 65),
                Size = new Size(200, 40),
                Shape = Shapes.Heart,
                Value = 5
            };

            // Add controls to form
            this.Controls.Add(lblProduct);
            this.Controls.Add(productRating);
            this.Controls.Add(lblService);
            this.Controls.Add(serviceRating);
        }
    }
}
```

## Next Steps

After completing the basic setup:

1. **Explore Custom Shapes** - Learn how to use custom images for rating items
2. **Configure Precision** - Implement half-star ratings for more granular feedback
3. **Apply Styling** - Customize colors, gradients, and visual themes
4. **Add Tooltips** - Enable tooltips to display rating values on hover
5. **Set Orientation** - Use vertical orientation for sidebar layouts

## Common Initial Issues

**Issue:** Control doesn't appear on the form
- **Solution:** Ensure `Size` property is set adequately (minimum 150x30)
- **Solution:** Verify the control is added to the form's `Controls` collection

**Issue:** Rating value doesn't update when clicked
- **Solution:** Ensure the control is enabled (`Enabled = true`)
- **Solution:** Check that no other control is overlapping the rating control

**Issue:** Assembly reference errors
- **Solution:** Verify all dependent assemblies are referenced
- **Solution:** Ensure the Syncfusion version matches across all assemblies
