
# LegendEntry 对象

代表指定的图表图例中的图例项。 **LegendEntry** 对象是包含在图例中的所有 **LegendEntry** 对象的 **[LegendEntries](98f5f860-7648-e3a6-f2af-985b383fed24.md)** 集合的成员。

每个图例项有两个部分: 条目，这是项; 相关联的系列的名称的文本和项的标记，与它相关联的系列或趋势线的图表图例项。 **[LegendKey](ab90cb64-1f81-dfcb-7542-cba68964acba.md)** 对象中包含的项的标记及其相关联的系列或趋势线的格式设置属性。

不能更改图例项的文字。 **LegendEntry** 对象支持字体格式，而且可以被删除。图例项不支持图案格式。图例项的位置和尺寸是固定的。


## LegendEntry 对象用法

可用  **LegendEntries** ( _index_ )（其中 _index_ 为图例项的编号）返回单个 **LegendEntry** 对象。不能用名称返回图例项。

索引号代表图例项在图例中的位置。 `LegendEntries(1)`顶部的图例中，并且在图例中，顶部和底部是 `LegendEntries(LegendEntries.Count)` 。下面的示例更改图例项的图例 (通常是第一个数据系列的图例) 顶部的文本的字体样式。




```
myChart.Legend.LegendEntries(1).Font.Italic = True
```


## 说明

没有返回相应于某图例项的数据系列或趋势线的直接方法。

图例项已被删除之后, 恢复它们的唯一方法是删除并重新创建包含这些图表将 **[HasLegend](b4dbef39-9d83-2f6e-fe06-8ca38cceeeec.md)** 属性设置为 **False** 的图例然后再返回到 **真** 。

