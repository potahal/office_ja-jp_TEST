
# Panes.Add Method (Word)

Returns a  **Pane** object that represents a new pane to a window.


## Syntax

 _表达式_. **Add**( ** _SplitVertical_** )

 _表达式_ Required. A variable that represents a **[Panes](6ed6353c-9134-f47d-a108-13e84eced8ff.md)** collection.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _SplitVertical_|可选|**Variant**|A number that represents the percentage of the window, from top to bottom, you want to appear above the split.|

### Return Value

Pane


## Remarks

This method will fail if it is applied to a window that has already been split.


## Example

The following example splits the active window such that the top pane is 30 percent of the total window size.


```
ActiveDocument.ActiveWindow.Panes.Add SplitVertical:=30
```


## 另请参阅


#### 概念


[Panes Collection Object](6ed6353c-9134-f47d-a108-13e84eced8ff.md)
#### 其他资源


[Panes Object Members](http://msdn.microsoft.com/library/22673447-a48d-afea-0642-5eb2a3efd221%28Office.15%29.aspx)