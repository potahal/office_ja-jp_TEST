
# WorksheetFunction.IsNA Method (Excel)

Checks the type of value and returns TRUE or FALSE depending if the value refers to the #N/A (value not available) error value.


## Syntax

 _表达式_. **IsNA**( ** _Arg1_** )

 _表达式_ A variable that represents a **WorksheetFunction** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Arg1_|必需|**Variant**|Value - the value you want tested. Value can be a blank (empty cell), error, logical, text, number, or reference value, or a name referring to any of these, that you want to test.|

### Return Value

Boolean


## Remarks




- The value arguments of the IS functions are not converted. For example, in most other functions where a number is required, the text value "19" is converted to the number 19. However, in the formula ISNUMBER("19"), "19" is not converted from a text value, and the ISNUMBER function returns FALSE.
    
- The IS functions are useful in formulas for testing the outcome of a calculation. When combined with the IF function, they provide a method for locating errors in formulas (see the following examples).
    

## 另请参阅


#### 概念


[WorksheetFunction Object](7b1d5639-363d-632c-2cf0-2232562646b6.md)
#### 其他资源


[WorksheetFunction Object Members](http://msdn.microsoft.com/library/6811ca87-4b53-0bff-88c9-30bf7497879a%28Office.15%29.aspx)