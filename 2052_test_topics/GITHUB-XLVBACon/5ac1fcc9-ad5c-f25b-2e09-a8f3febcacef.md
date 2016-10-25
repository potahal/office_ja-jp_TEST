
# ShapeRange.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **ShapeRange** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[ShapeRange Object](e1b8229c-73a0-4a77-5e00-4bcec9032260.md)
#### 其他资源


[ShapeRange Object Members](http://msdn.microsoft.com/library/1d1950c5-32ac-dfc0-8c19-07159a29a2a0%28Office.15%29.aspx)