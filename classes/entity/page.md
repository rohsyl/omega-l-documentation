[Retour](../../classes.md)

# La classe `Page`

## Récuprer une instance de page
- Récupérer la page actuelle
    
    Voir la classe [Entity](./entity.md)

- Récupérer une page avec son `$id`
```
    $page = new Page($id);
```

## Que peut on faire avec une instance de la classe `Page` ?
La classe `Page` donne accès à toutes les propriétés suivantes :

Propriété | Description
--- | ---
`$id` | L'id de la page
`$idText` | L'idText de la page
`$title` | Le titre de la page
`$isTitleShown` | Si le titre de la page doit être affiché
`$subtitle` |  Le sous-titre de la page
`$isSubtitleShow` | Si le sous-titre de la page doit être affiché
`$model` | Le template de page utilisé
`$cssTheme` | Le thème CSS utilisé
`$keyword` | Liste de mots clés
`$order` | L'ordre de la page
`$idParent` | L'id de la page parent. `null` si pas de parent
`$idCreator` | L'id de l'utilisateur qui a créer la page
`$isEnabled` | Si la page est activée ou non.
`$idMenu` | L'id du menu utilisé avec cette page
`$created` | La date de création de la page
`$updated` | La date de dernière modification de la page
`$exists` | Si la page existe ou non

La classe `Page` donne accès à toutes les méthodes suivantes :

Méthode | Description
--- | ---
`RenderCssTheme()` | Créer et affiche la balise <link /> pour afficher le thème CSS
`getComponentsViewArray()` | Récupère toutes les vues des composants de la page sous forme d'un `array`
`reload()` | Recharche la page
`isVisibleTitle()` | Si le titre de la page doit être affiché
`isVisibleSubTitle()` | Si le sous-titre de la page doit être affiché

## Méthode statique de la classe `Page`

Méthode | Description
--- | ---
`GetHome($lang = null)` | Retourne l'id de la page d'accueil du site (si `$lang` est défini, retourne la page d'accueil de la langue donnée)
`GetId($url)` | Retourne l'id d'une page selon l'url (l'url vient de la classe `Router`, avec la méthode `getUrl()`)
`GetUrl($id)` | Retourne l'url d'une page selon son id
`GetCorrespondingInLang($id, $langSlug)` | Retourne l'id de la page correspondante à celle donnée (`$id`) dans la langue donnée (`$langSlug`)