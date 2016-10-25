
# Conversation.GetAlwaysMoveToFolder 方法 （Outlook）

返回一个  **[Folder](3cf6cda8-6d70-666e-2643-9d9c5b9cacfc.md)** 对象，该对象指示始终将到达会话的新项目移至的指定送达存储中的文件夹。


## 语法

 _表达式_. **GetAlwaysMoveToFolder**( ** _Store_** )

 _表达式_ 一个代表 **[Conversation](2705d38a-ebc0-e5a7-208b-ffe1f5446b1b.md)** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Store_|必需|**[Store](1eb22fe9-8849-7476-5388-2515b48591b9.md)**|会话项目移至的目标文件夹所驻留的存储。|

### 返回值

在到达该会话中的所有新项目始终移动到指定存储 **文件夹** 对象。


## 说明

如果 _存储_ 参数表示一个存档.pst 存储区未送达存储， **GetAlwaysMoveToFolder** 方法将返回一个 **Folder** 对象，适用于默认传送存储中的会话项。

如果未不指定任何文件夹中的，除 **已删除邮件**文件夹中，要始终移动对话项目到，  **GetAlwaysMoveToFolder** 方法返回 **Null** ( **不** 在 Visual Basic 中)。


## 示例

下面的 Microsoft Visual Basic 应用程序 (VBA) 示例演示如何查找到达对话在阅读窗格中显示的第一个邮件项的新项总是移动到其中的文件夹。该代码示例中，  `DemoGetAlwaysMoveToFolder`，验证会话中的所选的邮件项的存储区启用，如果对话存在，将使用 **GetAlwaysMoveToFolder** 来获取该文件夹，并显示文件夹名称获取该邮件项的对话对象。


```
Sub DemoGetAlwaysMoveToFolder() 
 
 Dim oMail As Outlook.MailItem 
 
 Dim oConv As Outlook.Conversation 
 
 Dim oStore As Outlook.Store 
 
 
 
 ' Get Item displayed in Reading Pane. 
 
 Set oMail = ActiveExplorer.Selection(1) 
 
 Set oStore = oMail.Parent.Store 
 
 If oStore.IsConversationEnabled Then 
 
 Set oConv = oMail.GetConversation 
 
 If Not (oConv Is Nothing) Then 
 
 Dim oFolder As Outlook.folder 
 
 Set oFolder = _ 
 
 oConv.GetAlwaysMoveToFolder(oStore) 
 
 If Not (oFolder Is Nothing) Then 
 
 Debug.Print "MoveToFolder: " &amp; oFolder.name 
 
 Else 
 
 Debug.Print "MoveToFolder action not set" 
 
 End If 
 
 End If 
 
 End If 
 
End Sub
```


## 另请参阅


#### 概念


[会话对象](2705d38a-ebc0-e5a7-208b-ffe1f5446b1b.md)
#### 其他资源


[会话对象成员](09ff1e8e-7c5a-0b1e-e8e2-e259f66f71c8.md)