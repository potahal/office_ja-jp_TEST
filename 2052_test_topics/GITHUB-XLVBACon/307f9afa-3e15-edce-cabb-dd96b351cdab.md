
# WorksheetFunction.F_Dist_RT Method (Excel)

Returns the right-tailed F probability distribution. You can use this function to determine whether two data sets have different degrees of diversity. For example, you can examine the test scores of men and women entering high school and determine if the variability in the females is different from that found in the males.


## Syntax

 _表达式_. **F_Dist_RT**( ** _Arg1_**, ** _Arg2_**, ** _Arg3_** )

 _表达式_ A variable that represents a **[WorksheetFunction](7b1d5639-363d-632c-2cf0-2232562646b6.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Arg1_|必需|**Double**|X - the value at which to evaluate the function.|
| _Arg2_|必需|**Double**|Degrees_freedom1 - the numerator degrees of freedom.|
| _Arg3_|必需|**Double**|Degrees_freedom2 - the denominator degrees of freedom.|

### Return Value

Double


## Remarks




- If any argument is nonnumeric, F_DIST_RT returns the #VALUE! error value.
    
- If x is negative, F_DIST_RT returns the #NUM! error value.
    
- If degrees_freedom1 or degrees_freedom2 is not an integer, it is truncated.
    
- If degrees_freedom1 < 1 or degrees_freedom1 ? 10^10, F_DIST_RT returns the #NUM! error value.
    
- If degrees_freedom2 < 1 or degrees_freedom2 ? 10^10, F_DIST_RT returns the #NUM! error value.
    
- F_DIST_RT is calculated as F_DIST_RT=P( F>x ), where F is a random variable that has an F distribution with degrees_freedom1 and degrees_freedom2 degrees of freedom.
    

## 另请参阅


#### 概念


[WorksheetFunction Object](7b1d5639-363d-632c-2cf0-2232562646b6.md)
#### 其他资源


[WorksheetFunction Object Members](http://msdn.microsoft.com/library/6811ca87-4b53-0bff-88c9-30bf7497879a%28Office.15%29.aspx)