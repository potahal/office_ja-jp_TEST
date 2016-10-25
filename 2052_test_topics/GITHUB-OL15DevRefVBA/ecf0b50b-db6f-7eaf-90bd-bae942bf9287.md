
# Application.Quit 事件 (Outlook)

当 Microsoft Outlook 开始关闭时发生。


## 语法

 _表达式_. **Quit**

 _表达式_ 一个返回 **Application** 对象的表达式。


## 注解

此事件在 Microsoft Visual Basic Scripting Edition (VBScript) 中不可用。


## 示例

本 Microsoft Visual Basic for Applications (VBA) 示例在 Outlook 退出时显示再见消息。示例代码必须放在类模块中。


```
Private Sub Application_Quit() 
 
 MsgBox "Goodbye, " &amp; Application.GetNamespace("MAPI").CurrentUser 
 
End Sub
```


## 另请参阅


#### 概念


[应用程序对象](797003e7-ecd1-eccb-eaaf-32d6ddde8348.md)
#### 其他资源


[应用程序对象的成员](3519c89c-2353-85ee-7ddc-62e5dd85a8e7.md)