
# ODBCErrors.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents an **ODBCErrors** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[ODBCErrors Object](2f1c8a6b-2b9d-fc2c-7caa-289652ac8e24.md)
#### 其他资源


[ODBCErrors Object Members](http://msdn.microsoft.com/library/f59038ac-2664-73db-5165-6940a1cf1dd7%28Office.15%29.aspx)