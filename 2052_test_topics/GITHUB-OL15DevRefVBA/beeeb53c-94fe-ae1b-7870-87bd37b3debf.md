
# ContactItem.Close 事件 (Outlook)

与项目（父对象的一个实例）相关的检查器关闭时发生。


## 语法

 _表达式_. **Close**( ** _Cancel_** )

 _表达式_ 一个代表 **ContactItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Cancel_|必需|**Boolean**|(不使用在 VBScript 中)。 **假** 的事件发生时。如果事件过程将此参数设置为 **True** ，则不完成关闭操作并保持检查器为打开。|

## 注解

在 Microsoft Visual Basic 脚本版 (VBScript) 中此函数的返回值设置为 **False** 时，如果不完成关闭操作并保持检查器为打开。

如果使用 **[Close](17cd04b5-1bf1-5df1-b1f4-f6e488d00fd5.md)** 方法以触发该事件，则可以仅取消 **Close** 方法使用 **olPromptForSave** 参数。


## 另请参阅


#### 概念


[创建 ContactItem 对象](8e32093c-a678-f1fd-3f35-c2d8994d166f.md)
#### 其他资源


[创建 ContactItem 对象成员](a8b13369-4c87-02aa-e62a-1f3067e559fa.md)