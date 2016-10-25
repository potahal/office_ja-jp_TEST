
# Application.Explorers 属性 (Outlook)

返回 **[浏览器](8398532a-1fad-7390-6778-109ac5e6c67c.md)** 集合对象，该对象包含表示所有打开的浏览器中的 **[资源管理器](026591e5-049f-503a-4166-34e6dbc225fb.md)** 对象。只读的。


## 语法

 _表达式_. **Explorers**

 _表达式_ 一个代表 **Application** 对象的变量。


## 示例

以下 Microsoft Visual Basic for Applications (VBA) 示例显示已打开的资源浏览器窗口的数目。


```
Private Sub CountExplorers() 
 
 MsgBox "There are " &amp; _ 
 
 Application.Explorers.Count &amp; " Explorers." 
 
End Sub
```

以下 VBA 示例使用 **[Count](ea7a19d2-6261-ce07-97f3-ebe95489a265.md)** 属性和 **Selection** 属性返回的 **[选择](0b06a3ce-0445-db8f-e6e8-bb7bd469c50f.md)** 集合的 **[Item](981b107a-14d7-2dd3-6449-2737b2801c3c.md)** 方法可以显示所有所选浏览器显示 **收件箱**中的邮件项目的发件人。若要运行此示例，您需要浏览器显示收件箱中选定至少一个邮件项目。您选择之外例如任务要求的邮件项目的项目，如 **SenderName** 属性对于 **[TaskRequestItem](2908a28a-634c-e786-aa53-f3e32038b727.md)** 对象不存在，则可能会收到错误。




```
Sub GetSelectedItems() 
 
 Dim myOlExp As Outlook.Explorer 
 
 Dim myOlSel As Outlook.Selection 
 
 Dim MsgTxt As String 
 
 Dim x As Integer 
 
 
 
 MsgTxt = "You have selected items from: " 
 
 Set myOlExp = Application.Explorers.Item(1) 
 
 If myOlExp = "Inbox" Then 
 
 Set myOlSel = myOlExp.Selection 
 
 For x = 1 To myOlSel.Count 
 
 MsgTxt = MsgTxt &amp; myOlSel.Item(x).SenderName &amp; ";" 
 
 Next x 
 
 MsgBox MsgTxt 
 
End If 
 
End Sub
```


## 另请参阅


#### 概念


[应用程序对象](797003e7-ecd1-eccb-eaaf-32d6ddde8348.md)
#### 其他资源


[应用程序对象的成员](3519c89c-2353-85ee-7ddc-62e5dd85a8e7.md)