
# WorksheetFunction.Fixed Method (Excel)

Rounds a number to the specified number of decimals, formats the number in decimal format using a period and commas, and returns the result as text.


## Syntax

 _表达式_. **Fixed**( ** _Arg1_**, ** _Arg2_**, ** _Arg3_** )

 _表达式_ A variable that represents a **WorksheetFunction** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Arg1_|必需|**Double**|Number - the number you want to round and convert to text.|
| _Arg2_|可选|**Variant**|Decimals - the number of digits to the right of the decimal point.|
| _Arg3_|可选|**Variant**|No_commas - a logical value that, if TRUE, prevents FIXED from including commas in the returned text.|

### Return Value

String


## Remarks




- Numbers in Microsoft Excel can never have more than 15 significant digits, but decimals can be as large as 127.
    
- If decimals is negative, number is rounded to the left of the decimal point.
    
- If you omit decimals, it is assumed to be 2.
    
- If no_commas is FALSE or omitted, then the returned text includes commas as usual.
    
- The major difference between formatting a cell containing a number with the  **Cells** command ( **Format** menu) and formatting a number directly with the FIXED function is that FIXED converts its result to text. A number formatted with the **Cells** command is still a number.
    

## 另请参阅


#### 概念


[WorksheetFunction Object](7b1d5639-363d-632c-2cf0-2232562646b6.md)
#### 其他资源


[WorksheetFunction Object Members](http://msdn.microsoft.com/library/6811ca87-4b53-0bff-88c9-30bf7497879a%28Office.15%29.aspx)