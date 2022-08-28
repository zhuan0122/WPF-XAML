
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
  *  create event handler for buttons both in xaml file and in .xmal.cs file. Code in .xmal.cs and .xmal are VIEW codes.



###  MVVM patterns -- the patterns nautural to XAML applications 

** Data binding is the key** 

** Structure of of the MVVM pattern 
    * Model <Sends notifications> to VIEWMODEL for any changes EVENTS did in MODEL > 
    * VIEWMODEL <Updates> to MODEL: UPDATE MODEL OR RETRIVE MODEL
    * VIEWMODEL <Sends notifications> To VIEW about changes from MODEL. VIEWMODEL is the intermediary between MODEL and VIEW(layout/xmal file)
    * VIEW <Data binding> TO VIEWMODEL  how??? how to bind data grid or any interface to the property in database? 
  
** MODEL 
-- it is about the data so it is data model 
-- contains business and validation logic(SERVICE LAYER + DATA LAYER) so it is where we implement our classes with properties and interface that are gonna to be used in the 
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

-- if our goal is to get rid of these methods inside the event handler in \xmal.cs file then we need to find another way to handle or react to the event like buttonClick. 

--most imporatantly, all of the prediction and all of the calls and the communication between the WPF application and the service is going to be hanppen from the view model. what is service do in WPF? 

-- start to think of the VIEW MODEL is the way of communicating VIEW with MODEL. 

-- THE View MODEL is going to perfroming the entire functionality that is needed for us to get information shaped as the MODEL is intending to over to the VIEW so VIEW can start and display it .- using classes and methods that we define in MODEL. definition != Implementation. In MODEL, most of are definition of classes. 

we could eventually have all of that code(functionality of getting information from MODL to VIEW) in a separate project. projects could precisely be focused on having all of these functionality away from the VIEW. IN fact, having it in a different project will effectively allow is to reuse this code even in all kind of projects not exclusively for WPF. 

-- in VIEWMODEL, we sent API request to get information from web. 
..................................................................................

### 1.3 VIEW 
in ST2, presentation layer are   VIEW + VIEW MODEL

VIEW shows what we are gonna to display to the user and for the codebehind if we have them we should get rid of them here.
-- for how to get rid of the code behind like eventHandler menthods inside the .xmal.cs, we could replace event handlers with command. by using command we could call methods we need directly from VIEW MODEL. So we achieve to implement view logic in VIEW MODEL, VIEW will use command to call them not from VIEW but from VIEW MODEL. 

-- but I do not think ST2 use command to replace event handler.??? // we have both 

Istead of call methos to return data in our view .xmal.cs file we could use context biding database into our layout. so each element in .xmal, like grid, table, LsitView they all have a dataContext, how does a context to be applied all the way to the window is that the window has also added a context. 

so this means if you add a context in the window block then this context will be the same all for children under this window. but you also could add context only to childern itself. then we could get information from the data context. so trhough the power of binding, we could get information from source or ata context directly in xaml instead of using code behind in .xaml.cs file- 


when you create a View folder we could put our xaml file here and design your user interface in xmal.file. after we have intercace added in window then we re going to start to take a look at these interface that are going to enable MVVM that is going to be designed to notify property changed. 
and then that interface is going to allow us to bind elements directly from the view model right here in xaml. 
so instead of in our code behind which was in .xmal.cs to get elements from elements of its .xaml file, and set value to it we are going to use binding to do it. 

### 1.4 Glue all parts in MVVM like model, view model and view TOGETHER 

so until now we have created adnd isolated contents in different folder. like classes and properties definitions in our Model which is businness layer in ST2, and classes with functionality inside the view model folder. views and windows inside our View model. 

now we need to implement the comminucation between these 3 parts. INotifyPropertyChanged interface is gonna to help us knowing when a property value are changed. we start with this one kind of communication among these 3 parts. 

why we need to use this interface? 
- precisely by implementing this interface is that we will be able to generate events for when these value changes so both the view and the models stay updated. by reacting the events when the value are changed will allow us to update the view, view model and model 
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
      // property  why one private quey will follwed by a public Query?? these are automatically added. and get set methods are inside this Query why? : check here : https://www.w3schools.com/cs/cs_properties.php
      // in summary we define string as this way to be able to avoid user to change this string. 
      <!-- Properties and Encapsulation
        Before we start to explain properties, you should have a basic understanding of "Encapsulation".

        The meaning of Encapsulation, is to make sure that **"sensitive" data is hidden from users.** To achieve this, you must:
        declare fields/variables as private
        provide public get and set methods, through properties, to access and update the value of a private field 
        A property is like a combination of a variable and a method, and it has two methods: a get and a set method: 
        -->

      private string query; -- this is variable definition or field definition
      
      // this is property definition. property has get/set methods inside 
      public string Query
      {
        get {return query;}

        set
        {
          // if this property is changed, it will be changed here from c# side. 
          query = value; 
          OnPropertyChanged("Query") //event 
          // passing property name that is changing. this will trigger the event and now this event is now going to update anyone who subscribes to it. we could make our element from UI like text box to subscribe this event. then everytime textbox is changed then this Query will get updated or this Query is updated to query data with a new key then our textbox will be udapted from UI
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
   * access property over the xaml or view: first add new reference or namespace that we will use or access 
     so all namespace xmlns stands for xmal namespace and we need these as our reference to be able to access items defined inside these namespace. in order to bound properies in xaml, we need to access these properties then we need to add viewmodel.cs our namespace. so we could access this folder VieModel and access files under this folder. 


  * when we bind Query to search textbox in twoway, then when user enter in search textbox, the Query inside view model will be updated then if we bind Query to another textbox like Display query inside UI(we just bind Query inside this textbox not intwo way since we won't use this textbox to update viewmodel, so one way is correct) then this display textbox will be updated as well through the first search textbox > Query in viewmode > textbox display. all along the way.  once Query is changed it will go to set method and invoke the event and then update items where use this property. 'Use' here means bind in xaml. 
   
   This is two way binding between search textbox and Query so if we change Query inside viewmodel then this search textbox will be updated as well.

   we shoud design our bindings and UI and properties to make the UI works more resonanle.

    we are not using query in textblock we usually use query in button pressed or command .so when user click button or command, it will upodate query and query will query data from web or data server for example and then display query result in one textbox. -- this is the logic pratical way we use query. 

  * in our current pratice, it is more useful to bind Temperature and Currentcondition properties in xmal or UI.  so we have one CurrentCondition.cs file in Model folder where we define classes and property. we want to bind this property in xmal then we need to add these property in our view model folder of weatherVM.cs file. 
  with get and set. we do this way since we update property in viewmodel not in MODEL. so we have properties added in an ecapsulation way in ViewMODEL. This is not dupilcates to define Property. BUt it is a safe way and follow the MVVM pattern to implement codes.



  x:Class="WeatherApp.View.WeatherWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WeatherApp.View"  -- access View folder
        xmlns:vm="clr-namespace:WeatherApp.ViewModel"  -- access View MOdel folder 
        mc:Ignorable="d"
        Title="WeatherWindow" Height="600" Width="400">
  <Window.Resources>
        <vm:WeatherVM x:Key="vm"/>    --create an instance of reference. so like access objects instead of class
    </Window.Resources>
    
    <Grid DataContext="{StaticResource vm}">   -- all childs inside this grid have vm as its context. 
        <Grid.RowDefinitions>               -- so its childs will be able to bind properties in the context(vm)
            <RowDefinition Height="*"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Margin="20">
            <TextBlock Text="Search for a city:"/>
            <TextBox Text="{Binding Query, Mode=TwoWay}"/>  --this user input box is binding to property Query
            <Button Margin="0,10"        --textbox is entered by user so we need to bind twoWay 
                    Content="Search"/>    -- to be able to update view model when view changes from UI
            <ListView>                   -- so if we only "biding Query" -- only updates view from viewmodel
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
              DataContext="{Binding CurrrentConditions}"> --this new grid has the same context vm from its 
            <Grid.ColumnDefinitions>            --parent Grid. so it is possible to bind to any property from vm
                <ColumnDefinition Width="*"/>   -- but this currentConditions is a class!!! so in Model, 
                <ColumnDefinition Width="Auto"/>  -- convert object from json file as classes. 
            </Grid.ColumnDefinitions>
            <StackPanel Margin="0,10">
                <TextBlock DataContext="{StaticResource vm}"  -- this textbox is for displaying City name 
                           Text="{Binding SelectedCity.LocalizedName}"  -- so it has its own dataContext not 
                           Foreground="#f4f4f8"               --just inherites from parent grid like other box
                           FontSize="20"
                           Margin="20,0"/>
                <TextBlock Text="{Binding WeatherText}" -- this child block inherites class CurrrentConditions 
                           Foreground="#f4f4f8"         -- from its parent grid so it is able to access
                           FontSize="18"                --properties inside the class 
                           Margin="20,0"/>
            </StackPanel>
            <TextBlock Grid.Column="1"
                       VerticalAlignment="Center"
                       Text="{Binding Temperature.Metric.Value, StringFormat={}{0}°C}" --{} is empty and only
                       Foreground="#f4f4f8"             --requiered if we need any text before value, {0} points
                       FontSize="30"                    -- to the Temperature.Metric.Value itself then append 
                       Margin="20,0"/>                -- unit celsisus but why here we need to call value_
        </Grid>
    </Grid>
</Window>


* Using design mode binding
  -- what is this and why we need this? so I want to initialize some values in our binding property and build the application and see if UI shows what I want to initialize but when we run the application by clicking start. we do not want to see these initialized value on UI. so what we can do is to set the design mode in our constructor. so only ir is in the design mode then it will run these initialize 
  which is adding a if statement with DesignProperties(coming from System built-in).GetInDesignMode(new System.Windows.DependencyObjects()) -- this will return true if we don not start application and just write code and build. 

we have these bindings and they are coming from view model. if you want to initialize some property in viewmodel and show in UI, we could do these initialiation in the viewmodel calss constructor. so these value will be assigned once the application is built. 

### 1.3 ICommand interface**
// REPLACING Event HANDLER
-- provides the functionality that gives the ability to load data. we ususally do it with Button but we are trying to get rid of all of the code behind in view. the event handlers we have exist in the code behind in xxx.xmal.cs or we say in view. so do we have code in view file for event handlers???? 

* why we need to use ICommand interface?
    * Cleans up view -- we could move these code behind in view to our view model 
    * can be reused between windows

    * Exist as property for Buttons 
      * Perfect replacement for event handelrs-- then we do not need the click action so we could reduce event handler for click. with command, we call property directly 

* how it works?
  * The Viewmodel class will implements the ICommand interface
  * The functionality is added to the execute member 
  * Optional CanExecute evaluations can be perfromed -- so the button could appear washed out or disabled when this CanExcute is false as return
  * The command is assigned -- assignable 
   -- The view model or the context of the View that is going to be pointing to our VIEW MODEL that model has to have a property of type of the class 

* Example 

    in UI we have a login button in login page, it is bound to the ICommand. right now the username and password are empty so the CanExcute is returned as false. CanExcute?=false 

    if we do our bindings crrectly then once user writes soemthing down in the login text boxes then the CanExcute will return as True. for doing this, we need to bind the username and password text boxes to certian object like property we have in ViewModel and pass the objects to the CanExcute() method so the method could evaluate whether to return true or false.

  
* Implement ICommand in ViewMoel
  task:  bound a Command property to our view. whenver the button is pressed that button is actually reacting to what is inside the MakeQury method inside ViewModel. Make sure the MakeQuery method is only excuted only when the query actually has some text in the textbox in UI. if it is emtpty or space in textbox then the 'Serach' button is greayed out- 
  

### ObervableCollection<T> class 
- A list aware of changes or a List<T> , it is a generic type of T in a list. T defines what in the collection or in the OberableColletion is gonna to be.
- OberableCollection is actually going to be able to update the view and collection cells we have insertions and deletions.

* why to use it? 
    * will be able to make these bindings silimar to waht we are doing with the INotifyPropertyChanged interface to a collection in c# for exaplme a ListView which is a collection
    * Updates with insertions and deletions
    * working with not a single property that we do before using ICommand and EventHandler but also with a list of objects. a list of changes and so on

* how it works? 
  * we will create a class inheriting from the ObservableCollection<T> in our ViewModel. This ObserableCollection<T> is already implemented in INotifyCollectionChanged interface 
  * INotifyCollectionChanged interface is smilar as INotifyPropertyChanged interface. but this interface is working with collections.
  * Binding source is going to be established which is also the binding context we call it source 
  * The changes are going to updating UI 
  * example: in UI there is a list with 3 elements and they are bound to the ListView in c#. when we eliminate element 2 and add element 4 then it will be updatd to UI automatically. 

* useful: return a list of objects to the user and update this list in pratical use-


* Implement Obserable Collection class 
  
  task: we are gonna visual a list of city when a query is excuted for GetCities inside MakeQuery method in VIEWMODEL.

  -- we cannot use the list of ciities itself to bind the search button since it is not obserable so we wouldn't know when your cities are added or when cities are removed.

  stpe1: create a property of type of a Collection of cties
      * notify the view whennever it is changed 

        public ObservableCollection<City> Cities {get; set}; 

  step2: add items to our variable defined in step1 which is a list as return
  the item we are gonna add is what we get from query asking from API. so in the MakeQueryMethod()

  public async void MakeQuery
  {
    var cities = await AccuWeatherHelper.GetCities(Query);

  }
-- concerns of initializeing the obseravleCollection, we only can initialize this once. Note: if the binding is set to an object like the class MakeQuery() instance. if this object is initizaled again then it is basicallty a whole new object. so the binding will be lost.

so if you bind Observable Collection to something in the view and your observable collection has to be updated only once othweirse the binding to this observable collection is lost. !!! so this is why we initialize the ObserbleCollection list we defined in step2 in our View model constructor where will be only run once when it is start over application:

In WeatherVM.cs :

public WeatherVM()
{
  -- this is a constructor

  Cities = new ObservableCollection<City>(); // initialize it as a empty list here since we do not have cities in our consturctor itself
}


step3: add cities into our Collection every time we did query from web
public async void MakeQuery
  {
    var cities = await AccuWeatherHelper.GetCities(Query); 
    Cities.Clear(); --clear the previous query result. Clear is builtin method of a ObservableCollection

    Cities.AddRange(cities); -- or using foeach method to add one by one into Cities, this is populating the collection with data
  }


step4: bind ObersbaleCollection to the ListView of UI
-- remeber once we have a list cities showed in the listView, we will be able to bind something over to the SelectedCity property which in turn will trigger the other method called GetCurrentConditions inside the Helper class to get current coditions for selected city. 

-- so recap: you list cities in the ListView block in UI and then you will click one city from the list ones which is bind to SelectedCity property. then it will will trigger GetCurrentCondition method. but how to trigger this method? ????? 

 inside the ListView block in xaml:
 <ListView ItemSource="{Bidning Cities}"> -- the list will be aware of what items would be display
      <DataTemplate>
          <Grid>
              <TextBlock/ Text ="Binding LocalizedName ">  -- TextBlock is part of the datatemplate and does not know how to display thoese cities. 
          </Grid>           -- each city is a datacontext of datatemplate of his grid aand of this textblock. bind it to property of a city.
      </DataTemplate>
 </ListView>

step5 : able to selecet city from the cities listview 
after your select one city in UI, it will be able to clear the ObserableCollection, clear the query for fetching these cities and make the request over to query CurrentConditions for selected city and displayed it in the textbox below.

inside the ListView block in xaml:
 <ListView ItemSource="{Bidning Cities}" -- the list will be aware of what items would be display
           SelectedValue="{Bidning SelectedCity}"> -- now everysignle time the selected item changes, it will run the set part inside the SelectedCity 
      <DataTemplate>
          <Grid>
              <TextBlock/ Text ="Binding LocalizedName ">  -- TextBlock is part of the datatemplate and does not know how to display thoese cities. 
          </Grid>           -- each city is a datacontext of datatemplate of his grid aand of this textblock. bind it to property of a city.
                            -- LocalizedName here is not a property of our VM (ViewModel class like SelectedCity) it is a property of our VM property:
                            --SelectedCity in our view model class. we could do this binding here since we have bound Cities in our upplayer: ListView. 
      </DataTemplate>
 </ListView>

 private City selectedCity;

public City SelectedCity
{
    get { return selectedCity; }
    set
    {
        selectedCity = value;  -- breakpoint here after you selected one item in UI from list view. then value has key and City itself and 
                                -- other info. but we need the key to get CurrentCoditions for this city. HOW ?
        OnPropertyChanged("SelectedCity");
        GetCurrentConditions() -- once we selected city it will come to this set part. so we would create this GetCurrentConditions method
    }
}

private async GetCurrentConsitions()
{
  Query = string.Empty; -- clear current query since we have get cities and select one of them 
  Cities.Clear() -- clear the quried ObservableColelction 
  CurrentConditions = AccuWeattherHelper.GetCurrentConditions(SelectedCity.Key); -- we need key to fetch currentconditions from api web
}

run -- get error, it says the value in Temperature.Metric.Value is a string


 step6> show the bool property in UI using IValueConverter interface in MVVM arhictetcure. 
        this part uses the IValueConverter interface that we explain below


### IValueConverter 
-- adapt model to the view 

* why we use it? 
    -- change the model to what the view needs.
    -- help to change view inputs into data model 
    -- we will explain these in detail in our example that we are gonna use it in our weather application and 
        we will see why we need it. 
    -- so it is could help us to change inputs into te model from view or change teh output from the model to view. this will be important when we want to reuse code inside the VIEWMODEL o different views.

    -- the model could get some information and we need to use the information in some format for other views

* How it works? 
    * similar, it is a class impelmented inside our view model like other interface we have leanred above like 
      OberservableCollection<T>, INotifyPropertyChanged, and so on. But when we say it is inside our viewmodel. 
      we mean it is under this ViewModel folder and we could access this class whereever inside this folder.

    * we are gonna have two methods inside: 
      * Convert() -- allow us to cast Model to View 
      * ConvertBack() -- allow us to cast View to Model
  
    * small example to explain: 
      -- we have a class in the model, it has one property called DateTime which is in datetime format. but we do not want to display this complicated redunant format to the   user. we want to let the user see just two hours ago. In we could evaluate this datatime in the Convert method. for example if the date is less than one day old, we are going to return our time as hours.if it is more than one day old , then we say yesterday

* Implemeent IValueConvert interface 
  
  we have one property in the CurrentConditions class: HasPercipitation 
  which is a bool. so we want to visulize this in UI as ... 
  we just add another textbox inside the grid where we show Weather in UI. well we do not want to simply just show True or false to user in UI. so we could reformat this to be more descriptive or presentable in UI.

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
                <TextBlock Text="{Binding HasPrecipitation, Converter={StaticResource boolToRain}}"
                           Foreground="#f4f4f8"
                           FontSize="16"
                           Margin="20,0"/>
            </StackPanel>
            <TextBlock Grid.Column="1"
                       VerticalAlignment="Center"
                       Text="{Binding Temperature.Metric.Value, StringFormat={}{0}°C}"
                       Foreground="#f4f4f8"
                       FontSize="30"
                       Margin="20,0"/>
        </Grid>

we create a folder called ValueConverter folder inside the ViewModel folder. Then implement code: 

public class BoolToRainConverter : IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            bool isRaining = (bool)value; -- cast it to boolean type 

            if (isRaining)
                return "Currently raining"; 

            return "Currently not raining";
        }

        public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        {
            string isRaining = (string)value;
            if (isRaining != null)
            {
                if (isRaining == "Currently raining")
                    return true;
            }
            return false;
        }
    }

-- in covert method, we could evaluate lots of things. lIKE Culture is useful for currecny for specific formatting. but in our case, we do not need it at this moment. 
so we take object that we get and display it to user into whatwever we want to show.

-- in our case User will never be able to set from UI that is raining or not raining. But in other case. we could do ConvertBack like changes from view to the Model.

-- we need to bind this BoolToRainConverter to our view in xaml file: 
  -- since we create a folder to contain this ConverValue class so we need to add it as namespace in xaml. since it is not in WeatherVM.cs. so it is inside another folder, we cannot access it without adding its path as namepsace in xlms. then added it in Resources block as well

  <Window.Resources>
        <vm:WeatherVM x:Key="vm"/>
        <converters:BoolToRainConverter x:Key="boolToRain"/>
    </Window.Resources> 

  -- then we could bind to boolToRain's property. 

  <TextBlock Text="{Binding HasPrecipitation, Converter={StaticResource boolToRain}}" -- The Converter will grab what we get from HasPrecipitation and evaluate it there- 


### Custom User Control 
 
 we know from window, we could add textbox, button, listview, and so on, these controls are from window in xmal. these controls if we want to use them or display them in different UI location then we need to to add this control code over and over again in xmal. so for more efficiently we could use custom user control to do reduce our duplicate codes. 

 so in this part we are gonna learn how to build our own custom control just like Textbox, List view and any layout and then we could resue the format whenerver or wherever we need it. we are gonna use these custom user control the same way we use button, textbox before.

 VS provides a User Control template which you could add like a class by right clikcing.  so we name it as classNameControl.xmal then it will create on .xmal.cs automatically along with the .xmal file. this classNameConrol.xmal inherites from UserControl class so it will use heights, width and any common properties from UserCOntrol template.

 if you want to set the textblock in your user control layout to what contents you want. then you need to set name to these blocks and then you could assign values to them in .xmal.cs file. 

 but why you mention we should recude code behind in .xmal.cs file so we remove them to .cs model files and define our property there and then bind them in xaml file. 
 here in our custom user control, we define layout in xmal file and then add property in its .xaml.cs file with assignment value to Name of blocks in xmal file. then what is the purpose of this ussage? should we just bind property inside model class? 
 Answer: **For a UC to work with Binding you need to add the Dependency Properties with C#, so I guess the answer is no. But is this what you were referring to?**

 regardless of Dependency properties here we just want to use user control. so in our main window xmal, we do not want to binding properties from contact class over and over again. so we replace these blocks for displaying name of Contact, email of contact... with uc directly:

 Import namesapce xmlns:uc="clr-namespace:DesktopContactsApp.Controls" 
 <ListView.ItemTemplate>
                <DataTemplate>
                    <uc:ContactControl Contact="{Binding}"/> -- still can bound property from itemSource. but when you run build, app is 
                                                              -- working and throw error on binding here cannot be on a  property. it should be set on a dependency propertity or Dependency Object. so this means in this way when we use User Control as item source and then we have a simple property inside the User COntrol class here it is ContactControl: UserControl(inherites from User Control) then this does not work. so for UC in xmal. we need to bind not on simple property from itemsource but a Dependency properties or dependency object. see next step 
                </DataTemplate>
            </ListView.ItemTemplate>


####  Create Dependency Property and bind on them 
  * Transform the Contact property inside ContactControl to be a Dependency Property.
  
      public partial class ContactControl : UserControl
    {
        private Contact contact;

        public Contact Contact -- change this property to be a Dependency Property
        {
            get { return contact; }
            set
            {
                contact = value;
                nameTextBlock.Text = contact.Name;
                phoneTextBlock.Text = contact.Phone;
                emailTextBlock.Text = contact.Email;
            }
        }

when you type property in side class we have snippet: prop, propf, propdp + tab then it will show entire template for this property

namespace DesktopContactsApp.Controls
{
    /// <summary>
    /// Interaction logic for ContactControl.xaml
    /// </summary>
    public partial class ContactControl : UserControl
    {
        public Contact Contact
        {
            get { return (Contact)GetValue(ContactProperty); } -- use GetValue method from Contactproperty then cast it as Contact type.
            set { SetValue(ContactProperty, value); }
        }

        // Using a DependencyProperty as the backing store for Contact.  This enables animation, styling, binding, etc...
        // here we define ContactProperty as a readonly and static property which is registed as type of ITS oWNERclass which
        // is ContactControl in this case. as it mentioned this will enabling animation, binding which we need now 
        // through the propertyMetadata we will be able to define a method that will be excuted when the property changes. // 
        //PropertyMedatda has 3 arguments, first one is defualt, the second one is the value to initialize or defualt one before the property get changed, this initial value should be set as correct format here is Contact, otherwise, errror will be thrown.so we set it as a new Contact object with New keyword. the third is the callback 
        //method that will be excuted everytime when property changes which behaves like a event handler. 
        public static readonly DependencyProperty ContactProperty =
            DependencyProperty.Register("Contact", typeof(Contact), typeof(ContactControl), new PropertyMetadata(new Contact() { Name = "Name Lastname", Email = "example@domain.com", Phone = "123 1234 1234" }, SetText));
        


        private static void SetText(DependencyObject d, DependencyPropertyChangedEventArgs e)
        {
            ContactControl control = d as ContactControl; -- variable control here has access to all Textblock in ContactControl.xaml 
                                                          --file. since when we create ContactControl.xmal, it is partial class and all contents inside are created in different class and different files automatically.

            if(control != null)
            {
                control.nameTextBlock.Text = (e.NewValue as Contact).Name; -- NewValue is from our EventArgs we have oldValue as well
                                                                           -- we should cast it AS Contact type so we could have access to properties of Contact. 
                control.emailTextBlock.Text = (e.NewValue as Contact).Email;
                control.phoneTextBlock.Text = (e.NewValue as Contact).Phone;
            }
        }

        public ContactControl()
        {
            InitializeComponent();
        }
    }
}

### Dependency Injection --UnityContainer/prism framework




### Service and Service request in WPF application 

3.  service and service request in WPF applications: https://www.codeproject.com/articles/38332/how-to-consume-a-web-service-in-a-wpf-application


### what is async Task and methods, await async??

when and why we need use async task and methods? 
-- as I see from httpClient, we have response.Content.ReadAsStringAsync() is a task of string and GetAsync is a task of HTTP response message;
 so if we call these task methods without await, we receive the task itself . these methods are called inside the async Task class. 

 it is only we await that method that we atucally get the HTTP response message itself. '
 so if we call Async Task<List<City>> GetCities() itself, we will get the task itself . until we await these GetAsync() methods inside the task are called and returned then we will get List<City> as return. 

eaxample in course: using HttpClient() to get url response and read it out as json file.  then we could use json file to covert to be classes with properties inc# 


4. Async Web Service Requests from WPF https://stackoverflow.com/questions/27542623/async-web-service-requests-from-wpf
5. why async one: https://stackoverflow.com/questions/26158789/why-should-i-create-async-webapi-operations-instead-of-sync-ones
6. perfrom async tasks in WPF: https://stackoverflow.com/questions/26158789/why-should-i-create-async-webapi-operations-instead-of-sync-ones
   
   **HOW AND WHEN i should use async and wait? 
7. https://stackoverflow.com/questions/26158789/why-should-i-create-async-webapi-operations-instead-of-sync-ones 
8. https://docs.microsoft.com/en-us/dotnet/framework/network-programming/making-asynchronous-requests


**Constructors will be run once the class is created** so