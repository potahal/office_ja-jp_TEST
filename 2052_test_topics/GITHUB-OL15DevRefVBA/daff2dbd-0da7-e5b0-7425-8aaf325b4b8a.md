
# OlkInfoBar.MouseUp 事件 (Outlook)

当用户在控件上释放已经按下的鼠标按钮后发生。


## 语法

 _表达式_. **MouseUp**( ** _Button_**, ** _Shift_**, ** _X_**, ** _Y_** )

 _表达式_ 一个代表 **OlkInfoBar** 对象的变量。


### 参数



|**名称**|**必需/可选**|**数据类型**|**说明**|
|:-----|:-----|:-----|:-----|
| _Button_|必需|**Integer**|一个  **[OlMouseButton](f654f074-f7e7-6128-9d7d-8ec6adbfe5f7.md)** 常量，指定按下了鼠标的哪个按钮。|
| _Shift_|必需|**Integer**|中指定的 **SHIFT**、  **ctrl 键**或 **ALT**键是否已被按下的 **[OlShiftState](f71dd27d-6930-1450-e8e9-11ab1eace6ca.md)** 枚举常量按位或掩码。|
| _X_|必需|**[OLE_XPOS_CONTAINER]**|标识鼠标指针在 X 轴上相对于窗体的位置。|
| _Y_|必需|**[OLE_YPOS_CONTAINER]**|标识鼠标指针在 Y 轴上相对于窗体的位置。|

## 另请参阅


#### 概念


[OlkInfoBar 对象](1aec19db-d28b-ef9b-3227-45aa4a296de6.md)
#### 其他资源


[OlkInfoBar 对象成员](e7675cde-b1f0-153a-f4a9-b2d3bf5a0aff.md)