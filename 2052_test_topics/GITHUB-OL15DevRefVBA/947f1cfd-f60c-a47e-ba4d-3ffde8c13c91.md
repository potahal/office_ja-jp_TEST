
# AppointmentItem.Copy 方法 （Outlook）

创建对象的另一个实例。


## 语法

 _表达式_. **Copy**

 _表达式_ 一个代表 **AppointmentItem** 对象的变量。


## 示例

此示例的 Visual Basic for Applications 创建一封电子邮件，设置为"演讲"的 **主题** ，使用 **Copy** 方法复制它，然后将副本移到新创建的电子邮件文件夹的收件箱文件夹中名为"保存邮件"。


```
Sub CopyItem() 
 
 Dim myNameSpace As Outlook.NameSpace 
 
 Dim myFolder As Outlook.Folder 
 
 Dim myNewFolder As Outlook.Folder 
 
 Dim myItem As Outlook.MailItem 
 
 Dim myCopiedItem As Outlook.MailItem 
 
 
 
 Set myNameSpace = Application.GetNamespace("MAPI") 
 
 Set myFolder = myNameSpace.GetDefaultFolder(olFolderInbox) 
 
 Set myNewFolder = myFolder.Folders.Add("Saved Mail", olFolderDrafts) 
 
 Set myItem = Application.CreateItem(olMailItem) 
 
 myItem.Subject = "Speeches" 
 
 Set myCopiedItem = myItem.Copy 
 
 myCopiedItem.Move myNewFolder 
 
End Sub
```


## 另请参阅


#### 概念


[AppointmentItem 对象](204a409d-654e-27aa-643a-8344c631b82d.md)
#### 其他资源


[AppointmentItem 对象成员](c72c459d-6d3c-7a05-aa4a-b1b767ddc0b2.md)