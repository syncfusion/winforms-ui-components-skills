# Suggestions in AI AssistView

The Suggestions feature displays AI-driven suggestion chips in the bottom-right corner of the AssistView, enabling users to quickly respond with predefined options or explore relevant topics without typing.

## Overview

Suggestions enhance the conversational flow by:
- Providing quick response options after bot messages
- Reducing typing effort for common queries
- Guiding users through conversation paths
- Improving user experience with one-click responses

## When to Use Suggestions

Use suggestions when:
- You want to offer common follow-up questions
- The AI provides options that users can explore
- You need to guide users through a conversational flow
- Users might benefit from example queries
- You want to speed up interaction with preset responses

## Suggestions Property

The `Suggestions` property accepts an `IEnumerable<string>` collection that displays as clickable chips.

```csharp
public IEnumerable<string> Suggestions { get; set; }
```

## Basic Implementation

### Step 1: Add Suggestion Property to ViewModel

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> chats;
    private Author currentUser;
    private IEnumerable<string> suggestion;
    
    public ViewModel()
    {
        this.Chats = new ObservableCollection<object>();
        this.CurrentUser = new Author { Name = "John" };
        this.Suggestion = new ObservableCollection<string>();
        this.GenerateMessages();
    }
    
    private async void GenerateMessages()
    {
        // User message
        this.Chats.Add(new TextMessage 
        { 
            Author = CurrentUser, 
            Text = "What is Windows Forms?" 
        });
        
        await Task.Delay(1000);
        
        // Bot response
        this.Chats.Add(new TextMessage 
        { 
            Author = new Author { Name = "Bot" }, 
            Text = "Windows Forms (also known as WinForms) is a graphical user interface (GUI) framework developed by Microsoft for building desktop applications for the Windows operating system." 
        });
        
        // Add suggestions after bot response
        Suggestion = new ObservableCollection<string> 
        { 
            "What is the future of WinForms?", 
            "How does WinForms handle UI rendering?" 
        };
    }
    
    public IEnumerable<string> Suggestion
    {
        get => this.suggestion;
        set
        {
            this.suggestion = value;
            RaisePropertyChanged("Suggestion");
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

### Step 2: Bind Suggestions to Control

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
        
        // Bind suggestions
        sfAIAssistView1.DataBindings.Add("Suggestions", viewModel, "Suggestion", 
            true, DataSourceUpdateMode.OnPropertyChanged);
    }
}
```

## Dynamic Suggestions

Update suggestions dynamically based on conversation context:

```csharp
private async void Chats_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
{
    if (e.Action != NotifyCollectionChangedAction.Add) return;
    
    foreach (var newItem in e.NewItems ?? new object[0])
    {
        if (newItem is TextMessage message && message.Author.Name == "Bot")
        {
            // Update suggestions based on bot response
            if (message.Text.Contains("Windows Forms"))
            {
                Suggestion = new ObservableCollection<string>
                {
                    "What is WPF?",
                    "Compare WinForms vs WPF",
                    "Show me a WinForms example"
                };
            }
            else if (message.Text.Contains("database"))
            {
                Suggestion = new ObservableCollection<string>
                {
                    "How to connect to SQL Server?",
                    "Show ADO.NET example",
                    "What is Entity Framework?"
                };
            }
        }
    }
}
```

## Context-Aware Suggestions

Provide suggestions based on user intent:

```csharp
private void UpdateSuggestionsBasedOnContext(string botResponse)
{
    if (botResponse.Contains("error") || botResponse.Contains("issue"))
    {
        // Troubleshooting suggestions
        Suggestion = new ObservableCollection<string>
        {
            "How do I fix this?",
            "Show me alternative approaches",
            "What causes this error?"
        };
    }
    else if (botResponse.Contains("example") || botResponse.Contains("code"))
    {
        // Code-related suggestions
        Suggestion = new ObservableCollection<string>
        {
            "Explain this code",
            "Show more examples",
            "What are best practices?"
        };
    }
    else
    {
        // General suggestions
        Suggestion = new ObservableCollection<string>
        {
            "Tell me more",
            "Show an example",
            "What are alternatives?"
        };
    }
}
```

## Clearing Suggestions

Clear suggestions when not needed:

```csharp
// Clear suggestions
Suggestion = new ObservableCollection<string>();

// Or set to null
Suggestion = null;
```

## Handling Suggestion Clicks

When a user clicks a suggestion, it's automatically added as a user message and triggers the `PromptRequest` event:

```csharp
sfAIAssistView1.PromptRequest += async (sender, e) =>
{
    var userMessage = e.Message as TextMessage;
    if (userMessage == null) return;
    
    // Clear suggestions while processing
    viewModel.Suggestion = null;
    
    // Process the message
    var response = await ProcessUserInput(userMessage.Text);
    
    viewModel.Chats.Add(new TextMessage
    {
        Author = new Author { Name = "Bot" },
        Text = response
    });
    
    // Show new suggestions
    viewModel.Suggestion = GetRelevantSuggestions(response);
};
```

## Suggestion Best Practices

**Keep suggestions concise**: Limit to 2-4 words when possible
```csharp
// Good
Suggestion = new[] { "Tell me more", "Show example", "Alternatives?" };

// Too verbose
Suggestion = new[] { "Could you please tell me more about this topic?" };
```

**Limit quantity**: Show 2-4 suggestions at a time
```csharp
// Good - focused options
Suggestion = new[] { "How?", "Why?", "Example?" };

// Too many - overwhelming
Suggestion = new[] { "How?", "Why?", "When?", "Where?", "What?", "Who?", "Example?" };
```

**Make them actionable**: Use question format or clear action verbs
```csharp
// Good - actionable
Suggestion = new[] { "Show code example", "Explain concept", "Compare approaches" };

// Vague
Suggestion = new[] { "More", "Info", "Stuff" };
```

**Context-relevant**: Suggestions should relate to the current conversation
```csharp
// After discussing databases
Suggestion = new[] { "Connect to SQL Server", "Use Entity Framework", "NoSQL options" };

// Not relevant after database discussion
Suggestion = new[] { "What is CSS?", "JavaScript basics", "HTML structure" };
```

## Complete Example

```csharp
using System;
using System.ComponentModel;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using System.Threading.Tasks;
using System.Windows.Forms;
using Syncfusion.WinForms.AIAssistView;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> chats;
    private IEnumerable<string> suggestion;
    private Author currentUser;
    
    public ViewModel()
    {
        this.Chats = new ObservableCollection<object>();
        this.CurrentUser = new Author { Name = "User" };
        this.Chats.CollectionChanged += Chats_CollectionChanged;
        
        // Initial greeting
        this.Chats.Add(new TextMessage
        {
            Author = new Author { Name = "Bot" },
            Text = "Hello! How can I help you today?"
        });
        
        // Initial suggestions
        this.Suggestion = new ObservableCollection<string>
        {
            "What is WinForms?",
            "Show me examples",
            "Getting started guide"
        };
    }
    
    private void Chats_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
    {
        if (e.Action != NotifyCollectionChangedAction.Add) return;
        
        foreach (var newItem in e.NewItems ?? new object[0])
        {
            if (newItem is TextMessage message && message.Author.Name == "Bot")
            {
                // Update suggestions after bot responds
                UpdateSuggestions(message.Text);
            }
        }
    }
    
    private void UpdateSuggestions(string botText)
    {
        if (botText.Contains("WinForms"))
        {
            Suggestion = new[] { "WPF comparison", "Code example", "Best practices" };
        }
        else
        {
            Suggestion = new[] { "Tell me more", "Show example", "Related topics" };
        }
    }
    
    public ObservableCollection<object> Chats
    {
        get => chats;
        set { chats = value; RaisePropertyChanged(nameof(Chats)); }
    }
    
    public IEnumerable<string> Suggestion
    {
        get => suggestion;
        set { suggestion = value; RaisePropertyChanged(nameof(Suggestion)); }
    }
    
    public Author CurrentUser
    {
        get => currentUser;
        set { currentUser = value; RaisePropertyChanged(nameof(CurrentUser)); }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    protected void RaisePropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }
}
```

## Troubleshooting

**Suggestions not appearing:**
- Verify data binding is established: `DataBindings.Add("Suggestions", ...)`
- Ensure property type is `IEnumerable<string>` (not `List<string>` without interface)
- Check that RaisePropertyChanged is called when updating Suggestion property
- Verify suggestions collection is not null or empty

**Suggestions don't update:**
- Use `ObservableCollection<string>` for automatic updates
- Always call `RaisePropertyChanged("Suggestion")` when updating
- Don't modify the collection directly; replace with new collection

**Click not working:**
- Suggestion clicks are handled automatically by the control
- Listen to `PromptRequest` event to process the clicked suggestion
- The clicked suggestion text is automatically added as a user message
