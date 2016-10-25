
# Range.Revisions Property (Word)

Returns a  **Revisions** collection that represents the tracked changes in the range. Read-only.


## Syntax

 _表达式_. **Revisions**

 _表达式_ A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


## Remarks

For information about returning a single member of a collection, see [Returning a Single Object from a Collection](8c0b84c0-582b-32f7-68e0-6383d0661e74.md).


## Example

This example displays the number of tracked changes in the first section in the active document.


```
MsgBox ActiveDocument.Sections(1).Range.Revisions.Count
```

This example accepts all tracked changes in the first paragraph in the selection.




```
Set myRange = Selection.Paragraphs(1).Range 
myRange.Revisions.AcceptAll
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)