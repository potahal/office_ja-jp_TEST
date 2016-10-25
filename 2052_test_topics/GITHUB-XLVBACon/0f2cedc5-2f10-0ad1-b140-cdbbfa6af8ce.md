
# WorksheetFunction.Fv Method (Excel)

Returns the future value of an investment based on periodic, constant payments and a constant interest rate.


## Syntax

 _表达式_. **Fv**( ** _Arg1_**, ** _Arg2_**, ** _Arg3_**, ** _Arg4_**, ** _Arg5_** )

 _表达式_ A variable that represents a **WorksheetFunction** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Arg1_|必需|**Double**|Rate - the interest rate per period.|
| _Arg2_|必需|**Double**|Nper - the total number of payment periods in an annuity.|
| _Arg3_|必需|**Double**|Pmt - the payment made each period; it cannot change over the life of the annuity. Typically, pmt contains principal and interest but no other fees or taxes. If pmt is omitted, you must include the pv argument.|
| _Arg4_|可选|**Variant**|Pv - the present value, or the lump-sum amount that a series of future payments is worth right now. If pv is omitted, it is assumed to be 0 (zero), and you must include the pmt argument.|
| _Arg5_|可选|**Variant**|Type - the number 0 or 1 and indicates when payments are due. If type is omitted, it is assumed to be 0.|

### Return Value

Double


## Remarks

For a more complete description of the arguments in FV and for more information on annuity functions, see PV.



|**Set type equal to**|**If payments are due**|
|:-----|:-----|
|0|At the end of the period|
|1|At the beginning of the period|

- Make sure that you are consistent about the units you use for specifying rate and nper. If you make monthly payments on a four-year loan at 12 percent annual interest, use 12%/12 for rate and 4*12 for nper. If you make annual payments on the same loan, use 12% for rate and 4 for nper.
    
- For all the arguments, cash you pay out, such as deposits to savings, is represented by negative numbers; cash you receive, such as dividend checks, is represented by positive numbers.
    

## 另请参阅


#### 概念


[WorksheetFunction Object](7b1d5639-363d-632c-2cf0-2232562646b6.md)
#### 其他资源


[WorksheetFunction Object Members](http://msdn.microsoft.com/library/6811ca87-4b53-0bff-88c9-30bf7497879a%28Office.15%29.aspx)