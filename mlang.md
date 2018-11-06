[Plugin](./plugin.md) -
[Install](./install.md) - 
[Classes](./classes.md) - 
[JavaScript](./js.md) - 
[Multi-langue](./mlang.md) - 
[Theme](./theme.md)

# Le système multi-langue de OmegaCMS

OmegaCMS gère le multilangue dans la partie front-end mais aussi dans la partie back-office.

## Partie front-end

Pour activer le multi-langue, il faut se rendre dans la section Paramètre, onglet Front-end langugages.

Dans cette page, vous pouvez activer le multi-langue, choisir la langue par défaut et gérer les langues disponibles

### Ajouter une nouvelle langues

Dans les paramètres de OmegaCMS (onglet Front-end languages), vous pouvez ajoutez et éditer les langues pour la partie front-end.

Vous pouvez aussi choisir la langues par défaut du front-end.

### Traduction des textes "in-code"

Les fichiers de traductions pour les blabla écrit directement dans le code se trouvent dans `assets/i18n/frontoffice/`

Il y a un fichier `.json` par langue.

Le contenu du fichier `en.json` ressemble à ça : 
```
{
    "Login":"Login",
    "Mail":"Mail",
    "Password twice":"Password twice",
    ...
}
```
> Tiens, c'est bizzard, c'est écrit deux fois la même chose sur chaque ligne...

He bien c'est normal !!

Pour afficher un texte et qu'il soit affiché dans les bonnes langues, il existe la fonction : `oo($key);`

En paramètre de cette fonction, on passe directement le texte en **anglais**. 

```
use function \Omega\Library\oo;
oo('Login');
```

> Remarque : Si la traduction n'existe pas dans le fichier `en.json`, elle sera créée automatiquement (Uniquement pour l'anglais).
Si la traduction n'existe pas lorsque on est en `fr`, la fonction va afficher la traduction anglaise entre crochet pour dire que 
il n'est as pas d'équivalent en `fr`. Par exemple : `[Login]`

- Comment ajouter les traductions dans les autres langues ?

    Il suffit de créer un fichier `fr.json`. Et de copier-coller le contenu du fichier `en.json`.
    
    ```
    {
        "Login":"Login",
        "Mail":"Mail",
        "Password twice":"Password twice",
        ...
    }
    ```
    
    > Et ensuite de mettre les valeurs en francais.
    
    ```
    {
        "Login":"Identifiant",
        "Mail":"E-mail",
        "Password twice":"Mot de passe, deux fois",
        ...
    }
    ```
    
    Si le fichier existe déjà, il faut juste ajouter des lignes dans le fichier `fr.json`
    
    > Astuce : Si après modification d'un fichier de langue, toutes les traductions s'affiche en mode `[Login]`, c'est qu'il y a une erreur dans la structure du fichier `.json`.
    
### Traduction du contenu du site

TODO



## Partie back-office

Les fichiers de traductions pour les blabla écrit directement dans le code se trouvent dans `assets/i18n/backoffice/`

### Traduction du back-office

Pareil que `oo()` mais sauf que la fonction s'appel `__()`.