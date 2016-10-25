
# PrintOut, méthode

La méthode  **PrintOut** exécute l'action Imprimer dans Visual Basic.


## Syntaxe

 _expression_. **PrintOut**( ** _PrintRange_**, ** _PageFrom_**, ** _PageTo_**, ** _PrintQuality_**, ** _Copies_**, ** _CollateCopies_** )

 _expression_ Variable représentant un objet **DoCmd**.


### Paramètres


|
|

## Remarques

Faites appel à la méthode PrintOut pour imprimer l'objet actif dans la base de données ouverte. Vous pouvez imprimer des feuilles de données, des rapports, des formulaires, des page d'accès aux données et des modules.


## Exemple

Cet exemple imprime deux copies triées des quatre premières pages de la feuille de données ou du formulaire actif&amp;nbsp:


```
DoCmd.PrintOut acPages, 1, 4, , 2
```

