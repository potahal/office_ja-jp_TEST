
# Column.Session 属性 (Outlook)

返回当前会话的  **[NameSpace](f0dcaa19-07f5-5d42-a3bf-2e42b7885644.md)** 对象。只读。


## 语法

 _表达式_. **Session**

 _表达式_ 一个代表 **Column** 对象的变量。


## 说明

 **会话** 属性和 **[Application.GetNamespace](6175d0d9-5a61-ce45-35c0-b70895d757b3.md)** 方法可互换为当前会话获取 **命名空间** 的对象。这两个成员可以实现同一目的。例如，下列语句执行相同功能:


```
Set objNamespace = Application.GetNamespace("MAPI") 
```


```
Set objSession = Application.Session
```


## 另请参阅


#### 概念


[列对象](b7eb6916-2d80-57c3-2077-47a2a4c73185.md)
#### 其他资源


[列对象成员](c9b724b2-49e3-8cd5-95c7-0e4ea423df46.md)