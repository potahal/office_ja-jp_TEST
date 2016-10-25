
# ShapeNodes.Insert Method (Word)

Inserts a node into a freeform shape.


## Syntax

 _表达式_. **Insert**( ** _Index_**, ** _SegmentType_**, ** _EditingType_**, ** _X1_**, ** _Y1_**, ** _X2_**, ** _Y2_**, ** _X3_**, ** _Y3_** )

 _表达式_ Required. A variable that represents a **[ShapeNodes](f2e13db2-102f-1a14-fd7a-d179f63e513e.md)** collection.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Index_|必需|**Long**|The number of the shape node after which to insert a new node.|
| _SegmentType_|必需|**MsoSegmentType**|The type of line that connects the inserted node to the neighboring nodes.|
| _EditingType_|必需|**MsoEditingType**|The editing property of the inserted node.|
| _X1_|必需|**Single**|If the EditingType of the new segment is  **msoEditingAuto**, this argument specifies the horizontal distance, measured in points, from the upper-left corner of the document to the starting point of the new segment. If the EditingType of the new node is **msoEditingCorner**, this argument specifies the horizontal distance, measured in points, from the upper-left corner of the document to the first control point for the new segment.|
| _Y1_|必需|**Single**|If the EditingType of the new segment is  **msoEditingAuto**, this argument specifies the vertical distance, measured in points, from the upper-left corner of the document to the starting point of the new segment. If the EditingType of the new node is **msoEditingCorner**, this argument specifies the vertical distance, measured in points, from the upper-left corner of the document to the first control point for the new segment.|
| _X2_|可选|**Single**|If the EditingType of the new segment is  **msoEditingCorner**, this argument specifies the horizontal distance, measured in points, from the upper-left corner of the document to the second control point for the new segment. If the EditingType of the new segment is **msoEditingAuto**, don't specify a value for this argument.|
| _Y2_|可选|**Single**|If the EditingType of the new segment is  **msoEditingCorner**, this argument specifies the vertical distance, measured in points, from the upper-left corner of the document to the second control point for the new segment. If the EditingType of the new segment is **msoEditingAuto**, don't specify a value for this argument.|
| _X3_|可选|**Single**|If the EditingType of the new segment is  **msoEditingCorner**, this argument specifies the horizontal distance, measured in points, from the upper-left corner of the document to the ending point of the new segment. If the EditingType of the new segment is **msoEditingAuto**, don't specify a value for this argument.|
| _Y3_|可选|**Single**|If the EditingType of the new segment is  **msoEditingCorner**, this argument specifies the vertical distance, measured in points, from the upper-left corner of the document to the ending point of the new segment. If the EditingType of the new segment is **msoEditingAuto**, don't specify a value for this argument.|

## Example

This example selects the third shape in the active document, checks whether the shape is a  **Freeform** object, and if it is, inserts a node.


```
Sub InsertShapeNode() 
 ActiveDocument.Shapes(3).Select 
 With Selection.ShapeRange 
 If .Type = msoFreeform Then 
 .Nodes.Insert _ 
 Index:=3, SegmentType:=msoSegmentCurve, _ 
 EditingType:=msoEditingSymmetric, x1:=35, y1:=100 
 .Fill.ForeColor.RGB = RGB(0, 0, 200) 
 .Fill.Visible = msoTrue 
 Else 
 MsgBox "This shape is not a Freeform object." 
 End If 
 End With 
End Sub
```


## 另请参阅


#### 概念


[ShapeNodes Collection Object](f2e13db2-102f-1a14-fd7a-d179f63e513e.md)
#### 其他资源


[ShapeNodes Object Members](http://msdn.microsoft.com/library/1c404c66-24ad-0e6d-2135-ebe5857bfb23%28Office.15%29.aspx)