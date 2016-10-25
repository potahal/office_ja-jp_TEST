
# MailItem.Display 方法 （Outlook）

为项目显示一个新的  **[Inspector](d7384756-669c-0549-1032-c3b864187994.md)** 对象。


## 语法

 _表达式_. **Display**( ** _Modal_** )

 _表达式_ 一个代表 **MailItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Modal_|可选|**Variant**|**真正** 要使窗口模式。默认值为 **False** 。|

## 注解

为了向后兼容的浏览器和检查器窗口支持 **显示** 方法。若要激活一个资源管理器或检查器窗口，使用 **[Activate](d7784df0-b595-6f5a-2195-27ad021db6de.md)** 方法。

如果您尝试打开"不安全"文件系统对象 (或"freedoc"文件)，请使用Microsoft Outlook对象模型，您将收到 **E_FAIL** 返回代码在 C 或 c + + 编程语言中。在 Outlook 2000 和早期版本，您可以通过 **显示** 方法打开"不安全"文件系统对象。


## 示例

此 Visual Basic for Applications 示例显示 **"收件箱"**文件夹中的第一个项目。如果 **"收件箱"**为空，本示例将返回一个错误，因为您希望显示一个特定的项目。如果该文件夹中没有项目，将显示一个消息框通知用户。


 **注释**   **Items** 集合对象中的项都不能保证在任何特定的顺序。


```
Sub DisplayFirstItem() 
 
 Dim myNameSpace As Outlook.NameSpace 
 
 Dim myFolder As Outlook.Folder 
 
 
 
 Set myNameSpace = Application.GetNamespace("MAPI") 
 
 Set myFolder = myNameSpace.GetDefaultFolder(olFolderInbox) 
 
 On Error GoTo ErrorHandler 
 
 myFolder.Items(1).Display 
 
 Exit Sub 
 
ErrorHandler: 
 
 MsgBox "There are no items to display." 
 
End Sub
```


## 另请参阅


#### 概念


[MailItem 对象](14197346-05d2-0250-fa4c-4a6b07daf25f.md)
#### 其他资源


[MailItem 对象成员](1094d7df-ee80-a4b0-5a21-db2979506e6b.md)