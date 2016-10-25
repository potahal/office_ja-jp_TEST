
# SortFields.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **SortFields** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[SortFields Object](a9c83ea1-1cd9-1552-1f03-71bd92a2cc72.md)
#### 其他资源


[SortFields Object Members](http://msdn.microsoft.com/library/3fe54843-d34a-5d1a-75d6-2645da2755bc%28Office.15%29.aspx)