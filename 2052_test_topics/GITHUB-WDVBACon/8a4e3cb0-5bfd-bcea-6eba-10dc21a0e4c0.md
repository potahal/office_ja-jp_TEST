
# Find.Font Property (Word)

Returns or sets a  **[Font](bc97f4df-fc81-d6c8-e99a-d50dc793b7ae.md)** object that represents the character formatting of the specified object. Read/write **Font**.


## Syntax

 _表达式_. **Font**

 _表达式_ A variable that represents a **[Find](da822788-cad5-992a-a835-18cc574cc324.md)** object.


## Remarks

To set this property, specify an expression that returns a  **[Font](bc97f4df-fc81-d6c8-e99a-d50dc793b7ae.md)** object.


## Example

This example finds the next range of text that's formatted with the Times New Roman font.


```
With Selection.Find 
 .ClearFormatting 
 .Font.Name = "Times New Roman" 
 .Execute FindText:="", ReplaceWith:="", Format:=True, _ 
 Forward:=True 
End With
```


## 另请参阅


#### 概念


[Find Object](da822788-cad5-992a-a835-18cc574cc324.md)
#### 其他资源


[Find Object Members](http://msdn.microsoft.com/library/21f00da0-4c84-ace3-fc79-a55a9ed64360%28Office.15%29.aspx)