
# Font.Underline Property (Excel)

Returns or sets the type of underline applied to the font. Can be one of the following  **[XlUnderlineStyle](4b847715-a0eb-6db0-f358-870b4012b242.md)** constants. Read/write **Variant**.


## Syntax

 _表达式_. **Underline**

 _表达式_ A variable that represents a **Font** object.


## Remarks




||
|:-----|
|**XlUnderlineStyle** can be one of these **XlUnderlineStyle** constants.|
|**xlUnderlineStyleNone**|
|**xlUnderlineStyleSingle**|
|**xlUnderlineStyleDouble**|
|**xlUnderlineStyleSingleAccounting**|
|**xlUnderlineStyleDoubleAccounting**|

## Example

This example sets the font in the active cell on Sheet1 to single underline.


```
Worksheets("Sheet1").Activate 
ActiveCell.Font.Underline = xlUnderlineStyleSingle
```


## 另请参阅


#### 概念


[Font Object](f4788ba4-1c4c-2f03-4d73-194bc9316825.md)
#### 其他资源


[Font Object Members](http://msdn.microsoft.com/library/537d89ae-59c5-0420-029a-32a2c385f02c%28Office.15%29.aspx)