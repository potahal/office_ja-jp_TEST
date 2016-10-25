
# Shape.ScaleWidth Method (Word)

Scales the width of the shape by a specified factor.


## Syntax

 _表达式_. **ScaleWidth**( ** _Factor_**, ** _RelativeToOriginalSize_**, ** _Scale_** )

 _表达式_ Required. A variable that represents a **[Shape](604029ce-9b2f-9748-5d4e-b458796fa2f0.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Factor_|必需|**Single**|Specifies the ratio between the width of the shape after you resize it and the current or original width. For example, to make a rectangle 50 percent larger, specify 1.5 for this argument.|
| _RelativeToOriginalSize_|必需|**MsoTriState**|**True** to scale the shape relative to its original size. **False** to scale it relative to its current size. You can specify **True** for this argument only if the specified shape is a picture or an OLE object.|
| _Scale_|可选|**MsoScaleFrom**|The part of the shape that retains its position when the shape is scaled.|

## Remarks

For pictures and OLE objects, you can indicate whether you want to scale the shape relative to the original size or relative to the current size. Shapes other than pictures and OLE objects are always scaled relative to their current width.


## Example

This example scales all pictures and OLE objects on  _myDocument_ to 175 percent of their original height and width, and it scales all other shapes to 175 percent of their current height and width.


```
Set myDocument = ActiveDocument 
For Each s In myDocument.Shapes 
 Select Case s.Type 
 Case msoEmbeddedOLEObject, msoLinkedOLEObject, _ 
 msoOLEControlObject, _ 
 msoLinkedPicture, msoPicture 
 s.ScaleHeight 1.75, True 
 s.ScaleWidth 1.75, True 
 Case Else 
 s.ScaleHeight 1.75, False 
 s.ScaleWidth 1.75, False 
 End Select 
Next
```


## 另请参阅


#### 概念


[Shape Object](604029ce-9b2f-9748-5d4e-b458796fa2f0.md)
#### 其他资源


[Shape Object Members](http://msdn.microsoft.com/library/4aa8e2f4-5629-3922-11e4-df028bd1e1de%28Office.15%29.aspx)