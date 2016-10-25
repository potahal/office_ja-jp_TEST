
# View.RevisionsBalloonWidthType Property (Word)

Sets or returns a  **WdRevisionsBalloonWidthType** constant representing the global setting that specifies how Microsoft Word measures the width of revision balloons. Read/write.


## Syntax

 _表达式_. **RevisionsBalloonWidthType**

 _表达式_ Required. A variable that represents a **[View](8bf5b26b-14c0-1985-65b2-3e034360baeb.md)** object.


## Remarks

The  **RevisionsBalloonWidthType** property sets the measurement unit to use when setting the **RevisionsBalloonWidth** property.


## Example

This example sets the width of the revision balloons to twenty-five percent of the document's width. This example assumes that the document in the active window contains revisions made by one or more reviewers and that revisions are displayed in balloons.


```
Sub BalloonWidthType() 
 With ActiveWindow.View 
 .RevisionsBalloonWidthType = wdBalloonWidthPercent 
 .RevisionsBalloonWidth = 25 
 End With 
End Sub
```


## 另请参阅


#### 概念


[View Object](8bf5b26b-14c0-1985-65b2-3e034360baeb.md)
#### 其他资源


[View Object Members](http://msdn.microsoft.com/library/b7d2bd4e-c96d-3b8f-98a0-57c145f9aa42%28Office.15%29.aspx)