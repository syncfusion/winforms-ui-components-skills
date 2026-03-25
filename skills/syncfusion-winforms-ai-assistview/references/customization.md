# Customization in AI AssistView

## Table of Contents
- [Overview](#overview)
- [Banner Customization](#banner-customization)
- [Custom BotView](#custom-botview)
- [Custom UserView](#custom-userview)
- [Advanced Customization](#advanced-customization)
- [Best Practices](#best-practices)

## Overview

The AI AssistView offers extensive customization options to match your application's branding and UX requirements. You can customize:
- **BannerView**: The header area with title, subtitle, and icon
- **BotView**: The appearance of AI/bot messages
- **UserView**: The appearance of user messages

## Banner Customization

The banner appears at the top of the AssistView and typically displays a welcome message, AI service information, or branding.

### SetBannerView Method

```csharp
public void SetBannerView(string title, string subtitle, Image image, BannerStyle style)
```

**Parameters:**
- `title`: Main heading text
- `subtitle`: Secondary text below the title
- `image`: Icon or logo image
- `style`: BannerStyle object with appearance settings

### BannerStyle Properties

```csharp
public class BannerStyle
{
    public Font TitleFont { get; set; }
    public Font SubTitleFont { get; set; }
    public AvatarSize ImageSize { get; set; }
    public Color TitleColor { get; set; }
    public Color SubTitleColor { get; set; }
}
```

### Basic Banner Example

```csharp
private void SetupBanner()
{
    BannerStyle customStyle = new BannerStyle
    {
        TitleFont = new Font("Segoe UI", 14F, FontStyle.Bold),
        SubTitleFont = new Font("Segoe UI", 12F, FontStyle.Italic),
        ImageSize = AvatarSize.Medium,
        SubTitleColor = Color.Gray,
        TitleColor = Color.DarkBlue
    };
    
    string title = "AI Assistant";
    string subTitle = "Your intelligent companion";
    
    sfAIAssistView1.SetBannerView(
        title, 
        subTitle, 
        Image.FromFile(@"Assets\AI_Assist.png"), 
        customStyle
    );
}
```

### Complete Banner Implementation

```csharp
public partial class Form1 : Form
{
    ViewModel viewModel;
    private SfAIAssistView sfAIAssistView1;
    
    public Form1()
    {
        InitializeComponent();
        viewModel = new ViewModel();
        
        sfAIAssistView1 = new SfAIAssistView();
        sfAIAssistView1.Dock = DockStyle.Fill;
        this.Controls.Add(sfAIAssistView1);
        
        sfAIAssistView1.DataBindings.Add("Messages", viewModel, "Chats", 
            true, DataSourceUpdateMode.OnPropertyChanged);
        
        viewModel.CurrentUser = sfAIAssistView1.User;
        
        // Customize banner
        CustomizeBanner();
    }
    
    private void CustomizeBanner()
    {
        BannerStyle style = new BannerStyle
        {
            TitleFont = new Font("Arial", 16F, FontStyle.Bold),
            SubTitleFont = new Font("Arial", 11F, FontStyle.Regular),
            ImageSize = AvatarSize.Large,
            TitleColor = Color.FromArgb(0, 120, 212), // Blue
            SubTitleColor = Color.FromArgb(96, 94, 92) // Gray
        };
        
        sfAIAssistView1.SetBannerView(
            "Customer Support AI",
            "Available 24/7 to help you",
            Properties.Resources.SupportIcon,
            style
        );
    }
}
```

### Banner Use Cases

**Welcome Banner:**
```csharp
sfAIAssistView1.SetBannerView(
    "Welcome!",
    "Ask me anything about our products",
    welcomeImage,
    standardStyle
);
```

**AI Service Branding:**
```csharp
sfAIAssistView1.SetBannerView(
    "Powered by GPT-4",
    "Advanced AI language model",
    openAILogo,
    brandedStyle
);
```

**Application Context:**
```csharp
sfAIAssistView1.SetBannerView(
    "Document Assistant",
    "Help with forms and documentation",
    documentIcon,
    professionalStyle
);
```

## Custom BotView

Create custom bot message layouts to enhance the chat experience with buttons, rich formatting, or interactive elements.

### SetBotView Method

```csharp
public void SetBotView(object message, Control customView)
```

**Parameters:**
- `message`: The TextMessage object to customize
- `customView`: Your custom WinForms Control

### Basic Custom BotView

```csharp
public Form1()
{
    InitializeComponent();
    viewModel = new ViewModel();
    
    // Subscribe to collection changes
    viewModel.Chats.CollectionChanged += Chats_CollectionChanged;
    
    // Apply custom views to existing messages
    foreach (var item in viewModel.Chats)
    {
        if (item is TextMessage tm && tm.Author.Name == "Bot")
        {
            sfAIAssistView1.SetBotView(tm, CreateBotView(tm));
        }
    }
}

private void Chats_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
{
    if (e.Action != NotifyCollectionChangedAction.Add) return;
    
    foreach (var newItem in e.NewItems ?? new object[0])
    {
        if (newItem is TextMessage message && message.Author.Name == "Bot")
        {
            sfAIAssistView1.SetBotView(message, CreateBotView(message));
        }
    }
}

private Control CreateBotView(TextMessage message)
{
    string text = message?.Text ?? "Hello from the bot.";
    
    var label = new Label
    {
        Text = text,
        AutoSize = true,
        BackColor = Color.FromArgb(230, 240, 255),
        ForeColor = Color.FromArgb(24, 24, 24),
        Padding = new Padding(8),
        Margin = new Padding(0, 0, 0, 6)
    };
    
    return label;
}
```

### Interactive BotView with Buttons

```csharp
private Control CreateBotView(TextMessage message)
{
    string text = message?.Text ?? string.Empty;
    
    var container = new FlowLayoutPanel
    {
        AutoSize = true,
        WrapContents = true,
        Padding = new Padding(6),
        BackColor = Color.Transparent,
        FlowDirection = FlowDirection.TopDown
    };
    
    // Message text
    var messageLabel = new Label
    {
        Text = text,
        AutoSize = true,
        MaximumSize = new Size(500, 0),
        BackColor = Color.FromArgb(230, 240, 255),
        ForeColor = Color.FromArgb(24, 24, 24),
        Padding = new Padding(8),
        Margin = new Padding(0, 0, 0, 6)
    };
    
    container.Controls.Add(messageLabel);
    
    // Add interactive buttons for specific messages
    const string prompt = "I am an AI assistant. Please choose from the options below";
    if (string.Equals(text?.Trim(), prompt, StringComparison.OrdinalIgnoreCase))
    {
        var buttonRow = new FlowLayoutPanel
        {
            AutoSize = true,
            WrapContents = true,
            Margin = new Padding(0)
        };
        
        string[] choices = new[] { "What is WinForms?", "What is AI?" };
        
        foreach (var choice in choices)
        {
            var button = new Button
            {
                Text = choice,
                AutoSize = true,
                Tag = choice,
                BackColor = Color.WhiteSmoke,
                ForeColor = Color.Black,
                Margin = new Padding(0, 0, 6, 0),
                Cursor = Cursors.Hand
            };
            
            button.Click += (s, e) =>
            {
                try
                {
                    var selectedChoice = (string)((Button)s).Tag;
                    viewModel?.Chats.Add(new TextMessage
                    {
                        Author = viewModel.CurrentUser,
                        Text = selectedChoice,
                        DateTime = DateTime.Now
                    });
                }
                catch { }
            };
            
            buttonRow.Controls.Add(button);
        }
        
        container.Controls.Add(buttonRow);
    }
    
    return container;
}
```

### Rich Formatted BotView

```csharp
private Control CreateBotView(TextMessage message)
{
    var panel = new Panel
    {
        AutoSize = true,
        Padding = new Padding(10),
        BackColor = Color.FromArgb(240, 248, 255),
        BorderStyle = BorderStyle.None
    };
    
    // Avatar
    var avatar = new PictureBox
    {
        Image = Image.FromFile(@"Assets\bot-avatar.png"),
        SizeMode = PictureBoxSizeMode.StretchImage,
        Size = new Size(32, 32),
        Location = new Point(10, 10)
    };
    
    // Message with author name
    var authorLabel = new Label
    {
        Text = "AI Assistant",
        Font = new Font("Segoe UI", 9F, FontStyle.Bold),
        ForeColor = Color.DarkBlue,
        AutoSize = true,
        Location = new Point(50, 10)
    };
    
    var messageLabel = new Label
    {
        Text = message.Text,
        Font = new Font("Segoe UI", 10F),
        MaximumSize = new Size(450, 0),
        AutoSize = true,
        Location = new Point(50, 30)
    };
    
    panel.Controls.Add(avatar);
    panel.Controls.Add(authorLabel);
    panel.Controls.Add(messageLabel);
    panel.Height = Math.Max(50, messageLabel.Bottom + 10);
    
    return panel;
}
```

### Code Highlighting BotView

```csharp
private Control CreateBotView(TextMessage message)
{
    // Check if message contains code
    if (message.Text.Contains("```"))
    {
        return CreateCodeMessageView(message);
    }
    
    return CreateStandardMessageView(message);
}

private Control CreateCodeMessageView(TextMessage message)
{
    var container = new FlowLayoutPanel
    {
        AutoSize = true,
        FlowDirection = FlowDirection.TopDown,
        Padding = new Padding(8)
    };
    
    // Split message into text and code blocks
    var parts = message.Text.Split(new[] { "```" }, StringSplitOptions.None);
    
    for (int i = 0; i < parts.Length; i++)
    {
        if (i % 2 == 0)
        {
            // Regular text
            if (!string.IsNullOrWhiteSpace(parts[i]))
            {
                container.Controls.Add(new Label
                {
                    Text = parts[i].Trim(),
                    AutoSize = true,
                    Padding = new Padding(4)
                });
            }
        }
        else
        {
            // Code block
            var codeBox = new TextBox
            {
                Text = parts[i].Trim(),
                Multiline = true,
                ReadOnly = true,
                BackColor = Color.FromArgb(30, 30, 30),
                ForeColor = Color.FromArgb(220, 220, 220),
                Font = new Font("Consolas", 9F),
                BorderStyle = BorderStyle.None,
                Width = 450,
                Height = parts[i].Split('\n').Length * 20 + 10
            };
            
            container.Controls.Add(codeBox);
        }
    }
    
    return container;
}
```

## Custom UserView

Customize the appearance of user messages to distinguish them from bot messages or match your application theme.

### SetUserView Method

```csharp
public void SetUserView(object message, Control customView)
```

### Basic Custom UserView

```csharp
private void Chats_CollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
{
    if (e.Action != NotifyCollectionChangedAction.Add) return;
    
    foreach (var newItem in e.NewItems ?? new object[0])
    {
        if (newItem is TextMessage message)
        {
            if (message.Author.Name == viewModel.CurrentUser.Name)
            {
                sfAIAssistView1.SetUserView(message, CreateUserView(message));
            }
            else
            {
                sfAIAssistView1.SetBotView(message, CreateBotView(message));
            }
        }
    }
}

private Control CreateUserView(TextMessage message)
{
    string content = message?.Text ?? string.Empty;
    
    var label = new Label
    {
        Text = content,
        AutoSize = true,
        MaximumSize = new Size(520, 0),
        BackColor = Color.FromArgb(0, 120, 212), // Blue
        ForeColor = Color.White,
        Padding = new Padding(8),
        Margin = new Padding(0, 0, 0, 6)
    };
    
    return label;
}
```

### Right-Aligned UserView

```csharp
private Control CreateUserView(TextMessage message)
{
    var container = new FlowLayoutPanel
    {
        AutoSize = true,
        FlowDirection = FlowDirection.RightToLeft,
        Dock = DockStyle.Right,
        Padding = new Padding(6)
    };
    
    var messageLabel = new Label
    {
        Text = message.Text,
        AutoSize = true,
        MaximumSize = new Size(450, 0),
        BackColor = Color.FromArgb(0, 120, 215),
        ForeColor = Color.White,
        Padding = new Padding(10, 8, 10, 8),
        TextAlign = ContentAlignment.MiddleRight
    };
    
    container.Controls.Add(messageLabel);
    
    return container;
}
```

### UserView with Timestamp

```csharp
private Control CreateUserView(TextMessage message)
{
    var container = new Panel
    {
        AutoSize = true,
        Padding = new Padding(8)
    };
    
    var messageLabel = new Label
    {
        Text = message.Text,
        AutoSize = true,
        MaximumSize = new Size(400, 0),
        BackColor = Color.DodgerBlue,
        ForeColor = Color.White,
        Padding = new Padding(8),
        Location = new Point(0, 0)
    };
    
    var timestampLabel = new Label
    {
        Text = message.DateTime.ToString("HH:mm"),
        Font = new Font("Segoe UI", 8F),
        ForeColor = Color.Gray,
        AutoSize = true,
        Location = new Point(messageLabel.Width + 5, messageLabel.Height - 15)
    };
    
    container.Controls.Add(messageLabel);
    container.Controls.Add(timestampLabel);
    container.Height = messageLabel.Height;
    
    return container;
}
```

## Advanced Customization

### Conditional Styling

```csharp
private Control CreateBotView(TextMessage message)
{
    Color backgroundColor;
    Color textColor;
    
    // Different colors for different message types
    if (message.Text.StartsWith("Error:"))
    {
        backgroundColor = Color.FromArgb(255, 230, 230);
        textColor = Color.DarkRed;
    }
    else if (message.Text.StartsWith("Success:"))
    {
        backgroundColor = Color.FromArgb(230, 255, 230);
        textColor = Color.DarkGreen;
    }
    else
    {
        backgroundColor = Color.FromArgb(240, 240, 240);
        textColor = Color.Black;
    }
    
    return new Label
    {
        Text = message.Text,
        AutoSize = true,
        BackColor = backgroundColor,
        ForeColor = textColor,
        Padding = new Padding(8)
    };
}
```

## Best Practices

### Responsive Design

**Use MaximumSize for text wrapping:**
```csharp
var label = new Label
{
    Text = message.Text,
    AutoSize = true,
    MaximumSize = new Size(500, 0), // Wrap at 500 pixels
    Padding = new Padding(8)
};
```

## Troubleshooting

**Custom view not appearing:**
- Verify SetBotView/SetUserView is called for each message
- Check CollectionChanged event is subscribed
- Ensure controls have non-zero size

**Layout issues:**
- Set AutoSize = true on containers
- Use FlowLayoutPanel for dynamic layouts
- Set appropriate MaximumSize for wrapping

**Buttons not clickable:**
- Verify button is added to container
- Check z-order and overlapping controls
- Ensure container doesn't have Click handlers that interfere
