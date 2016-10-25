
# Application.Help Method (Word)

Displays installed Help information.


## Syntax

 _表达式_. **Help**( ** _HelpType_** )

 _表达式_ A variable that represents an **[Application](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _HelpType_|必需|**Variant**|The on-line Help topic or window. Can be any of these  **[WdHelpType](798a78ec-73cc-62aa-32fd-19f08c4c556f.md)** constants: **wdHelp**, **wdHelpAbout**, **wdHelpActiveWindow**, **wdHelpContents**, **wdHelpHWP**, **wdHelpIchitaro**, **wdHelpIndex**, **wdHelpPE2**, **wdHelpPSSHelp**, **wdHelpSearch**, **wdHelpUsingHelp**. (Some of the constants listed here may not be available to you, depending on the language that you have selected or installed.)|

## Example

This example displays the  **Help Topics** dialog box.


```
Help HelpType:=wdHelp
```

This example displays a list of Help topics that describe how to use Help.




```
Help HelpType:=wdHelpUsingHelp
```


## 另请参阅


#### 概念


[Application Object](d1cf6f8f-4e88-bf01-93b4-90a83f79cb44.md)
#### 其他资源


[Application Object Members](http://msdn.microsoft.com/library/71669f1e-65f1-b0f1-b67d-355dfdbebe50%28Office.15%29.aspx)