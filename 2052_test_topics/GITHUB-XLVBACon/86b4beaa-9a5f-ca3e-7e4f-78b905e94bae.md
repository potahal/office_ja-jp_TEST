
# DataLabels.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **DataLabels** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[DataLabels Object](3d79271e-c702-e785-6984-d838d060a8c5.md)
#### 其他资源


[DataLabels Object Members](http://msdn.microsoft.com/library/3c9d909d-d090-b6ed-8a28-ba62c3459044%28Office.15%29.aspx)