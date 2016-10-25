
# Browser.Previous Method (Word)

Moves the selection to the previous item indicated by the browser target. Use the  **Target** property to change the browser target.


## Syntax

 _表达式_. **Previous**

 _表达式_ Required. A variable that represents a **[Browser](447bcab6-cfb2-77b0-9bbd-35e774417a60.md)** object.


## Example

This example moves the insertion point into the first cell (the cell in the upper-left corner) of the previous table.


```
With Application.Browser 
 .Target = wdBrowseTable 
 .Previous 
End With
```


## 另请参阅


#### 概念


[Browser Object](447bcab6-cfb2-77b0-9bbd-35e774417a60.md)
#### 其他资源


[Browser Object Members](http://msdn.microsoft.com/library/ab97f30f-71c5-4360-0f6d-c47b7b45f0a3%28Office.15%29.aspx)