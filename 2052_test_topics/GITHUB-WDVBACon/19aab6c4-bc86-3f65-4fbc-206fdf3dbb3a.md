
# Range.PreviousBookmarkID Property (Word)

Returns the number of the last bookmark that starts before or at the same place as the specified range. Read-only  **Long**.


## Syntax

 _表达式_. **PreviousBookmarkID**

 _表达式_ A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


## Remarks

This property returns 0 (zero) if there is no corresponding bookmark


## Example

This example displays the name of the bookmark that precedes the second paragraph.


```
num = ActiveDocument.Paragraphs(2).Range.PreviousBookmarkID 
If num <> 0 Then MsgBox ActiveDocument.Content.Bookmarks(num).Name
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)