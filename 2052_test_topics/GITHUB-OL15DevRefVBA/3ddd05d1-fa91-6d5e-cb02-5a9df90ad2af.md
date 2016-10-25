
# TaskRequestItem.DownloadState 属性 (Outlook)

返回一个指示项目下载状态的常量，该常量属于  **[OlDownloadState](ff5e00db-ad06-ddf1-6e3a-536c0ae4ef34.md)** 枚举。只读。


## 语法

 _表达式_. **DownloadState**

 _表达式_ 一个代表 **TaskRequestItem** 对象的变量。


## 示例

以下 Microsoft Visual Basic for Applications (VBA) 示例在用户的 **"收件箱"**中搜索尚未完全下载的项目。如果找到此类项目，则会向用户显示一条消息，并将该项目标记为下载。


```
Sub DownloadItems() 
 
 Dim mpfInbox As Outlook.Folder 
 
 Dim objItems As Outlook.Items 
 
 Dim obj As Object 
 
 Dim i As Integer 
 
 Dim iCount As Integer 
 
 
 
 Set mpfInbox = Application.GetNamespace("MAPI").GetDefaultFolder(olFolderInbox) 
 
 Set objItems = mpfInbox.Items 
 
 iCount = objItems.Count 
 
 'Loop all items in the Inbox folder 
 
 For i = 1 To iCount 
 
 Set obj = objItems.Item(i) 
 
 'Verify if the state of the item is olHeaderOnly 
 
 If obj.DownloadState = olHeaderOnly Then 
 
 MsgBox "This item has not been fully downloaded." 
 
 'Mark the item to be downloaded 
 
 obj.MarkForDownload = olMarkedForDownload 
 
 obj.Save 
 
 End If 
 
 Next 
 
End Sub
```


## 另请参阅


#### 概念


[TaskRequestItem 对象](2908a28a-634c-e786-aa53-f3e32038b727.md)
#### 其他资源


[TaskRequestItem 对象成员](d43114ee-be91-ff02-3424-525da2cf3a50.md)