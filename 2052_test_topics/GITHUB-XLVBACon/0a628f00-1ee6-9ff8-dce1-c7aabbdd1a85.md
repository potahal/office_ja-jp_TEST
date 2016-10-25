
# Chart.SeriesCollection Method (Excel)

Returns an object that represents either a single series (a  **[Series](c7d34b32-8172-f7a0-0a17-f01d44246b64.md)** object) or a collection of all the series (a **[SeriesCollection](93aa1f0b-4939-8c60-a444-2f791e8ce144.md)** collection) in the chart or chart group.


## Syntax

 _表达式_. **SeriesCollection**( ** _Index_** )

 _表达式_ A variable that represents a **Chart** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Index_|可选|**Variant**|The name or number of the series.|

### Return Value

Object


## Example

This example turns on data labels for series one in Chart1.


```
Charts("Chart1").SeriesCollection(1).HasDataLabels = True
```


## 另请参阅


#### 概念


[Chart Object](179c32ce-49bd-6f36-ea12-89fb5443f3ea.md)
#### 其他资源


[Chart Object Members](http://msdn.microsoft.com/library/a3f8ac44-02d6-6f3f-b5e0-23f4bd5d6baf%28Office.15%29.aspx)