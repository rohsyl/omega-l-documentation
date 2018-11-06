[Retour](../../classes.md)

# La classe `ModuleArea`
La classe `ModuleArea` permet de gérer les modulearea

Les méthodes statiques suivantes sont accessibles:

Méthode | Description
--- | ---
`Create($name, $theme)` | Créer un modulearea. 
`Delete($name, $theme)` | Supprimer un modulearea
`Exists($name, $theme)` | Vérifier si un modulearea existe
`Display($page, $name, $theme)` | Afficher le modulearea et les modules qu'il contient

>`$name` est le nom du modulearea et `$theme` est le nom du thème auquel le modulearea est lié

> `$page` est une instance de la classe [Page](./page.md)