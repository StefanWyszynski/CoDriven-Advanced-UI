# UI Compose documentation

:arrow_backward: [Go back](README.md).

# CmLiveData - observable field 

CmLiveData is for generic type observer. 

available methods:

- available methods are self-explanatory

```csharp
    public void Observe(Action<T> observer, DisposableObservers disposableObservers = null)
    public void Remove(Action<T> observer)
    public void RemoveAllObservers()
    public bool HasObservers()
```

## DisposableObservers

you can pass DisposableObservers to Observe() method to control lifecycle of observers and call dispose when you dont want to observe anymore

```csharp
    private DisposableObservers disposableObservers = new DisposableObservers();
    private CmLiveData<String> livedata = new CmLiveData<String>();
    
    // ...
    
    livedata.Observe(value => {
        // this is callback when that will be called when livedata string field change
        // do something with value
    }, disposableObservers);
    
    
    // someware in the code
    livedata.value = "new text"; // will trigger above callback "livedata.Observe(value => {"
    livedata.value = "other text"; // will trigger above callback "livedata.Observe(value => {"
    
    // ... somewhere elese in the code
    // when you don't want observer anymore 
    disposableObservers.dispose();
```

When you add multiple observers to CmLiveData which passing your disposableObservers, then you can remove all observers 
by using 

DisposableObservers.dispose()

# CmSingleObserverLiveData - observable field

CmSingleObserverLiveData - observable field with only one observer

available methods:

- available methods are self-explanatory

```csharp
    public void Observe(Action<T> observer)
    public bool HasObservers()
```