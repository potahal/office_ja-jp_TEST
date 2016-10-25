
# Événement TextBox.Change (Access)

L'événement  **Change** se produit lorsque le contenu du contrôle spécifié est modifié.


## Syntaxe

 _expression_. **Change**

 _expression_ Variable qui représente un objet **TextBox**.


## Remarques

Cet événement se produit notamment quand vous entrez un caractère directement dans une zone de texte ou dans la zone de liste déroulante, ou encore lorsque vous modifiez la valeur de la propriété  **Text** du contrôle à l'aide d'une macro ou de Visual Basic.


 **Remarque**  La définition de la valeur d'un contrôle à l'aide d'une macro ou de Visual Basic ne déclenche pas cet événement pour le contrôle. Vous devez entrer directement les données dans le contrôle ou définir sa propriété  **Text**.

Pour exécuter une macro ou une procédure événementielle lorsque cet événement se produit, attribuez le nom de la macro ou [Procédure événementielle] à la propriété  **OnChange**.

L'exécution d'une macro ou d'une procédure événementielle associée à un événement Change vous permet de coordonner l'affichage des données dans les contrôles. Vous pouvez également afficher des données ou une formule dans un contrôle et leur résultat dans un autre.

L'événement Change ne se produit pas lorsqu'une valeur change dans un contrôle calculé.


 **Remarque**  événement en cascade. C'est le cas lorsqu'une macro ou une procédure événementielle, exécutée en réponse à l'événement Change du contrôle, modifie le contenu de ce dernier (par exemple, lorsque la valeur de la propriété qui spécifie la valeur du contrôle, telle la propriété  **Text** d'une zone de texte, est modifiée. Pour empêcher qu'un événement en cascade ne se produise :


- Évitez, si possible, d'attacher à un contrôle une macro ou une procédure événementielle Change susceptible de modifier le contenu de celui-ci.
    
- Évitez de créer plusieurs contrôles dont les événements Change agissent les uns sur les autres, par exemple, deux zones de texte qui se mettent à jour mutuellement.
    
La modification des données d'une zone de texte ou d'une zone de liste déroulante à l'aide du clavier déclenche des événements de clavier, en plus des événements de contrôle tels que l'événement Change. Par exemple, si vous vous placez sur un nouvel enregistrement et que vous tapez un caractère ANSI dans une zone de texte de l'enregistrement, les événements suivants se produisent dans cet ordre :

 **KeyDown** → **KeyPress** → **BeforeInsert** → **Change** → **KeyUp**

Les événements  **BeforeUpdate** et **AfterUpdate** de la zone de texte ou de la zone de liste déroulante se produisent dès que vous avez entré les données nouvelles ou modifiées dans le contrôle et que vous vous êtes placé sur un autre contrôle (ou que vous avez cliqué sur **Sauvegarder enregistrement** dans le menu **Enregistrements** ), et donc après tous les événements Change du contrôle.

L'événement  **NotInList** a lieu pour les zones de liste déroulante pour lesquelles la valeur Oui est attribuée à la propriété **LimitToList**, lorsque vous entrez une valeur qui ne figure pas dans la liste et que vous essayez de vous placer sur un autre contrôle ou de sauvegarder l'enregistrement. Dans ce cas, les événements BeforeUpdate et AfterUpdate de la zone de liste déroulante ne se produisent pas, parce que Microsoft Access n'accepte aucune valeur ne figurant pas dans la liste.


## Voir aussi


#### Concepts


[Objet TextBox](d74fbe9a-0d40-7d28-956f-a2bfd0cfee45.md)
#### Autres ressources


[Membres de l'objet TextBox](bb55abbc-902e-fc2d-bdff-063c55426cd0.md)