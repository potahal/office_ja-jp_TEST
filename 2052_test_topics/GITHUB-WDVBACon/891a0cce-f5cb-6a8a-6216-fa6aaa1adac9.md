
# Axis.CategoryType Property (Word)

Returns or sets the category axis type. Read/write  **[XlCategoryType](10dad161-2a90-7915-51bb-ddc69427c003.md)**.


## Syntax

 _表达式_. **CategoryType**

 _表达式_ A variable that represents an **[Axis](3a7ad7d8-d397-a79a-eb6a-a5f0822cbe5d.md)** object.


## Remarks

You cannot set this property for a value axis.


## Example

The following example sets the category axis for the first chart in the active document to use a time scale, using months as the base unit.


```
With ActiveDocument.InlineShapes(1) 
 If .HasChart Then 
 With .Chart.Axes(xlCategory) 
 .CategoryType = xlTimeScale 
 .BaseUnit = xlMonths 
 End With 
 End If 
End With
```


## 另请参阅


#### 概念


[Axis Object](3a7ad7d8-d397-a79a-eb6a-a5f0822cbe5d.md)
#### 其他资源


[Axis Object Members](http://msdn.microsoft.com/library/44fa1b67-2a56-3d92-cb63-4144e1bb7282%28Office.15%29.aspx)