
# Selection.ColumnSelectMode Property (Word)

 **True** if column selection mode is active. Read/write **Boolean**.


## Syntax

 _表达式_. **ColumnSelectMode**

 _表达式_ A variable that represents a **[Selection](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)** object.


## Remarks

When this mode is active, the letters "COL" appear on the status bar.


## Example

This example selects a column of text that's two words across and three lines deep. The example copies the selection to the Clipboard and cancels column selection mode.


```
With Selection 
 .Collapse Direction:=wdCollapseStart 
 .ColumnSelectMode = True 
 .MoveRight Unit:=wdWord, Count:=2, Extend:=wdExtend 
 .MoveDown Unit:=wdLine, Count:=2, Extend:=wdExtend 
 .Copy 
 .ColumnSelectMode = False 
End With
```


## 另请参阅


#### 概念


[Selection Object](7b574a91-c33e-ecfd-6783-6b7528b2ed8f.md)
#### 其他资源


[Selection Object Members](http://msdn.microsoft.com/library/71e67a43-d40a-ad9a-8ef2-c5c487733e0d%28Office.15%29.aspx)