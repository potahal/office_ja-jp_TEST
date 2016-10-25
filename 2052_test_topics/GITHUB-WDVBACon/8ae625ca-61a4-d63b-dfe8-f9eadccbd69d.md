
# CustomLabels.Creator Property (Word)

Returns a 32-bit integer that indicates the application in which the specified object was created. Read-only  **Long**.


## Syntax

 _表达式_. **Creator**

 _表达式_ Required. A variable that represents a **[CustomLabels](407e75b5-4116-fdc7-f0c1-dfd3809cdb41.md)** collection.


## Remarks

If the object was created in Microsoft Word, the  **Creator** property returns the hexadecimal number 4D535744, which represents the string "MSWD." This property was primarily designed to be used on the Macintosh, where each application has a four-character creator code. For example, Microsoft Word has the creator code MSWD. For additional information about this property, consult the language reference Help included with Microsoft Office Macintosh Edition.


## 另请参阅


#### 概念


[CustomLabels Collection Object](407e75b5-4116-fdc7-f0c1-dfd3809cdb41.md)
#### 其他资源


[CustomLabels Object Members](http://msdn.microsoft.com/library/ee79f452-698d-3089-ed57-b2ca3b125e3d%28Office.15%29.aspx)