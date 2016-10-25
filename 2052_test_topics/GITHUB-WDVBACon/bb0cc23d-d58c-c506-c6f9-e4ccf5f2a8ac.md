
# Options.PictureWrapType Property (Word)

Sets or returns a  **WdWrapTypeMerged** that indicates how Microsoft Word wraps text around pictures. Read/write.


## Syntax

 _表达式_. **PictureWrapType**

 _表达式_ Required. A variable that represents an **[Options](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)** collection.


## Remarks

This is a default option setting and affects all pictures inserted unless picture wrapping is individually defined for a picture.


## Example

This example sets Word to insert and paste all pictures inline with the text if inline is not already specified.


```
Sub PicWrap() 
 With Application.Options 
 If .PictureWrapType <> wdWrapMergeInline Then 
 .PictureWrapType = wdWrapMergeInline 
 End If 
 End With 
End Sub
```


## 另请参阅


#### 概念


[Options Object](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)
#### 其他资源


[Options Object Members](http://msdn.microsoft.com/library/76cd9dfe-6bbb-4c3d-0bfc-79a62bedd15e%28Office.15%29.aspx)