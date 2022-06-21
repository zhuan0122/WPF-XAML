
1. https://docs.microsoft.com/en-us/visualstudio/xaml-tools/xaml-overview?view=vs-2022

2. https://www.guru99.com/wpf-tutorial.html
   

*  Private --
*  Piblic --
*  Virtual --
*  Partial -- 
*  static -- 

  

* Structure of WPF project 
  * fronted xaml file for UI 
  * code behind in .xaml.cs file in c#, one xaml file linked to on .xaml.cs file. 
* Difference between .NET core and .NET framework 
* XAML 
  *  assign sender AS button 
  *  create event handler for buttons both in xaml file and in .xmal.cs file 



###  MVVM patterns -- the patterns nautural to XAML applications 

** Data binding is the key** 

** Structure of of the MVVM pattern 
    * Model <Sends notifications> to VIEWMODEL for any changes did in MODEL 
    * VIEWMODEL <Updates> to MODEL 
    * VIEWMODEL <Sends notifications> To VIEW about changes from MODEL. VIEWMODEL is the intermediary between MODEL and VIEW(layout/xmal file)
    * VIEW <Data binding> TO VIEWMODEL  how??? how to bind data grid or any interface to the property in database? 
  
** MODEL 
-- it is about the data so it is data model 
-- contains business and validation logic so it is where we implement our classes with properties and interface that are gonna to be used in the 
VIEWMODEL 

** VIEW 
--Layout of what is on the screen, namely mapping to xaml file
-- limited code behind, usually we remove all our code behind from xaml file so no businees logic here 
-- view gets data from VIEWMODEL through bindings or methods-

** VIWMODEL 
-- intermediray between VIEW and MODEL 
-- iMPLEMENT view LOGIC 
-- Provides data from MODEL all the way to the VIEW so data is actually loaded in MODEL and represented as class with properties 


-- VIEW MODEL views MODEL.
-- VIEW views VIEW MODEL 

### 1.1 MODEL 

Defining the model by creating the classes that represent the objects. inside this classes we could have concreate functionality implemented if the functionality is exclusive for this class. if not, we should not implement functionality body here but we have interface here- 

**when you have json file and you want to get classes from this json file contents then use this one: https://jsonutils.com/* '

In ST2 project, Businness layer is our Model of MVVM patten.!


### 1.2 VIEW MODEL 

-- in ST2, presentation layer are   VIEW + VIEW MODEL

-- the view model is inside the MainWindow.xaml.cs in the form of code behind for view or layout in xx.xaml file. 
-- define interaction between the view and the code behind in the form of events.

-- if our goal is to get rid of these methods inside the event handler in xmal.cs file then we need to find another way to handle or react to the event like buttonClick. 

--most imporatantly, all of the prediction and all of the calls and the communication between the WPF application and the service is going to be hanppen from the view model. what is service do in WPF? 

-- start to think of the VIEW MODEL is the way of communicating VIEW with MODEL. 

-- THE View MODEL is going to perfroming the entire functionality that is needed for us to get information shaped as the MODEL is intending to over to the VIEW so VIEW can start and display it .- using classes and methods that we define in MODEL. definition != Implementation. In MODEL, most of are definition of classes. 

we could eventually have all of that code(functionality of getting information from MODL to VIEW) in a separate project. projects could precisely be focused on having all of these functionality away from the VIEW. IN fact, having it in a different project will effectively allow is to reuse this code even in all kind of projects not exclusively for WPF. 

-- in VIEWMODEL, we sent API request to get information from web. 

### 1.3 VIEW 
in ST2, presentation layer are   VIEW + VIEW MODEL

VIEW shows what we are gonna to display to the user and for the codebehind if we have them we should get rid of them here.
-- for how to get rid of the code behind like eventHandler menthods inside the .xmal.cs, we could replace event handlers with command. by using command we could call methods we need directly from VIEW MODEL. So we achieve to implement view logic in VIEW MODEL, VIEW will use command to call them not from VIEW but from VIEW MODEL. 

-- but I do not think ST2 use command to replace event handler.???

Istead of call methos to return data in our view .xmal.cs file we could use context biding database into our layout. so each ilement in .xmal, like grid, table, LsitView they all have a dataContext, how does a context to be applied all the way to the window is that the window has also added a context. 

so this means if you add a context in the window block then this context will be the same all for children under this window. but you also could add context only to childern itself. then we could get information from the data context. so trhough the power of binding, we could get information from source or ata context directly in xaml instead of using code behind in .xaml.cs file- 


when you create a View folder we could put our xaml file here and design your user interface in xmal.file. after we have intercace added in window then we re going to start to take a look at these interface that are going to enable MVVM that is going to be designed to notify property changed. 
and then that interface is going to allow us to bind elements directly from the view model right here in xaml. 
so instead of in our code behind which was in .xmal.cs to get elements from elements of its .xaml file, and set value to it we are going to use binding to do it. 

### 1.4 Glue all parts in MVVM like model, view model and view TOGETHER 

so until now we have created adnd isolated contents in different folder. like classes and properties definitions in our Model which is businness layer in ST2, and classes with functionality inside the view model folder. views and windows inside our View model. 

now we need to implement the comminucation between these 3 parts. INotifyPropertyChanged interface is gonna to help us knowing when a property value are changed. we start with this one kind of communication among these 3 parts. 

why we need to use this interface? 
- precisely by implementing this interface is that we will be able to generate events for when these value changes so both the view and the models stay updated. by reacying the events when the value are changed will allow us to update the view, view model and model 
- but why we need this automaticaly update???
  
  * when bound together, two properties receive notification
    * The view statys updated with the values from the model(OneWay)
    * The model stays updated with the values from the view(TwoWay)
    * viewmodel updates model all way from changes done in view and in view model. these will update model.

how it work? 
  * The data model class implements these interface. datamodel class is MODEL class
  * changes to a property trigger an event
  * bound properties respond to the event

fpr example: we have a class inside our c# class in MODEL, like A User and it has 3 properties, Name, email and Password then we have a interface in our window like 3 empty text boxes that giving user to log in. 

at the beginning is empty, but we we set bound two ways of these two,  then either when user class changes inside c# or the interface are given some input from keyboard, one of another part will be updated through the bound. 

INnotifyPropertyChanged is quite critical in MVVM pattern to get clean code behind. 


* Implement InotifyPropertyChanged interface in Viewmodel

-- FOR one of every view we usaully have a corresponding viewmodel inside our viewmodel folder. for example for our WeatherWindow.xmal, we will have to create a new class inside of The View Model called WeatherVM.cs, VM stands for view model for containning the code behind the xmal. this applies to all view elements like ListView.xmal => ListViewVM.cs

    * Intercae of INotifyPropertyChanged is implemented in our VIEWMODEL for each xxVM.cs. the xxxVM will implemet the interface by using : in the class definition this we know. 
    like 
    public class WeatherVM : INotifyPropertyChanged
    {
      // property  why one private quey will follwed by a public Query?? these are automatically added. and get set methods are inside this Query why? 
      private string query;

      public string Query
      {
        get {return query;}

        set
        {
          // if this property is changed, it will be changed here from c# side. 
          query = value; 
          OnPropertyChanged("Query") // passing property name that is changing. this will trigger the event and now this event is now going to update anyone who subscribes to it. we could make our element from UI like text box to subscribe this event. then everytime textbox is changed then this Query will get updated or this Query is updated to query data with a new key then our textbox will be udapted from UI
          but here we just create event and add trigger methods. we need to bind xmal to this property and then property change will invoke event.
        }

      }


      public event PropertyChangedEventHandler PropertyChanged

      // normally as soon as you implement this intercace then we are going to creating a property since we need to have property than bound to our user interface. so when UI is changed then the property will be updated and vice versa. 

      //define a property here and also we need to trigger this event whenver thoese property changed.
      

      // trigger event method
      private void OnPropertyChanged(string propertyName)
      {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs()); ? will check if it exists or not, if it does then excute code after . 
      }
    }

* Binding data Context and design Time binding
  
  Here is the WeatherWindow,xaml => bound to properties in WeatherVM.cs, VM stands for view model
  <Window.Resources>
        <vm:WeatherVM x:Key="vm"/>
    </Window.Resources>
    
    <Grid DataContext="{StaticResource vm}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Margin="20">
            <TextBlock Text="Search for a city:"/>
            <TextBox Text="{Binding Query, Mode=TwoWay}"/>
            <Button Margin="0,10"
                    Content="Search"/>
            <ListView>
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <Grid>
                            <TextBlock/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackPanel>
        <Grid Grid.Row="1"
              Background="#4392f1"
              DataContext="{Binding CurrrentConditions}">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*"/>
                <ColumnDefinition Width="Auto"/>
            </Grid.ColumnDefinitions>
            <StackPanel Margin="0,10">
                <TextBlock DataContext="{StaticResource vm}"
                           Text="{Binding SelectedCity.LocalizedName}"
                           Foreground="#f4f4f8"
                           FontSize="20"
                           Margin="20,0"/>
                <TextBlock Text="{Binding WeatherText}"
                           Foreground="#f4f4f8"
                           FontSize="18"
                           Margin="20,0"/>
            </StackPanel>
            <TextBlock Grid.Column="1"
                       VerticalAlignment="Center"
                       Text="{Binding Temperature.Metric.Value, StringFormat={}{0}°C}"
                       Foreground="#f4f4f8"
                       FontSize="30"
                       Margin="20,0"/>
        </Grid>
    </Grid>
</Window>


### Service and Service request in WPF application 

3.  service and service request in WPF applications: https://www.codeproject.com/articles/38332/how-to-consume-a-web-service-in-a-wpf-application

4. Async Web Service Requests from WPF https://stackoverflow.com/questions/27542623/async-web-service-requests-from-wpf
5. why async one: https://stackoverflow.com/questions/26158789/why-should-i-create-async-webapi-operations-instead-of-sync-ones
6. perfrom async tasks in WPF: https://stackoverflow.com/questions/26158789/why-should-i-create-async-webapi-operations-instead-of-sync-ones
   
   **HOW AND WHEN i should use async and wait? 
7. https://stackoverflow.com/questions/26158789/why-should-i-create-async-webapi-operations-instead-of-sync-ones 
8. https://docs.microsoft.com/en-us/dotnet/framework/network-programming/making-asynchronous-requests


### what is async Task and methods, await async??

when and why we need use async task and methods? 
-- as I see from httpClient, we have response.Content.ReadAsStringAsync() is a task of string and GetAsync is a task of HTTP response message;
 so if we call these task methods without await, we receive the task itself . these methods are called inside the async Task class. 

 it is only we await that method that we atucally get the HTTP response message itself. '
 so if we call Async Task<List<City>> GetCities() itself, we will get the task itself . until we await these GetAsync() methods inside the task are called and returned then we will get List<City> as return. 

eaxample in course: using HttpClient() to get url response and read it out as json file.  then we could use json file to covert to be classes with properties inc# 

