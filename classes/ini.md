[Retour](../classes.md)

# La classe `Ini`

La classe `Ini` gère les fichiers `.ini` se trouvant dans le dossier `config/`

Cette classe donne accès aux méthodes statiques suivantes:

Méthode | Description
--- | ---
`get($key)` | Retourne la valeur correspondant à la `$key` donnée. Retourne `null` si la clé n'existe pas.
`set($key, $value)` | Met à jour ou crée l'entrée dans la table `om_config`.

> Remarque la `$key` se construit de la manière suivante : [filename].[section].[key]

Exemple:
> Pour acceder à la clé `host` de la section `database` du fichier `omega.ini`
```
$value = Ini::Get('omega.database.host');
```