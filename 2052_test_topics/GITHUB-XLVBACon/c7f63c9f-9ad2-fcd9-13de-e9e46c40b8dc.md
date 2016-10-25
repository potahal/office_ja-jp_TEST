
# PivotField.AddPageItem Method (Excel)

Adds an additional item to a multiple item page field.


## Syntax

 _表达式_. **AddPageItem**( ** _Item_**, ** _ClearList_** )

 _表达式_ A variable that represents a **PivotField** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Item_|必需|**String**| Source name of a **PivotItem** object, corresponding to the specific Online Analytical Processing (OLAP) member unique name.|
| _ClearList_|可选|**Variant**|If  **False** (default), adds a page item to the existing list. If **True**, deletes all current items and adds _Item_.|

## Remarks

To avoid run-time errors, the data source must be an OLAP source, the field chosen must currently be in the page position, and the  **[EnableMultiplePageItems](989fa662-cafb-00a1-effb-4a6c18327ea3.md)** property must be set to **True**.


## Example

In this example, Microsoft Excel adds a page item with a source name titled "[Product].[All Products].[Food].[Eggs]". This example assumes an OLAP PivotTable exists on the active worksheet.


```
Sub UseAddPageItem() 
 
 ' The source is an OLAP database and you can manually reorder items. 
 ActiveSheet.PivotTables(1).CubeFields("[Product]"). _ 
 EnableMultiplePageItems = True 
 
 ' Add the page item titled "[Product].[All Products].[Food].[Eggs]". 
 ActiveSheet.PivotTables(1).PivotFields("[Product]").AddPageItem ( _ 
 "[Product].[All Products].[Food].[Eggs]") 
 
End Sub
```


## 另请参阅


#### 概念


[PivotField Object](52784960-e2da-b43a-1e37-2d4dae61c6d8.md)
#### 其他资源


[PivotField Object Members](http://msdn.microsoft.com/library/4a6ea12a-072c-a386-c855-7bf5f6eadd46%28Office.15%29.aspx)