---
layout: post
title: UI Automation in Windows Forms ComboBox control | Syncfusion
description: Learn about UI Automation support in Syncfusion Windows Forms ComboBox (SfComboBox) control and more details.
## UI Automation in Windows Forms ComboBox (SfComboBox)

Microsoft UI Automation provides accessibility and automated testing support for WinForms controls. SfComboBox supports Coded UI (CUIT) and Quick Test Professional (QTP/UFT) scenarios.

### CUIT support levels

| Level | Description |
|-------|-------------|
| **Level 1** | Record and playback: Recorder identifies elements involved in an action; playback uses generated code via Microsoft Active Accessibility. |
| **Level 2** | Property validation: Default properties are available per control type for adding assertions. |

### Requirements and configuration

Coded UI works with certain Visual Studio SKUs (see Microsoft docs linked in original guide).

### Enable assertion

```csharp
// To enable accessibility
this.sfComboBox1.AccessibilityEnabled = true;
```

```vbnet
' To enable accessibility
Me.sfComboBox1.AccessibilityEnabled = True
```

### Getting started

Follow the original doc steps to create a CodedUITest project and use CodedUITestBuilder to record/assert UI interactions.

![CodedUI_Create](CodedUI-Automation-Images/CodedUI_Create.jpg)

Refer to the product docs for full screenshots and step-by-step instructions.

## QTP

Refer to the UFT/QTP testing docs for SfComboBox automation.
</td>
