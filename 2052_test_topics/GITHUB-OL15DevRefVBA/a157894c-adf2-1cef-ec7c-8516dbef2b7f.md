
# SenderEmailAddress 属性

返回一个  **String** 类型的值，代表 Outlook 项目的发件人电子邮件地址。只读。


## 语法

 _表达式_. **SenderEmailAddress**

 _表达式_ 一个代表 **MailItem** 对象的变量。


## 注解

此属性对应于 MAPI 属性  **PidTagSenderEmailAddress**。


## 示例

以下 Microsoft Visual Basic for Applications (VBA) 示例循环 **"收件箱"**中 Test 文件夹的所有项目，并在"someone@example.com"发送的项目上设置黄色标志。若要使运行此示例时不会出现错误，请确保默认 **"收件箱"**文件夹中存在 Test 文件夹，并用 Test 文件夹中有效的发件人电子邮件地址替换"someone@example.com"。


```
Sub SetFlagIcon()
    Dim mpfInbox As Outlook.Folder
    Dim obj As Outlook.MailItem
    Dim i As Integer
    
    Set mpfInbox = Application.GetNamespace("MAPI").GetDefaultFolder(olFolderInbox).Folders("Test")
    ' Loop all items in the Inbox\Test Folder
    For i = 1 To mpfInbox.Items.Count
        If mpfInbox.Items(i).Class = olMail Then  
            Set obj = mpfInbox.Items.Item(i)
            If obj.SenderEmailAddress = "someone@example.com" Then
                'Set the yellow flag icon
                obj.FlagIcon = olYellowFlagIcon
                obj.Save
            End If
         End If
    Next
End Sub
```

