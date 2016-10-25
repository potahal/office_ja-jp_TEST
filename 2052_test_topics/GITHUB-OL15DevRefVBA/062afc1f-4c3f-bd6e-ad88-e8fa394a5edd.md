
# RemoteItem.Delete 方法 （Outlook）

从包含项目的文件夹中移除的项。


## 语法

 _表达式_. **Delete**

 _表达式_ 一个代表 **RemoteItem** 对象的变量。


## 注解

 **Delete** 方法删除集合中的单个项。若要删除一个文件夹的 **[项](441820e7-5fe8-e5ef-83c0-9c87fd3dc9e3.md)** 集合中的所有项目，您必须都删除开头的文件夹中的最后一项每个项。例如，在项集合中的一个文件夹中， `AllItems`，如果有 `n`文件夹中的项目数，开始删除 `AllItems.Item(n)`，递减每次删除 `AllItems.Item(1)`之前的索引处的项。

 **Delete**方法将该项目从包含它的文件夹移动到 **已删除邮件**文件夹。如果包含它的文件夹的 **已删除邮件**文件夹，  **Delete**方法将永久移除的项。


## 另请参阅


#### 概念


[RemoteItemObject](6302aaff-cdcf-4d86-60f1-4bed15540d9f.md)
#### 其他资源


[RemoteItemMembers](15c0872e-88cc-9b9b-c31e-c15d6971e6e0.md)
[删除所有项目和子文件夹在已删除的邮件文件夹中](http://msdn.microsoft.com/library/359a416b-43d4-396e-e348-5624c4ca3599%28Office.15%29.aspx)