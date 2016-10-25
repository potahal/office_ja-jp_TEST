
# Range.TCSCConverter Method (Word)

Converts the specified range from Traditional Chinese to Simplified Chinese or vice versa.


## Syntax

 _表达式_. **TCSCConverter**( ** _WdTCSCConverterDirection_**, ** _CommonTerms_**, ** _UseVariants_** )

 _表达式_ Required. A variable that represents a **[Range](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _WdTCSCConverterDirection_|可选|**WdTCSCConverterDirection**|Specifies the direction in which text is converted. If omitted, the default value is  **wdTCSCConverterDirectionAuto**, which converts in the appropriate direction based on the detected language of the specified range.|
| _UseVariants_|可选|**Boolean**|**True** if Word uses Taiwan, Hong Kong SAR, and Macao SAR character variants. Can only be used if translating from Simplified Chinese to Traditional Chinese.|

## Example

This example converts the current selection from Simplified Chinese to Traditional Chinese. It converts common expressions intact and uses regional character variants.


```
Selection.Range.TCSCConverter _ 
 wdTCSCConverterDirectionSCTC, True, True
```


## 另请参阅


#### 概念


[Range Object](15a7a1c4-5f3f-5b6e-60e9-29688de3f274.md)
#### 其他资源


[Range Object Members](http://msdn.microsoft.com/library/3c4a36d9-2a80-5aaf-827b-275a52bfa193%28Office.15%29.aspx)