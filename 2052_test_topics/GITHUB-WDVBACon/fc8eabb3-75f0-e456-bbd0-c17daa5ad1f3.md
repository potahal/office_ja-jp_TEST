
# Application.PointsToPixels Method (Word)

Converts a measurement from points to pixels. Returns the converted measurement as a  **Single**.


## Syntax

 _表达式_. **PointsToPixels**( ** _Points_**, ** _fVertical_** )

 _表达式_ Required. A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Points_|必需|**Single**|The point value to be converted to pixels.|
| _fVertical_|可选|**Variant**|**True** to return the result as vertical pixels; **False** to return the result as horizontal pixels.|

### Return Value

Single


## Example

This example displays the height and width in pixels of an object measured in points.


```
MsgBox "180x120 points is equivalent to " _ 
 &amp; PointsToPixels(180, False) &amp; "x" _ 
 &amp; PointsToPixels(120, True) _ 
 &amp; " pixels on this display."
```


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)