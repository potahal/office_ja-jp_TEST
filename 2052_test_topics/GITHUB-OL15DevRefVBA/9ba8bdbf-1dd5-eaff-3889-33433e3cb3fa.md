
# MailItem.GetInspector 属性 (Outlook)

返回一个表示初始化以包含指定的项的检查器 **[检查器](d7384756-669c-0549-1032-c3b864187994.md)** 对象。只读的。


## 语法

 _表达式_. **GetInspector**

 _表达式_ 一个代表 **MailItem** 对象的变量。


## 注解

此属性可用于返回要显示的项，而不是使用 **[Application.ActiveInspector](3f2b6491-7b4b-8165-327e-b319711d5656.md)** 方法和设置 **[Inspector.CurrentItem](eaaf0192-a169-c107-95a6-b8e759a3b873.md)** 属性 **检查器** 对象。如果 **检查器** 对象已存在的项， **GetInspector** 属性将返回而不是创建一个新的 **检查器** 对象。


## 示例

此 Visual Basic for Applications (VBA) 示例显示了函数 `InsertBodyTextInWordEditor` ，创建一封邮件，为它指定一个标题并添加正文文本。该函数设置 **[Subject](5f3e465d-ac2b-a573-0e85-1134e65df017.md)** 属性来指定标题"测试..."。然后，它调用要在检查器中打开该邮件项的 **[显示](19ead642-b7bd-579f-e43b-ef5c5d0cfecb.md)** 方法。若要在 Word 编辑器以正文的邮件项目，该函数使用 Word 对象模型中对象的 **[文档](http://msdn.microsoft.com/library/8d83487a-2345-a036-a916-971c9db5b7fb%28Office.15%29.aspx)** 对象和 **[范围](http://msdn.microsoft.com/library/15a7a1c4-5f3f-5b6e-60e9-29688de3f274%28Office.15%29.aspx)** 中插入文本。该函数使用项的 **GetInspector** 属性来获取现有的 **检查** 对象，并再使用 **[Inspector.WordEditor](9e09b772-f679-19e6-905e-552ccadb0d24.md)** 属性来获取项的 **Word.Document** 对象。使用 **Word.Document** 对象，该函数访问 **Word.Range** 对象并在项目正文中插入文字。

由于此示例访问 Word 对象模型，因此必须首先将引用添加到 Microsoft Word 对象库中，以便成功编译该示例。




```
Sub InsertBodyTextInWordEditor() 
 Dim myItem As Outlook.MailItem 
 Dim myInspector As Outlook.Inspector 
 'You must add a reference to the Microsoft Word Object Library 
 'before this sample will compile 
 Dim wdDoc As Word.Document 
 Dim wdRange As Word.Range 
 
 On Error Resume Next 
 Set myItem = Application.CreateItem(olMailItem) 
 myItem.Subject = "Testing..." 
 myItem.Display 
 'GetInspector property returns Inspector 
 Set myInspector = myItem.GetInspector 
 'Obtain the Word.Document for the Inspector 
 Set wdDoc = myInspector.WordEditor 
 If Not (wdDoc Is Nothing) Then 
 'Use the Range object to insert text 
 Set wdRange = wdDoc.Range(0, wdDoc.Characters.Count) 
 wdRange.InsertAfter ("Hello world!") 
 End If 
End Sub
```


## 另请参阅


#### 概念


[MailItem 对象](14197346-05d2-0250-fa4c-4a6b07daf25f.md)
#### 其他资源


[MailItem 对象成员](1094d7df-ee80-a4b0-5a21-db2979506e6b.md)