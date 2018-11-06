[Retour](../classes.md)

# La classe `Html`

Cette classe met a disposition les méthodes suivantes :


Méthode | Description
--- | ---
`partial( $path )` | `include()` d'une vue (`.php`) dans la page. Le `$path` doit être relatif par rapport à la racine de OmegaCMS
`requireMainJs( $path )` | Inclut un fichier `.js`
`requireMainCss( $path )` | Inclut un fichier `.css`
`requireJs( $path )` | Inclut un fichier `.js`
`requireCss( $path )` | Inclut un fichier `.css`
`renderJs()` | Affiche les balises `<script>` pour tous les fichiers `.js` qui ont été inclut avec les méthodes `requireJs()` et `requireMainJs()`.
`renderCss()` | Affiche les balises `<Link />` pour tous les fichiers `.css` qui ont été inclut avec les méthodes `requireCss()` et `requireMainCss()`.

> Remarque : Les `$path` vers les fichiers `.js` ou `.css` sont relatifs par rapport à la racine de OmegaCMS.

Quelle est la différence entre `requireMainJs()` et `requireJs()` ?
    
> Les fichiers inclut avec `requireMainJs()` vont être affiché avant lors du `renderJs()`.

Exemple:
```
<html>
    <head>
    <?php
        Html::requireJs('path/to/file.js');
        Html::requireMainJs('path/to/jquery.js');
        Html::requireJs('path/to/another/file.js');
        Html::requireMainJs('path/to/main.js');
    
        Html::renderJs();
    ?>
    </head>
    ...
```

Le code ci-dessus va donner le résultat suivant:
```
<html>
    <head>
        <script language="JavaScript" src="path/to/jquery.js"></script>
        <script language="JavaScript" src="path/to/main.js"></script>
        <script language="JavaScript" src="path/to/file.js"></script>
        <script language="JavaScript" src="path/to/another/file.js"></script>
    </head>
    ...
```

