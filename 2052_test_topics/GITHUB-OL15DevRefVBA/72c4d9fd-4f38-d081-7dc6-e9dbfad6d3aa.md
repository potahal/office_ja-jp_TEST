
# Conversation.GetRootItems 方法 （Outlook）

返回一个  **[SimpleItems](b929ae28-fe5f-607e-37b5-ed6a304d4896.md)** 集合，该集合包含会话中的所有根项目。


## 语法

 _表达式_. **GetRootItems**

 _表达式_ 一个代表 **[Conversation](2705d38a-ebc0-e5a7-208b-ffe1f5446b1b.md)** 对象的变量。


### 返回值

 **SimpleItems** 集合，该集合包含根项或会话的所有根项。


## 说明

一个会话可拥有一个或多个根项目。例如，如果会话的根项目拥有三个子项目，而该根项目被永久删除，则所有三个子项目均将变为根项目。

如果在获取 **[会话](2705d38a-ebc0-e5a7-208b-ffe1f5446b1b.md)** 对象后从对话中删除了所有项，则 **GetRootItems** 将返回具有零对象 **SimpleItems** 集合。在这种情况下， **SimpleItems** 集合的 **[Count](2656676b-ee82-aad0-21b9-8ca963cb57d2.md)** 属性返回 0。


## 另请参阅


#### 概念


[会话对象](2705d38a-ebc0-e5a7-208b-ffe1f5446b1b.md)
#### 其他资源


[会话对象成员](09ff1e8e-7c5a-0b1e-e8e2-e259f66f71c8.md)