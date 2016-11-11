
# Méthode Range.TextToColumns (Excel)

Cette méthode redistribue sur plusieurs colonnes une colonne de cellules qui comportent du texte.
 


## Syntaxe

*expression* . **TextToColumns**( ***Destination***, ***DataType***, ***TextQualifier***, ***ConsecutiveDelimiter***, ***Tab***, ***Semicolon***, ***Comma***, ***Space***, ***Other***, ***OtherChar***, ***FieldInfo***, ***DecimalSeparator***, ***ThousandsSeparator***, ***TrailingMinusNumbers*** )
 

 
*expression* Variable qui représente un objet **Range**.
 

 

### Paramètres



|**Nom**|**Obligatoire/Facultatif**|**Type de données**|**Description**|
|:-----|:-----|:-----|:-----|
| _Destination_|Facultatif|**Variante**|Objet  **Range** qui spécifie l'endroit où Microsoft Excel place les résultats. Si la plage contient plusieurs cellules, la cellule située dans le coin supérieur gauche est utilisée.|
| _DataType_|Facultatif|**[XlTextParsingType](71d76a41-c0b0-0b0f-27b5-7cac0d4c4ac4.md)**|Format du texte à diviser en colonnes.|
| _TextQualifier_|Facultatif|**[XlTextQualifier](ba209892-9dea-84db-eafd-629c7ab0b20f.md)**|Spécifie si des guillemets simples, doubles ou aucun guillemet doivent être utilisés comme qualificateur de texte.|
| _ConsecutiveDelimiter_|Facultatif|**Variante**|**True** pour que Microsoft Excel considère des séparateurs consécutifs comme un seul et même séparateur. La valeur par défaut est **False**.|
| _Tab_|Facultatif|**Variante**|**True** pour que _DataType_ ait la valeur **xlDelimited** et que la tabulation soit le séparateur. **False** est la valeur par défaut.|
| _Semicolon_|Facultatif|**Variante**|**True** pour que _DataType_ ait la valeur **xlDelimited** et que le point-virgule soit le séparateur. **False** est la valeur par défaut.|
| _Comma_|Facultatif|**Variante**|**True** pour que _DataType_ ait la valeur **xlDelimited** et que la virgule soit le séparateur. **False** est la valeur par défaut.|
| _Space_|Facultatif|**Variante**|**True** pour que _DataType_ ait la valeur **xlDelimited** et que l'espace soit le séparateur. **False** est la valeur par défaut.|
| _Other_|Facultatif|**Variante**|**True** pour que _DataType_ ait la valeur **xlDelimited** et que le caractère spécifié par l'argument _OtherChar_ soit le séparateur. La valeur par défaut est **False**.|
| _OtherChar_|Facultatif|**Variante**|(Obligatoire si l'argument  _Other_ a la valeur **True**.) Séparateur lorsque l'argument _Other_ a la valeur **True**. Si plusieurs caractères sont spécifiés, seul le premier caractère de la chaîne est utilisé ; les autres caractères sont ignorés.|
| _FieldInfo_|Facultatif|**Variante**|Tableau contenant des informations redistribuées pour des colonnes individuelles de données. L'interprétation dépend de la valeur de l'argument  _DataType_. Lorsque les données sont délimitées, cet argument est un tableau de tableaux à deux éléments, spécifiant les options de conversion pour une colonne particulière. Le premier élément est le numéro de la colonne (base 1) et le deuxième élément est une des constantes [xlColumnDataType](034f6011-c860-0887-9661-857821f630e4.md) spécifiant comment la colonne est analysée.|
| _DecimalSeparator_|Facultatif|**Variante**|Séparateur des milliers utilisé par Microsoft Excel lors de la reconnaissance des nombres. Le paramètre par défaut est le paramètre du système.|
| _ThousandsSeparator_|Facultatif|**Variante**|Séparateur décimal utilisé par Microsoft Excel lors de la reconnaissance des nombres. Le paramètre par défaut est le paramètre du système.|
| _TrailingMinusNumbers_|Facultatif|**Variante**|Nombres qui commencent par un signe moins.|

### Valeur renvoyée

Variante
 

 

## Remarques

Le tableau suivant contient les résultats de l'importation de texte dans Excel pour divers paramètres d'importation. Les résultats numériques sont affichés dans la colonne la plus à droite.
 

 


|**Séparateur décimal du système**|**Séparateur des milliers du système**|**Valeur de séparateur décimal**|**Valeur de séparateur des milliers**|**Texte d'origine**|**Valeur de la cellule (type de données)**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|Point|Virgule|Virgule|Point|123.123,45|123,123.45 (numérique)|
|Point|Virgule|Virgule|Virgule|123.123,45|123.123,45 (texte)|
|Virgule|Point|Virgule|Point|123.123,45|123,123.45 (numérique)|
|Point|Virgule|Point|Virgule|123.123,45|123 123.45 (texte)|
|Point|Virgule|Point|Espace|123.123,45|123,123.45 (numérique)|

||
|:-----|
|**XlColumnDataType** peut être l'une de ces constantes **XlColumnDataType**.|
|**xlGeneralFormat**. Général|
|**xlTextFormat**. Texte|
|**xlMDYFormat**. Format de date mois, jour, année|
|**xlDMYFormat**. Format de date jour, mois, année|
|**xlYMDFormat**. Format de date année, mois, jour|
|**xlMYDFormat**. Format de date mois, année, jour|
|**xlDYMFormat**. Format de date jour, année, mois|
|**xlYDMFormat**. Format de date année, jour, mois|
|**xlEMDFormat**. Format de date ère, mois, jour|
|**xlSkipColumn**. Non distribuée|
Vous pouvez utiliser la constante  **xlEMDFormat** uniquement si vous avez installé et sélectionné la prise en charge du chinois (Taiwan). La constante **xlEMDFormat** spécifie que les dates d'ères chinoises (Taiwan) sont utilisées.
 

 
Les séparateurs de colonne peuvent être définis dans n'importe quel ordre. Si, parmi les données d'une colonne, il manque un séparateur de colonne particulier, la colonne est redistribuée selon le paramètre  **xlGeneralFormat**. Cet exemple montre comment faire en sorte que la troisième colonne soit ignorée, que la première soit redistribuée sous la forme de texte et que les colonnes restantes des données sources soient redistribuées selon le paramètre **xlGeneralFormat**.
 

 
 `Array(Array(3, 9), Array(1, 2))`
 

 
Si les données sources sont composées de colonnes à largeur fixe, le premier élément de chaque tableau à deux éléments spécifie la position de départ du caractère dans la colonne (sous la forme d'un nombre entier ; le chiffre zéro (0) est le premier caractère). Le deuxième élément du tableau à deux éléments spécifie l'option de redistribution de la colonne sous la forme d'un nombre compris entre un et neuf, comme indiqué ci-dessus.
 

 
L'exemple suivant montre comment redistribuer deux colonnes à partir d'un fichier à largeur fixe. La première colonne commence au début de la ligne et s'étend sur 10 caractères, et la deuxième colonne commence à la position 15 et va jusqu'au bout de la ligne. Pour éviter d'inclure les caractères entre les positions 10 et 15, Microsoft Excel ajoute une entrée pour ignorer la colonne.
 

 
 `Array(Array(0, 1), Array(10, 9), Array(15, 1))`
 

 

## Exemple

Cet exemple montre comment convertir le contenu du Presse-papiers, qui contient un tableau dont le texte est séparé par des espaces, en colonnes distinctes dans la feuille Sheet1. Vous pouvez créer ce type de tableau dans le Bloc-notes ou WordPad (ou dans un autre éditeur de texte), copier le tableau de texte dans le Presse-papiers, basculer vers Microsoft Excel, puis exécuter cet exemple.
 

 

```
Worksheets("Sheet1").Activate 
ActiveSheet.Paste 
Selection.TextToColumns DataType:=xlDelimited, _ 
 ConsecutiveDelimiter:=True, Space:=True
```


## Voir aussi


#### Concepts


 
[Objet Range](b8207778-0dcc-4570-1234-f130532cc8cd.md)
#### Autres ressources


 
[Membres de l'objet Range](4336bf81-1e63-7e44-1792-baf366a027a7.md)