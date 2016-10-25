
# ColumnFormat 对象 （Outlook）

代表视图中顺序字段或视图字段的显示属性。


## 说明

 **ColumnFormat** 对象表示的显示属性，如 **[OrderField](4ae32270-bde9-3178-bca3-f8d145779d3d.md)** 或 **[ViewField](997319f0-7ff3-a712-8484-2e442965e187.md)** 对象的对齐方式或字段类型。使用 **ViewField** 对象的 **[ColumnFormat](0014f1d8-5380-3301-558a-7fd8d49afff9.md)** 属性访问视图字段的显示属性。

使用  **[Label](cf104506-3eca-6695-3d3b-05022ce6fba4.md)** 属性可以获取或更改用于为字段添加标签的文字，使用 **[Align](cea9e062-e338-ee1d-f769-dd5f8beef463.md)** 属性可以确定字段中内容的对齐方式。

使用  **[FieldType](84a40f6f-72fe-61e5-d85c-7a7c90f3e58a.md)** 属性可以确定该字段所显示数据的类型和形式，使用 **[FieldFormat](14064b56-65c2-1c7d-1e74-3bfa2d2ccaa7.md)** 属性可以确定如何设置该字段的数据格式。


## 示例

(VBA) 示例下面的 Visual Basic for Applications 循环 **[ViewFields](c4c6257e-fdbe-c187-86c5-34bee3eb0bd3.md)** 集合的当前的 **[表](026e27f8-1655-060d-e8cc-87eaaf4f1510.md)** 对象在集合中显示的标签和 XML 架构的每个 **ViewField** 对象的名称。


```
Private Sub DisplayTableViewFields() 
 
 Dim objTableView As TableView 
 
 Dim objViewField As ViewField 
 
 Dim strOutput As String 
 
 
 
 If Application.ActiveExplorer.CurrentView.ViewType = _ 
 
 olTableView Then 
 
 
 
 ' Obtain a TableView object reference for the 
 
 ' current table view. 
 
 Set objTableView = _ 
 
 Application.ActiveExplorer.CurrentView 
 
 
 
 ' Iterate through the ViewFields collection for 
 
 ' the table view, obtaining the label and the 
 
 ' XML schema name for each field included in 
 
 ' the view. 
 
 For Each objViewField In objTableView.ViewFields 
 
 With objViewField 
 
 strOutput = strOutput &amp; .ColumnFormat.Label &amp; _ 
 
 " (" &amp; .ViewXMLSchemaName &amp; ")" &amp; vbCrLf 
 
 End With 
 
 Next 
 
 
 
 ' Display a dialog box containing the concatenated 
 
 ' view field information. 
 
 MsgBox strOutput 
 
 End If 
 
End Sub 
 

```


## 另请参阅


#### 其他资源


[ColumnFormat 对象成员](7159f452-7a05-f3a3-53f8-0b3f5463d313.md)
[Outlook 对象模型引用](http://msdn.microsoft.com/library/73221b13-d8d8-99b8-3394-b95dbbfd5ddc%28Office.15%29.aspx)