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

> Don't forget to replace every occurence of `my-theme` by the name of your theme.

## Theme directory

First, you need to create a directory called `my-theme` in the `/omega/theme` directory.

## Theme hierarchy

Then you will have to create the following files and directories.

```
/omega/theme/my-theme
        /assets
            /css
            /js
            /img
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

This file allow you give all information to the theme installer and do action after the installation of the theme.

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

The **`colors`** option is an array of color used in your theme and that you want to allow the user to reuse them. Mainly in the settings of a component as the background color option.

The `postInstall` method is called after the theme is installed. You can put any code you want in there. It's also used to create the ModuleArea (see the [Modulearea](#modulearea) section below). The `postInstall()` method is optional.

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
> cf. [Menu](./classes/entity/menu.md) to learn more about menu (setMenuHtmlStruct, ...)

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

Templates must be placed under `/omega/theme/my-theme/template` and will have a content similar than `index.blade.php`.

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

All `.js`, `.css` and others files (images, ...) must be located in the `assets` directory of your theme.
You can publish these assets to make theme available in the public directory by running the following command:

`php artisan omega:theme:publish`

> This will publish files once into the `/public/theme` directory.

If you want to watch changes and automatically publish them you can use the following command:

`npm run watch-theme`

## CSS Theme

> CSS Theme allow you to custom the CSS on pages.

Add a `.css` file in the directory `/omega/theme/my-theme/assets/css/theme` to create your CSS Theme.

Then add some CSS rules in this file.

Finally you can choose the CSS Theme for a specific page. For that, you have to edit a page and go to the Settings tab and change the **Style** option.

## Modulearea

Modulearea are area where you can insert modules.

### Create Modulearea

Modulearea are created during the installation of the theme.

Add the following line in the `/omega/theme/my-theme/install/install.php` inside the `postInstall()` closure:
```
ModuleArea::Create('modulearea_name', $name);
```
> Replace `modulearea_name` by a name like `footer_center` or `sidebar_left`...

> **Tips**: If you think about *Moduleareas* for your theme after the installation, then you can create some directly in omega. Go to Apparences -> Theme -> Detail of your theme, and then you can manage modulearea from this page. You will have to add them **manually** in the `install.php` to keep the theme working if you install it again.

### Display Modulearea

To define where a modulearea should be displayed, you will add the follwing line in one file of your theme
(`index.php`, `footer.php` or any templates).
```
{!! ModuleArea::Display(Entity::Page(), 'modulearea_name', 'my-theme') !!}
```
> Don't forget to replace `my-theme` by the name of your theme and `modulearea_name` by the name of your modulearea.

## Component template

Component's templates allow you to customize the view of any plugins.

### Create
Create a `.blade.php` file in `/omega/theme/my-theme/template`

> Tips : It's easier to copy-paste the original view from the plugin directory.
  The view can be found in `/omega/plugin/plugin_name/view/display.blade.php`
  
It's a good idea to create a different directory for each plugin. So if you create a component template for the `text` plugin you will have to create a directory to hold templates for this plugin.
  
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

> `text/textSuperStylish` is the relative path (from the `/theme/my-theme/template/` directory) to the view file (without the ext). So this example will point to the file `/theme/my-theme/template/text/textSuperStylish.blade.php`.

You can register many component's templates just by chaining the `registerComponentTemplateView` method.
  
### How to use it on a component
We can choose the component template with the option "Composant's template" in the settings of a component while editing a page.


### Default component's template

You can define a component template that will be used by default for a theme by creating a new blade file in the in a sub-directory of the `omega/theme/my-theme/template/` directory.

The sub-directory must have the **same** name as the plugin and the file must be named `default.blade.php`.

Exemple:
```
omega/theme/my-theme/template/plugin_name/default.blade.php
```

