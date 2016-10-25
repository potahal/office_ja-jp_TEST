
# DistListItem.GetInspector 属性 (Outlook)

返回一个表示初始化以包含指定的项的检查器 **[检查器](d7384756-669c-0549-1032-c3b864187994.md)** 对象。只读的。


## 语法

 _表达式_. **GetInspector**

 _表达式_ 一个代表 **DistListItem** 对象的变量。


## 注解

此属性可用于返回要显示的项，而不是使用 **[Application.ActiveInspector](3f2b6491-7b4b-8165-327e-b319711d5656.md)** 方法和设置 **[Inspector.CurrentItem](eaaf0192-a169-c107-95a6-b8e759a3b873.md)** 属性 **检查器** 对象。如果 **检查器** 对象已存在的项， **GetInspector** 属性将返回而不是创建一个新的 **检查器** 对象。


## 另请参阅


#### 概念


[DistListItem 对象](027c3986-abff-d9b1-ecc2-26d60805e952.md)
#### 其他资源


[DistListItem 对象成员](3ba4af84-ce84-61d9-1bc9-fab41bf6f125.md)