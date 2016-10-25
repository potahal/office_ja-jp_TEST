
# OlkOptionButton.KeyUp 事件 (Outlook)

当用户释放某个键时发生。


## 语法

 _表达式_. **KeyUp**( ** _KeyCode_**, ** _Shift_** )

 _表达式_ 一个代表 **OlkOptionButton** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _KeyCode_|必需|**长整型**|所按键的数值。|
| _Shift_|必需|**Integer**|中指定的 **SHIFT**、  **ctrl 键**或 **ALT**键是否已被按下的 **[OlShiftState](f71dd27d-6930-1450-e8e9-11ab1eace6ca.md)** 枚举常量按位或掩码。|

## 说明

在 **KeyUp** 事件过程中按下的修改键 ( **SHIFT**、  **ctrl 键**或 **ALT**) 的状态是可通过 _按住 Shift_ 参数访问。


## 另请参阅


#### 概念


[OlkOptionButton 对象](a7aab427-a2f0-a153-f558-c13559610c99.md)
#### 其他资源


[OlkOptionButton 对象成员](e5d545e6-496f-6a11-af73-faa3eb20647c.md)