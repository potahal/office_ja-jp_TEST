
# SeriesCollection.Add Method (Excel)

Adds one or more new series to the  **SeriesCollection** collection.


## Syntax

 _表达式_. **Add**( ** _Source_**, ** _Rowcol_**, ** _SeriesLabels_**, ** _CategoryLabels_**, ** _Replace_** )

 _表达式_ A variable that represents a **SeriesCollection** object.


### Parameters


|
|

### Return Value

A  **[Series](c7d34b32-8172-f7a0-0a17-f01d44246b64.md)** object that represents the new series.


## Remarks

This method does not actually return a  **Series** object as stated in the Object Browser. This method is not available for PivotChart reports.


## Example

This example creates a new series in Chart1. The data source for the new series is range B1:B10 on Sheet1.


```
Charts("Chart1").SeriesCollection.Add _
    Source:=ActiveWorkbook.Worksheets("Sheet1").Range("B1:B10")
```


## Example

This example creates a new series on the embedded chart on Sheet1.


```
Worksheets("Sheet1").ChartObjects(1).Activate
ActiveChart.SeriesCollection.Add _
    Source:=Worksheets("Sheet1").Range("B1:B10")
```

