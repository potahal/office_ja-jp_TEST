
# Shapes.AddShape Method (Word)

Adds an AutoShape to a document. Returns a  **[Shape](604029ce-9b2f-9748-5d4e-b458796fa2f0.md)** object that represents the AutoShape and adds it to the **[Shapes](0907eed3-886e-8e73-0e5e-71f4b37ddd5b.md)** collection.


## Syntax

 _表达式_. **AddShape**( ** _Type_**, ** _Left_**, ** _Top_**, ** _Width_**, ** _Height_** )

 _表达式_ Required. A variable that represents a **[Shapes](0907eed3-886e-8e73-0e5e-71f4b37ddd5b.md)** collection.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Type_|必需|**Long**|The type of shape to be returned. Can be any  **MsoAutoShapeType** constant.|
| _Left_|必需|**Single**|The position, measured in points, of the left edge of the AutoShape.|
| _Top_|必需|**Single**|The position, measured in points, of the top edge of the AutoShape.|
| _Width_|必需|**Single**|The width, measured in points, of the AutoShape.|
| _Height_|必需|**Single**|The height, measured in points, of the AutoShape.|

### Return Value

 **Shape**


## Remarks

To change the type of an AutoShape that you've added, set the  **AutoShapeType** property.


## 另请参阅


#### 概念


[Shapes Collection Object](0907eed3-886e-8e73-0e5e-71f4b37ddd5b.md)
#### 其他资源


[Shapes Object Members](http://msdn.microsoft.com/library/045d4e8c-b838-24f8-5919-c5a05e9bb3c5%28Office.15%29.aspx)