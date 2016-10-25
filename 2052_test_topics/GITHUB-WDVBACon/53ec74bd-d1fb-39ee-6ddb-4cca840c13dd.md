
# Options.EnableMisusedWordsDictionary Property (Word)

 **True** if Microsoft Word checks for misused words when checking the spelling and grammar in a document. Read/write **Boolean**.


## Syntax

 _表达式_. **EnableMisusedWordsDictionary**

 _表达式_ A variable that represents a **[Options](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)** object.


## Remarks

Word looks for the following when checking for misused words: incorrect usage of adjectives and adverbs, comparatives and superlatives, "like" as a conjunction, "nor" versus "or," "what" versus "which," "who" versus "whom," units of measurement, conjunctions, prepositions, and pronouns.


## Example

This example sets Word to ignore misused words when checking spelling and grammar.


```
Options.EnableMisusedWordsDictionary = False
```


## 另请参阅


#### 概念


[Options Object](873b7b99-3fe1-fd89-9ece-a9355cb827dc.md)
#### 其他资源


[Options Object Members](http://msdn.microsoft.com/library/76cd9dfe-6bbb-4c3d-0bfc-79a62bedd15e%28Office.15%29.aspx)