
# FormField.DropDown Property (Word)

Returns a  **[DropDown](55233d61-d6d0-30f9-6825-ebbdbeb928b6.md)** object that represents a drop-down form field. Read-only.


## Syntax

 _表达式_. **DropDown**

 _表达式_ A variable that represents a **[FormField](c3c07344-06b2-fe86-6fcb-b9c63a991bcc.md)** object.


## Remarks

If the  **DropDown** property is applied to a **FormField** object that isn't a drop-down form field, the property won't fail, but the **Valid** property for the returned object will be **False**.


## Example

This example displays the text of the item selected in the drop-down form field named "Colors."


```
Dim ffDrop As FormField 
 
Set ffDrop = ActiveDocument.FormFields("Colors").DropDown 
 
MsgBox ffDrop.ListEntries(ffDrop.Value).Name
```

This example adds "Seattle" to the drop-down form field named "Places" in Form.doc.




```
With Documents("Form.doc").FormFields("Places") _ 
 .DropDown.ListEntries 
 .Add Name:="Seattle" 
End With
```


## 另请参阅


#### 概念


[FormField Object](c3c07344-06b2-fe86-6fcb-b9c63a991bcc.md)
#### 其他资源


[FormField Object Members](http://msdn.microsoft.com/library/e7d1b5d7-e1b3-b602-98c4-d0d4dc2288e5%28Office.15%29.aspx)