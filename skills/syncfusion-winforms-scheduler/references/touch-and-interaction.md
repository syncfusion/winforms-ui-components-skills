# Touch and Interaction

The Windows Forms Scheduler supports modern touch gestures and interactive features including swipe scrolling, zooming, drag-and-drop, and context detection.

## Touch Support

The ScheduleControl provides comprehensive touch support similar to Microsoft Outlook calendar, including swiping, panning, and zooming.

### Enabling Touch Mode

```csharp
// Enable touch mode
scheduleControl1.EnableTouchMode = true;
```

**Default:** `false` (disabled)

**When to Enable:**
- Tablet devices
- Touch-enabled laptops
- Kiosk applications
- Modern UI requirements

### Touch Gestures

Once enabled, the following gestures are supported:

1. **Vertical Swipe Scrolling** - Scroll through time slots in Day/WorkWeek views
2. **Horizontal Swipe Navigation** - Navigate to previous/next period
3. **Pinch Zoom** - Change between view types
4. **Pan** - Move around the schedule

## Touch Swiping

### Vertical Swipe Scrolling

Available in Day, WorkWeek, and Custom views.

**Behavior:**
- Swipe up: Scroll down to later time slots
- Swipe down: Scroll up to earlier time slots
- Momentum scrolling: Content continues moving after finger lift

**Use Cases:**
- Quickly navigate to morning/afternoon/evening time slots
- Review full day schedule on small screens
- Smooth scrolling through time ranges

### Horizontal Swipe Navigation

Available in all views (Month, Week, WorkWeek, Day, Custom).

**Behavior:**
- **Swipe left:** Navigate forward (next day, week, or month)
- **Swipe right:** Navigate backward (previous day, week, or month)

**Navigation by View:**
- **Month View:** Previous/next month
- **Week/WorkWeek View:** Previous/next week
- **Day View:** Previous/next day
- **Custom View:** Previous/next date range

**Example Use Cases:**
```csharp
// Month view: Swipe left = April → May
// Week view: Swipe left = Week 1 → Week 2
// Day view: Swipe left = Monday → Tuesday
```

### Momentum and Inertia

Touch swipes include momentum physics:
- Fast swipe: Smooth deceleration with momentum
- Slow swipe: Precise control without momentum
- Tap to stop: Immediately halt momentum scrolling

## Touch Zooming

Change schedule views using pinch zoom gestures, similar to Outlook calendar.

### Zoom Behavior

**Zoom Out (Pinch inward):**
- Day → Week
- Week → Month
- Month → (no change)

**Zoom In (Spread outward):**
- Month → Week
- Week → Day
- Day → (no change)

**Example Workflow:**
```csharp
// User starts in Month view
// Pinch outward (zoom in) → Changes to Week view
// Pinch outward again → Changes to Day view
// Pinch inward (zoom out) → Returns to Week view
```

### Zoom Levels

The control automatically cycles through view types based on zoom gestures:

```
Month View ↔ Week View ↔ Day View
  (Zoom Out)   (Zoom In/Out)   (Zoom In)
```

## Appointment Dragging

Drag appointments to reschedule them to different time slots.

### Drag Operations

**In Day/Week/WorkWeek Views:**
- **Vertical Drag:** Change appointment time
- **Horizontal Drag:** Change appointment date (multi-day views)
- **Visual Feedback:** Appointment moves with finger/mouse pointer

**In Month View:**
- **Drag to Different Date:** Move appointment to new day

### Drag and Drop Example

```csharp
// User drags appointment from 9:00 AM to 2:00 PM
// Appointment's StartTime and EndTime automatically updated
// ItemChanging event fires during drag
```

## Appointment Resizing

Resize appointments to change their duration (Day/Week/WorkWeek views only).

**Behavior:**
- Drag appointment bottom edge down: Extend duration
- Drag appointment bottom edge up: Reduce duration
- Snaps to time division intervals (15 min, 30 min, etc.)

**Example:**
```csharp
// 1-hour meeting (9:00 AM - 10:00 AM)
// Drag bottom edge to 10:30 AM
// Result: 1.5-hour meeting (9:00 AM - 10:30 AM)
```

## ItemChanging Event

The `ItemChanging` event fires when appointments are modified, including drag operations.

### Event Subscription

```csharp
scheduleControl1.ItemChanging += ScheduleControl1_ItemChanging;

private void ScheduleControl1_ItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
{
    // Handle appointment changes using e.ProposedItem
}
```

### Event Arguments

```csharp
public class ScheduleAppointmentCancelEventArgs : CancelEventArgs
{
    public IScheduleAppointment Appointment { get; }
    public ItemAction Action { get; }
    public ItemDragHitContext ItemDragHitContext { get; }
}
```

**Key Properties:**
- **Appointment:** The appointment being modified
- **Action:** Type of change (ItemDrag, Edit, Delete, etc.)
- **ItemDragHitContext:** Where the appointment was dropped (Schedule or Calendar area)
- **Cancel:** Set to `true` to prevent the change

## ItemAction Enumeration

Identifies the type of change occurring:

```csharp
public enum ItemAction
{
    ItemDrag,    // Appointment being dragged
    Edit,        // Appointment being edited
    Delete,      // Appointment being deleted
    Add,          // New appointment being added
    TimeDrag,    // Appointment duration is being resized (start/end time changed)
    Default   // Default/internal action

}
```

### Detecting Drag Operations

```csharp
private void ScheduleControl1_ItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
{
    if (e.Action == ItemAction.ItemDrag)
    {
        // Appointment is being dragged
        Console.WriteLine($"Dragging: {e.ProposedItem.Subject}");
    }
}
```

## ItemDragHitContext Enumeration

Identifies where an appointment was dropped during a drag operation.

```csharp
public enum ItemDragHitContext
{
    Schedule,   // Dropped in schedule grid area
    Calendar    // Dropped in navigation calendar area
}
```

### Detecting Drop Location

```csharp
private void ScheduleControl1_ItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
{
    if (e.Action == ItemAction.ItemDrag)
    {
        // Check where appointment was dropped
        if (e.ItemDragHitContext == ItemDragHitContext.Schedule)
        {
            Console.WriteLine("Dropped in schedule area");
            // Allow drop
        }
        else if (e.ItemDragHitContext == ItemDragHitContext.Calendar)
        {
            Console.WriteLine("Dropped in calendar area");
            // Potentially cancel drop
        }
    }
}
```

## Canceling Drag Operations

Prevent drops based on business rules or drop location.

### Cancel Drops to Calendar Area

```csharp
private void ScheduleControl1_ItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
{
    if (e.Action == ItemAction.ItemDrag && 
        e.ItemDragHitContext == ItemDragHitContext.Calendar)
    {
        // Don't allow drops to navigation calendar
        MessageBox.Show("Cannot drop appointments in calendar area");
        e.Cancel = true;
    }
}
```

### Business Rule Validation

```csharp
private void ScheduleControl1_ItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
{
    if (e.Action == ItemAction.ItemDrag)
    {
        // Prevent moving past appointments
        if (e.ProposedItem.StartTime < DateTime.Now)
        {
            MessageBox.Show("Cannot reschedule past appointments");
            e.Cancel = true;
            return;
        }
        
        // Prevent moving to weekends
        if (e.ProposedItem.StartTime.DayOfWeek == DayOfWeek.Saturday ||
            e.ProposedItem.StartTime.DayOfWeek == DayOfWeek.Sunday)
        {
            MessageBox.Show("Cannot schedule on weekends");
            e.Cancel = true;
            return;
        }
    }
}
```

## Complete Interaction Examples

### Touch-Enabled Scheduler

```csharp
public void ConfigureTouchScheduler(ScheduleControl scheduleControl)
{
    // Enable touch mode
    scheduleControl.EnableTouchMode = true;
    
    // Configure for touch-friendly time intervals
    scheduleControl.Appearance.DivisionsPerHour = 4; // 15-minute slots
    
    // Larger fonts for touch
    scheduleControl.Appearance.TimeBigFontSize = 12;
    scheduleControl.Appearance.TimeLittleFontSize = 10;
    
    // Subscribe to drag events
    scheduleControl.ItemChanging += TouchScheduler_ItemChanging;
}

private void TouchScheduler_ItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
{
    if (e.Action == ItemAction.ItemDrag)
    {
        // Provide haptic feedback (if hardware supports)
        // Log drag operation
        Console.WriteLine($"Appointment moved: {e.ProposedItem.Subject}");
    }
}
```

### Drag Validation Manager

```csharp
public class DragValidationManager
{
    private ScheduleControl scheduleControl;
    
    public DragValidationManager(ScheduleControl control)
    {
        scheduleControl = control;
        scheduleControl.ItemChanging += OnItemChanging;
    }
    
    private void OnItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
    {
        if (e.Action == ItemAction.ItemDrag)
        {
            // Cancel drops to calendar
            if (e.ItemDragHitContext == ItemDragHitContext.Calendar)
            {
                e.Cancel = true;
                ShowMessage("Cannot drop in calendar area");
                return;
            }
            
            // Validate time range
            if (!IsValidTimeRange(e.ProposedItem))
            {
                e.Cancel = true;
                ShowMessage("Invalid time range for appointment");
                return;
            }
            
            // Check for conflicts
            if (HasConflict(e.ProposedItem))
            {
                e.Cancel = true;
                ShowMessage("This time slot conflicts with another appointment");
                return;
            }
            
            // Log successful drag
            LogDragOperation(e.ProposedItem);
        }
    }
    
    private bool IsValidTimeRange(IScheduleAppointment appointment)
    {
        // Business hours: 8 AM - 6 PM
        TimeSpan start = appointment.StartTime.TimeOfDay;
        TimeSpan end = appointment.EndTime.TimeOfDay;
        
        return start >= new TimeSpan(8, 0, 0) && 
               end <= new TimeSpan(18, 0, 0);
    }
    
    private bool HasConflict(IScheduleAppointment appointment)
    {
        // Check for overlapping appointments
        IScheduleDataProvider dataProvider = scheduleControl.DataSource as IScheduleDataProvider;
        IScheduleAppointmentList daySchedule = 
            dataProvider.GetScheduleForDay(appointment.StartTime.Date);
        
        foreach (IScheduleAppointment existing in daySchedule)
        {
            if (existing.UniqueID == appointment.UniqueID)
                continue; // Skip self
                
            // Check overlap
            if (appointment.StartTime < existing.EndTime && 
                appointment.EndTime > existing.StartTime)
            {
                return true; // Conflict found
            }
        }
        
        return false;
    }
    
    private void LogDragOperation(IScheduleAppointment appointment)
    {
        Console.WriteLine($"Appointment rescheduled: {appointment.Subject}");
        Console.WriteLine($"New time: {appointment.StartTime} - {appointment.EndTime}");
    }
    
    private void ShowMessage(string message)
    {
        MessageBox.Show(message, "Schedule Validation", 
            MessageBoxButtons.OK, MessageBoxIcon.Information);
    }
}
```

### Context-Aware Drag Handler

```csharp
public class ContextAwareDragHandler
{
    private ScheduleControl scheduleControl;
    private DateTime originalStartTime;
    private DateTime originalEndTime;
    
    public ContextAwareDragHandler(ScheduleControl control)
    {
        scheduleControl = control;
        scheduleControl.ItemChanging += OnItemChanging;
    }
    
    private void OnItemChanging(object sender, ScheduleAppointmentCancelEventArgs e)
    {
        if (e.Action == ItemAction.ItemDrag)
        {
            // Store original times at drag start
                if (originalStartTime == DateTime.MinValue)
            {
                originalStartTime = e.ProposedItem.StartTime;
                originalEndTime = e.ProposedItem.EndTime;
            }
            
            // Provide context-specific feedback
            string context = GetDragContext(e.ItemDragHitContext);
            string message = $"Dropping in: {context}";
            
            // Update status bar or tooltip
            UpdateStatusBar(message);
            
            // Validate based on context
                if (!ValidateDragContext(e))
            {
                e.Cancel = true;
                ResetOriginalTimes(e.ProposedItem);
            }
        }
    }
    
    private string GetDragContext(ItemDragHitContext context)
    {
        switch (context)
        {
            case ItemDragHitContext.Schedule:
                return "Schedule Grid";
            case ItemDragHitContext.Calendar:
                return "Navigation Calendar";
            default:
                return "Unknown";
        }
    }
    
    private bool ValidateDragContext(ScheduleAppointmentCancelEventArgs e)
    {
        // Allow drops to schedule, prevent drops to calendar
        return e.ItemDragHitContext == ItemDragHitContext.Schedule;
    }
    
    private void ResetOriginalTimes(IScheduleAppointment appointment)
    {
        appointment.StartTime = originalStartTime;
        appointment.EndTime = originalEndTime;
        originalStartTime = DateTime.MinValue;
        originalEndTime = DateTime.MinValue;
    }
    
    private void UpdateStatusBar(string message)
    {
        // Update UI status bar or other feedback mechanism
        Console.WriteLine(message);
    }
}
```

## Best Practices

1. **Touch Mode for Tablets:** Always enable `EnableTouchMode` for tablet/touch devices
2. **Validate Drops:** Use `ItemChanging` event to enforce business rules (use `e.ProposedItem` for the appointment)
3. **Cancel Calendar Drops:** Typically prevent drops to navigation calendar area
4. **Provide Feedback:** Give visual/audio feedback during drag operations
5. **Check Conflicts:** Validate appointment doesn't overlap existing appointments
6. **Business Hours:** Enforce time range constraints based on business needs
7. **Gesture Testing:** Test all touch gestures on actual touch devices
8. **Fallback Support:** Ensure mouse/keyboard interaction still works when touch is enabled
9. **Performance:** Handle `ItemChanging` efficiently - it fires frequently during drags
10. **User Guidance:** Provide clear visual cues about where appointments can be dropped
