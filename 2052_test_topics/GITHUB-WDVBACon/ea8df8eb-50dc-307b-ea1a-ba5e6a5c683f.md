
# KeyBinding.Execute Method (Word)

Runs the command associated with the specified key combination.


## Syntax

 _表达式_. **Execute**

 _表达式_ Required. A variable that represents a **[KeyBinding](0f691196-76ef-135d-a8c9-b2fb9f9ac695.md)** object.


## Example

This example assigns the CTRL+SHIFT+C key combination to the  **FileClose** command and then executes the key combination (the document is closed).


```
CustomizationContext = ActiveDocument.AttachedTemplate 
Keybindings.Add KeyCode:=BuildKeyCode(wdKeyControl, _ 
 wdKeyShift, wdKeyC), KeyCategory:=wdKeyCategoryCommand, _ 
 Command:="FileClose" 
FindKey(BuildKeyCode(wdKeyControl, wdKeyShift, wdKeyC)).Execute
```


## 另请参阅


#### 概念


[KeyBinding Object](0f691196-76ef-135d-a8c9-b2fb9f9ac695.md)
#### 其他资源


[KeyBinding Object Members](http://msdn.microsoft.com/library/ff0776e1-3695-a392-992b-9d5a772449dc%28Office.15%29.aspx)