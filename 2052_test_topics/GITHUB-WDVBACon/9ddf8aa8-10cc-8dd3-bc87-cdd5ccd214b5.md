
# Application.GetSpellingSuggestions Method (Word)

Returns a  **[SpellingSuggestions](7e0fb008-e43c-c4cb-b7d2-9436d039a070.md)** collection that represents the words suggested as spelling replacements for a given word.


## Syntax

 _表达式_. **GetSpellingSuggestions**( ** _Word_**, ** _CustomDictionary_**, ** _IgnoreUppercase_**, ** _MainDictionary_**, ** _SuggestionMode_**, ** _CustomDictionary2_**, ** _CustomDictionary3_**, ** _CustomDictionary4_**, ** _CustomDictionary5_**, ** _CustomDictionary6_**, ** _CustomDictionary7_**, ** _CustomDictionary8_**, ** _CustomDictionary9_**, ** _CustomDictionary10_** )

 _表达式_ Required. A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _Word_|必需|**String**|The word whose spelling is to be checked.|
| _IgnoreUppercase_|可选|**Variant**|**True** to ignore words in all uppercase letters. If this argument is omitted, the current value of the **[IgnoreUppercase](4eff2832-3c66-0274-5403-d2fd8d31d04d.md)** property is used.|
| _SuggestionMode_|可选|**Variant**|Specifies the way Word makes spelling suggestions. Can be one of the following  **[WdSpellingWordType](7d0fd802-87c6-cf88-22d7-09800e256573.md)** constants: **wdAnagram**, **wdSpellword**, or **wdWildcard**. The default value is **WdSpellword**.|

## Remarks

If the word is spelled correctly, the  **Count** property of the **[SpellingSuggestions](7e0fb008-e43c-c4cb-b7d2-9436d039a070.md)** object returns 0 (zero).


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)