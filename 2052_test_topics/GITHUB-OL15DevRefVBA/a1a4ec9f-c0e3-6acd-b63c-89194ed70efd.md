
# Application.AdvancedSearchStopped 事件 (Outlook)

指定的 **[搜索](226a5d49-3caf-90dd-725c-265404d1939f.md)** 对象 **[停止](c087e5aa-a846-56e1-a808-e8718096c3c9.md)** 方法的执行时发生。


## 语法

 _表达式_. **AdvancedSearchStopped**( ** _SearchObject_** )

 _表达式_ 一个代表 **Application** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _SearchObject_|必需|**Search**|**[AdvancedSearch](7b433d8b-08b9-dff1-b854-287d76b47a90.md)** 方法返回的 **[搜索](226a5d49-3caf-90dd-725c-265404d1939f.md)** 对象。|

## 注解

激发此事件后，将不再更新 **搜索** object?s **[结果](59057f6f-8f6d-eed0-c945-240b9593b7ea.md)** 集合。此事件只能以编程方式触发。


## 示例

以下 Visual Basic for Applications (VBA) 示例开始搜索 **"收件箱"**中主题为"Test"的项目并立即停止搜索。这会导致运行  `AdvanceSearchStopped` 事件过程。示例代码必须放在类模块（如 `ThisOutlookSession`）中。在 Microsoft Outlook 调用该事件过程前，必须先调用  `StopSearch()` 过程。


```
Sub StopSearch() 
 
 Dim sch As Outlook.Search 
 
 Dim strScope As String 
 
 Dim strFilter As String 
 
 strScope = "Inbox" 
 
 strFilter = "urn:schemas:httpmail:subject = 'Test'" 
 
 Set sch = Application.AdvancedSearch(strScope, strFilter) 
 
 sch.Stop 
 
End Sub 
 
 
 
Private Sub Application_AdvancedSearchStopped(ByVal SearchObject As Search) 
 
 'Inform the user that the search has stopped. 
 
 MsgBox "An AdvancedSearch has been interrupted and stopped. " 
 
End Sub 
 
 
 

```


## 另请参阅


#### 概念


[应用程序对象](797003e7-ecd1-eccb-eaaf-32d6ddde8348.md)
#### 其他资源


[应用程序对象的成员](3519c89c-2353-85ee-7ddc-62e5dd85a8e7.md)