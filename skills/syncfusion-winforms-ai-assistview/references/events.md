# Events in AI AssistView

This guide covers the event system in the AI AssistView control, focusing on the PromptRequest event for handling user input and implementing custom behavior.

## Overview

The AI AssistView provides events to intercept and customize the chat interaction flow. The primary event is `PromptRequest`, which fires when a user submits a prompt through the text input or clicks a suggestion.

## PromptRequest Event

The `PromptRequest` event notifies your application when a user submits input, allowing you to:
- Validate user input before processing
- Trigger custom actions based on prompt content
- Prevent default message handling
- Log user interactions
- Implement custom AI response logic

### Event Signature

```csharp
public event EventHandler<PromptRequestEventArgs> PromptRequest;
```

### PromptRequestEventArgs

The event arguments provide access to the message and control over handling:

```csharp
public class PromptRequestEventArgs
{
    public object Message { get; set; }    // The input message (cast to TextMessage)
    public bool Handled { get; set; }      // Whether the event was handled
}
```

**Properties:**
- `Message`: The input message object (typically `TextMessage`)
- `Handled`: Set to `true` to prevent the message from being added to the Messages collection

## Basic Event Handling

### Subscribing to PromptRequest

```csharp
public partial class Form1 : Form
{
    ViewModel viewModel;
    SfAIAssistView sfAIAssistView1;
    
    public Form1()
    {
        InitializeComponent();
        viewModel = new ViewModel();
        
        sfAIAssistView1 = new SfAIAssistView();
        sfAIAssistView1.Dock = DockStyle.Fill;
        this.Controls.Add(sfAIAssistView1);
        
        // Subscribe to event
        sfAIAssistView1.PromptRequest += AIAssistView_PromptRequest;
        
        sfAIAssistView1.DataBindings.Add("Messages", viewModel, "Chats", 
            true, DataSourceUpdateMode.OnPropertyChanged);
    }
    
    private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
    {
        var message = e.Message as TextMessage;
        if (message == null) return;
        
        // Access the user's input
        string userInput = message.Text;
        
        // Perform custom actions
        Console.WriteLine($"User said: {userInput}");
    }
}
```

## Input Validation

### Preventing Empty Messages

```csharp
private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Validate input
    if (string.IsNullOrWhiteSpace(message.Text))
    {
        e.Handled = true; // Prevent adding to messages
        MessageBox.Show("Please enter a message.", "Empty Input", 
            MessageBoxButtons.OK, MessageBoxIcon.Warning);
        return;
    }
}
```

### Content Validation

```csharp
private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Check minimum length
    if (message.Text.Length < 3)
    {
        e.Handled = true;
        MessageBox.Show("Message must be at least 3 characters.", "Invalid Input");
        return;
    }
    
    // Check for prohibited content
    if (ContainsProhibitedWords(message.Text))
    {
        e.Handled = true;
        MessageBox.Show("Message contains prohibited content.", "Invalid Input");
        return;
    }
    
    // Validation passed, allow message
}

private bool ContainsProhibitedWords(string text)
{
    string[] prohibited = { "spam", "offensive" };
    return prohibited.Any(word => text.Contains(word, StringComparison.OrdinalIgnoreCase));
}
```

### Format Validation

```csharp
private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Check for specific command format
    if (message.Text.StartsWith("/"))
    {
        // Handle command
        HandleCommand(message.Text);
        e.Handled = true; // Don't add command to chat
        return;
    }
    
    // Regular message processing continues
}

private void HandleCommand(string command)
{
    if (command == "/help")
    {
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "System" },
            Text = "Available commands: /help, /clear, /history"
        });
    }
    else if (command == "/clear")
    {
        viewModel.Chats.Clear();
    }
}
```

## Triggering Custom Actions

### Logging User Input

```csharp
private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Log to file or database
    LogUserMessage(message.Text, message.Author.Name, message.DateTime);
    
    // Continue with default handling
}

private void LogUserMessage(string text, string author, DateTime timestamp)
{
    string logEntry = $"[{timestamp:yyyy-MM-dd HH:mm:ss}] {author}: {text}";
    File.AppendAllText("chat_log.txt", logEntry + Environment.NewLine);
}
```

### Analytics Tracking

```csharp
private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Track metrics
    Analytics.TrackEvent("ChatMessage", new Dictionary<string, string>
    {
        { "MessageLength", message.Text.Length.ToString() },
        { "Timestamp", DateTime.Now.ToString() },
        { "User", message.Author.Name }
    });
}
```

### Rate Limiting

```csharp
private DateTime lastMessageTime = DateTime.MinValue;
private const int MinimumDelaySeconds = 2;

private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Enforce rate limit
    TimeSpan timeSinceLastMessage = DateTime.Now - lastMessageTime;
    if (timeSinceLastMessage.TotalSeconds < MinimumDelaySeconds)
    {
        e.Handled = true;
        MessageBox.Show($"Please wait {MinimumDelaySeconds} seconds between messages.");
        return;
    }
    
    lastMessageTime = DateTime.Now;
}
```

## Async Response Generation

### Basic Async Pattern

```csharp
private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var userMessage = e.Message as TextMessage;
    if (userMessage == null) return;
    
    // Show typing indicator
    viewModel.ShowTypingIndicator = true;
    
    try
    {
        // Generate AI response asynchronously
        string response = await GenerateAIResponse(userMessage.Text);
        
        // Add bot response
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "Bot" },
            Text = response,
            DateTime = DateTime.Now
        });
    }
    catch (Exception ex)
    {
        // Handle errors
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "Bot" },
            Text = $"Error: {ex.Message}",
            DateTime = DateTime.Now
        });
    }
    finally
    {
        // Hide typing indicator
        viewModel.ShowTypingIndicator = false;
    }
}

private async Task<string> GenerateAIResponse(string userInput)
{
    // Simulate AI processing
    await Task.Delay(1000);
    return $"You asked: {userInput}";
}
```

### With Suggestions Update

```csharp
private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var userMessage = e.Message as TextMessage;
    if (userMessage == null) return;
    
    viewModel.ShowTypingIndicator = true;
    viewModel.Suggestion = null; // Clear old suggestions
    
    try
    {
        var response = await GetAIResponse(userMessage.Text);
        
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "Bot" },
            Text = response.Text,
            DateTime = DateTime.Now
        });
        
        // Update suggestions based on response
        viewModel.Suggestion = response.Suggestions;
    }
    finally
    {
        viewModel.ShowTypingIndicator = false;
    }
}
```

## Advanced Patterns

### Intent Recognition

```csharp
private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Detect user intent
    string intent = DetectIntent(message.Text);
    
    switch (intent)
    {
        case "Question":
            await HandleQuestion(message.Text);
            break;
        case "Command":
            await HandleCommand(message.Text);
            break;
        case "Feedback":
            await HandleFeedback(message.Text);
            break;
        default:
            await HandleGeneral(message.Text);
            break;
    }
}

private string DetectIntent(string text)
{
    if (text.Contains("?")) return "Question";
    if (text.StartsWith("/")) return "Command";
    if (text.ToLower().Contains("thanks") || text.ToLower().Contains("great")) 
        return "Feedback";
    return "General";
}
```

### Context Preservation

```csharp
private Stack<string> conversationContext = new Stack<string>();

private void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    // Add to context
    conversationContext.Push(message.Text);
    
    // Limit context size
    if (conversationContext.Count > 10)
    {
        conversationContext = new Stack<string>(conversationContext.Take(10).Reverse());
    }
    
    // Use context for AI response
    string contextualResponse = GenerateResponseWithContext(
        message.Text, 
        conversationContext.ToList()
    );
}
```

### Error Recovery

```csharp
private int consecutiveErrors = 0;
private const int MaxConsecutiveErrors = 3;

private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var message = e.Message as TextMessage;
    if (message == null) return;
    
    try
    {
        await ProcessMessage(message);
        consecutiveErrors = 0; // Reset on success
    }
    catch (Exception ex)
    {
        consecutiveErrors++;
        
        if (consecutiveErrors >= MaxConsecutiveErrors)
        {
            e.Handled = true;
            MessageBox.Show(
                "Multiple errors occurred. Please restart the application.",
                "Critical Error"
            );
            return;
        }
        
        // Show error message
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "System" },
            Text = $"Error: {ex.Message}. Please try again.",
            DateTime = DateTime.Now
        });
    }
}
```

## Complete Example

```csharp
using System;
using System.ComponentModel;
using System.Collections.ObjectModel;
using System.Threading.Tasks;
using System.Windows.Forms;
using Syncfusion.WinForms.AIAssistView;

public partial class Form1 : Form
{
    ViewModel viewModel;
    SfAIAssistView sfAIAssistView1;
    
    public Form1()
    {
        InitializeComponent();
        viewModel = new ViewModel();
        
        sfAIAssistView1 = new SfAIAssistView();
        sfAIAssistView1.Dock = DockStyle.Fill;
        this.Controls.Add(sfAIAssistView1);
        
        // Bind properties
        sfAIAssistView1.DataBindings.Add("Messages", viewModel, "Chats", 
            true, DataSourceUpdateMode.OnPropertyChanged);
        sfAIAssistView1.DataBindings.Add("ShowTypingIndicator", viewModel, 
            "ShowTypingIndicator", true, DataSourceUpdateMode.OnPropertyChanged);
        sfAIAssistView1.DataBindings.Add("Suggestions", viewModel, "Suggestion", 
            true, DataSourceUpdateMode.OnPropertyChanged);
        
        viewModel.CurrentUser = sfAIAssistView1.User;
        
        // Subscribe to event
        sfAIAssistView1.PromptRequest += AIAssistView_PromptRequest;
        
        // Configure typing indicator
        sfAIAssistView1.TypingIndicator.Author = new Author 
        { 
            Name = "Bot", 
            AvatarImage = Image.FromFile(@"Assets\bot.png") 
        };
        sfAIAssistView1.TypingIndicator.DisplayText = "Thinking...";
    }
    
    private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
    {
        var userMessage = e.Message as TextMessage;
        if (userMessage == null) return;
        
        // Validate input
        if (string.IsNullOrWhiteSpace(userMessage.Text))
        {
            e.Handled = true;
            MessageBox.Show("Please enter a message.");
            return;
        }
        
        // Log the message
        Console.WriteLine($"User: {userMessage.Text}");
        
        // Show typing indicator
        viewModel.ShowTypingIndicator = true;
        
        try
        {
            // Generate response
            await Task.Delay(1000); // Simulate processing
            string response = $"You said: {userMessage.Text}";
            
            // Add bot response
            viewModel.Chats.Add(new TextMessage
            {
                Author = new Author { Name = "Bot" },
                Text = response,
                DateTime = DateTime.Now
            });
            
            // Update suggestions
            viewModel.Suggestion = new[] { "Tell me more", "Ask another question" };
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Error: {ex.Message}");
        }
        finally
        {
            viewModel.ShowTypingIndicator = false;
        }
    }
}
```

## Best Practices

### Always Validate Input
Check for null, empty strings, and malformed content before processing.

### Use Async/Await
Long-running operations should be asynchronous to prevent UI freezing.

### Handle Errors Gracefully
Wrap event logic in try-catch and provide user feedback for errors.

### Set e.Handled Appropriately
Use `e.Handled = true` only when you want to prevent the message from being added to the collection.

### Clean Up Resources
If you create resources in the event handler, ensure they're disposed properly.

## Troubleshooting

**Event not firing:**
- Verify event subscription: `sfAIAssistView1.PromptRequest += ...`
- Check that messages are being added through the control's input box
- Ensure control is properly initialized

**Message still added when e.Handled = true:**
- Verify you're setting `e.Handled = true` before the event handler returns
- Check for multiple event handlers that might override

**UI freezing during event:**
- Use async/await for long operations
- Move processing off the UI thread
- Show typing indicator for user feedback

**Cannot access message properties:**
- Cast `e.Message` to `TextMessage` first
- Check for null before accessing properties
- Verify message type matches expected format
