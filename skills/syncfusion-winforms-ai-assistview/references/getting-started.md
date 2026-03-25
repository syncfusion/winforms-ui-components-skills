# Getting Started with AI AssistView

This guide covers the essential steps to add the Syncfusion Windows Forms AI AssistView (SfAIAssistView) control to your application and implement basic chat functionality.

## Assembly Deployment

The SfAIAssistView control requires specific assemblies to function properly. You can add them via NuGet Package Manager or manual assembly references.

### NuGet Package Installation

Install the required package using NuGet Package Manager:

```powershell
Install-Package Syncfusion.SfAIAssistView.WinForms
```

Or search for "Syncfusion.SfAIAssistView.WinForms" in Visual Studio's NuGet Package Manager UI.

### Required Dependencies

The package automatically includes these dependencies:
- Syncfusion.Core.WinForms
- Syncfusion.Shared.Base
- Syncfusion.Licensing

## Creating a Windows Forms Application

### Step 1: Create New Project

1. Open Visual Studio
2. Select **File > New > Project**
3. Choose **Windows Forms App (.NET Framework)** or **Windows Forms App (.NET)**
4. Name your project (e.g., "AIAssistViewDemo")
5. Click **Create**

### Step 2: Add SfAIAssistView via Designer

The easiest way to add the control is through the Visual Studio Designer:

1. Open your Form in Designer view
2. Open the **Toolbox** (View > Toolbox or Ctrl+Alt+X)
3. Search for "SfAIAssistView"
4. Drag and drop the control onto your Form
5. Required assembly references are added automatically

![Dragging SfAIAssistView from Toolbox](example: control appears in toolbox)

### Step 3: Add SfAIAssistView via Code

Alternatively, add the control programmatically:

```csharp
using Syncfusion.WinForms.AIAssistView;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            
            // Create an instance of the AIAssistView control
            SfAIAssistView sfAIAssistView1 = new SfAIAssistView();
            sfAIAssistView1.Location = new System.Drawing.Point(41, 40);
            sfAIAssistView1.Size = new System.Drawing.Size(818, 457);
            sfAIAssistView1.Dock = DockStyle.Fill;
            
            this.Controls.Add(sfAIAssistView1);
        }
    }
}
```

## Creating a ViewModel

A proper ViewModel is essential for managing chat messages and implementing the MVVM pattern with data binding.

### Basic ViewModel Structure

Create a new class file `ViewModel.cs`:

```csharp
using System.ComponentModel;
using System.Collections.ObjectModel;
using Syncfusion.WinForms.AIAssistView;

public class ViewModel : INotifyPropertyChanged
{
    private ObservableCollection<object> chats;
    private Author currentUser;
    
    public ViewModel()
    {
        this.Chats = new ObservableCollection<object>();
        this.CurrentUser = new Author { Name = "John" };
        this.GenerateMessages();
    }
    
    private async void GenerateMessages()
    {
        // Add user message
        this.Chats.Add(new TextMessage 
        { 
            Author = CurrentUser, 
            Text = "What is Windows Forms?" 
        });
        
        // Simulate AI processing delay
        await Task.Delay(1000);
        
        // Add bot response
        this.Chats.Add(new TextMessage 
        { 
            Author = new Author { Name = "Bot" }, 
            Text = "Windows Forms (also known as WinForms) is a graphical user interface (GUI) framework developed by Microsoft for building desktop applications for the Windows operating system." 
        });
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
    
    public void RaisePropertyChanged(string propName)
    {
        if (PropertyChanged != null)
        {
            PropertyChanged(this, new PropertyChangedEventArgs(propName));
        }
    }
    
    public event PropertyChangedEventHandler PropertyChanged;
}
```

### Key Components Explained

**ObservableCollection\<object\>**: The chat collection that automatically notifies the UI of changes. Use `object` type to support different message types (TextMessage, ImageMessage, etc.).

**Author**: Represents a chat participant with properties like Name and AvatarImage.

**TextMessage**: Represents a text-based chat message with Author, Text, and DateTime properties.

**INotifyPropertyChanged**: Interface that enables two-way data binding by notifying the UI when properties change.

## Binding Messages to the Control

Connect your ViewModel to the SfAIAssistView control:

```csharp
public partial class Form1 : Form
{
    ViewModel viewModel;
    
    public Form1()
    {
        InitializeComponent();
        viewModel = new ViewModel();
        
        // Create control
        SfAIAssistView sfAIAssistView1 = new SfAIAssistView();
        sfAIAssistView1.Location = new System.Drawing.Point(41, 40);
        sfAIAssistView1.Size = new System.Drawing.Size(818, 457);
        sfAIAssistView1.Dock = DockStyle.Fill;
        this.Controls.Add(sfAIAssistView1);
        
        // Bind Messages property to ViewModel
        sfAIAssistView1.DataBindings.Add("Messages", viewModel, "Chats", 
            true, DataSourceUpdateMode.OnPropertyChanged);
    }
}
```

### Data Binding Parameters

- **"Messages"**: The control property to bind
- **viewModel**: The data source object
- **"Chats"**: The ViewModel property name
- **true**: Enable formatting
- **DataSourceUpdateMode.OnPropertyChanged**: Update when property changes

## Complete Working Example

Here's a complete, runnable example:

```csharp
using System;
using System.ComponentModel;
using System.Collections.ObjectModel;
using System.Threading.Tasks;
using System.Windows.Forms;
using Syncfusion.WinForms.AIAssistView;

namespace AIAssistViewDemo
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
        }
    }
    
    public class ViewModel : INotifyPropertyChanged
    {
        private ObservableCollection<object> chats;
        private Author currentUser;
        
        public ViewModel()
        {
            this.Chats = new ObservableCollection<object>();
            this.CurrentUser = new Author { Name = "John" };
            this.GenerateMessages();
        }
        
        private async void GenerateMessages()
        {
            this.Chats.Add(new TextMessage 
            { 
                Author = CurrentUser, 
                Text = "What is Windows Forms?" 
            });
            
            await Task.Delay(1000);
            
            this.Chats.Add(new TextMessage 
            { 
                Author = new Author { Name = "Bot" }, 
                Text = "Windows Forms is a GUI framework for building Windows desktop applications." 
            });
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
}
```

## Adding User Input Handling

To handle user input from the text box:

```csharp
// In Form constructor
sfAIAssistView1.PromptRequest += AIAssistView_PromptRequest;

private async void AIAssistView_PromptRequest(object sender, PromptRequestEventArgs e)
{
    var userMessage = e.Message as TextMessage;
    if (userMessage == null) return;
    
    // Add user message (already added by control)
    // Simulate AI response
    await Task.Delay(1000);
    
    viewModel.Chats.Add(new TextMessage
    {
        Author = new Author { Name = "Bot" },
        Text = $"You said: {userMessage.Text}"
    });
}
```

## Common Setup Issues

**Issue: Control doesn't appear**
- Verify the control was added to `this.Controls`
- Check Size and Location properties
- Try setting `Dock = DockStyle.Fill`

**Issue: Messages not showing**
- Ensure data binding is established correctly
- Verify ViewModel uses `ObservableCollection<object>` (not List)
- Check RaisePropertyChanged is called when Chats property updates

**Issue: NuGet package not found**
- Ensure you have the Syncfusion NuGet feed configured
- Try searching for "Syncfusion.SfAIAssistView.WinForms" explicitly
- Check your internet connection

**Issue: License error on runtime**
- Register your Syncfusion license key in application startup
- Get a free community license at syncfusion.com

## Next Steps

- **Add Suggestions**: Read `suggestions.md` to display quick response options
- **Show Typing Indicator**: Read `typing-indicator.md` for async feedback
- **Customize Appearance**: Read `customization.md` for banner and view customization
- **Integrate OpenAI**: Read `openai-integration.md` for AI service connection
