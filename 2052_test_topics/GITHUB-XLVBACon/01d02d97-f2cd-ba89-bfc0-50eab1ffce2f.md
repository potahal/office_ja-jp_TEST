
# SolverLoad 函数

加载现有的已保存于工作表中的规划求解模型参数。


 **注释**  规划求解加载项默认处于禁用状态。使用此函数之前，必须先安装并启用规划求解加载项。有关如何执行该操作的信息，请参阅[使用规划求解 VBA 函数](37d0aa49-2e5c-5efe-1c69-b5168af1f231.md)。安装规划求解加载项之后，必须创建对该规划求解加载项的引用。在 Visual Basic 编辑器中，在一个模块处于活动状态的情况下，单击 **"工具"**菜单上的 **"引用"**，然后选择 **"可使用的引用"**下面的 **"规划求解"**。如果 **"规划求解"**没有显示在 **"可使用的引用"**下面，请单击 **"浏览"**，然后打开 \Program Files\Microsoft Office\Office14\Library\SOLVER 子文件夹中的 Solver.xlam。


 **SolverLoad( _LoadArea_**, ** _Merge_ )**

 **LoadArea** **Variant** 类型，必需。活动工作表上对单元格区域的引用，完整问题说明将从该区域加载。 ** _LoadArea_** 中的第一个单元格包含"规划求解参数"对话框中"设置目标单元格"框的公式；第二个单元格包含"可变单元格"框的公式；后续单元格包含以逻辑公式的形式给出的约束。最后一个单元格可以选择包含规划求解选项值的数组。有关详细信息，请参阅 **[SolverOptions](270d5440-ac1e-2436-b632-5877ede0820e.md)** 。由 ** _LoadArea_** 参数表示的区域可位于任意工作表上，但如果该工作表不是活动工作表，就必须明确地指定。例如， `SolverLoad("Sheet2!A1:A3")` 从工作表 Sheet2 加载模型，即使该工作表不是活动工作表。
 **Merge** **Variant** 类型，可选。一个逻辑值，与您选择 **LoadArea** 引用并单击 **"确定"**后显示的对话框中的 **"合并"**按钮或 **"替换"**按钮相对应。如果为  **True** ，则会将 LoadArea 中的可变单元格选区和约束与当前定义的变量和约束合并。如果为 **False** 或被省略，则会在加载新规范之前删除当前的模型规范和选项（等效于调用 **[SolverReset](5c8f99e7-9451-3e72-1d93-4fcd72fc3e71.md)** 函数）。

## 示例

本示例加载以前计算过的规划求解模型（该模型保存于工作表 Sheet1 上），更改其中的一个约束，然后再次求解该模型。


```
Worksheets("Sheet1").Activate 
SolverLoad loadArea:=Range("A33:A38") 
SolverChange cellRef:=Range("F4:F6"), _ 
 relation:=1, _ 
 formulaText:=200 
SolverSolve userFinish:=False
```

