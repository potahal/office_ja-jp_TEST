
# JournalItem.AutoResolvedWinner 属性 (Outlook)

返回 **boolean 类型的值** ，确定该项目是自动冲突解决入选方。只读的。


## 语法

 _表达式_. **AutoResolvedWinner**

 _表达式_ 一个代表 **JournalItem** 对象的变量。


## 注解

 **False** 值不一定表示项目是自动冲突解决的落选方。该项目可能与另一个项冲突。

如果物料具有大于零的 **[JournalItem.Conflicts](27e68a60-745a-43a3-b1db-e4c80a9e4a38.md)** 属性的 **[Conflicts.Count](4a7445ff-8628-50d6-f4c0-ada85f3b3f5c.md)** 和其 **AutoResolvedWinner** 属性为 **True** ，则自动冲突解决入选方。另一方面，如果该项处于冲突和 **AutoResolvedWinner** 属性为 **False** ，则自动冲突解决的落选方。


## 另请参阅


#### 概念


[JournalItem 对象](6e850295-39f9-47b8-e866-9622e9958c69.md)
#### 其他资源


[JournalItem 对象成员](13a0cd10-44bc-a167-c613-93985f698d95.md)