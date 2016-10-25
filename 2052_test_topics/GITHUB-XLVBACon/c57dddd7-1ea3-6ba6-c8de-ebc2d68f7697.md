
# PivotLineCells.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **PivotLineCells** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[PivotLineCells Object](cfa51fcd-3384-4c75-3ae9-4a2c1d92a489.md)
#### 其他资源


[PivotLineCells Object Members](http://msdn.microsoft.com/library/77db0767-34ff-6bb4-25e2-8a9361afe7f6%28Office.15%29.aspx)