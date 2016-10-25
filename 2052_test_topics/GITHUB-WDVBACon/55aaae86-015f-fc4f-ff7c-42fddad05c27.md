
# Fields.Update Method (Word)

Updates the result of the fields object.


## Syntax

 _表达式_. **Update**

 _表达式_ Required. A variable that represents a **[Fields](c79065bb-ba29-22fd-a9d7-90bb10550035.md)** collection.


### Return Value

Long


## Remarks

Returns 0 (zero) if no errors occur when the fields are updated, or returns a  **Long** that represents the index of the first field that contains an error.


## Example

This example updates all the fields in the main story (that is, the main body) of the active document. A return value of 0 (zero) indicates that the fields were updated without error.


```
If ActiveDocument.Fields.Update = 0 Then 
 MsgBox "Update Successful" 
Else 
 MsgBox "Field " &amp; ActiveDocument.Fields.Update &amp; _ 
 " has an error" 
End If
```


## 另请参阅


#### 概念


[Fields Collection Object](c79065bb-ba29-22fd-a9d7-90bb10550035.md)
#### 其他资源


[Fields Object Members](http://msdn.microsoft.com/library/b480b07e-2a71-0e3d-113c-962fcd484bd4%28Office.15%29.aspx)