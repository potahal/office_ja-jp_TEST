
# Table.Shading Property (Word)

Returns a  **Shading** object that refers to the shading formatting for the specified object.


## Syntax

 _表达式_. **Shading**

 _表达式_ Required. A variable that represents a **[Table](996b58dd-ebc6-ee30-5bfe-c5e51a0f71d6.md)** object.


## Example

This example applies horizontal line texture to the first table in the active document.


```
If ActiveDocument.Tables.Count >= 1 Then 
 With ActiveDocument.Tables(1)Shading 
 .Texture = wdTextureHorizontal 
 End With 
End If
```


## 另请参阅


#### 概念


[Table Object](996b58dd-ebc6-ee30-5bfe-c5e51a0f71d6.md)
#### 其他资源


[Table Object Members](http://msdn.microsoft.com/library/5367ee92-b5a3-92c7-787b-46a302586a0d%28Office.15%29.aspx)