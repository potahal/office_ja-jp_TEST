
# PrintOut 方法

在 Visual Basic 中， **PrintOut** 方法执行 PrintOut 操作。


## 语法

 _表达式_. **PrintOut**( ** _PrintRange_**, ** _PageFrom_**, ** _PageTo_**, ** _PrintQuality_**, ** _Copies_**, ** _CollateCopies_** )

 _表达式_ 一个代表 **DoCmd** 对象的变量。


### 参数


|
|

## 注解

可以使用 PrintOut 操作来打印在打开的数据库中的活动对象。可以打印数据表、报表、窗体、数据访问页和模块。


## 示例

下面的示例以逐份打印的方式将活动窗体或数据表的前四页打印两份。


```
DoCmd.PrintOut acPages, 1, 4, , 2
```

