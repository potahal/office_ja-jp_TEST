
# SelectNamesDialog.CcLabel 属性 (Outlook)

返回或设置在 **选择姓名**对话框中的 **抄送**命令按钮上显示的文本 **字符串** 。读/写。


## 语法

 _表达式_. **CcLabel**

 _表达式_ 一个代表 **SelectNamesDialog** 对象的变量。


## 说明

若要提供收件人编辑框的快捷键，包括一个 and 符 (&amp;) 字符在标签参数字符串，用作访问键的字符前面。例如，如果 **CcLabel** 是字符串"本地及与会者"，用户可以按 **ALT + A 组合键**将焦点移到第一个收件人编辑框。

如果未设置 **CcLabel** ，默认值将为"Cc"的本地化的字符串。如果将 **CcLabel** 设置为一个空字符串，然后在 **抄送**命令按钮显示 **->**。如果 **CcLabel** 属性包含超过 32 个字符 (包括 and 符 (&amp;) 的访问键)，只有前 32 个字符将显示在命令按钮中。


## 另请参阅


#### 概念


[SelectNamesDialog 对象](1522736a-3cad-9f1c-4da9-b52a3a01731c.md)
#### 其他资源


[SelectNamesDialog 对象成员](0f5546af-f89a-8a8b-ced9-a2d646bf9634.md)