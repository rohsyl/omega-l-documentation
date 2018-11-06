[Retour](../classes.md)

# La classe `Message`

La classe `Message` permet d'afficher des messages dans la page. Utile **uniquement** dans la partie back-office!

Cette classe met a disposition les méthodes suivantes :

Méthode | Description
--- | ---
`success($message)` | Stock dans la variable `$_SESSION` un message de succès
`info($message)` | Stock dans la variable `$_SESSION` un message d'information
`error($message)` | Stock dans la variable `$_SESSION` un message d'erreur
`warning($message)` | Stock dans la variable `$_SESSION` un message d'attention
`displayAll()` | Affiche tous les messages stockés dans la variable `$_SESSION`

> La librarie jquery.growl.js est utilisée pour afficher les messages.