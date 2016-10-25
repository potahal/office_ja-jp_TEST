
# Window.SmallScroll Method (Excel)

Scrolls the contents of the window by rows or columns.


## Syntax

 _表达式_. **SmallScroll**( ** _Down_**, ** _Up_**, ** _ToRight_**, ** _ToLeft_** )

 _表达式_ A variable that represents a **Window** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Down_|可选|**Variant**|The number of rows to scroll the contents down.|
| _Up_|可选|**Variant**|The number of rows to scroll the contents up.|
| _ToRight_|可选|**Variant**|The number of columns to scroll the contents to the right.|
| _ToLeft_|可选|**Variant**|The number of columns to scroll the contents to the left.|

### Return Value

Variant


## Remarks

If  _Down_ and _Up_ are both specified, the contents of the window are scrolled by the difference of the arguments. For example, if _Down_ is 3 and _Up_ is 6, the contents are scrolled up three rows.

If  _ToLeft_ and _ToRight_ are both specified, the contents of the window are scrolled by the difference of the arguments. For example, if _ToLeft_ is 3 and _ToRight_ is 6, the contents are scrolled to the right three columns.

Any of these arguments can be a negative number.


## Example

This example scrolls the contents of the active window of Sheet1 down three rows.


```
Worksheets("Sheet1").Activate 
ActiveWindow.SmallScroll down:=3
```


## 另请参阅


#### 概念


[Window Object](8591b1ad-76f8-14e2-9120-406b65093f5a.md)
#### 其他资源


[Window Object Members](http://msdn.microsoft.com/library/f11db427-24a4-041c-2fd5-03ce73ae6c16%28Office.15%29.aspx)