
# Axes.Parent Property (Excel)

Returns the parent object for the specified object. Read-only.


## Syntax

 _表达式_. **Parent**

 _表达式_ A variable that represents an **Axes** object.


## Example

This example displays the name of the chart that contains  `myAxis`.


```
Sub DisplayParentName() 
 
 Set myAxis = Charts(1).Axes(xlValue) 
 MsgBox myAxis.Parent.Name 
 
End Sub
```


## 另请参阅


#### 概念


[Axes Collection](581e51e5-3dbb-7f0c-a87d-2d44f67dad0b.md)
#### 其他资源


[Axes Object Members](http://msdn.microsoft.com/library/10a6fffe-65ff-e9b2-813c-357664e276a5%28Office.15%29.aspx)