﻿# CoDriven Advanced UI documentation

[Go back](index.md)

<!-- TOC -->

* [CoDriven Advanced UI documentation](#codriven-advanced-ui-documentation)
  * [Introduction](#introduction)
  * [How to use](#how-to-use)
  * [Other functionality](#other-functionality)
  * [Complete example](#complete-example)

<!-- TOC -->

## Introduction

CmScreenManager is the main container of screens. You can have multiple screen managers.

When you have multiple screen managers in one scene then you should show only one screen manager at once.

## How to use

> NOTE: Before you will read about screen managers please read about theming [here](theming.md).

To create your own CmScreenManager you will have to select if you will use theme or not.

> NOTE: If you won't use the theme then you can pass the default builtin **CmThemeDefault** empty theme interface like in
> example below:

```csharp
    // CmThemeDefault will be used as a base theme and this is the empty theme for filling purposes
    public class MyScreenManager : CmScreenManager<CmThemeDefault>
    {
        ...
        public override void OnAddSupportedScreens(List<CmScreen<CmThemeDefault>> supportedScreenList) {}
        public override void OnAddSupportedThemes(List<CmThemeDefault> supportedThemes) {}
        ...
    }
```

But in the theming section of the documentation we created a base interface for theme **MyExampleTheme** and two implementations of themes **MyThemeWhite** and **MyThemeBlack** so let's use it here:

```csharp
    // abstract base class MyExampleTheme for theming this window will be used as a base theme
    public class MyScreenManager : CmScreenManager<MyExampleTheme>
    {
        public override void OnBindUiEventsHandler(CmUIEventsHandler eventsHandler)
        {
            // handle global actions on UI like clicks and other
            eventsHandler.ButtonClick = button =>
            {
                // check globally if the user clicked the button, recognize the button by its name
                if (button.GetName().Equals("here_will_be_your_button_name_to_check_for"))
                {
                    //Here you will do some actions
                }
            };
        }

        public override void OnAddSupportedScreens(List<CmScreen<MyExampleTheme>> supportedScreens)
        {
            //Here you will add all your screens for this screen manager, and you will be able to change screen runtime
            supportedScreens.Add(new MyMainScreen(this));
        }

        public override void OnAddSupportedThemes(List<MyExampleTheme> theme)
        {
            //Add all your themes here for this screen manager
            themes.Add(new MyThemeWhite());
            themes.Add(new MyThemeBlack());
        }

        public override void OnSetMainScreen()
        {
            //You can set the main screen here at the start if you have multiple screens. by default main screen is the first Screen
            //All screens have their names as strings so you can create your screens as ENUM names and pass it here so there
            // will be no magic strings :)
            SetCurrentScreen("ScreenMain");
        }
    }
```

As you can see, your themes were added in **OnAddSupportedThemes** function. So the system will know how many styles will be used for theming and you can change the theme anytime runtime using:

```csharp
    SetCurrentTheme(new MyThemeBlack());
```

the **SetCurrentTheme** is available in your **CmScreenManager**, **CmScreen**, **CmScreenPart**

Definition:

```csharp
    public void SetCurrentTheme(THEME originalTheme, bool rebuildControlsWithNewTheme = false)
```

**rebuildControlsWithNewTheme** parameter is used to rebuild the whole screen when you change the theme

---

## Other functionality

**MyScreenManager** from the above example has also functions like:

- OnBindUiEventsHandler() - responsible for handling UI events emitted by controls created inside screens. Because these controls were created inside you will handle their events inside screens, but sometimes you will have to handle events globally.
  To reduce code duplication on multiple screens you could handle this event here. More about events
- see the ui_events section [here](ui_events.md) ,
- OnAddSupportedScreens() - here you will add all your screen instances for this screen manager.

You will be able to use **setCurrentScreen** to switch screen runtime.

```csharp
    public virtual void SetCurrentScreen(string currentScreenName, bool makeContent = true)
```

this function is available in **CmScreenManager**, **CmScreen**, **CmScreenPart**

When game starts the first screen what will be shown is set in:

**OnSetMainScreen**() - which is called by the system on start so you can set the main screen here.

> **NOTE**: If you won't set the screen here then the first screen from screens list in OnAddSupportedScreens() will be used

---

**CmScreenManager** has more functionality. Here are more methods overview:

- **MakeScreenContent**() - you will use it to rebuild the whole screen (if you want to refresh the screen)

---

- UIDocument **GetUIDocument**() - you can get the current bound UIDocument

---

- List\<string\> **GetScreensNames**() - will return names of all screens added to this CmScreenManager

---

- **HideMenu**() - will deactivate UI to hide CmScreenManager from the screen. So there will be no screens visible

---

- **ShowMenu(string currentScreenName)** - will activate UI to show CmScreenManager on screen.

When you show the screen the manager you will have to tell the system which Screen to show by passing screen name to **currentScreenName** parameter

---

- **SetUserData(Object data)** - while in the game you can pass your own data as an object to **data** parameter.

```csharp
   // in your monobehaviour - somewhere in your player game logic
   public void UpdateUI()
   {
       if (_screensManager == null)
       {
           _screensManager = CmScreenManagerHelper.FindScreenManager();
       }

       _screensManager.SetUserData(someUserData);
   }
```

This data change will trigger callback **OnUserDataChange** for your active screen (CmScreen and other components like CmScreenPart)

```csharp
public virtual void OnUserDataChange(Object data)
```

so you can update UI state there

```csharp
public class PlayerInGameScreen : CmScreenPart<MyExampleTheme>{
   ...
   public override void OnUserDataChange(Object data)
   {
       if (data is PlayerData playerData)
       {
           _userName.SetText(playerData.UserName);
           _healtBar.SetProgress(playerData.Health);
       }
   }
}
```

---

## Complete example

here is a complete example of CmScreenManager implementation from "One Theme scene example"

```csharp

    public class CmExample2ThemesScreenManager : CmScreenManager<CmExample2ThemesBase>
    {
        public override void OnBindUiEventsHandler(CmUIEventsHandler eventsHandler)
        {
            // handle global actions on UI like clicks and other
            eventsHandler.ButtonClick = @base => { };
        }

        public override void OnAddSupportedScreens(List<CmScreen<CmExample2ThemesBase>> supportedScreenList)
        {
            //Add all screens here for this screen manager
            supportedScreenList.Add(new CmSimpleThemeMainScreen(this));
        }

        public override void OnAddSupportedThemes(List<CmExample2ThemesBase> themeBases)
        {
            //Add all themes here for this screen manager
            themeBases.Add(new CmExample2ThemeOne());
            themeBases.Add(new CmExample2ThemeTwo());
        }

        public override void OnSetMainScreen()
        {
            //You can set the main screen here at the start if you have multiple screens. by default main screen is the first Screen
            // added to supportedScreenList in above OnAddSupportedScreens(..) function
            SetCurrentScreen(CmSimpleThemeMainScreen.MAIN_SCREEN_NAME);
        }
    }
```
