# CoDriven Advanced UI documentation

[Go back](../visual_controls.md)

# CmColumn

CmColumn a container. Is is based on VisualElement. You can add controls to it by using:

```csharp
	CmButton someButton = new CmButton();
	CmColumn column = new CmColumn();
	column.AddContent(someButton);
```

All controls added to CmColumn will be hold in column way. To change how added controls will be positioned you can use
modifier:

```csharp
	CmColumn column = new CmColumn();
    
```

- HorizontalArrangement allow you to set how added controls to this container will be positioned horizontally. Available
  values: START, CENTER, END, SPACE_BETWEEN, SPACE_AROUND.
- VerticalAlignment allow you to set how added controls to this container will be positioned vetrically. Available
  values: START, CENTER, END, STRETCH, TEXT_BASELINE
  STRETCH will stretch control to parent size vertically.

# Style CmColumn elements

CmColumn modifiers contains only one element to style - the "Container"

```csharp

        public CmColumn createColumn()
        {
            CmColumn column = new CmColumn();
            column.Modifiers.Column(new CmModifierColumn(CmSelector.DEFAULT_STATE)
                .HorizontalAlignment(CmAlignment.START)
                .VerticalArrangement(CmArrangement.SPACE_BETWEEN)
                .FillMaxWidth()
                .FillMaxHeight()
            );
            return container;
        }
```

to learn about modifiers for all controls [click here](../modifiers.md)