
# DistListItem.Close 事件 (Outlook)

与项目（父对象的一个实例）相关的检查器关闭时发生。


## 语法

 _表达式_. **Close**( ** _Cancel_** )

 _表达式_ 一个代表 **DistListItem** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Cancel_|必需|**Boolean**|(不使用在 VBScript 中)。 **假** 的事件发生时。如果事件过程将此参数设置为 **True** ，则不完成关闭操作并保持检查器为打开。|

## 注解

在 Microsoft Visual Basic 脚本版 (VBScript) 中此函数的返回值设置为 **False** 时，如果不完成关闭操作并保持检查器为打开。

如果使用 **[Close](6e56d716-ec8b-4a4c-1b1a-061f659f5c08.md)** 方法以触发该事件，则可以仅取消 **Close** 方法使用 **olPromptForSave** 参数。


## 另请参阅


#### 概念


[DistListItem 对象](027c3986-abff-d9b1-ecc2-26d60805e952.md)
#### 其他资源


[DistListItem 对象成员](3ba4af84-ce84-61d9-1bc9-fab41bf6f125.md)