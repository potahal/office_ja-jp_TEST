
# Événement TextBox.BeforeUpdate (Access)

L'événement  **BeforeUpdate** se produit avant que les données modifiées d'un contrôle ou d'un enregistrement soient mises à jour.


## Syntaxe

 _expression_. **BeforeUpdate**( ** _Cancel_** )

 _expression_ Variable qui représente un objet **TextBox**.


### Paramètres



|**Nom**|**Obligatoire/Facultatif**|**Type de données**|**Description**|
|:-----|:-----|:-----|:-----|
| _Cancel_|Obligatoire|**Entier**|Le paramètre détermine si l'événement  **BeforeUpdate** se produit. Si vous définissez l'argument _Cancel_ sur **True** (?1), l'événement **BeforeUpdate** est annulé.|

## Remarques

Si vous modifiez les données d'un contrôle à l'aide de Visual Basic ou d'une macro contenant l'action DéfinirValeur, ces événements ne se déclenchent pas pour le contrôle. Cependant, si vous passez ensuite à un autre enregistrement ou si vous sauvegardez l'enregistrement, l'événement  **BeforeUpdate** se déclenche pour le formulaire.

Pour exécuter une macro ou une procédure événementielle lorsque cet événement se produit, définissez la propriété  **BeforeUpdate** sur le nom de la macro ou [Procédure événementielle].

Cet événement ne s'applique pas aux boutons d'options, cases à cocher, boutons bascule d'un groupe d'options. Il s'applique uniquement au groupe d'options lui-même.

L'événement  **BeforeUpdate** est déclenché lorsqu'un contrôle ou un enregistrement est mis à jour. À l'intérieur d'un enregistrement, les données modifiées de chaque contrôle sont mises à jour lorsque le contrôle est désactivé ou lorsque l'utilisateur appuie sur la touche ENTRÉE ou sur la touche de tabulation. Lorsque l'enregistrement est désactivé ou lorsque l'utilisateur clique sur Sauvegarder enregistrement dans le menu Enregistrements, tout l'enregistrement est mis à jour et les données sont enregistrées dans la base de données.

Lorsque vous entrez des données nouvelles ou modifiées dans un contrôle de formulaire pour ensuite passer à un autre enregistrement ou le sauvegarder en cliquant sur  **Sauvegarder enregistrement** du menu **Enregistrements**, l'événement **AfterUpdate** du formulaire se produit immédiatement après l'événement **AfterUpdate** du contrôle. Lorsque vous passez à un autre enregistrement, les événements **Exit** et **LostFocus** se produisent pour le contrôle, suivis de l'événement **Current** pour l'enregistrement auquel vous êtes passé, et des événements **Enter** et **GotFocus** pour le premier contrôle de cet enregistrement. Pour exécuter la macro ou la procédure événementielle **AfterUpdate** sans exécuter les macros ou procédures événementielles **Exit** et **LostFocus**, sauvegardez l'enregistrement avec la commande **Sauvegarder enregistrement** du menu **Enregistrements**.

Les macros et les procédures événementielles  **BeforeUpdate** s'exécutent uniquement si vous modifiez les données d'un contrôle. Cet événement ne se produit pas lorsqu'une valeur change dans un contrôle calculé. Les macros et les procédures événementielles **BeforeUpdate** d'un formulaire s'exécutent uniquement si vous modifiez les données d'un ou de plusieurs contrôles de l'enregistrement.

Pour les formulaires, vous pouvez utiliser l'événement  **BeforeUpdate** pour annuler la mise à jour d'un enregistrement avant de passer à un autre enregistrement.

Si l'utilisateur entre une nouvelle valeur dans le contrôle, le paramètre de la propriété  **OldValue** ne change qu'une fois les données sauvegardées (l'enregistrement mis à jour). Si vous annulez une mise à jour, la valeur dans le contrôle est remplacée par la valeur de la propriété **OldValue**.

L'événement AvantMAJ est fréquemment utilisé pour valider des données, notamment lors de validations complexes, telles que :


- Celles qui sont liées à des conditions qui dépendent de plusieurs valeurs dans le formulaire.
    
- Celles qui affichent des messages d'erreur différents en fonction des données entrées.
    
- Celles que l'utilisateur peut supplanter.
    
- Celles qui contiennent des références à des contrôles d'autres formulaires ou des fonctions définies par l'utilisateur.
    

 **Remarque**  Pour effectuer des validations simples ou des validations plus complexes exigeant la valeur d'un champ ou validant plusieurs contrôles d'un formulaire, vous pouvez utiliser la propriété  **ValidationRule** pour les contrôles et les propriétés **ValidationRule** et **Required** pour les champs et les enregistrements des tables.

Une erreur d'exécution se produit si vous essayez de modifier les données se trouvant dans le contrôle qui a déclenché l'événement  **BeforeUpdate** dans la procédure de l'événement.


## Exemple

L'exemple suivant montre comment utiliser une procédure événementielle  **BeforeUpdate** pour contrôler si un nom de produit a déjà été entré dans la base de données. Après que l'utilisateur a tapé un nom de produit dans la zone ProductName (Nom du produit), la valeur est comparée avec le champ ProductName (Nom du produit) de la table Products (Produits). Si cette table contient une valeur correspondante, un message avertit l'utilisateur que le produit a déjà été entré.

Pour essayer l'exemple, ajoutez la procédure événementielle suivante à un formulaire nommé Products contenant une zone de texte appelée ProductName.




```
Private Sub ProductName_BeforeUpdate(Cancel As Integer) 
 If(Not IsNull(DLookup("[ProductName]", _ 
 "Products", "[ProductName] ='" _ 
 &amp; Me!ProductName &amp; "'"))) Then 
 MsgBox "Product has already been entered in the database." 
 Cancel = True 
 Me!ProductName.Undo 
 End If 
End Sub
```


## Voir aussi


#### Concepts


[Objet TextBox](d74fbe9a-0d40-7d28-956f-a2bfd0cfee45.md)
#### Autres ressources


[Membres de l'objet TextBox](bb55abbc-902e-fc2d-bdff-063c55426cd0.md)