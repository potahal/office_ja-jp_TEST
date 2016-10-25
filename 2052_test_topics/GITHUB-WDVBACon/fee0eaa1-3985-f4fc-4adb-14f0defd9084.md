
# HTMLDivision.HTMLDivisionParent Method (Word)

Returns an  **HTMLDivision** object that represents a parent division of the current HTML division.


## Syntax

 _表达式_. **HTMLDivisionParent**( ** _LevelsUp_** )

 _表达式_ Required. A variable that represents an **[HTMLDivision](a38918ed-61aa-3fd1-3522-d077f1ff312f.md)** object.


### Parameters



|**Name**|**Required/Optional**|**Data Type**|**Description**|
|:-----|:-----|:-----|:-----|
| _LevelsUp_|可选|**Long**|The number of parent divisions to count back to return the desired division. If the LevelsUp argument is omitted, the HTML division returned is one level up from the current HTML division.|

### Return Value

HTMLDivision


## Example

This example formats the borders for two HTML divisions in the active document. This example assumes that the active document is an HTML document with at least two divisions.


```
Sub FormatHTMLDivisions() 
 With ActiveDocument.HTMLDivisions(1) 
 With .HTMLDivisions(1) 
 .LeftIndent = InchesToPoints(1) 
 .RightIndent = InchesToPoints(1) 
 With .Borders(wdBorderLeft) 
 .Color = wdColorBlue 
 .LineStyle = wdLineStyleDouble 
 End With 
 With .Borders(wdBorderRight) 
 .Color = wdColorBlue 
 .LineStyle = wdLineStyleDouble 
 End With 
 With .HTMLDivisionParent 
 .LeftIndent = InchesToPoints(1) 
 .RightIndent = InchesToPoints(1) 
 With .Borders(wdBorderTop) 
 .Color = wdColorBlack 
 .LineStyle = wdLineStyleDot 
 End With 
 With .Borders(wdBorderBottom) 
 .Color = wdColorBlack 
 .LineStyle = wdLineStyleDot 
 End With 
 End With 
 End With 
 End With 
End Sub
```


## 另请参阅


#### 概念


[HTMLDivision Object](a38918ed-61aa-3fd1-3522-d077f1ff312f.md)
#### 其他资源


[HTMLDivision Object Members](http://msdn.microsoft.com/library/c1b64462-f1a2-daf9-ca43-46bd6c9aef1b%28Office.15%29.aspx)