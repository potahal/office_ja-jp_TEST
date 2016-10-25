
# LegendEntries.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **LegendEntries** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[LegendEntries Object](51d98149-b90b-432b-7771-0815a0e89655.md)
#### 其他资源


[LegendEntries Object Members](http://msdn.microsoft.com/library/dddeca68-d207-60af-9c16-afe670851a08%28Office.15%29.aspx)