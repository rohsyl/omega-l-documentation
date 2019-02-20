[Plugin](./plugin.md) -
[Install](./install.md) - 
[Classes](./classes.md) - 
[JavaScript](./js.md) - 
[Multi-langue](./mlang.md) - 
[Theme](./theme.md)

# Create a theme
In this section, you will learn how to create a theme for Omega-L

First, you have to choose a name for your theme (slugified).

In this guide, we will name our theme `my-theme`.

> Don't forget to replace every occurence of `my-them` by the name of your theme.


## Theme hierarchy
```
/omega/theme/my-theme
        /assets
            /css
            /js
        /template
            /register.php
        /install
            /install.php
        /template
        /header.blade.php
        /index.blade.php
        /footer.blade.php
```

> You will had to put all your assets (js and css) in the `assets` directory. (Mandatory)


### The `install.php` file

This file allow you give all information to the theme installer.

```
<?php
use Omega\Facades\ModuleArea;
use Omega\Utils\Theme\Installer;

/**
 * @return Installer
 */
return Installer::For('my-theme')
    ->set([
        'title' => 'My theme',
        'description' => 'Description',
        'website' => 'http://rohs.ch',
        'colors' => ['#000000', '#ffffff', '#a6a6a6']
    ])
    ->postInstall(function($name){
        ModuleArea::Create('footer', $name);
        ModuleArea::Create('sidebar', $name);
    });

```
> Replace `my-theme` with the name of your theme.

Here you have to set the title, the description, the URL of your website and some colors.

The `postInstall` method is called after the theme is installed. You can put any code you want in there. It's also used to create the ModuleArea (see the [Modulearea](#modulearea) section below)

### The `header.php` file

This file contains the `<head>` of your theme.

```
<!DOCTYPE HTML>
<html lang="{{ Entity::LangSlug() }}">
<head>

    <title>{{ Entity::Site()->name }} - {{ Entity::Page()->name }}</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    {!! OmegaUtils::renderMeta() !!}

    <!-- OmegaCMS assets -->
    {!! OmegaUtils::renderOmegaAssets() !!}

    <!-- Styles -->
    <link href="{{ theme_asset('css/main.css') }}" rel="stylesheet"/>

    <!-- Scripts -->

    <script src="{{ theme_asset('js/jquery.scrollex.min.js') }}"></script>
    <script src="{{ theme_asset('js/skel.min.js') }}"></script>
    <script src="{{ theme_asset('js/util.js') }}"></script>
    <script src="{{ theme_asset('js/main.js') }}"></script>

    {!! OmegaUtils::renderDependencies() !!}
</head>
```
> cf. [`Entity`](./classes/entity/entity.md)
> cf. [`OmegaUtils`](./classes/omegautils.md)
> cf. [`theme_asset()`](./helpers/theme_asset.md)


### The `index.php` file

This file contains the content of your page. It also include the footer file.

```
<body class="subpage">

<!-- Header -->
<header id="header">
    <div class="logo"><a href="{{ url('/') }}">{{ Entity::Site()->name }}</a></div>
    <a href="#menu">Menu</a>
</header>


<!-- Nav -->
@php
    //Customize the HTML hierarchy of the menu
    Entity::Menu()->setMenuHtmlStruct([
        'ul_main' => ' <ul class="links">%1$s</ul>',
        'li_nochildren' => '<li class="nav-item-%3$s"><a href="%1$s" %4$s>%2$s</a></li>',
        'li_nochildrenactiv' => '<li class="nav-item-%3$s"><a href="%1$s" %4$s>%2$s</a></li>',
        'li_children' => '<li class=" nav-item-%3$s"><a href="%1$s" %5$s >%2$s <span class="caret"></span></a>%4$s</li>',
        'ul_children' => '<ul class="">%1$s</ul>',
        'li_childrenactiv' => '<li class="nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown" %5$s>%2$s</a>%4$s</li>'
    ]);
@endphp
<nav id="menu">
    {!! Entity::Menu()->getBySecurity() !!}
</nav>

<!-- Heading -->
<section id="One" class="wrapper style3">
    <div class="inner">
        <header class="align-center">
            @if(Entity::Page()->showSubtitle)
                <p>{{ Entity::Page()->subtitle }}</p>
            @endif

            @if(Entity::Page()->showName)
                <h2>{{ Entity::Page()->name }}</h2>
            @endif
        </header>
    </div>
</section>


<!-- Content -->
{!! Entity::Page()->content !!}

<!-- Footer -->
@include('theme::footer')
```
> cf. [Menu](./classes/entity/menu.md)

### The `footer.php` file
```
<!-- Footer -->
<footer id="footer">
    <div class="container">
        {!! ModuleArea::Display(Entity::Page(), 'footer', 'my-theme') !!}
    </div>
    <div class="copyright">
        &copy; Sylvain Roh. Powered by OmegaCMS.
    </div>
</footer>

</body>
</html>
```

## Templates
Templates allow you to have a different layout than the default `index.blade.php`.

Templates must be placed under `/omega/theme/my-theme/template` and will have a content similar to `index.blade.php`.

### Exemple :
```
<body class="subpage">

<!-- Header -->
<header id="header">
    <div class="logo"><a href="{{ url('/') }}">{{ Entity::Site()->name }}</a></div>
    <a href="#menu">Menu</a>
</header>


<!-- Nav -->
@php
    //Personaliser la structure du menu via la method setMenuHtmlStruct.
    Entity::Menu()->setMenuHtmlStruct([
        'ul_main' => ' <ul class="links">%1$s</ul>',
        'li_nochildren' => '<li class="nav-item-%3$s"><a href="%1$s">%2$s</a></li>',
        'li_nochildrenactiv' => '<li class="nav-item-%3$s"><a href="%1$s">%2$s</a></li>',
        'li_children' => '<li class=" nav-item-%3$s"><a href="%1$s" class="" >%2$s <span class="caret"></span></a>%4$s</li>',
        'ul_children' => '<ul class="">%1$s</ul>',
        'li_childrenactiv' => '<li class="nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown">%2$s</a>%4$s</li>'
    ]);
@endphp
<nav id="menu">
    {!! Entity::Menu()->getBySecurity() !!}
</nav>



<!-- Heading -->
<section id="One" class="wrapper style3">
    <div class="inner">
        <header class="align-center">
            @if(Entity::Page()->showSubtitle)
                <p>{{ Entity::Page()->subtitle }}</p>
            @endif

            @if(Entity::Page()->showName)
                <h2>{{ Entity::Page()->name }}</h2>
            @endif
        </header>
    </div>
</section>

<div class="row">
    <div class="col-md-9">
        {!! Entity::Page()->content !!}
    </div>
    <div class="col-md-3">
        {!! ModuleArea::Display(Entity::Page(), 'sidebar', 'my-theme') !!}
    </div>
</div>
<!-- Content -->

<!-- Footer -->
@include('theme::footer')
```

## Assets

All `.js`, `.css` and others files must be located in the assets directory of your theme.
You can publish these assets to make theme available in the public directory by running the following command:

`php artisan omega:theme:publish`

> This will publish files once.

If you want to watch the changes and automatically publish you can use the following command:

`npm run watch-theme`

## CSS Theme

> CSS Theme allow you to custom the CSS on pages.

Add a `.css` file in the directory `/omega/theme/my-theme/assets/css/theme` to create your CSS Theme.

Then add your CSS rules in this file.

Finally you can choose the CSS Theme for a page in the Settings tabs with the **Style** option.

## Modulearea

Modulearea are places where you can insert modules.

### Create Modulearea

Modulearea are created during the installation of the theme.

Add the following line in the `/omega/theme/my-theme/install/install.php` inside the `postInstall()` closure:
```
ModuleArea::Create('modulearea_name', $name);
```
> Replace `modulearea_name` by a name like `footer_center`.

### Display Modulearea

To define where a modulearea should be displayed, you will add the follwing line in one file of your theme
(`index.php`, `footer.php` or any template).
```
{!! ModuleArea::Display(Entity::Page(), 'footer', 'my-theme') !!}
```
> Don't forget to replace `my-theme` by the name of your theme

## Component template

Customizing the HTML of a component.

### Create
Create a `.blade.php` file in `/omega/theme/my-theme/template`

> Tips : It's easier to copy-paste the original view from the plugin directory.
  The view can be found in `/omega/plugin/plugin_name/view/display.blade.php`
  
It's a good idea to create a different directory for each plugin. So if you create a component template for the `text` plugin you will have to create a directory to hold all template for this plugin.
  
### Register the component template
Create or edit the file `/theme/my-theme/template/register.php`.

This file must contain the following code:
```
<?php
use Omega\Utils\Theme\Template;

/**
 * @var Template
 */
return Template::For('templated-hielo')
    ->registerComponentTemplateView('text', 'display', '1.0.0', 'text/textSuperStylish', 'Text on 3 column')
    ;
```

The `registerComponentTemplateView` method will register the component template `text/textSuperStylish` for the view `display` of the plugin `text`. We also give the version of the plugin at the time we copied the original view. So with the version we can check if the component template need to be updated or not. We can also give a Label (optional).

> `text/textSuperStylish` is the relative path (from the `/theme/my-theme/template/` directory) to the view file (without the ext).

You can register many component template just by chaining the `registerComponentTemplateView` method.
  
### How to use it on a component
We can choose the component template with the option "Composant's template" in the settings of a component.

## Theme exemple
You can download a theme exemple here.
[Download](./assets/omegaV3_default_theme.zip)
