[Retour](../classes.md)

# La classe `Config`

La classe `Config` gère les entrées dans la table `om_config` dans la base de données.

Cette classe donne accès aux méthodes statiques suivantes:

Méthode | Description
--- | ---
`get($key)` | Retourne la valeur correspondant à la `$key` donnée. Retourne `null` si la clé n'existe pas.
`set($key, $value)` | Met à jour ou crée l'entrée dans la table `om_config`.