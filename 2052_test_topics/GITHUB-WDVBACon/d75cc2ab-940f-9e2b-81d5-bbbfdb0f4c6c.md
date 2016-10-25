
# Window.Panes Property (Word)

Returns a  **[Panes](6ed6353c-9134-f47d-a108-13e84eced8ff.md)** collection that represents all the window panes for the specified window.


## Syntax

 _表达式_. **Panes**

 _表达式_ An expression that returns a **[Window](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)** object.


## Remarks

For information about returning a single member of a collection, see [Returning an Object from a Collection](28f76384-f495-9640-a7c8-10ada3fac727.md).


## Example

This example splits the active window in half.


```
If ActiveDocument.ActiveWindow.Panes.Count = 1 Then _ 
 ActiveDocument.ActiveWindow.Panes.Add
```

This example activates the first pane in the window for Document2.




```
Windows("Document2").Panes(1).Activate
```


## 另请参阅


#### 概念


[Window Object](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)
#### 其他资源


[Window Object Members](http://msdn.microsoft.com/library/c0dec747-3695-4f96-ea25-05b6494aad7e%28Office.15%29.aspx)