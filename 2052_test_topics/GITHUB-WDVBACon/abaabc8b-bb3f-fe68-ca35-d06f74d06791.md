
# PageSetup.RightMargin Property (Word)

Returns or sets the distance (in points) between the right edge of the page and the right boundary of the body text. Read/write  **Single**.


## Syntax

 _表达式_. **RightMargin**

 _表达式_ A variable that represents a **[PageSetup](1879d601-80ad-4fc0-1a87-92e999b59f88.md)** object.


## Remarks

If the  **[MirrorMargins](ae7c53d9-7669-fb22-323f-2ad3984e2dfa.md)** property is set to **True**, the **RightMargin** property controls the setting for outside margins and the **[LeftMargin](873d6cf2-da9f-5d88-314f-9820284a54ee.md)** property controls the setting for inside margins.


## Example

This example displays the right margin setting for the active document. The  **[PointsToInches](e3d6ab40-3919-55e0-5829-603fca24c226.md)** method is used to convert the result to inches.


```
With ActiveDocument.PageSetup 
 Msgbox "The right margin is set to " _ 
 &amp; PointsToInches(.RightMargin) &amp; " inches." 
End With
```

This example sets the right margin for section two in the selection. The  **[InchesToPoints](67a7e59c-bc61-be03-852d-05fadebef148.md)** method is used to convert inches to points.




```
Selection.Sections(2).PageSetup.RightMargin = InchesToPoints(1)
```


## 另请参阅


#### 概念


[PageSetup Object](1879d601-80ad-4fc0-1a87-92e999b59f88.md)
#### 其他资源


[PageSetup Object Members](http://msdn.microsoft.com/library/9ff8b896-933b-1a19-19d5-5e5d87aab1b5%28Office.15%29.aspx)