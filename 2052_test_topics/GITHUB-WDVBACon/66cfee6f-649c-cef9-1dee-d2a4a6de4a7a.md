
# Find.Frame Property (Word)

Returns a  **[Frame](d36d3361-9e93-7dd9-b8c9-0ce503e03810.md)** object that represents the frame formatting for the specified style or find-and-replace operation. Read-only.


## Syntax

 _表达式_. **Frame**

 _表达式_ A variable that represents a **[Find](da822788-cad5-992a-a835-18cc574cc324.md)** object.


## Example

This example finds the first frame with wrap around formatting. If such a frame is found, a message is displayed on the status bar.


```
With ActiveDocument.Content.Find 
 .Text = "" 
 .Frame.TextWrap = True 
 .Execute Forward:=True, Wrap:=wdFindContinue, Format:=True 
 If .Found = True Then StatusBar = "Frame was found" 
 .Parent.Select 
End With
```


## 另请参阅


#### 概念


[Find Object](da822788-cad5-992a-a835-18cc574cc324.md)
#### 其他资源


[Find Object Members](http://msdn.microsoft.com/library/21f00da0-4c84-ace3-fc79-a55a9ed64360%28Office.15%29.aspx)