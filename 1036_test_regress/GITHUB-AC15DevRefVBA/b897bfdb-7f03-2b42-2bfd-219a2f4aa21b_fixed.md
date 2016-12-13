
# DoCmd.DoMenuItem, méthode (Access)

Affiche la commande de menu ou de barre d'outils appropriée pour Microsoft Access.
 


## Syntaxe

*expression* . **DoMenuItem**( ***MenuBar***, ***MenuName***, ***Command***, ***Subcommand***, ***Version*** )
 

 
*expression* Variable représentant un objet **DoCmd**.
 

 

### Paramètres



|**Nom**|**Obligatoire/Facultatif**|**Type de données**|**Description**|
|:-----|:-----|:-----|:-----|
| _MenuBar_|Obligatoire|**Variante**|Utilisez la constante intrinsèque  **acFormBar** pour la barre de menus en mode formulaire. Pour d'autres modes, utilisez le numéro du mode dans la liste des arguments de la barre de menus, comme indiqué dans la fenêtre Macro dans les versions antérieures de Microsoft Access (comptez à partir de 0 dans la liste).|
| _MenuName_|Obligatoire|**Variante**|Vous pouvez utiliser l'une des constantes intrinsèques suivantes. **acFile** **acEditMenu** **acRecordsMenu**Vous pouvez utiliser  **acRecordsMenu** uniquement pour la barre de menus en mode formulaire dans les bases de données Microsoft Access version 2.0 et Microsoft Access 95. Pour d'autres menus, utilisez le numéro du menu dans la liste d'arguments Nom menu, comme indiqué dans la fenêtre Macro dans les versions précédentes de Microsoft Access (nombre vers le bas de la liste, à partir de 0).|
| _Command_|Obligatoire|**Variante**|Vous pouvez utiliser l'une des constantes intrinsèques suivantes. **acNew** **acSaveForm** **acSaveFormAs** **acSaveRecord** **acUndo** **acCut** **acCopy** **acPaste** **acDelete** **acSelectRecord** **acSelectAllRecords** **acObjacRefreshect**Pour d'autres commandes, utilisez le numéro de la commande dans la liste de l'argument commande, comme indiqué dans la fenêtre Macro dans les versions antérieures de Microsoft Access (comptez à partir de 0 dans la liste).|
| _Subcommand_|Facultatif|**Variante**|Vous pouvez utiliser l'une des constantes intrinsèques suivantes. **acObjectVerb** **acObjectUpdate**La constante  **acObjectVerb** représente la première commande dans le sous-menu de la commande **Objet** du menu **Edition**. Le type de l'objet détermine la première commande du sous-menu. Par exemple, cette commande est Edition pour un objet Paintbrush qui peut être modifié.Pour d'autres commandes de sous-menus, utilisez le numéro de la sous-commande dans la liste de l'argument souscommande, comme indiqué dans la fenêtre Macro dans les versions antérieures de Microsoft Access (comptez à partir de 0 dans la liste).|
| _Version_|Facultatif|**Variante**|Utilisez la constante intrinsèque  **acMenuVer70** pour le code écrit pour les bases de données Microsoft Access 95, la constante intrinsèque **acMenuVer20** pour le code écrit pour les bases de données Microsoft Access version 2.0 et la constante intrinsèque **acMenuVer1X** pour le code écrit pour les bases de données Microsoft Access version 1.x. Cet argument est disponible uniquement dans Visual Basic. <br>**Remarque**   La valeur par défaut pour cet argument est **acMenuVer1X** de sorte que tout code écrit pour les bases de données Microsoft Access version 1.x s'exécute normalement. Si vous écrivez du code pour une base de données Microsoft Access 95 ou version 2.0 et que vous souhaitez utiliser les commandes de menu Microsoft Access 95 ou version 2.0 avec la méthode **DoMenuItem**, vous devez définir cet argument sur **acMenuVer70** ou **acMenuVer20**.
 

En outre, lorsque vous faites défiler les listes des arguments d'action de Barre de menus, Nom de menu, Commande et Sous-commande dans la fenêtre Macro pour obtenir les numéros à utiliser pour les arguments dans la méthode  **DoMenuItem**, vous devez utiliser les listes de Microsoft Access 95 si l'argument Version est **acMenuVer70**, les listes de Microsoft Access version 2.0 si l'argument Version est Version et les listes Microsoft Access version 1.x si la Version est **acMenuVer1X** (ou vide). 
**Remarque**  Pas de paramètre  **acMenuVer80** pour cet argument. Vous ne pouvez pas utiliser la méthode **DoMenuItem** pour afficher les commandes Access (bien que les méthodes **DoMenuItem** existantes du code Visual Basic continuent à fonctionner). Utilisez plutôt la méthode **ExécuterCommande**.
 

|

## Remarques


 **Remarque**  Dans Microsoft Access 97 et version ultérieure, la méthode  **DoMenuItem** avait été remplacée par la méthode **[RunCommand](2731352f-7f2d-db3a-314c-e8a789755dd5.md)**. La méthode **DoMenuItem** figure dans cette version de Microsoft Access uniquement par souci de compatibilité avec les versions antérieures. Lorsque vous exécutez du code Visual Basic existant contenant une méthode **DoMenuItem**, Microsoft Access affiche la commande de menu ou de barre d'outils appropriée pour Microsoft Access 2000. Toutefois, contrairement à l'action ExécuterElémentMenu dans une macro, une méthode **DoMenuItem** dans Visual Basic n'est pas convertie en une méthode **RunCommand** lorsque vous convertissez une base de données créée dans une version antérieure de Microsoft Access.
 

Certaines commandes de versions antérieures de Microsoft Access ne sont pas disponibles dans Access et les méthodes  **DoMenuItem** qui exécutent ces commandes provoquent une erreur lorsqu'elles sont exécutées dans Visual Basic. Vous devez modifier votre code Visual Basic pour remplacer ou supprimer les occurrences de telles méthodes **DoMenuItem**.
 

 
Les sélections dans les listes d'arguments d'actions relatives au nom de menu, à la commande et à la sous-commande dans la fenêtre Macro dépendent de ce que vous avez sélectionné pour les arguments précédents. Vous devez utiliser des nombres ou des constantes intrinsèques qui sont appropriés pour chaque argument BarreMenus, NomMenu, Commande et SousCommande.
 

 
Si vous laissez l'argument Souscommande vierge mais que vous spécifiez l'argument version, vous devez inclure la virgule de l'argument souscommande. Si vous laissez les arguments Souscommande et Version vierges, n'utilisez pas de virgule à la suite de l'argument Commande.
 

 

## Voir aussi


#### Concepts


 
[Objet DoCmd](3ce44cca-9979-0a1e-9787-079a52ce528f.md)
#### Autres ressources


 
[Membres de l'objet DoCmd](3e7ade9e-86e4-0751-188b-5d31c9101651.md)