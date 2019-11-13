[Retour](../../classes.md)

# The class `Menu`
Namespace: `Omega\Utils\Entity\Menu`.

The `Menu` class allow you to display the menu in a theme.

# How to get access to this class

`Entity::Menu()` give you an instance of this class. It should be used only in themes.

# Methods
This class give you access to some methods.

Method | Description
--- | ---
`getMenuById($id)` | Get the menu html by menu `$id`
`getBySecurity()` | Get menu by membergroup and lang. If a member is connected, it will take the matching menu.
`getAsArray($id = null)` | Get the menu as array. If `$id` isn't given it will work the same as `getBySecurity()`.
`getLangaugeMenuAsArray()` | Get an array with the language menu link and title.
`PrepareUrl($url, àlang = null)` | Ensure an URL is well formatted to be displayed in the menu.
`setMenuHtmlStruct($menuHtmlStruct)` | Define the HTML structure of the menu.

# Exemples

Here is some examples of methods described above.

## Exemple of `setMenuHtmlStruct()`

The `setMenuHtmlStruct()` method allow you to set the html structure of your memu. Usually menu item are `<li class="menu-item">` tag placed in a `<ul id="menu">` and sub-menu item are `<li>` in a `<ul>` both inside `.menu-item`.

Here is an exemple of a menu structure :
```
Entity::Menu()->setMenuHtmlStruct([
    'ul_main' => ' <ul class="links">%1$s</ul>',
    'li_nochildren' => '<li class="nav-item-%3$s"><a href="%1$s" %4$s>%2$s</a></li>',
    'li_nochildrenactiv' => '<li class="nav-item-%3$s active"><a href="%1$s" %4$s>%2$s</a></li>',
    'li_children' => '<li class="nav-item-%3$s"><a href="%1$s" %5$s >%2$s <span class="caret"></span></a>%4$s</li>',
    'ul_children' => '<ul>%1$s</ul>',
    'li_childrenactiv' => '<li class="nav-item-%3$s"><a href="%1$s" %5$s>%2$s</a>%4$s</li>'
    ]);
```

- `ul_main` : The menu
    - `%1$s` : Is a placeholder that will be replaced by all menu items (`li_nochildren`, `li_nochildrenactiv`, `li_children`, `li_childrenactiv`)
- `li_nochildren` : Menu item (used for sub-menu item too)
    - `%1$s` : The URL
    - `%2$s` : The title of the page
    - `%3$s` : The slug (the title in lowercase, with no special char and no space)
    - `%4$s` : The `target="_blank"`attribute if the page needs to be opened in a new tab
- `li_nochildrenactiv` : Active menu item  (used for active sub-menu item too)
    - `%1$s` : The URL
    - `%2$s` : The title of the page
    - `%3$s` : The slug (the title in lowercase, with no special char and no space)
    - `%4$s` : The `target="_blank"`attribute if the page needs to be opened in a new tab
- `li_children` : Menu item with a sub-menu
    - `%1$s` : The URL
    - `%2$s` : The title of the page
    - `%3$s` : The slug (the title in lowercase, with no special char and no space)
    - `%4$s` : The submenu (`ul_children`)
    - `%5$s` : The `target="_blank"`attribute if the page needs to be opened in a new tab
- `ul_children` : Sub-menu
    - `%1$s` : Is a placeholder that will be replaced by all sub menu items (`li_nochildren`, `li_nochildrenactiv`)
- `li_childrenactiv` : Active menu item with a sub-menu
    - `%1$s` : The URL
    - `%2$s` : The title of the page
    - `%3$s` : The slug (the title in lowercase, with no special char and no space)
    - `%4$s` : The submenu (`ul_children`)
    - `%5$s` : The `target="_blank"`attribute if the page needs to be opened in a new tab

The HTML structure above will generate a menu similar to this :
```
<!-- ul_main -->
<ul class="links">

    <!-- li_nochildrenactiv -->
    <li class="nav-item-home active"><a href="/home">Home</a></li>
    
    <!-- li_nochildren -->
    <li class="nav-item-about"><a href="/about">About</a></li>
    
    <!-- li_children -->
    <li class="nav-item-details">
        <a href="/details" >Details <span class="caret"></span></a>
        
        <!-- ul_children -->
        <ul>
        
            <!-- li_nochildren -->
            <li class="nav-item-page1"><a href="/page1">Page 1</a></li>
        
            <!-- li_nochildren -->
            <li class="nav-item-page2"><a href="/page2">Page 2</a></li>
        </ul>
    </li>
</ul>
```

## Exemple of `getLangaugeMenuAsArray()`

```
@php
        $lang_menu = Entity::Menu()->getLangaugeMenuAsArray()
    @endphp
    @if(sizeof($lang_menu) > 0)
         <li class="nav-item dropdown d-none d-lg-block language-p">
            <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#"><img src="{{ asset($lang_menu['selected_lang']->media->path) }}"/> {!! $lang_menu['selected_lang']->name !!}</a>
            <div class="dropdown-menu dropdown-menu-right language-p-dropdown">
                @foreach($lang_menu['menu'] as $item)
                    <a class="dropdown-item" href="{!! $item['url'] !!}"><img src="{{ asset($item['lang']->media->path) }}"/>  {!! $item['title'] !!}</a>
                @endforeach
            </div>
        </li>
    @endif
```

## Exemple of `getAsArray()`

```
@php
    $menu = Entity::Menu()->getAsArray();
@endphp
<div class="col col-xs-4 menu-p-col">
    <div>
    @php
    if (isset($menu['menu']){
        echo '<ul class="list-group list-group-flush">';
        foreach($menu['menu'] as $item){

            $target = '';
            $icon = '';

            if (isset($item->isnewtab) && $item->isnewtab == 1) {
                $target = 'target="_blank"';
                $icon = '<i class="fa fa-external-link"></i>';
            }

            echo '<li class="menu-p-child"><a class="list-group-item list-group-item-action menu-p-child-content" ' . $target . ' href="' . Entity::Menu()->PrepareUrl($item->url, $menu['lang']) . '">' . $item->title . ' ' . $icon . '</a></li>';
        }
        echo '</ul>';
    }
    @endphp
    </div>
</div>
```
