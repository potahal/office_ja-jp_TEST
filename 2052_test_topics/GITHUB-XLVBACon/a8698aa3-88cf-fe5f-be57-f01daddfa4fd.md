
# WorksheetFunction.Delta Method (Excel)

Tests whether two values are equal. Returns 1 if number1 = number2; returns 0 otherwise.


## Syntax

 _表达式_. **Delta**( ** _Arg1_**, ** _Arg2_** )

 _表达式_ A variable that represents a **WorksheetFunction** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Arg1_|必需|**Variant**|Number1 - the first number.|
| _Arg2_|可选|**Variant**|Number2 - the second number. If omitted, number2 is assumed to be zero.|

### Return Value

Double


## Remarks

 Use this function to filter a set of values. For example, by summing several DELTA functions you calculate the count of equal pairs. This function is also known as the Kronecker Delta function.


- If number1 is nonnumeric, DELTA returns the #VALUE! error value.
    
- If number2 is nonnumeric, DELTA returns the #VALUE! error value.
    

## 另请参阅


#### 概念


[WorksheetFunction Object](7b1d5639-363d-632c-2cf0-2232562646b6.md)
#### 其他资源


[WorksheetFunction Object Members](http://msdn.microsoft.com/library/6811ca87-4b53-0bff-88c9-30bf7497879a%28Office.15%29.aspx)