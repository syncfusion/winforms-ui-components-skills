# High DPI Support in Syncfusion WinForms Controls

DPI (Dots Per Inch) refers to the number of pixels rendered per inch. High DPI displays pack more pixels into each inch, resulting in sharper visuals — but only for applications that are DPI-aware.

---

## DPI Awareness Overview

### Non-DPI-Aware Applications

- Always rendered at 96 DPI
- The OS stretches/scales the entire window as a bitmap
- Results in blurry text, icons, and controls at higher DPI settings

### DPI-Aware Applications

- Render at the actual monitor DPI
- Controls, text, and images are sharp at all DPI settings
- Requires explicit opt-in via manifest or code

---

## DPI Compatibility Test Matrix

Test your application at these recommended DPI settings:

| DPI | Windows Scaling | Minimum Resolution |
|---|---|---|
| 96 | 100% | 1024 × 720 |
| 120 | 125% | 1280 × 960 |
| 144 | 150% | 1536 × 1080 |
| 192 | 200% | 2048 × 1440 |

---

## DPI Scaling Behavior

WinForms uses two properties on `ContainerControl` (e.g., `Form`) to handle DPI scaling:

- **`AutoScaleDimensions`** — Visual Studio serializes the DPI or font dimensions used when designing the form. At runtime, the framework compares current DPI to this value and computes a scale factor.
- **`AutoScaleMode`** — Controls how the scale factor is calculated (`Font` or `Dpi`). Set to `None` to disable automatic scaling.

```csharp
this.AutoScaleDimensions = new System.Drawing.SizeF(5F, 12F);
this.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
```

```vb
Me.AutoScaleDimensions = New System.Drawing.SizeF(5F, 12F)
Me.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font
```

---

## Enabling DPI Awareness via App Manifest

Add an application manifest to mark the application as DPI-aware:

1. Right-click the project in Solution Explorer → **Add > New Item**.
2. Search for **Application Manifest File** → click **Add**.
3. Set `dpiAware` to `true` in the manifest:

```xml
<application xmlns="urn:schemas-microsoft-com:asm.v3">
  <windowsSettings>
    <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
  </windowsSettings>
</application>
```

> Most Syncfusion WinForms controls support high DPI through the manifest file alone. The Grid family of controls also needs the `DpiAware` property set in code (see below).

---

## Enabling DpiAware for Grid Controls

The following controls require both the manifest file **and** the `DpiAware` property to be set:

- `GridControl`
- `GridGroupingControl`
- `GridDataBoundGrid`
- `GridListControl`

```csharp
// GridControl
gridControl1.DpiAware = true;

// GridDataBoundGrid
gridDataBoundGrid1.DpiAware = true;

// GridGroupingControl
this.gridGroupingControl1.TableControl.DpiAware = true;

// GridListControl
gridListControl1.Grid.DpiAware = true;
```

```vb
gridControl1.DpiAware = True
gridDataBoundGrid1.DpiAware = True
Me.gridGroupingControl1.TableControl.DpiAware = True
gridListControl1.Grid.DpiAware = True
```

---

## Auto-Switching Images by DPI with ImageListAdv

The `ImageListAdv` component (from `Syncfusion.Tools.Windows`) lets you define different images for different DPI scaling levels. The correct image is selected automatically at runtime.

### DPIAwareImage Properties

| Property | Applies at Scaling |
|---|---|
| `DPI120Image` | 125% and above |
| `DPI144Image` | 150% and above |
| `DPI192Image` | 200% and above |
| `Index` | Maps to default image index in `Images` collection |

> If no DPI-specific image is set for a given scaling level, the control falls back to the lower scaling image or the default from the `Images` collection.

### Setup Steps

1. Drag **ImageListAdv** from the Toolbox to the designer.
2. Add default images to the `Images` collection — these display at 100% scaling and as fallbacks.
3. Open the `DPIImages` collection and click **Add** to create a `DPIAwareImage` item.
4. Set `DPI120Image`, `DPI144Image`, and `DPI192Image` to the appropriate higher-resolution images.
5. Set the `Index` property to match the position of the corresponding default image in `Images`.
6. Bind the control to the default image in code:

```csharp
buttonAdv1.Image = imageListAdv1.Images[0];
```

```vb
buttonAdv1.Image = imageListAdv1.Images(0)
```

At runtime, `ImageListAdv` automatically serves the DPI-appropriate image when scaling changes.

---

## Gotchas

- **Manifest required even with DpiAware property:** For Grid controls, both the manifest and the `DpiAware` property are needed — one without the other won't work.
- **Designer DPI mismatch:** If you design a form on a 100% DPI machine and run it on a 200% DPI machine, `AutoScaleDimensions` ensures the layout is rescaled. Always let the Visual Studio designer serialize this value — don't remove it.
- **Image blurriness at high DPI:** Provide separate higher-resolution images via `ImageListAdv.DPIImages` to avoid blurry icons at 150% and 200% scaling.
