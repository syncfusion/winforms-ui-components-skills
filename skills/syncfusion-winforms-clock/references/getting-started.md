# Getting Started with the Clock Control

## Assembly References

Add the following assemblies to your WinForms project:

| Assembly | Purpose |
|---|---|
| `Syncfusion.Grid.Base` | Core grid infrastructure |
| `Syncfusion.Grid.Windows` | WinForms grid rendering |
| `Syncfusion.Shared.Base` | Shared base components |
| `Syncfusion.Shared.Windows` | Shared WinForms components |
| `Syncfusion.Tools.Base` | Tools base (includes Clock) |
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

## Adding Clock via Designer

1. Create a new Windows Forms project in Visual Studio.
2. Drag the **Clock** control from the Toolbox onto the form.
3. All required assemblies are added automatically.

---

## Adding Clock Programmatically

**C#:**
```csharp
Clock clock1 = new Clock();
this.Controls.Add(clock1);
```

**VB.NET:**
```vb
Dim clock1 As New Clock()
Me.Controls.Add(clock1)
```

The clock immediately begins displaying the current system time in analog mode by default.

---

## Switching Between Analog and Digital

Use the `ClockType` property with the `ClockTypes` enum:

**C# — Digital:**
```csharp
clock1.ClockType = Syncfusion.Windows.Forms.Tools.ClockTypes.Digital;
```

**C# — Analog:**
```csharp
clock1.ClockType = Syncfusion.Windows.Forms.Tools.ClockTypes.Analog;
```

**VB.NET:**
```vb
clock1.ClockType = Syncfusion.Windows.Forms.Tools.ClockTypes.Digital
clock1.ClockType = Syncfusion.Windows.Forms.Tools.ClockTypes.Analog
```

| `ClockTypes` Value | Description |
|---|---|
| `Analog` | Traditional round clock face with hands (default) |
| `Digital` | Text-based time display with optional frames and shapes |

---

## Displaying a Custom or Fixed Time

To show a time other than the current system time:

1. Set `ShowCustomTimeClock = true`
2. Assign a `DateTime` value to `CustomTime`

**C#:**
```csharp
clock1.ShowCustomTimeClock = true;
clock1.CustomTime = new System.DateTime(2019, 7, 3, 16, 50, 1, 0);
```

**VB.NET:**
```vb
clock1.ShowCustomTimeClock = True
clock1.CustomTime = New System.DateTime(2019, 7, 3, 16, 50, 1, 0)
```

> The clock will display the specified time and continue advancing from that point. To freeze it completely, use `StopTimer` instead.

---

## Freezing the Clock (StopTimer)

To display a fixed, non-advancing time:

**C#:**
```csharp
// Freeze the clock at the current moment
clock1.StopTimer = true;
```

**VB.NET:**
```vb
Me.clock1.StopTimer = True
```

- When `StopTimer = true`, the clock stops updating and holds its current display.
- Set `StopTimer = false` to resume live time display.
- Combine with `ShowCustomTimeClock` + `CustomTime` to freeze at a specific time value.
