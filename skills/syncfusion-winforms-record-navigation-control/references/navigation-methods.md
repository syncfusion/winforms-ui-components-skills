# Navigation Methods

This guide covers the built-in navigation methods provided by RecordNavigationControl for programmatic record navigation.

## Overview

RecordNavigationControl provides four core navigation methods that allow you to programmatically navigate between records in your grid:

- **MoveFirst()** - Navigate to the first record
- **MoveLast()** - Navigate to the last record
- **MoveNext()** - Navigate to the next record
- **MovePrevious()** - Navigate to the previous record

These methods are accessible through the control's NavigationBar property.

## Navigation Method Reference

### MoveFirst()

Navigates to the first record in the dataset.

**Signature:**
```csharp
void MoveFirst()
```

**Parameters:** None

**Return Type:** void

**Usage:**
```csharp
this.recordNavigationControl1.NavigationBar.MoveFirst();
```

**VB.NET:**
```vb
Me.recordNavigationControl1.NavigationBar.MoveFirst()
```

**When to Use:**
- Initialize view at the first record
- Reset navigation after filtering or searching
- Implement "Go to Beginning" functionality
- Return to start after reaching the end

### MoveLast()

Navigates to the last record in the dataset.

**Signature:**
```csharp
void MoveLast()
```

**Parameters:** None

**Return Type:** void

**Usage:**
```csharp
this.recordNavigationControl1.NavigationBar.MoveLast();
```

**VB.NET:**
```vb
Me.recordNavigationControl1.NavigationBar.MoveLast()
```

**When to Use:**
- Jump to the end of the dataset
- View most recent entries (if sorted by date)
- Implement "Go to End" functionality
- Navigate to newly added records

### MoveNext()

Navigates to the next record in the dataset. If already at the last record, stays at the current position.

**Signature:**
```csharp
void MoveNext()
```

**Parameters:** None

**Return Type:** void

**Usage:**
```csharp
this.recordNavigationControl1.NavigationBar.MoveNext();
```

**VB.NET:**
```vb
Me.recordNavigationControl1.NavigationBar.MoveNext()
```

**When to Use:**
- Sequential record browsing
- Step through records one by one
- Implement forward navigation buttons
- Automated record processing loops

### MovePrevious()

Navigates to the previous record in the dataset. If already at the first record, stays at the current position.

**Signature:**
```csharp
void MovePrevious()
```

**Parameters:** None

**Return Type:** void

**Usage:**
```csharp
this.recordNavigationControl1.NavigationBar.MovePrevious();
```

**VB.NET:**
```vb
Me.recordNavigationControl1.NavigationBar.MovePrevious()
```

**When to Use:**
- Navigate backward through records
- Undo navigation actions
- Implement back buttons
- Review previous entries

## Complete Navigation Implementation

### Example: Custom Navigation Buttons

Add custom buttons to your form for programmatic navigation:

```csharp
using System;
using System.Windows.Forms;
using Syncfusion.Windows.Forms.Grid;

public partial class NavigationForm : Form
{
    private RecordNavigationControl recordNavigationControl1;
    private Button btnFirst, btnPrevious, btnNext, btnLast;
    
    private void InitializeNavigationButtons()
    {
        // Create navigation buttons
        btnFirst = new Button { Text = "First", Location = new System.Drawing.Point(10, 420) };
        btnPrevious = new Button { Text = "Previous", Location = new System.Drawing.Point(90, 420) };
        btnNext = new Button { Text = "Next", Location = new System.Drawing.Point(180, 420) };
        btnLast = new Button { Text = "Last", Location = new System.Drawing.Point(260, 420) };
        
        // Wire up event handlers
        btnFirst.Click += BtnFirst_Click;
        btnPrevious.Click += BtnPrevious_Click;
        btnNext.Click += BtnNext_Click;
        btnLast.Click += BtnLast_Click;
        
        // Add to form
        this.Controls.AddRange(new Control[] { btnFirst, btnPrevious, btnNext, btnLast });
    }
    
    private void BtnFirst_Click(object sender, EventArgs e)
    {
        recordNavigationControl1.NavigationBar.MoveFirst();
    }
    
    private void BtnPrevious_Click(object sender, EventArgs e)
    {
        recordNavigationControl1.NavigationBar.MovePrevious();
    }
    
    private void BtnNext_Click(object sender, EventArgs e)
    {
        recordNavigationControl1.NavigationBar.MoveNext();
    }
    
    private void BtnLast_Click(object sender, EventArgs e)
    {
        recordNavigationControl1.NavigationBar.MoveLast();
    }
}
```

### Example: Keyboard Navigation

Implement keyboard shortcuts for navigation:

```csharp
using System.Windows.Forms;

protected override bool ProcessCmdKey(ref Message msg, Keys keyData)
{
    switch (keyData)
    {
        case Keys.Home:
            recordNavigationControl1.NavigationBar.MoveFirst();
            return true;
            
        case Keys.End:
            recordNavigationControl1.NavigationBar.MoveLast();
            return true;
            
        case Keys.PageUp:
            recordNavigationControl1.NavigationBar.MovePrevious();
            return true;
            
        case Keys.PageDown:
            recordNavigationControl1.NavigationBar.MoveNext();
            return true;
    }
    
    return base.ProcessCmdKey(ref msg, keyData);
}
```

### Example: Sequential Record Processing

Process all records sequentially:

```csharp
private void ProcessAllRecords()
{
    // Start at the first record
    recordNavigationControl1.NavigationBar.MoveFirst();
    
    int currentRecord = 1;
    int totalRecords = recordNavigationControl1.MaxRecord;
    
    do
    {
        // Process current record
        ProcessCurrentRecord();
        
        // Move to next record
        if (currentRecord < totalRecords)
        {
            recordNavigationControl1.NavigationBar.MoveNext();
            currentRecord++;
        }
        else
        {
            break; // Reached the end
        }
        
    } while (currentRecord <= totalRecords);
}

private void ProcessCurrentRecord()
{
    // Your record processing logic here
    // Access current grid row data and perform operations
}
```

## Use Case Scenarios

### Use Case 1: Data Entry Application

Navigate between records while entering or editing data:

```csharp
// Save current record and move to next
private void SaveAndNext()
{
    if (ValidateCurrentRecord())
    {
        SaveCurrentRecord();
        recordNavigationControl1.NavigationBar.MoveNext();
        LoadCurrentRecordData();
    }
}

// Save current record and move to previous
private void SaveAndPrevious()
{
    if (ValidateCurrentRecord())
    {
        SaveCurrentRecord();
        recordNavigationControl1.NavigationBar.MovePrevious();
        LoadCurrentRecordData();
    }
}
```

### Use Case 2: Record Review Workflow

Implement a review workflow where users step through records:

```csharp
private int reviewedCount = 0;

private void ApproveAndMoveNext()
{
    ApproveCurrentRecord();
    reviewedCount++;
    
    if (reviewedCount < recordNavigationControl1.MaxRecord)
    {
        recordNavigationControl1.NavigationBar.MoveNext();
    }
    else
    {
        MessageBox.Show("All records reviewed!", "Complete");
    }
}

private void SkipAndMoveNext()
{
    MarkRecordAsSkipped();
    reviewedCount++;
    recordNavigationControl1.NavigationBar.MoveNext();
}
```

### Use Case 3: Search and Navigate

Find a record and navigate to it:

```csharp
private void SearchAndNavigate(string searchTerm)
{
    // Start from first record
    recordNavigationControl1.NavigationBar.MoveFirst();
    
    for (int i = 0; i < recordNavigationControl1.MaxRecord; i++)
    {
        if (CurrentRecordMatches(searchTerm))
        {
            // Found the record - stay here
            MessageBox.Show($"Found at record {i + 1}");
            return;
        }
        
        if (i < recordNavigationControl1.MaxRecord - 1)
        {
            recordNavigationControl1.NavigationBar.MoveNext();
        }
    }
    
    MessageBox.Show("Record not found");
}

private bool CurrentRecordMatches(string searchTerm)
{
    // Your search logic to check current record
    // Access grid cells and compare with searchTerm
    return false; // Placeholder
}
```

### Use Case 4: Batch Navigation

Navigate in larger steps (e.g., jump 10 records):

```csharp
private void MoveNextBatch(int batchSize = 10)
{
    for (int i = 0; i < batchSize; i++)
    {
        recordNavigationControl1.NavigationBar.MoveNext();
    }
}

private void MovePreviousBatch(int batchSize = 10)
{
    for (int i = 0; i < batchSize; i++)
    {
        recordNavigationControl1.NavigationBar.MovePrevious();
    }
}
```

## Working with Large Datasets

When dealing with large datasets (1000+ records), these methods help users efficiently navigate without overwhelming the UI.

### Example: Navigation Status Display

Show current position while navigating:

```csharp
private void UpdateNavigationStatus()
{
    // Get current position (you'll need to track this based on your implementation)
    int currentPosition = GetCurrentRecordPosition();
    int totalRecords = recordNavigationControl1.MaxRecord;
    
    statusLabel.Text = $"Record {currentPosition} of {totalRecords}";
}

private void NavigateWithStatusUpdate(Action navigationAction)
{
    navigationAction.Invoke();
    UpdateNavigationStatus();
}

// Usage
private void BtnNext_Click(object sender, EventArgs e)
{
    NavigateWithStatusUpdate(() => 
        recordNavigationControl1.NavigationBar.MoveNext()
    );
}
```

### Example: Enable/Disable Navigation Based on Position

Disable buttons when at boundaries:

```csharp
private void UpdateNavigationButtonStates()
{
    int currentPos = GetCurrentRecordPosition();
    int maxRecords = recordNavigationControl1.MaxRecord;
    
    btnFirst.Enabled = currentPos > 1;
    btnPrevious.Enabled = currentPos > 1;
    btnNext.Enabled = currentPos < maxRecords;
    btnLast.Enabled = currentPos < maxRecords;
}
```

## VB.NET Complete Example

```vb
Imports Syncfusion.Windows.Forms.Grid
Imports System.Windows.Forms

Public Class NavigationForm
    Private recordNavigationControl1 As GridRecordNavigationControl
    
    ' Navigate to first record
    Private Sub BtnFirst_Click(sender As Object, e As EventArgs)
        Me.recordNavigationControl1.NavigationBar.MoveFirst()
    End Sub
    
    ' Navigate to last record
    Private Sub BtnLast_Click(sender As Object, e As EventArgs)
        Me.recordNavigationControl1.NavigationBar.MoveLast()
    End Sub
    
    ' Navigate to next record
    Private Sub BtnNext_Click(sender As Object, e As EventArgs)
        Me.recordNavigationControl1.NavigationBar.MoveNext()
    End Sub
    
    ' Navigate to previous record
    Private Sub BtnPrevious_Click(sender As Object, e As EventArgs)
        Me.recordNavigationControl1.NavigationBar.MovePrevious()
    End Sub
    
    ' Process all records sequentially
    Private Sub ProcessAllRecords()
        Dim currentRecord As Integer = 1
        Dim totalRecords As Integer = Me.recordNavigationControl1.MaxRecord
        
        ' Start at first record
        Me.recordNavigationControl1.NavigationBar.MoveFirst()
        
        Do While currentRecord <= totalRecords
            ProcessCurrentRecord()
            
            If currentRecord < totalRecords Then
                Me.recordNavigationControl1.NavigationBar.MoveNext()
                currentRecord += 1
            Else
                Exit Do
            End If
        Loop
    End Sub
    
    Private Sub ProcessCurrentRecord()
        ' Your processing logic here
    End Sub
End Class
```

## Best Practices

✅ **Do:**
- Call navigation methods through NavigationBar property
- Update UI/status after navigation operations
- Consider boundary conditions (first/last record)
- Implement keyboard shortcuts for better UX
- Provide visual feedback during navigation

❌ **Don't:**
- Call navigation in tight loops without delays (can freeze UI)
- Forget to handle edge cases (already at first/last record)
- Navigate without saving pending changes (if applicable)
- Call multiple navigation methods simultaneously

## Navigation Method Summary Table

| Method | Action | Use When |
|--------|--------|----------|
| `MoveFirst()` | Go to first record | Starting point, reset view |
| `MoveLast()` | Go to last record | End of dataset, latest entries |
| `MoveNext()` | Go to next record | Forward browsing, sequential processing |
| `MovePrevious()` | Go to previous record | Backward browsing, undo navigation |

## Next Steps

- **Learn about Styling** - Apply visual themes to your navigation control
- **GridGroupingControl Integration** - Use navigation with grouped data
- **Custom Navigation UI** - Build advanced navigation interfaces
