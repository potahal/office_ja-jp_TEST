
# Cells, propriété

Renvoie un objet Range qui représente les cellules dans la plage spécifiée, telles qu'elles s'appliquent à l'objet Range. Renvoie également un objet Range qui représente toutes les cellules de la feuille de données (pas seulement les cellules en cours d'utilisation), telles qu'elles s'appliquent à l'objet DataSheet. Objet Range en lecture seule.

 _expression_. **Cells**

 _expression_ Obligatoire. Expression qui renvoie un des objets répertoriés dans la liste S'applique à.


## Exemples

Cet exemple montre comment effacer la formule de la cellule A1 de la feuille de données. Notez que, sur la feuille de données, la colonne A est la deuxième colonne, et la ligne 1 la deuxième ligne.


```
myChart.Application.DataSheet.Cells(2,2).ClearContents
```

Cet exemple montre comment effectuer une boucle sur les cellules A1 à I3 de la feuille de données. Si l'une d'entre elles contient une valeur inférieure à 0.001, l'exemple remplace cette valeur par 0 (zéro).




```
Set mySheet = myChart.Application.DataSheet 
For rwIndex = 2 to 4 
 For colIndex = 2 to 10 
 If mySheet.Cells(rwIndex, colIndex) < .001 Then 
 mySheet.Cells(rwIndex, colIndex).Value = 0 
 End If 
 Next colIndex 
Next rwIndex
```

