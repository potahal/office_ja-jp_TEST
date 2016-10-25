
# FormDescription.ScriptText 属性 (Outlook)

返回一个 **字符串** ，包含所有的 VBScript 代码在窗体的脚本编辑器。只读的。


## 语法

 _表达式_. **ScriptText**

 _表达式_ 一个代表 **[FormDescription](c88f92c4-4cac-84b3-6118-1150d42d7cff.md)** 对象的变量。


## 示例

此 Microsoft Visual Basic 脚本版本 (VBScript) 示例使用 **[Open](656c16f7-d561-a8f7-e859-9ac24f357769.md)** 事件访问 **[MailItem](14197346-05d2-0250-fa4c-4a6b07daf25f.md)** 的 **[HTMLBody](c340fe05-9a99-3a32-3d6b-f2f7a568b299.md)** 属性。这将 **MailItem** 的 **[检查器](d7384756-669c-0549-1032-c3b864187994.md)** 的 **[EditorType](b19e552b-1e8a-8915-f793-396860910f40.md)** 属性设置为 **olEditorHTML** 。当 **MailItem** 的 **[Body](578567b1-893b-db4e-dddb-f3c237952c03.md)** 属性设置时， **EditorType** 属性更改为默认值。例如，如果默认电子邮件编辑器设置为 rtf 格式， **EditorType** 设置为 **olEditorRTF** 。如果此代码放在设计模式下的窗体的脚本编辑器中，运行时消息框将反映 **EditorType** 中的变化，按窗体更改正文。最后一条消息框使用 **脚本文本** 属性显示在脚本编辑器中的所有的 VBScript 代码。


```
Function Item_Open() 
 
 'Set the HTMLBody of the item. 
 
 Item.HTMLBody = "<HTML><H2>My HTML page.</H2><BODY>My body.</BODY></HTML>" 
 
 'Item displays HTML message. 
 
 Item.Display 
 
 'MsgBox shows EditorType is 2. 
 
 MsgBox "HTMLBody EditorType is " &amp; Item.GetInspector.EditorType 
 
 'Access the Body and show 
 
 'the text of the Body. 
 
 MsgBox "This is the Body: " &amp; Item.Body 
 
 'After accessing, EditorType 
 
 'is still 2. 
 
 MsgBox "After accessing, the EditorType is " &amp; Item.GetInspector.EditorType 
 
 'Set the item's Body property. 
 
 Item.Body = "Back to default body." 
 
 'After setting, EditorType is 
 
 'now back to the default. 
 
 MsgBox "After setting, the EditorType is " &amp; Item.GetInspector.EditorType 
 
 'Access the items's 
 
 'FormDescription object. 
 
 Set myForm = Item.FormDescription 
 
 'Display all the code 
 
 'in the Script Editor. 
 
 MsgBox myForm.ScriptText 
 
End Function
```


## 另请参阅


#### 概念


[答复对象](c88f92c4-4cac-84b3-6118-1150d42d7cff.md)
#### 其他资源


[答复对象成员](664724e9-e74b-32ad-93e4-8d4cb27b3082.md)