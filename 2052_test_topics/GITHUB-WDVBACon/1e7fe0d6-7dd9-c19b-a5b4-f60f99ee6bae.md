
# CheckBox.Size Property (Word)

Returns or sets the size of a check box, in points. Read/write  **Single**.


## Syntax

 _表达式_. **Size**

 _表达式_ A variable that represents a **[CheckBox](e72b57b7-0328-9e78-94ca-ab7fb3c64afb.md)** object.


## Example

This example sets the size of the check box named "Check1" in the active document to 14 points and then sets the check box as selected.


```
With ActiveDocument.FormFields("Check1").CheckBox 
 .AutoSize = False 
 .Size = 14 
 .Value = True 
End With
```


## 另请参阅


#### 概念


[CheckBox Object](e72b57b7-0328-9e78-94ca-ab7fb3c64afb.md)
#### 其他资源


[CheckBox Object Members](http://msdn.microsoft.com/library/f23d6b68-17f6-6238-66c8-c27f225bbd14%28Office.15%29.aspx)