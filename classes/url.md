[Retour](../classes.md)

# La classe `Url`

Cette classe donne accès aux méthodes statiques suivantes:

Méthode | Description
--- | ---
`Link($link, $param, $id)` | Crée un lien. `$link` est le lien vers une page. `$param` est un `array` qui sera transformer en querystring. `$id` sera ajouté à la fin du lien `#id`.
`Combine()` | Combine des partie d'URL.
`CombAndAbs()` | Combine des parties d'URL et rend l'URL absolu mais sans le nom de domaine.
`Action($controller, $action, $param = null)` | Crée l'URL vers le `$controller` et l'`$action` spécifiée (**Uniquement** pour la partie back-office). `$param`  est un `array` qui sera transformer en querystring.
`Absolute($url)` | Rend l'URL absolu mais sans le nom de domaine.
