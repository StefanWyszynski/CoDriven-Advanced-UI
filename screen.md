# CoDriven Advanced UI documentation

[Go back](index.md)

# CmScreen

<!-- TOC -->

* [CoDriven Advanced UI documentation](#codriven-advanced-ui-documentation)
* [CmScreen](#cmscreen)
  * [Introduction](#introduction)
  * [How to use](#how-to-use)
  * [GetScreenName()](#getscreenname)
  * [bindUIEventsOnStart()](#binduieventsonstart)
  * [OnMakeScreenContent(List<CmControlBase> contentList) the most important section](#onmakescreencontentlistcmcontrolbase-contentlist-the-most-important-section)
  * [Complete example of CmScreen implementation from "One Theme scene example"](#complete-example-of-cmscreen-implementation-from-one-theme-scene-example)

<!-- TOC -->

### Introduction

CmScreen is a screen container for controls on your game screen. You can have multiple screens but you show only one screen at once. For example map select screen, main menu screen or in game lobby screen

### How to use

> NOTE: Before you will read about CmScreen please read about theming [here in theming section](theming.md) and CmScreenManager which is a
> container for CmScreen [here in screen manager section](screen_manager.md).

To create your own CmScreen you will have to select if you will use a theme or not.

> NOTE: If you won't use the theme then you can pass the default builtin **CmThemeDefault** empty theme interface like in
> example below:

```csharp
    // CmThemeDefault will be used as a base theme and this is the empty theme for filling purposes
    public class MyMainScreen : CmScreen<CmThemeDefault>
    {
        ...
        // needed constructor 
        public MyMainScreen(CmScreenManager<CmThemeDefault> parent = null, VisualElement root = null) 
            : base(parent, root) {}
    
        ...
    }
```

But in the theming section of the documentation we created a base class for theme **MyExampleTheme** and two implementations of themes **MyThemeWhite** and **MyThemeBlack** that inherit the base class. Now we can create screen using our theme as base, so that we can use it for theming:

```csharp
    // CmThemeDefault will be used as a base theme and this is the empty theme for filling purposes
    public class MyMainScreen : CmScreen<MyExampleTheme>
    {
        public static string MYMAIN_SCREEN_NAME = "MyMainScreen";

        // needed empty constructor 
        public MyMainScreen(CmScreenManager<MyExampleTheme> parent = null, VisualElement root = null) 
            : base(parent, root) {}
    
        public override void bindUIEventsOnStart(CmUIEventsHandler eventsHandler)
        {
            // handle events here
        }

        public override void OnMakeScreenContent(List<CmControlBase> contentList)
        {
            //create controls here and add them to contentList
            contentList.Add(...create content here...);
        }

        public override string GetScreenName()
        {
            // return screen name. Screen name is useful for further screen changing in game/menu
            return MYMAIN_SCREEN_NAME;
        }
    }
```

As you can see adding a screen is nothing difficult.

I will tell you what are the reason for this methods.

### GetScreenName()

We need to override **GetScreenName**() to return the screen name string (you can use an enum with the names of your screens and
cast enum to string here so you wouldn't use magic strings)

Screen name is used to move to screen by its name so it is good practice to use constants or static to hold that name.

---

### bindUIEventsOnStart()

If you read the section about ScreenManager which I will suggest doing first then you will notice method **bindUIEventsOnStart**(). On CmScreenManager this method was for handling global UI events but here we handle events
more local because it will be connected with this screen.

This way we can handle events here and additionaly in CmScreenManager if you want. More about events see the ui_events section [here in ui_events section](ui_events.md)

---

### OnMakeScreenContent(List<CmControlBase> contentList) the most important section

content of the screen needs to be filled with controls. **OnMakeScreenContent**() function is just for that purpose.
Below will be added a full working example from example projects that will show you how to create a simple screen with
a button

You can create any control extended from CmControl in OnMakeScreenContent and add it to contentList. For example CmButton, CmToggle, CmScrollView, CmColumn, CmRow ect.

You can also split your game menu into sections and each section could be **CmScreenPart** which is a part of the main menu see [here in screen_part section](screen_part.md).

You can use this screenPart inside multiple screens. For example to create a simple static header which is the same on a few screens.

So you will fulfill the known programming pattern "don't repeat yourself (DRY)"

---

## Complete example of CmScreen implementation from "One Theme scene example"

```csharp
    /// <summary>
    /// This is a simple main screen that extends CmScreen but you have to tell us that you will be using CmExample1ThemeBase
    /// as the base theme for this screen, so we will know that all themes extended from CmExample1ThemeBase will be compatible
    /// for styling controls
    /// </summary>
    public class CmExample1MainScreen : CmScreen<CmExample1ThemeBase>
    {
        public static string MAIN_SCREEN_NAME = "Example1MainScreen";

        /// <summary>
        /// button names (could be enums but finally should be converted to a string to set the name of control)
        /// </summary>
        private const string MainButtonName = "MyButtonName";

        public CmExample1MainScreen(CmScreenManager<CmExample1ThemeBase> parent = null, VisualElement root = null) :
            base(parent, root)
        {
        }

        public override void bindUIEventsOnStart(CmUIEventsHandler eventsHandler)
        {
            // handle clicks for all buttons on this screen
            eventsHandler.ButtonClick = button =>
            {
                //Check if the user clicked button with name "MyButtonName"
                if (button.GetName().Equals(MainButtonName))
                {
                    // handle on click for the main button
                }
            };
        }

        public override void OnMakeScreenContent(List<CmControlBase> contentList)
        {
            contentList.Add(createMainPanel());
        }

        private CmColumn createMainPanel()
        {
            //Add column container and set vertical and horizontal position for all controls that will be added to it to the center
            //They will be on the screen center
            //Set background color for this column and make it fullscreen by setting Weight(1) - see modifiers section for more details
            var column = new CmColumn();
            column.Modifiers.Column(new CmModifierColumn(CmSelector.DEFAULT_STATE)
                .VerticalArrangement(CmArrangement.CENTER)
                .HorizontalAlignment(CmAlignment.CENTER)
                .BackgroundColorRGBA(new Color(0,0,1,0.5f))
                .Weight(1)
            );

            //Create button with text and name (name will be used in the event handler to recognize events from this control)
            var button = createMainButton("My main button (hello world button :)", MainButtonName);

            //Add button to column
            column.AddContent(button);

            return column;
        }

        public CmButton createMainButton(string text, string name)
        {
            //Create a simple button with text and UI event handler from this screen to handle events in this screen
            var button = new CmButton(text, cmUIEventsHandler: GetCmUiEventsHandler());

            //Set button name for further event handling
            button.SetName(name);

            // Get the theme assigned to this screen and use its function to style this button.
            // We will not call theme method but pass method address to StyleButton function
            button.SetStyle(GetTheme().StyleButton);
            return button;
        }

        public override string GetScreenName()
        {
            // return the screen name for this screen. Screen name is useful for further screen changing in-game/menu
            return MAIN_SCREEN_NAME;
        }
    }
```

For more information about this example. See examples for project. Just read the code and you will know how to use this

