# OpenAI Integration for AI AssistView

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Setting Up OpenAI Connection](#setting-up-openai-connection)
- [Creating the Service Class](#creating-the-service-class)
- [Implementing Chat Functionality](#implementing-chat-functionality)
- [Complete Implementation](#complete-implementation)
- [Advanced Scenarios](#advanced-scenarios)
- [Troubleshooting](#troubleshooting)

## Overview

This guide demonstrates how to connect the AI AssistView control with OpenAI services (including ChatGPT and Azure OpenAI) to create intelligent conversational interfaces powered by large language models.

## Prerequisites

### Required NuGet Packages

Install these packages via NuGet Package Manager:

```powershell
# Syncfusion AI AssistView
Install-Package Syncfusion.SfAIAssistView.WinForms

# Microsoft Semantic Kernel for AI integration
Install-Package Microsoft.SemanticKernel
```

### OpenAI Account Setup

You need one of the following:

**Option 1: OpenAI API**
- Account at https://platform.openai.com/
- API key from https://platform.openai.com/api-keys
- Sufficient credits or active subscription

**Option 2: Azure OpenAI Service**
- Azure subscription
- Azure OpenAI resource deployed
- Deployment name (e.g., "gpt-4o-mini")
- API key and endpoint URL

## Setting Up OpenAI Connection

### Step 1: Create Windows Forms Application

1. Create a new Windows Forms App (.NET)
2. Add reference to `Syncfusion.SfAIAssistView.WinForms` NuGet
3. Add reference to `Microsoft.SemanticKernel` NuGet
4. Import the namespace: `using Syncfusion.WinForms.AIAssistView;`
5. Initialize the SfAIAssistView control

### Step 2: Configure API Credentials

Store your OpenAI credentials securely. **Never hardcode API keys in production code.**

```csharp
// Configuration constants
private const string OPENAI_KEY = "your-api-key-here";        // Replace with your key
private const string OPENAI_MODEL = "gpt-4o-mini";            // Model name
private const string API_ENDPOINT = "https://api.openai.com"; // Or Azure endpoint
```

**Security Best Practices:**
- Use environment variables: `Environment.GetEnvironmentVariable("OPENAI_KEY")`
- Use configuration files with encryption
- Use Azure Key Vault for production
- Never commit keys to source control

## Creating the Service Class

### AIAssistChatService Class

Create a service class to handle OpenAI communication:

```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.ChatCompletion;

public class AIAssistChatService
{
    private IChatCompletionService chatService;
    private Kernel kernel;
    
    private string OPENAI_KEY = "your-api-key";
    private string OPENAI_MODEL = "gpt-4o-mini";
    private string API_ENDPOINT = "https://openai.azure.com";
    
    public string Response { get; set; }
    
    public async Task Initialize()
    {
        var builder = Kernel.CreateBuilder();
        
        // Azure OpenAI configuration
        builder.AddAzureOpenAIChatCompletion(
            deploymentName: OPENAI_MODEL,
            apiKey: OPENAI_KEY,
            endpoint: API_ENDPOINT
        );
        
        kernel = builder.Build();
        chatService = kernel.GetRequiredService<IChatCompletionService>();
    }
    
    public async Task NonStreamingChat(string userMessage)
    {
        Response = string.Empty;
        var response = await chatService.GetChatMessageContentAsync(userMessage);
        Response = response.ToString();
    }
}
```

### For Standard OpenAI API

If using OpenAI's API (not Azure):

```csharp
public async Task Initialize()
{
    var builder = Kernel.CreateBuilder();
    
    // Standard OpenAI configuration
    builder.AddOpenAIChatCompletion(
        modelId: OPENAI_MODEL,
        apiKey: OPENAI_KEY
    );
    
    kernel = builder.Build();
    chatService = kernel.GetRequiredService<IChatCompletionService>();
}
```

## Implementing Chat Functionality

### ViewModel with OpenAI Integration

Create a ViewModel that integrates the AIAssistChatService:

```csharp
using System.ComponentModel;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using Syncfusion.WinForms.AIAssistView;

public class ViewModel : INotifyPropertyChanged
{
    private AIAssistChatService service;
    private ObservableCollection<object> chats;
    private IEnumerable<string> suggestion;
    private bool showTypingIndicator;
    private Author currentUser;
    
    public ViewModel()
    {
        this.Chats = new ObservableCollection<object>();
        this.CurrentUser = new Author { Name = "User" };
        this.Chats.CollectionChanged += Chats_CollectionChanged;
        
        // Initialize AI service
        service = new AIAssistChatService();
        _ = service.Initialize(); // Fire and forget initialization
    }
    
    private async void Chats_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
    {
        if (e.Action != NotifyCollectionChangedAction.Add) return;
        
        var item = e.NewItems?[0] as TextMessage;
        if (item == null) return;
        
        // Only process user messages
        if (item.Author?.Name == CurrentUser?.Name)
        {
            ShowTypingIndicator = true;
            
            try
            {
                // Get AI response
                await service.NonStreamingChat(item.Text);
                
                // Add bot response
                Chats.Add(new TextMessage
                {
                    Author = new Author 
                    { 
                        Name = "Bot", 
                        AvatarImage = Image.FromFile(@"Assets\AI_Assist.png") 
                    },
                    DateTime = DateTime.Now,
                    Text = service.Response
                });
            }
            catch (Exception ex)
            {
                // Handle errors gracefully
                Chats.Add(new TextMessage
                {
                    Author = new Author { Name = "Bot" },
                    DateTime = DateTime.Now,
                    Text = $"I encountered an error: {ex.Message}"
                });
            }
            finally
            {
                ShowTypingIndicator = false;
            }
        }
    }
    
    // Properties
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
    
    public bool ShowTypingIndicator
    {
        get => showTypingIndicator;
        set { showTypingIndicator = value; RaisePropertyChanged(nameof(ShowTypingIndicator)); }
    }
    
    public Author CurrentUser
    {
        get => currentUser;
        set { currentUser = value; RaisePropertyChanged(nameof(CurrentUser)); }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    public void RaisePropertyChanged(string propName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
    }
}
```

### Form Implementation

Wire up the ViewModel to the SfAIAssistView control:

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
        
        // Bind properties
        sfAIAssistView1.DataBindings.Add("Messages", viewModel, "Chats", 
            true, DataSourceUpdateMode.OnPropertyChanged);
        sfAIAssistView1.DataBindings.Add("ShowTypingIndicator", viewModel, 
            "ShowTypingIndicator", true, DataSourceUpdateMode.OnPropertyChanged);
        sfAIAssistView1.DataBindings.Add("Suggestions", viewModel, "Suggestion", 
            true, DataSourceUpdateMode.OnPropertyChanged);
        
        viewModel.CurrentUser = sfAIAssistView1.User;
        
        // Configure typing indicator
        sfAIAssistView1.TypingIndicator.Author = new Author 
        { 
            Name = "Bot", 
            AvatarImage = Image.FromFile(@"Assets\AI_Assist.png") 
        };
        sfAIAssistView1.TypingIndicator.DisplayText = "Typing";
    }
}
```

## Complete Implementation

### Full Working Example

```csharp
using System;
using System.ComponentModel;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using System.Threading.Tasks;
using System.Windows.Forms;
using Syncfusion.WinForms.AIAssistView;
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.ChatCompletion;

namespace AIAssistViewOpenAI
{
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
            
            sfAIAssistView1.DataBindings.Add("Messages", viewModel, "Chats", 
                true, DataSourceUpdateMode.OnPropertyChanged);
            sfAIAssistView1.DataBindings.Add("ShowTypingIndicator", viewModel, 
                "ShowTypingIndicator", true, DataSourceUpdateMode.OnPropertyChanged);
            sfAIAssistView1.DataBindings.Add("Suggestions", viewModel, 
                "Suggestion", true, DataSourceUpdateMode.OnPropertyChanged);
            
            viewModel.CurrentUser = sfAIAssistView1.User;
            
            sfAIAssistView1.TypingIndicator.Author = new Author 
            { 
                Name = "Bot", 
                AvatarImage = Image.FromFile(@"Assets\AI_Assist.png") 
            };
            sfAIAssistView1.TypingIndicator.DisplayText = "Typing";
        }
    }
    
    public class ViewModel : INotifyPropertyChanged
    {
        private AIAssistChatService service;
        private ObservableCollection<object> chats;
        private IEnumerable<string> suggestion;
        private bool showTypingIndicator;
        private Author currentUser;
        
        public ViewModel()
        {
            this.Chats = new ObservableCollection<object>();
            this.CurrentUser = new Author { Name = "User" };
            this.Chats.CollectionChanged += Chats_CollectionChanged;
            
            service = new AIAssistChatService();
            _ = service.Initialize();
        }
        
        private async void Chats_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
        {
            if (e.Action != NotifyCollectionChangedAction.Add) return;
            
            var item = e.NewItems?[0] as TextMessage;
            if (item?.Author?.Name == CurrentUser?.Name)
            {
                ShowTypingIndicator = true;
                
                try
                {
                    await service.NonStreamingChat(item.Text);
                    Chats.Add(new TextMessage
                    {
                        Author = new Author { Name = "Bot" },
                        DateTime = DateTime.Now,
                        Text = service.Response
                    });
                }
                catch (Exception ex)
                {
                    Chats.Add(new TextMessage
                    {
                        Author = new Author { Name = "Bot" },
                        Text = $"Error: {ex.Message}"
                    });
                }
                finally
                {
                    ShowTypingIndicator = false;
                }
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
        
        public bool ShowTypingIndicator
        {
            get => showTypingIndicator;
            set { showTypingIndicator = value; RaisePropertyChanged(nameof(ShowTypingIndicator)); }
        }
        
        public Author CurrentUser
        {
            get => currentUser;
            set { currentUser = value; RaisePropertyChanged(nameof(CurrentUser)); }
        }
        
        public event PropertyChangedEventHandler PropertyChanged;
        
        public void RaisePropertyChanged(string propName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName));
        }
    }
    
    public class AIAssistChatService
    {
        private IChatCompletionService chatService;
        private Kernel kernel;
        
        private string OPENAI_KEY = "your-api-key-here";
        private string OPENAI_MODEL = "gpt-4o-mini";
        private string API_ENDPOINT = "https://openai.azure.com";
        
        public string Response { get; set; }
        
        public async Task Initialize()
        {
            var builder = Kernel.CreateBuilder();
            builder.AddAzureOpenAIChatCompletion(
                deploymentName: OPENAI_MODEL,
                apiKey: OPENAI_KEY,
                endpoint: API_ENDPOINT
            );
            
            kernel = builder.Build();
            chatService = kernel.GetRequiredService<IChatCompletionService>();
        }
        
        public async Task NonStreamingChat(string userMessage)
        {
            Response = string.Empty;
            var response = await chatService.GetChatMessageContentAsync(userMessage);
            Response = response.ToString();
        }
    }
}
```

## Advanced Scenarios

### Streaming Responses

For real-time token-by-token response display:

```csharp
public async Task StreamingChat(string userMessage, Action<string> onTokenReceived)
{
    Response = string.Empty;
    
    await foreach (var token in chatService.GetStreamingChatMessageContentsAsync(userMessage))
    {
        Response += token.Content;
        onTokenReceived?.Invoke(token.Content);
    }
}

// In ViewModel
private async void ProcessStreamingResponse(string userMessage)
{
    ShowTypingIndicator = false; // Hide indicator for streaming
    
    var botMessage = new TextMessage
    {
        Author = new Author { Name = "Bot" },
        Text = "",
        DateTime = DateTime.Now
    };
    Chats.Add(botMessage);
    
    await service.StreamingChat(userMessage, (token) =>
    {
        botMessage.Text += token;
        // Trigger UI update
        RaisePropertyChanged(nameof(Chats));
    });
}
```

### Conversation History

Maintain context across multiple messages:

```csharp
public class AIAssistChatService
{
    private ChatHistory chatHistory;
    
    public async Task Initialize()
    {
        // ... existing initialization ...
        chatHistory = new ChatHistory();
        chatHistory.AddSystemMessage("You are a helpful assistant.");
    }
    
    public async Task ChatWithHistory(string userMessage)
    {
        chatHistory.AddUserMessage(userMessage);
        
        var response = await chatService.GetChatMessageContentAsync(chatHistory);
        
        chatHistory.AddAssistantMessage(response.Content);
        Response = response.Content;
    }
}
```

### System Prompts and Personality

```csharp
public async Task Initialize(string systemPrompt = null)
{
    var builder = Kernel.CreateBuilder();
    builder.AddAzureOpenAIChatCompletion(
        deploymentName: OPENAI_MODEL,
        apiKey: OPENAI_KEY,
        endpoint: API_ENDPOINT
    );
    
    kernel = builder.Build();
    chatService = kernel.GetRequiredService<IChatCompletionService>();
    
    chatHistory = new ChatHistory();
    
    // Custom system prompt
    string prompt = systemPrompt ?? 
        "You are a friendly and knowledgeable Windows Forms development assistant. " +
        "Provide clear, concise answers with code examples when appropriate.";
    
    chatHistory.AddSystemMessage(prompt);
}
```

### Token Usage Tracking

```csharp
public class AIAssistChatService
{
    public int TotalTokensUsed { get; private set; }
    
    public async Task NonStreamingChat(string userMessage)
    {
        Response = string.Empty;
        
        var response = await chatService.GetChatMessageContentAsync(userMessage);
        Response = response.ToString();
        
        // Track token usage (if available in metadata)
        if (response.Metadata?.ContainsKey("Usage") == true)
        {
            var usage = response.Metadata["Usage"];
            // Parse and accumulate token usage
        }
    }
}
```

### Error Handling and Retry Logic

```csharp
public async Task<string> ChatWithRetry(string userMessage, int maxRetries = 3)
{
    int retryCount = 0;
    Exception lastException = null;
    
    while (retryCount < maxRetries)
    {
        try
        {
            var response = await chatService.GetChatMessageContentAsync(userMessage);
            return response.ToString();
        }
        catch (HttpRequestException ex)
        {
            lastException = ex;
            retryCount++;
            await Task.Delay(1000 * retryCount); // Exponential backoff
        }
    }
    
    throw new Exception($"Failed after {maxRetries} retries", lastException);
}
```

## Troubleshooting

### Common Issues

**"Unauthorized" or 401 Error:**
- Verify API key is correct and active
- Check if key has proper permissions
- Ensure endpoint URL is correct
- For Azure: Verify deployment name matches

**"Rate limit exceeded":**
- Implement rate limiting in your application
- Add delays between requests
- Consider upgrading API tier
- Cache responses when possible

**Slow responses:**
- Check network connectivity
- Consider using streaming responses
- Verify you're using appropriate model (gpt-4o-mini is faster than gpt-4)
- Implement timeout handling

**Empty or null responses:**
- Check if model deployment is active (Azure)
- Verify prompt is not empty
- Check for API service outages
- Inspect response metadata for errors

### Debug Configuration

```csharp
public async Task Initialize(bool enableLogging = false)
{
    var builder = Kernel.CreateBuilder();
    
    if (enableLogging)
    {
        builder.Services.AddLogging(logging =>
        {
            logging.AddConsole();
            logging.SetMinimumLevel(LogLevel.Debug);
        });
    }
    
    builder.AddAzureOpenAIChatCompletion(
        deploymentName: OPENAI_MODEL,
        apiKey: OPENAI_KEY,
        endpoint: API_ENDPOINT
    );
    
    kernel = builder.Build();
    chatService = kernel.GetRequiredService<IChatCompletionService>();
}
```

### Testing Without API Calls

For development/testing without consuming API credits:

```csharp
public class MockAIAssistChatService : AIAssistChatService
{
    public override async Task NonStreamingChat(string userMessage)
    {
        await Task.Delay(1000); // Simulate network delay
        Response = $"Mock response to: {userMessage}";
    }
}
```

## Best Practices

1. **Secure API Keys**: Never hardcode, use environment variables or secure vaults
2. **Handle Errors**: Wrap API calls in try-catch with user-friendly messages
3. **Show Feedback**: Use typing indicators during API calls
4. **Rate Limiting**: Implement delays to avoid rate limit errors
5. **Timeout Handling**: Set reasonable timeouts for API calls
6. **Cost Management**: Track token usage, cache responses when possible
7. **User Experience**: Provide fallback responses if API is unavailable
8. **Testing**: Use mock services during development

## Resources

- **Semantic Kernel Docs**: https://learn.microsoft.com/semantic-kernel/
- **OpenAI API Docs**: https://platform.openai.com/docs/
- **Azure OpenAI**: https://azure.microsoft.com/products/ai-services/openai-service
- **Syncfusion Docs**: https://help.syncfusion.com/windowsforms/aiassistview/
