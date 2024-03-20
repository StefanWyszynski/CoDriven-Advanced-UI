# UI Compose documentation

:arrow_backward: [Go back](../visual_controls.md).

# CmRow

CmRow is a container. Is is based on VisualElement. You can add controls to it by using:

```csharp
	CmButton someButton = new CmButton();
    //
	CmRow row = new CmRow();
	row.AddContent(someButton);
```

This is similar to CmColumn [read here](cmcolumn.md)

All controls added to CmRow will be hold in column way. To change how added controls will be positioned you can use
modifier:

```csharp
	CmRow row = new CmRow();
    row.Modifiers.Container(new CmModifierRow(CmSelector.DEFAULT_STATE)
                .HorizontalArrangement(CmArrangement.START)
                .VerticalAlignment(CmAlignment.CENTER)
            );
```

- HorizontalArrangement allow you to set how added controls to this container will be positioned horizontally. Available
  values: START, CENTER, END, SPACE_BETWEEN, SPACE_AROUND.
- VerticalAlignment allow you to set how added controls to this container will be positioned vetrically. Available
  values: START, CENTER, END, STRETCH, TEXT_BASELINE
  STRETCH will stretch control to parent size vertically.

# Style CmRow elements

CmRow modifiers contains only one element to style - the "Container"

```csharp

        public CmRow createRow()
        {
            CmRow row = new CmRow();
            row.Modifiers.Container(new CmModifierRow(CmSelector.DEFAULT_STATE)
                .HorizontalArrangement(CmArrangement.CENTER)
                .VerticalAlignment(CmAlignment.STRETCH)
            );

            return container;
        }
```

to learn about modifiers for all controls [click here](../modifiers.md)