[Plugin](./plugin.md) -
[Install](./install.md) - 
[Classes](./classes.md) - 
[JavaScript](./js.md) - 
[Multi-langue](./mlang.md) - 
[Theme](./theme.md)

# Creation d'un thème pour OmegaCMS
Dans cette page vous allez apprendre comment créer un thème pour OmegaCMS.
## Structure de base
```
/theme
    /yourtheme
        /css
            /theme
        /js
        /template
        /install
            /install.php
        /header.php
        /index.php
        /footer.php
```
Les fichiers `install.php`, `header.php`, `index.php` et `footer.php` doivent obligatoirement exister.

### Exemple de fichier `install.php`
```
<?php
use Omega\Library\Entity\ModuleArea;

/*********************************************************************
 *
 *       clean_blog THEME INSTALLATION FILE FOR OmegaCMSv3
 *
/*********************************************************************/
// The name of your theme (this value must be the same as the name of the theme's directory) ..
$name = "clean_blog";
// The title of your theme ...
$title = "Clean Blog by templated";
// A little description ...
$description = "Clean blog is a blog theme that is perfect for personal or company blogs.";
// And your website ...
$website = "https://startbootstrap.com/template-overviews/clean-blog/";
$colors = array(
    '#0085A1', '#ffffff', '#dddddd'
);
//Custom action that must be executed after the installation
function post_install() {
    // Ici nous avons la création d'un module area.
    ModuleArea::Create('footer', 'clean_blog');
}
```

### Exemple de fichier `header.php`

```
<?php
    use Omega\Library\Util\OmegaUtil;
    use Omega\Library\Entity\Entity;
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <?php OmegaUtil::renderMeta() ?>
    <title><?php echo Entity::Site()->name ?> - <?php echo Entity::Page()->title ?></title>
    <link href="<?= Entity::Site()->template_directory_uri ?>/css/styles.css" rel="stylesheet">
    <script src="<?= Entity::Site()->template_directory_uri ?>/js/scripts.js"></script>
    <?php Entity::Page()->RenderCssTheme() ?>
    <?php OmegaUtil::renderDependencies() ?>
</head>
```
> cf. [Entity](./classes/entity/entity.md)

### Exemple de fichier `index.php`

```
<?php
    use Omega\Library\Entity\Entity;
?>
<body>
    <?php //Personaliser la structure du menu via la method setMenuHtmlStruct.
          //Exemple :
        Entity::Menu()->setMenuHtmlStruct(array(
            'ul_main' => '<ul class="nav navbar-nav navbar-right">%1$s</ul>',
            'li_nochildren' => '<li class="nav-item-%3$s"><a href="%1$s">%2$s</a></li>',
            'li_nochildrenactiv' => '<li class="nav-item-%3$s"><a href="%1$s">%2$s</a></li>',
            'li_children' => '<li class="dropdown nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown" role="button" >%2$s <span class="caret"></span></a>%4$s</li>',
            'ul_children' => '<ul class="dropdown-menu">%1$s</ul>',
            'li_childrenactiv' => '<li class="dropdown nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown">%2$s</a>%4$s</li>'
        )); ?>
    <?php echo Entity::Menu()->getBySecurity(); ?>
    
    <h1><?php echo Entity::Page()->title ?></h1>
    <span class="subheading"><?php echo Entity::Page()->subtitle ?></span>
    
    <?php echo Entity::Page()->content ?>
<?php include(Entity::Site()->php_template_path . DS . 'footer.php'); ?>
```
> cf. [Menu](./classes/entity/menu.md) pour plus de détails

### Exemple de fichier `footer.php`
```
<?php
    use Omega\Library\Entity\Entity;
    use Omega\Library\Entity\ModuleArea;
?>
    <footer>
        <!-- Affichage du modulearea -->
        <?php ModuleArea::Display(Entity::Page(), 'footer', 'clean_blog') ?>
    </footer>
</body>
</html>
```

## Templates
Les fichiers de template permette d'avoir une mise en page différente que la mise en page par défaut de `index.php`
Les fichiers de template doivent se trouver dans le dossier `theme/your_theme/template` et ont un contenu similaire à `index.php`
### Exemple :
```
<?php
    use Omega\Library\Entity\Entity;
?>
<!-- Navigation -->
<nav class="navbar navbar-default navbar-custom navbar-fixed-top">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header page-scroll">
            <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                <span class="sr-only">Toggle navigation</span>
                Menu <i class="fa fa-bars"></i>
            </button>
            <a class="navbar-brand" href="<?php echo ABSPATH ?>"><?php echo Entity::Site()->name ?></a>
        </div>
        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <?php Entity::Menu()->setMenuHtmlStruct(array(
                'ul_main' => '<ul class="nav navbar-nav navbar-right">%1$s</ul>',
                'li_nochildren' => '<li class="nav-item-%3$s"><a href="%1$s">%2$s</a></li>',
                'li_nochildrenactiv' => '<li class="nav-item-%3$s"><a href="%1$s">%2$s</a></li>',
                'li_children' => '<li class="dropdown nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown" role="button">%2$s <span class="caret"></span></a>%4$s</li>',
                'ul_children' => '<ul class="dropdown-menu">%1$s</ul>',
                'li_childrenactiv' => '<li class="dropdown nav-item-%3$s"><a href="%1$s" class="dropdown-toggle" data-toggle="dropdown">%2$s</a>%4$s</li>'
            )); ?>
            <?php echo Entity::Menu()->getBySecurity(); ?>
        </div>
        <!-- /.navbar-collapse -->
    </div>
    <!-- /.container -->
</nav>
<!-- Page Header -->
<!-- Set your background image for this header on the line below. -->
<header class="intro-header">
    <div class="container">
        <div class="row">
            <div class="col-lg-8 col-lg-offset-2 col-md-10 col-md-offset-1">
                <div class="site-heading">
                    <h1><?php echo Entity::Page()->title ?></h1>
                    <hr class="small">
                    <span class="subheading"><?php echo Entity::Page()->subtitle ?></span>
                </div>
            </div>
        </div>
    </div>
</header>
<!-- Main Content -->
<?php echo Entity::Page()->content ?>
<?php include(Entity::Site()->php_template_path . DS . 'footer.php'); ?>
```

## CSS Thèmes
Ajoutez des fichier `.css` dans le dossier `/theme/your_theme/css/theme` pour créer des CSS Thèmes.
Ensuite vous pouvez choisir un de ces thèmes CSS dans les paramètres d'une page, avec l'option **Style**.

## Modulearea
Les moduleareas sont des zones dans lesquels peuvent être placés des modules.
### Création de modulearea
Ajouter dans le fichier `/theme/your_theme/install/install.php` dans la fonction `post_install()` la ligne suivante:
```
ModuleArea::Create('modulearea_name', 'your_theme');
```
> Remplacer `modulearea_name` par un nom comme `footer_center` et `your_theme` par le nom de votre thème.
### Affichage
Pour définir l'emplacement de votre modulearea, il faut placer la ligne de code php suivante dans un des fichiers (`header.php`, `index.php`, `footer.php` ou dans les templates) de votre thème
```
<?php
    ModuleArea::Display(Entity::Page(), 'footer', 'clean_blog');
?>
```

## Templates de composant
Comment personnaliser la vue HTML d'un composant ?
### Création
Créer une fichier `.php` dans le dossier `/theme/your_theme/template/plugin_name`

> Astuce : Parfois il est plus simple de copier coller la vue de base du plugin et ensuite d'adapter le contenu.
  La vue de base se trouve dans `/plugin/plugin_name/view/view-display.php`
  
### Utilisation
On peut choisir d'utiliser un template de composant via l'option "Composant's template" dans les paramètres d'un composant

## Exemple complet de thème
Vous pouvez télécharger ici un exemple complet de thème pour OmegaCMS.
[Télécharger](./assets/omegaV3_default_theme.zip)


## Utils

Bootstrap est déjà disponible dans les fichiers du CMS

```
assets/js/bootstrap.min.js
```

```
assets/css/bootstrap.min.css
```

Il est possible de les inclure dans votre thème en placant les lignes suivante dans le head de votre thème.

```
<link rel="stylesheet" href="<?php echo Url::CombAndAbs(ABSPATH, 'assets/css/bootstrap.min.css') ?>">
```

``` 
<script src="<?php echo Url::CombAndAbs(ABSPATH, 'assets/js/bootstrap.min.js') ?>"></script>
```