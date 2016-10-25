
# FormatCondition.Priority Property (Excel)

Returns or sets the priority value of the conditional formatting rule. The priority determines the order of evaluation when multiple conditional formatting rules exist in a worksheet.


## Syntax

 _表达式_. **Priority**

 _表达式_ A variable that represents a **FormatCondition** object.


## Remarks

When setting the priority, the value must be a positive integer between 1 and the total number of conditional formatting rules on the worksheet. The priority must be a unique value for all rules on the worksheet, so changing the priority for the specified conditional formatting rule may cause the priority value of the other rules on the worksheet to be shifted.


## 另请参阅


#### 概念


[FormatCondition Object](38a2bca9-9b28-3ef2-8c7a-4d35a27229ec.md)
#### 其他资源


[FormatCondition Object Members](http://msdn.microsoft.com/library/8f4bebce-0bf4-03de-62f0-4454ea699c5f%28Office.15%29.aspx)