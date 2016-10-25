
# Find.MatchByte Property (Word)

 **True** if Microsoft Word distinguishes between full-width and half-width letters or characters during a search. Read/write **Boolean**.


## Syntax

 _表达式_. **MatchByte**

 _表达式_ A variable that represents a **[Find](da822788-cad5-992a-a835-18cc574cc324.md)** object.


## Example

This example searches for the term "マイクロソフト" in the specified range without distinguishing between full-width and half-width characters.


```
With Selection.Find 
    .ClearFormatting 
    .MatchWholeWord = True 
    .MatchByte = False 
    .Execute FindText:="マイクロソフト" 
End With
```


## 另请参阅


#### 概念


[Find Object](da822788-cad5-992a-a835-18cc574cc324.md)
#### 其他资源


[Find Object Members](http://msdn.microsoft.com/library/21f00da0-4c84-ace3-fc79-a55a9ed64360%28Office.15%29.aspx)