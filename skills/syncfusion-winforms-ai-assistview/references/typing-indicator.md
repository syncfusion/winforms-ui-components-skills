# Typing Indicator in AI AssistView

The Typing Indicator feature provides visual feedback to users during asynchronous AI processing by displaying an animated "typing" indicator, enhancing the conversational experience and setting proper expectations.

## Overview

The typing indicator:
- Shows when the AI is processing or generating a response
- Provides real-time feedback during async operations
- Improves user experience by indicating activity
- Can be customized with author information and text

## When to Use the Typing Indicator

Display the typing indicator when:
- Waiting for AI/API responses
- Processing complex computations
- Fetching data from external services
- Any operation that takes >500ms
- Simulating human-like conversation pacing

## Key Properties

### ShowTypingIndicator

Controls the visibility of the typing indicator:

```csharp
public bool ShowTypingIndicator { get; set; }
```

**Usage:**
- Set to `true` before starting async operation
- Set to `false` after operation completes

### TypingIndicator

The `TypingIndicator` object provides configuration options:

```csharp
public TypingIndicator TypingIndicator { get; set; }
```

**Properties:**
- `Author`: The author shown with the indicator (typically the bot)
- `DisplayText`: The text shown next to the indicator (e.g., "Typing", "Thinking")

## Basic Implementation

### Step 1: Add ShowTypingIndicator to ViewModel

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> chats;
    private Author currentUser;
    private bool showTypingIndicator;
    
    public ViewModel()
    {
        this.Chats = new ObservableCollection<object>();
        this.CurrentUser = new Author { Name = "User" };
    }
    
    public bool ShowTypingIndicator
    {
        get => this.showTypingIndicator;
        set
        {
            this.showTypingIndicator = value;
            RaisePropertyChanged("ShowTypingIndicator");
        }
    }
    
    public ObservableCollection<object> Chats
    {
        get => this.chats;
        set
        {
            this.chats = value;
            RaisePropertyChanged("Chats");
        }
    }
    
    public Author CurrentUser
    {
        get => this.currentUser;
        set
        {
            this.currentUser = value;
            RaisePropertyChanged("CurrentUser");
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    public void RaisePropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }
}
```

### Step 2: Bind and Configure in Form

```csharp
public partial class Form1 : Form
{
    ViewModel viewModel;
    
    public Form1()
    {
        InitializeComponent();
        viewModel = new ViewModel();
        
        SfAIAssistView sfAIAssistView1 = new SfAIAssistView();
        sfAIAssistView1.Dock = DockStyle.Fill;
        this.Controls.Add(sfAIAssistView1);
        
        // Bind messages
        sfAIAssistView1.DataBindings.Add("Messages", viewModel, "Chats", 
            true, DataSourceUpdateMode.OnPropertyChanged);
        
        // Bind typing indicator
        sfAIAssistView1.DataBindings.Add("ShowTypingIndicator", viewModel, 
            "ShowTypingIndicator", true, DataSourceUpdateMode.OnPropertyChanged);
        
        // Set current user
        viewModel.CurrentUser = sfAIAssistView1.User;
        
        // Configure typing indicator appearance
        sfAIAssistView1.TypingIndicator.Author = new Author 
        { 
            Name = "Bot", 
            AvatarImage = Image.FromFile(@"Assets\AI_Assist.png") 
        };
        sfAIAssistView1.TypingIndicator.DisplayText = "Typing";
    }
}
```

## Using the Typing Indicator

### Pattern 1: Simple Async Operation

```csharp
private async void ProcessUserMessage(string userText)
{
    // Show typing indicator
    viewModel.ShowTypingIndicator = true;
    
    try
    {
        // Simulate AI processing
        await Task.Delay(2000);
        string response = GenerateResponse(userText);
        
        // Add bot response
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "Bot" },
            DateTime = DateTime.Now,
            Text = response
        });
    }
    finally
    {
        // Always hide indicator, even if error occurs
        viewModel.ShowTypingIndicator = false;
    }
}
```

### Pattern 2: With PromptRequest Event

```csharp
private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var userMessage = e.Message as TextMessage;
    if (userMessage == null) return;
    
    // Show typing indicator
    viewModel.ShowTypingIndicator = true;
    
    try
    {
        // Call AI service
        var response = await GetAIResponse(userMessage.Text);
        
        // Add response
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "Bot", AvatarImage = botAvatar },
            Text = response,
            DateTime = DateTime.Now
        });
    }
    catch (Exception ex)
    {
        // Handle error
        viewModel.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "Bot" },
            Text = "Sorry, I encountered an error. Please try again.",
            DateTime = DateTime.Now
        });
    }
    finally
    {
        // Hide indicator
        viewModel.ShowTypingIndicator = false;
    }
}
```

### Pattern 3: With OpenAI Integration

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private bool showTypingIndicator;
    private AIAssistChatService service;
    
    public ViewModel()
    {
        this.Chats = new ObservableCollection<object>();
        this.CurrentUser = new Author { Name = "User" };
        this.Chats.CollectionChanged += Chats_CollectionChanged;
        
        service = new AIAssistChatService();
        service.Initialize();
    }
    
    private async void Chats_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
    {
        if (e.Action != NotifyCollectionChangedAction.Add) return;
        
        var item = e.NewItems?[0] as TextMessage;
        if (item == null || item.Author.Name != CurrentUser.Name) return;
        
        // Show typing indicator
        ShowTypingIndicator = true;
        
        try
        {
            // Get AI response
            await service.NonStreamingChat(item.Text);
            
            // Add bot response
            Chats.Add(new TextMessage
            {
                Author = new Author { Name = "Bot", AvatarImage = botImage },
                DateTime = DateTime.Now,
                Text = service.Response
            });
        }
        finally
        {
            // Hide typing indicator
            ShowTypingIndicator = false;
        }
    }
    
    public bool ShowTypingIndicator
    {
        get => showTypingIndicator;
        set
        {
            showTypingIndicator = value;
            RaisePropertyChanged(nameof(ShowTypingIndicator));
        }
    }
}
```

## Customizing Appearance

### Custom Display Text

Change the text shown next to the indicator:

```csharp
// Different display texts
sfAIAssistView1.TypingIndicator.DisplayText = "Thinking...";
sfAIAssistView1.TypingIndicator.DisplayText = "Processing...";
sfAIAssistView1.TypingIndicator.DisplayText = "Generating response...";
sfAIAssistView1.TypingIndicator.DisplayText = "AI is typing";
```

### Custom Author with Avatar

Provide a custom author with an avatar image:

```csharp
sfAIAssistView1.TypingIndicator.Author = new Author
{
    Name = "AI Assistant",
    AvatarImage = Image.FromFile(@"Assets\bot-avatar.png")
};
```

### Loading Avatar from Resources

```csharp
// From embedded resource
sfAIAssistView1.TypingIndicator.Author = new Author
{
    Name = "Bot",
    AvatarImage = Properties.Resources.BotAvatar
};
```

## Best Practices

### Always Use Try-Finally

Ensure the indicator is hidden even if errors occur:

```csharp
viewModel.ShowTypingIndicator = true;
try
{
    await LongRunningOperation();
}
finally
{
    viewModel.ShowTypingIndicator = false;
}
```

### Minimum Display Time

For very fast operations, consider a minimum display time:

```csharp
private async Task ShowIndicatorWithMinimumTime(Func<Task<string>> operation)
{
    viewModel.ShowTypingIndicator = true;
    
    var operationTask = operation();
    var delayTask = Task.Delay(500); // Minimum 500ms
    
    await Task.WhenAll(operationTask, delayTask);
    
    viewModel.ShowTypingIndicator = false;
    return await operationTask;
}
```

### Don't Stack Indicators

Only show one typing indicator at a time:

```csharp
// Bad - multiple sequential operations
viewModel.ShowTypingIndicator = true;
await Operation1();
viewModel.ShowTypingIndicator = false;

viewModel.ShowTypingIndicator = true;
await Operation2();
viewModel.ShowTypingIndicator = false;

// Good - single indicator for entire flow
viewModel.ShowTypingIndicator = true;
try
{
    await Operation1();
    await Operation2();
}
finally
{
    viewModel.ShowTypingIndicator = false;
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
        
        viewModel.CurrentUser = sfAIAssistView1.User;
        
        // Configure typing indicator
        sfAIAssistView1.TypingIndicator.Author = new Author 
        { 
            Name = "AI Bot", 
            AvatarImage = Image.FromFile(@"Assets\bot.png") 
        };
        sfAIAssistView1.TypingIndicator.DisplayText = "Thinking...";
        
        // Handle prompt
        sfAIAssistView1.PromptRequest += AIAssistView_PromptRequest;
    }
    
    private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
    {
        var userMessage = e.Message as TextMessage;
        if (userMessage == null) return;
        
        viewModel.ShowTypingIndicator = true;
        
        try
        {
            // Simulate AI processing
            await Task.Delay(2000);
            
            string response = $"You said: {userMessage.Text}";
            
            viewModel.Chats.Add(new TextMessage
            {
                Author = new Author { Name = "AI Bot" },
                Text = response,
                DateTime = DateTime.Now
            });
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

## Troubleshooting

**Indicator not showing:**
- Verify `ShowTypingIndicator = true` is set before async operation
- Check data binding is established correctly
- Ensure RaisePropertyChanged is called

**Indicator doesn't hide:**
- Always use try-finally to ensure hiding
- Check for exceptions that prevent reaching the hide statement
- Verify async/await is used correctly

**Indicator appears but no animation:**
- The animation is built-in; verify the control is visible
- Check if theme or styling is interfering

**Wrong author shown:**
- Configure `TypingIndicator.Author` property
- Ensure avatar image path is correct
- Verify Author object is created properly
