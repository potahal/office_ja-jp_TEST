
# Application.ShowStartupDialog Property (Word)

 **True** to display the **Task Pane** when starting Microsoft Word. Read/write **Boolean**.


## Syntax

 _表达式_. **ShowStartupDialog**

 _表达式_ A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


## Remarks

The  **ShowStartupDialog** property is a global option, and the new setting will take effect only after you restart Word. Use the **Visible** property of the **CommandBars** collection show or hide the Task Pane without restarting Word.


## Example

This example turns off the  **Task Pane**, so it won't display upon starting Word. This will not take effect until the next time the user starts Word.


```
Sub HideStartUpDlg() 
 Application.ShowStartupDialog = False 
End Sub
```


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)