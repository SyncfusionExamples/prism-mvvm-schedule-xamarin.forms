# How to bind appointments in Xamarin.Forms Schedule (SfSchedule) using Prism framework ?
The SfSchedule allows you to work with Prism framework. To achieve this, follow these steps:

**Step 1:** Install the Prism Unity Forms NuGet package in your shared code project.

**Step 2:** Inherit the App.xaml and App.xaml.cs files from PrismApplication instead of Application. 

**XAML**
``` xml
<prism:PrismApplication
        xmlns:prism="clr-namespace:Prism.Unity;assembly=Prism.Unity.Forms"
        xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="ScheduleXamarin.App">
    <Application.Resources>
    </Application.Resources>
</prism:PrismApplication>
```
**C#**
``` c#
public partial class App : PrismApplication
{
        public App(IPlatformInitializer initializer = null) : base(initializer) { }

        protected override async void OnInitialized()
        {
            InitializeComponent();

            await NavigationService.NavigateAsync("SchedulerPage");
        }

        protected override void RegisterTypes(IContainerRegistry containerRegistry)
        {
        }
 }
```
**Step 3:** Add platform initializers to each cross-platform project. Create a new class that implements IPlatformInitializer and pass it to the LoadApplication method on each platform.

**Android**
``` c#
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);
        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App(new AndroidInitializer()));
    }
}

public class AndroidInitializer : IPlatformInitializer
{
    public void RegisterTypes(IContainerRegistry containerRegistry)
    {
        // Register any platform specific implementations
    }
}
```
**iOS**
``` c#
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    //
    // This method is invoked when the application has loaded and is ready to run. In this 
    // Method you should instantiate the window, load the UI into it and then make the window
    // Visible
    //
    // You have 17 seconds to return from this method, or iOS will terminate your application
    //
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init();
        SfScheduleRenderer.Init();
        LoadApplication(new App(new iOSInitializer()));

        return base.FinishedLaunching(app, options);
    }

    public class iOSInitializer : IPlatformInitializer
    {
        public void RegisterTypes(IContainerRegistry containerRegistry)
        {
            // Register any platform specific implementations
        }
    }
}
```
**UWP**
``` c#
public sealed partial class MainPage
{
    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new DataFormXamarin.App(new UwpInitializer()));
    }
}

public class UwpInitializer : IPlatformInitializer
{
    public void RegisterTypes(IContainerRegistry containerRegistry)
    {
        // Register any platform specific implementations
    }
}
```
**Step 4:** In App.xaml.cs file, register the View and respective ViewModel pages.
``` C#
protected override void RegisterTypes(IContainerRegistry containerRegistry)
{
        containerRegistry.RegisterForNavigation<SchedulerPage, SchedulerViewModel>();
}
```
**Step 5:** Bind the appointment collection to the Schedule DataSource without mentioning the binding context.
``` xml
<schedule:SfSchedule x:Name="schedule" ScheduleView="MonthView" DataSource="{Binding Meetings}">
            <schedule:SfSchedule.MonthViewSettings>
                <schedule:MonthViewSettings ShowAgendaView="True"/>
            </schedule:SfSchedule.MonthViewSettings>
            <schedule:SfSchedule.AppointmentMapping>
                <schedule:ScheduleAppointmentMapping SubjectMapping="EventName"
                                                     StartTimeMapping="From"
                                                     EndTimeMapping="To"
                                                     ColorMapping="Color"/>
            </schedule:SfSchedule.AppointmentMapping>        
</schedule:SfSchedule>
```


