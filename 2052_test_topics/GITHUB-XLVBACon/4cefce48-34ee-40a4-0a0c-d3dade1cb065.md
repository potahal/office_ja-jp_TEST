
# HPageBreak.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **HPageBreak** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[HPageBreak Object](8fc96958-33ab-8251-f627-4769b5eab97f.md)
#### 其他资源


[HPageBreak Object Members](http://msdn.microsoft.com/library/32b561ff-a0cf-142b-0a46-c622a42b6125%28Office.15%29.aspx)