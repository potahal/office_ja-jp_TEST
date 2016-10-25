
# Window.LargeScroll Method (Word)

Scrolls a window or pane by the specified number of screens.


## Syntax

 _表达式_. **LargeScroll**( ** _Down_**, ** _Up_**, ** _ToRight_**, ** _ToLeft_** )

 _表达式_ Required. A variable that represents a **[Window](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Down_|可选|**Variant**|The number of screens to scroll the window down.|
| _Up_|可选|**Variant**|The number of screens to scroll the window up.|
| _ToRight_|可选|**Variant**|The number of screens to scroll the window to the right.|
| _ToLeft_|可选|**Variant**|The number of screens to scroll the window to the left.|

## Remarks

This method is equivalent to clicking just before or just after the scroll boxes on the horizontal and vertical scroll bars.

If Down and Up are both specified, the window is scrolled by the difference of the arguments. For example, if Down is 2 and Up is 4, the window is scrolled up two screens. Similarly, if ToLeft and ToRight are both specified, the window is scrolled by the difference of the arguments.

Any of these arguments can be a negative number. If no arguments are specified, the window is scrolled down one screen.


## Example

This example scrolls the active window down one screen.


```
ActiveDocument.ActiveWindow.LargeScroll Down:=1
```

This example splits the active window and then scrolls up two screens and to the right one screen.




```
With ActiveDocument.ActiveWindow 
 .Split = True 
 .LargeScroll Up:=2, ToRight:=1 
End With
```


## 另请参阅


#### 概念


[Window Object](d92f83f9-ae44-56c0-4584-7a9359253c6d.md)
#### 其他资源


[Window Object Members](http://msdn.microsoft.com/library/c0dec747-3695-4f96-ea25-05b6494aad7e%28Office.15%29.aspx)