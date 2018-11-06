[Retour](../../classes.md)

# La classe `Menu`
La classe `Menu` menu permet d'afficher un menu.

Cette classe donne accès à différentes méthodes :

Méthode | Description
--- | ---
`getPublic()` | Retourne un menu qui correspond directement à la liste des pages comme dans le back-office
`getMenuById($id)` | Retourne un menu par son `$id`
`getBySecurity()` | Retourne un menu selon le membergroup défini pour ce menu. Si un membre est connecté, selon le membergroup dans lequel se trouve ce membre
`setMenuHtmlStruct($menuHtmlStruct)` | Définir la structure HTML du menu

## Exemple de `setMenuHtmlStruct()`
```
Entity::Menu()->setMenuHtmlStruct(array(
    'ul_main' => '<ul class="nav navbar-nav navbar-right">%1$s</ul>',
    'li_nochildren' => '<li class="nav-item-%3$s"><a href="%1$s">%2$s</a></li>',
    'li_nochildrenactiv' => '<li class="nav-item-%3$s active"><a href="%1$s">%2$s</a></li>',
    'li_children' => '<li class="dropdown nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown" role="button" >%2$s <span class="caret"></span></a>%4$s</li>',
    'ul_children' => '<ul class="dropdown-menu">%1$s</ul>',
    'li_childrenactiv' => '<li class="dropdown nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown">%2$s</a>%4$s</li>'
));
```
> Cet exemple utilise la structure de menu de [Bootstrap v3](https://getbootstrap.com/docs/3.3/components/#navbar). Donc hésite pas à copier-coller si tu en a besoin.

Le code ci dessus génèrera un menu comme cela :
```
<!-- ul_main -->
<ul class="nav navbar-nav navbar-right"> 

    <!-- li_nochildrenactiv -->
    <li class="nav-item-home active"><a href="/home">Home</a></li>
    
    <!-- li_nochildren -->
    <li class="nav-item-about"><a href="/about">About</a></li>
    
    <!-- li_children -->
    <li class="dropdown nav-item-details">
        <a href="/details" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Détails <span class="caret"></span></a>
        
        <!-- ul_children -->
        <ul class="dropdown-menu">
        
            <!-- li_nochildren -->
            <li class="nav-item-page1"><a href="/page1">Page 1</a></li>
        
            <!-- li_nochildren -->
            <li class="nav-item-page2"><a href="/page2">Page 2</a></li>
        </ul>
    </li>
</ul>
```
