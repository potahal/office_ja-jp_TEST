
# Range.Editors Property (Word)

Returns an  **Editors** object that represents all the users authorized to modify a selection or range within a document.


## Syntax

 _表达式_. **Editors**

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


## Example

The following example gives the current user editing permission to modify the active selection.


```
Dim objEditor As Editor 
 
Set objEditor = Selection.Editors.Add(wdEditorCurrent)
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)