
# Comment.Edit Method (Word)

Opens the specified OLE object for editing in the application it was created in.


## Syntax

 _表达式_. **Edit**

 _表达式_ Required. A variable that represents a **[Comment](0a2841f3-ca3c-8186-afab-f634ebd97d4c.md)** object.


## Example

This example opens (for editing) the first embedded OLE object (defined as a shape) on the active document.


```
Dim shapesAll As Shapes 
 
Set shapesAll = ActiveDocument.Shapes 
If shapesAll.Count >= 1 Then 
 If shapesAll(1).Type = msoEmbeddedOLEObject Then 
 shapesAll(1).OLEFormat.Edit 
 End If 
End If
```

This example opens (for editing) the first linked OLE object (defined as an inline shape) in the active document.




```
Dim colIS As InlineShapes 
 
Set colIS = ActiveDocument.InlineShapes 
If colIS.Count >= 1 Then 
 If colIS(1).Type = wdInlineShapeLinkedOLEObject Then 
 colIS(1).OLEFormat.Edit 
 End If 
End If
```


## 另请参阅


#### 概念


[Comment Object](0a2841f3-ca3c-8186-afab-f634ebd97d4c.md)
#### 其他资源


[Comment Object Members](http://msdn.microsoft.com/library/1f1dbb3e-d0ae-9eb7-108a-697a10533e2b%28Office.15%29.aspx)