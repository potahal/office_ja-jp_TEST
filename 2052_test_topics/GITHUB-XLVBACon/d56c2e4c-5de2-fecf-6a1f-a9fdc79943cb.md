
# Workbook.DeleteNumberFormat Method (Excel)

Deletes a custom number format from the workbook.


## Syntax

 _表达式_. **DeleteNumberFormat**( ** _NumberFormat_** )

 _表达式_ A variable that represents a **Workbook** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _NumberFormat_|必需|**String**|Names the number format to be deleted.|

## Example

This example deletes the number format "000-00-0000" from the active workbook.


```
ActiveWorkbook.DeleteNumberFormat("000-00-0000")
```


## 另请参阅


#### 概念


[Workbook Object](8c00aa60-c974-eed3-0812-3c9625eb0d4c.md)
#### 其他资源


[Workbook Object Members](http://msdn.microsoft.com/library/dce102a3-25de-3ff4-2ce5-bc56e08baca7%28Office.15%29.aspx)