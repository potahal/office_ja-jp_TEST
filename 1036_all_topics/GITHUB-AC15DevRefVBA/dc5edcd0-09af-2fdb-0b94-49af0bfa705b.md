
# Méthode TextBox.SetFocus (Access)

La méthode  **SetFocus** active le formulaire spécifié, le contrôle spécifié du formulaire actif ou le champ spécifié de la feuille de données active.


## Syntaxe

 _expression_. **SetFocus**

 _expression_ Variable qui représente un objet **TextBox**.


### Valeur renvoyée

Nothing


## Remarques

Vous pouvez utiliser la méthode  **SetFocus** pour activer un champ ou un contrôle particulier de manière à ce que toutes les données entrées par l'utilisateur soient dirigées vers cet objet.

Pour lire certaines propriétés d'un contrôle, assurez-vous que celui-ci est activé. Par exemple, une zone de texte doit être activée pour que vous puissiez lire sa propriété  **Text**.

D'autres propriétés ne peuvent être définies que lorsqu'un contrôle n'est pas activé. Par exemple, vous ne pouvez pas attribuer la valeur  **False** (0) aux propriétés **Visible** et **Enabled** d'un contrôle si ce dernier est activé.

Vous pouvez aussi utiliser la méthode  **SetFocus** pour vous déplacer dans un formulaire sous certaines conditions. Par exemple, si l'utilisateur sélectionne **Non applicable** pour répondre à la première question d'un groupe de questions sur un formulaire présenté sous forme de questionnaire, il est possible que le code Visual Basic ignore automatiquement les questions de ce groupe et active le premier contrôle du prochain groupe de questions.

Seuls les contrôles ou les formulaires visibles peuvent être activés. Les formulaires et les contrôles ne sont pas visibles tant que l'événement  **Load** n'est pas terminé. Par conséquent, si vous utilisez la méthode **SetFocus** dans l'événement Load d'un formulaire afin d'activer ce dernier, la méthode **Repaint** doit être appliquée avant la méthode **SetFocus**.

Vous ne pouvez pas activer un contrôle si sa propriété  **Enabled** a la valeur **False**. Vous devez affecter à la propriété **Enabled** la valeur **True** (-1) avant de pouvoir activer ce contrôle. Cependant, vous pouvez activer ce dernier si la propriété **Locked** a la valeur **True**.

Si un formulaire contient des contrôles dont la valeur de la propriété  **Enabled** est **True**, il ne peut être lui-même activé. Seuls les contrôles de ce formulaire peuvent être activés. Dans ce cas, si vous tentez d'utiliser **SetFocus** pour activer un formulaire, c'est le dernier contrôle actif du formulaire qui est activé.

Utilisez la méthode  **SetFocus** pour activer un sous-formulaire, lequel représente un type de contrôle. Vous pouvez aussi activer un contrôle d'un sous-formulaire en utilisant deux fois la méthode **SetFocus**, pour activer d'abord le sous-formulaire, puis le contrôle de ce dernier.


## Exemple

Dans l'exemple suivant, la méthode  **SetFocus** active une zone de texte EmployeeID (RéfEmployé) d'un formulaire Employees (Employés) :


```
Forms!Employees!EmployeeID.SetFocus
```


## Voir aussi


#### Concepts


[Objet TextBox](d74fbe9a-0d40-7d28-956f-a2bfd0cfee45.md)
#### Autres ressources


[Membres de l'objet TextBox](bb55abbc-902e-fc2d-bdff-063c55426cd0.md)