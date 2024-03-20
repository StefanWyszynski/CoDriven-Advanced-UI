# UI Compose documentation

:arrow_backward: [Go back](README.md).

# Themes

<!-- TOC -->

* [UI Compose documentation](#ui-compose-documentation)
* [Themes](#themes)
	* [Introduction](#introduction)
	* [How to use](#how-to-use)

<!-- TOC -->

![themes image](images/features/themes.gif)

### Introduction

Each game should have at least one theme so theming is optional, but if you won't use themes then you will have to use
the default theme built in. This theme will not change your style. It is the empty theme for filling space purposes. It is
strongly recommended to use at least one theme to make sure that your screen is not responsible for styling and this is
delegated to themes. This will improve code readability and project maintenance.

### How to use

To use themes you have to create a base interface that extends **ICMThemeBase** interface like this:

```csharp
public interface IMyExampleTheme : ICMThemeBase
{
    public void StyleButton(CmButton button);
}
```

**IMyExampleTheme** interface will be passed as a generic argument to:

- screen manager: CmScreenManager\<**IMyExampleTheme**\> more about [here](screen_manager.md).
- screens: CmScreen\<**IMyExampleTheme**\>  [here](screen.md).
- **(optionally)** screen part: CmScreenPart\<**IMyExampleTheme**\> [here](screen_part.md).

The next thing to do is to create your implementation for our theme

We will style the button with modifiers - please read the modifiers section [here](modifiers.md).

```csharp
public interface MyThemeWhite : IMyExampleTheme
{
    public void StyleButton(CmButton button)
    {
        // style with modifiers
        button.Modifiers.Button(
                new CmModifierText(CmSelector.DEFAULT_STATE)
                    .BackgroundColorRGBA(Color.white)
                    .TextFontSize(30.px())
            );
        }
}
```

optionally you can create a second theme like this (as an example)

```csharp
public interface MyThemeBlack : IMyExampleTheme
{
    public void StyleButton(CmButton button)
    {
        button.Modifiers.Button(
                new CmModifierText(CmSelector.DEFAULT_STATE)
                    .BackgroundColorRGBA(Color.black)
            );
    }
}
```

As you can see we have two themes **MyThemeWhite** and **MyThemeBlack**. These themes you will have to add to the screen
manager:

```csharp
    // IMyExampleTheme is a base theme interface
    public class MyScreenManager : CmScreenManager<IMyExampleTheme>
    {
        ...

        //This is called by the system to add themes to the list
        public override void OnAddSupportedThemes(List<IMyExampleTheme> themes)
        {
            //Add all themes here for this screen manager
            themes.Add(new MyThemeWhite());
            themes.Add(new MyThemeBlack());
        }
        ...
    }
```

and now you can use themes in your screens. Or change themes. More about screen manager [here](screen_manager.md).

You will use the theme in part of your code responsible for creating controls:

```csharp
    public CmButton createMainButton(string buttonTitle)
    {
        //Create a simple button with buttonTitle and UI event handler from this screen to handle events in this screen
        var button = new CmButton(buttonTitle, cmUIEventsHandler: GetCmUiEventsHandler());

        //Get the current assigned theme to this screen and use your function to style this button
        //This theme could be one of your added themes: MyThemeWhite, MyThemeBlack
        GetTheme().StyleButton(button);
        return button;
    }
```

Now you know how to add a theme and use it.
