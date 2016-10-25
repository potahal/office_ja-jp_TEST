
# RemoteItem.MarkForDownload 属性 (Outlook)

返回或 **[设置用于确定某个项的状态远程用户接收到后](2df0404c-26c9-87d4-6916-d75aff8e3fbc.md)** 。读/写。


## 语法

 _表达式_. **MarkForDownload**

 _表达式_ 一个代表 **RemoteItem** 对象的变量。


## 注解

利用此属性，数据传输能力较弱的远程用户可以更加灵活地进行邮件收发。


## 示例

本示例在用户的 **"收件箱"**中搜索尚未全部下载的项目。如果找到此类项目，将向用户显示一条消息，并将该项目标记为下载。


```
Sub DownloadItems() 
 
 Dim mpfInbox As Outlook.Folder 
 
 Dim obj As Object 
 
 Dim i As Integer 
 
 
 
 Set mpfInbox = Application.GetNamespace("MAPI").GetDefaultFolder(olFolderInbox) 
 
 'Loop all items in the Inbox folder 
 
 For i = 1 To mpfInbox.Items.Count 
 
 Set obj = mpfInbox.Items.Item(i) 
 
 'Verify if the state of the item is olHeaderOnly 
 
 If obj.DownloadState = olHeaderOnly Then 
 
 MsgBox ("This item has not been fully downloaded.") 
 
 'Mark the item to be downloaded. 
 
 obj.MarkForDownload = olMarkedForDownload 
 
 End If 
 
 Next 
 
End Sub
```


## 另请参阅


#### 概念


[RemoteItem 对象](6302aaff-cdcf-4d86-60f1-4bed15540d9f.md)
#### 其他资源


[RemoteItem 对象成员](15c0872e-88cc-9b9b-c31e-c15d6971e6e0.md)