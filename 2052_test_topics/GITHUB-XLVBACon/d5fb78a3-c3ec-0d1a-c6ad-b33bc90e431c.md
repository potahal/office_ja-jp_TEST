
# Range.Consolidate Method (Excel)

Consolidates data from multiple ranges on multiple worksheets into a single range on a single worksheet.  **Variant**.


## Syntax

 _表达式_. **Consolidate**( ** _Sources_**, ** _Function_**, ** _TopRow_**, ** _LeftColumn_**, ** _CreateLinks_** )

 _表达式_ A variable that represents a **Range** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Sources_|可选|**Variant**|The sources of the consolidation as an array of text reference strings in R1C1-style notation. The references must include the full path of sheets to be consolidated.|
| _Function_|可选|**Variant**|One of the constants of  **[XlConsolidationFunction](a3d0e4c0-8463-340c-a258-219d49f715d7.md)** which specifies the type of consolidation.|
| _TopRow_|可选|**Variant**|**True** to consolidate data based on column titles in the top row of the consolidation ranges. **False** to consolidate data by position. The default value is **False**.|
| _LeftColumn_|可选|**Variant**|**True** to consolidate data based on row titles in the left column of the consolidation ranges. **False** to consolidate data by position. The default value is **False**.|
| _CreateLinks_|可选|**Variant**|**True** to have the consolidation use worksheet links. **False** to have the consolidation copy the data. The default value is **False**.|

### Return Value

Variant


## Example

This example consolidates data from Sheet2 and Sheet3 onto Sheet1, using the SUM function.


```
Worksheets("Sheet1").Range("A1").Consolidate _ 
 Sources:=Array("Sheet2!R1C1:R37C6", "Sheet3!R1C1:R37C6"), _ 
 Function:=xlSum
```


## 另请参阅


#### 概念


[Range Object](b8207778-0dcc-4570-1234-f130532cc8cd.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/4336bf81-1e63-7e44-1792-baf366a027a7%28Office.15%29.aspx)