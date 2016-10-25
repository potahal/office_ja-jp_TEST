
# Range.EndnoteOptions Property (Word)

Returns an  **EndnoteOptions** object that represents the endnotes in a range.


## Syntax

 _表达式_. **EndnoteOptions**

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


## Example

This example sets the starting number for endnotes in section two of the active document to one if the starting number is not one.


```
Sub SetEndnoteOptionsRange() 
 With ActiveDocument.Sections(2).Range.EndnoteOptions 
 If .StartingNumber <> 1 Then 
 .StartingNumber = 1 
 End If 
 End With 
End Sub
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)