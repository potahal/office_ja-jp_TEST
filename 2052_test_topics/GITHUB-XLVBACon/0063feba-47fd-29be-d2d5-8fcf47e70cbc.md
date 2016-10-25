
# 对 Application 对象使用事件

在对  **Application** 对象使用事件之前，必须创建一个类模块并声明一个带有事件的 **Application** 类型对象。例如，假定新建了一个 EventClassModule 类模块，该模块包含以下代码：


```
Public WithEvents App As Application
```


在对新对象带事件声明完之后，该对象将显示在类模块的"对象"下拉列表框中，并可以为此新对象编写事件过程（在"对象"框中选取新对象时，"过程"下拉列表框中会自动显示该对象的有效事件）。

在运行过程之前，必须将类模块中声明的对象与  **Application** 对象连接起来。可在任意的模块中用下列代码完成这一操作。

## 示例


```
Dim X As New EventClassModule 
 
Sub InitializeApp() 
 Set X.App = Application 
End Sub
```

运行  **InitializeApp** 过程之后，类模块中的 **App** 对象指向 Microsoft Excel **Application** 对象，并且类模块中的事件过程将在事件发生时运行。

