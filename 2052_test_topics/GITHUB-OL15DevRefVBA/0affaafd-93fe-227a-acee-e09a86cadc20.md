
# 表对象 (Outlook)

代表来自于  **[Folder](3cf6cda8-6d70-666e-2643-9d9c5b9cacfc.md)** 或 **[Search](226a5d49-3caf-90dd-725c-265404d1939f.md)** 对象的项目数据的集合，它将项目作为表的行，而将属性作为表的列。


## 备注

 **表** 表示 **文件夹** 或 **搜索** 对象中数据的只读动态行集。您可以使用 **[Folder.GetTable](08d184cb-0c41-01b1-abc5-305476380f8b.md)** 或 **[Search.GetTable](3aba6b77-73a3-9620-9c18-b2e03c7b63bc.md)** 以获得一个 **Table** 对象，该对象表示一组文件夹或搜索文件夹中的项目。如果 **Table** 对象从 **Folder.GetTable** 获得，可以进一步指定筛选器 （ **[Table.Restrict](ecdd30f6-e12c-8025-3ded-592d2fad2bb8.md)** ) 来获取文件夹中的项目的子集。如果未指定任何筛选器，您将获取文件夹中的所有项目。

默认情况下，返回的 **表** 中的每个项包含其属性的默认子集。可将该文件夹中的项作为 **表** 的每一行、 每一列视为一个属性，该属性的项，以及 **表格** 为内存中的轻量行集允许快速枚举和筛选文件夹中的项目。虽然基础文件夹的添加和删除 **表** 中的行反映，但 **表** 不支持添加、 更改和删除行的任何事件。如果需要 **表** 中的行从一个可写对象，从默认条目 Id 列在 **表** 中获得行条目 ID，然后使用该 **[命名空间](f0dcaa19-07f5-5d42-a3bf-2e42b7885644.md)** 对象的 **[GetItemFromID](f2abff80-4c04-998b-654b-28600424a16f.md)** 方法来获取完整的项目，例如 **[MailItem](14197346-05d2-0250-fa4c-4a6b07daf25f.md)** 或 **[联系人](8e32093c-a678-f1fd-3f35-c2d8994d166f.md)** ，支持读写操作。 **表** 中的默认列的详细信息，请参见[Table 对象中显示的默认属性](http://msdn.microsoft.com/library/649c64f3-2d1e-23f1-bf13-3368da79e62b%28Office.15%29.aspx)。

 **Table** 对象的详细信息，请参阅[枚举、 搜索和筛选文件夹中的项目](http://msdn.microsoft.com/library/d786d292-7a0e-0e1a-e132-affbfde37744%28Office.15%29.aspx)。


## 示例

下面的代码示例阐释了如何 **表** 对象才能返回经过筛选的项目根据其 **LastModificationTime** 属性集。它还演示如何列出默认属性以及特定的项的属性。


```
Sub DemoTable() 
 
 'Declarations 
 
 Dim Filter As String 
 
 Dim oRow As Outlook.Row 
 
 Dim oTable As Outlook.Table 
 
 Dim oFolder As Outlook.Folder 
 
 
 
 'Get a Folder object for the Inbox 
 
 Set oFolder = Application.Session.GetDefaultFolder(olFolderInbox) 
 
 
 
 'Define Filter to obtain items last modified after May 1, 2005 
 
 Filter = "[LastModificationTime] > '5/1/2005'" 
 
 'Restrict with Filter 
 
 Set oTable = oFolder.GetTable(Filter) 
 
 
 
 'Remove all columns in the default column set 
 
 oTable.Columns.RemoveAll 
 
 'Specify desired properties 
 
 With oTable.Columns 
 
 .Add ("Subject") 
 
 .Add ("LastModificationTime") 
 
 'PR_ATTR_HIDDEN referenced by the MAPI proptag namespace 
 
 .Add ("http://schemas.microsoft.com/mapi/proptag/0x10F4000B") 
 
 End With 
 
 
 
 'Enumerate the table using test for EndOfTable 
 
 Do Until (oTable.EndOfTable) 
 
 Set oRow = oTable.GetNextRow() 
 
 Debug.Print (oRow("Subject")) 
 
 Debug.Print (oRow("LastModificationTime")) 
 
 Debug.Print (oRow("http://schemas.microsoft.com/mapi/proptag/0x10F4000B")) 
 
 Loop 
 
End Sub
```


## 方法



|**名称**|
|:-----|
|[FindNextRow](e09019ca-e4bb-2597-7b9e-a56c1b5fce6c.md)|
|[FindRow](5722cf58-d026-007a-558f-90b73bad920d.md)|
|[GetArray](2594bb2e-290f-8e88-52d1-cd2b2191bbe3.md)|
|[GetNextRow](e01ddaa0-a869-2f52-5e46-84d4d4090e61.md)|
|[GetRowCount](06014c43-700a-8502-bad7-b3f93a22e870.md)|
|[MoveToStart](af499471-dd21-9374-7399-3ce977368015.md)|
|[限制](ecdd30f6-e12c-8025-3ded-592d2fad2bb8.md)|
|[排序](4e4867c2-27b8-f920-59ce-b60116d22054.md)|

## 属性



|**名称**|
|:-----|
|[应用程序](10e7611e-e3b3-a07c-da85-f8c270a37212.md)|
|[类](bea314b0-9db9-ac67-a897-49e619da1066.md)|
|[列](57005ab1-ad49-296d-5b34-24dfd8f0987f.md)|
|[EndOfTable](8c185230-65ce-1b66-7b63-8de3533dea86.md)|
|[父级](1c6a54ac-ba4d-72a2-0871-a3522582dbde.md)|
|[会话](8a17876d-6637-f30b-6c0f-32cfc8b77d51.md)|

## 另请参阅


#### 其他资源


[表对象成员](bd9db35d-0738-22cf-a936-425d5a0ead87.md)
[Outlook 对象模型引用](http://msdn.microsoft.com/library/73221b13-d8d8-99b8-3394-b95dbbfd5ddc%28Office.15%29.aspx)