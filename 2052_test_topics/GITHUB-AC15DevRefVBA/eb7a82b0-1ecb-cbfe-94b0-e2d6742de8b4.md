
# DoCmd.SearchForRecord 方法 (Access)

可以使用  **SearchForRecord** 方法搜索表、查询、窗体或报表中的特定记录。


## 语法

 _表达式_. **SearchForRecord**( ** _ObjectType_**, ** _ObjectName_**, ** _Record_**, ** _WhereCondition_** )

 _表达式_ 一个表示 **DoCmd** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _ObjectType_|可选|**AcDataObjectType**|**[AcDataObjectType](0e9f8481-ef01-2415-414a-64788c18e6ef.md)** 常量，用于指定在其中搜索的数据库对象的类型。默认值为 **acActiveDataObject** 。|
| _ObjectName_|可选|**Variant**|包含要搜索的记录的数据库对象的名称。|
| _Record_|可选|**AcRecord**|**[AcRecord](39ece328-d461-9f4d-a3af-205ed3228929.md)** 常量，用于指定搜索的起始点和方向。默认值为 **acFirst** 。|
| _WhereCondition_|可选|**Variant**|String，用于定位记录。它类似于 SQL 语句中的 WHERE 子句，但不包括单词 WHERE。|

## 注解




- 在多个记录满足 WhereCondition 参数中的条件时，以下因素将确定找到哪个记录：
    
      - Record 参数设置。
    
  - 记录的排序顺序。例如，如果 Record 参数设置为  **acFirst** ，更改记录的排序顺序时，可能会更改找到的记录。
    
- 运行此操作之前，必须打开 ObjectName 参数中指定的对象。否则，会发生错误。
    
- 如果不满足 WhereCondition 参数中的条件，不会发生错误，焦点仍保留在当前记录。
    
- 在搜索下一个或上一个记录时，搜索到达数据末尾时不会"回绕"。如果没有满足条件的其他记录，不会发生错误，且焦点仍保留在当前记录。要确认找到了匹配的记录，可以输入下一步操作的条件，并使条件与 WhereCondition 参数中的条件相同。
    
-  **SearchForRecord** 方法类似于 **[FindRecord](dc48bc3d-5408-40a8-509b-e52b48b26187.md)** 方法，但 **SearchForRecord** 具有更强大的搜索功能。 **FindRecord** 方法主要用于查找字符串，其功能与" **查找**"对话框的功能相似。 **SearchForRecord** 方法使用的条件更像筛选或 SQL 查询中的条件。下表演示使用 **SearchForRecord** 方法可实现的一些功能：
    
      - 可以使用 WhereCondition 参数中的复杂条件，例如  `Description = "Beverages" and CategoryID = 11`
    
    
    
  - 可以引用在窗体或报表的记录源中但不显示在窗体或报表的字段。在前面的示例中， `Description` 和 `CategoryID` 均不必显示在窗体或报表中，条件即可起作用。
    
  - 可以使用逻辑运算符，例如  **<** 、 **>** 、 **AND** 、 **OR** 及 **BETWEEN** 。 **FindRecord** 方法仅与满足以下条件的字符串匹配：等于、始于或包含被搜索的字符串。
    

## 另请参阅


#### 概念


[DoCmd 对象](3ce44cca-9979-0a1e-9787-079a52ce528f.md)
#### 其他资源


[DoCmd 对象成员](3e7ade9e-86e4-0751-188b-5d31c9101651.md)