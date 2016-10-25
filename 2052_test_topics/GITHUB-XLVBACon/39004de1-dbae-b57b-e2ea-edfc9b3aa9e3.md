
# ShapeRange.IncrementTop Method (Excel)

Moves the specified shape vertically by the specified number of points.


## Syntax

 _表达式_. **IncrementTop**( ** _Increment_** )

 _表达式_ A variable that represents a **ShapeRange** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Increment_|必需|**Single**|Specifies how far the shape object is to be moved vertically, in points. A positive value moves the shape down; a negative value moves it up.|

## Example

This example duplicates shape one on  `myDocument`, sets the fill for the duplicate, moves it 70 points to the right and 50 points up, and rotates it 30 degrees clockwise.


```
Set myDocument = Worksheets(1) 
With myDocument.Shapes(1).Duplicate 
 .Fill.PresetTextured msoTextureGranite 
 .IncrementLeft 70 
 .IncrementTop -50 
 .IncrementRotation 30 
End With
```


## 另请参阅


#### 概念


[ShapeRange Object](e1b8229c-73a0-4a77-5e00-4bcec9032260.md)
#### 其他资源


[ShapeRange Object Members](http://msdn.microsoft.com/library/1d1950c5-32ac-dfc0-8c19-07159a29a2a0%28Office.15%29.aspx)