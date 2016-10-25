
# Font.Creator Property (Excel)

Returns a 32-bit integer that indicates the application in which this object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ A variable that represents a **Font** object.


## Remarks

If the object was created in Microsoft Excel, this property returns the string XCEL, which is equivalent to the hexadecimal number 5843454C. The  **Creator** property is designed to be used in Microsoft Excel for the Macintosh, where each application has a four-character creator code. For example, Microsoft Excel has the creator code XCEL.


## 另请参阅


#### 概念


[Font Object](f4788ba4-1c4c-2f03-4d73-194bc9316825.md)
#### 其他资源


[Font Object Members](http://msdn.microsoft.com/library/537d89ae-59c5-0420-029a-32a2c385f02c%28Office.15%29.aspx)