[Plugin](./plugin.md) -
[Install](./install.md) - 
[Classes](./classes.md) - 
[JavaScript](./js.md) - 
[Multi-langue](./mlang.md) - 
[Theme](./theme.md)

Enable the multi-lang in your plugin ? See [Muti-lang Plugin](./multilang_plugin.md)

# Table of content
1. [Create a plugin](#create-a-plugin)
2. [How it works](#how-omega-plugin-works)
    1. [plugin.json](#1-plugin-json)
    1. [BController.php - Back-end controller](#2-bcontroller-php-back-end-controller)
    1. [FController.php - Front-end controller](#3-fcontroller-php-front-end-controller)
    1. [View](#4-view)
1. [Components & Modules](#components-modules)
1. [Form](#form)
1. [Database](#database)
1. [Multi-langue](#multi-langue)
1. [Migrations](#migrations)

# Create a plugin

How to create the basics files for a new plugin.

`php artisan omega:plugin:generate`

> `omega:plugin:generate` is an artisan command to create a plugin.

1. Open a terminal

2. Go to the root of your project

3. Run the command `php artisan omega:plugin:generate`

    - The tool will ask you for a plugin name.
    ```
    Please provide a plugin name:
    ```

    - Write the name of the plugin
    
    - Then it will ask you confirmations for the creation
    ```
    Plugin name must follow some rules and be formatted. After formatting, your plugin name will be :
    .....

    Do you want to keep this plugin name ? (yes/no) [no]:
    ```
    
    - Just answer with `yes` of you really want to create it.
    
    - Finally, the plugin will be generated.
    ```
    Directory created : /media/data/git/omega-l/omega/plugin/quote_request
    Directory created : /media/data/git/omega-l/omega/plugin/quote_request/view
    Directory created : /media/data/git/omega-l/omega/plugin/quote_request/assets
    Directory created : /media/data/git/omega-l/omega/plugin/quote_request/assets/css
    Directory created : /media/data/git/omega-l/omega/plugin/quote_request/assets/images
    File generated : /media/data/git/omega-l/omega/plugin/quote_request/BControllerQuoteRequest.php
    File generated : /media/data/git/omega-l/omega/plugin/quote_request/FControllerQuoteRequest.php
    File generated : /media/data/git/omega-l/omega/plugin/quote_request/view/display.blade.php
    File generated : /media/data/git/om-skycinema/omega/plugin/quote_request/assets/css/styles.css
    File copied : assets/images/component-logo.png
    File generated : /media/data/git/omega-l/omega/plugin/quote_request/plugin.json
    ```
    
    - You've created successfully a plugin !
    
    - Provide more informations about the plugin by editing the `plugin.json` and filling all information.
    
# How Plugins Work

## 1. `plugin.json`

This file is the file that contain description of the plugin and some configuration options.

```
{
  "pluginName": "twitterfeed",
  "pluginTitle": "Twitter Feed",
  "pluginVersion": "0.0.1",
  "author": "Sylvain Roh",
  "authorMail": "syzin12@gmail.com",
  "authorWebsite": "http://rohs.ch",
  "description": "Display a feed of tweets in a modulearea",
  "options": {
    "displayInMenu": 1
  }
}
```

### Options
All available options are described here.

option | value | description
--- | --- | ---
`displayInMenu` | `0` or `1` | If true, an entry will be added in the back-end menu under Plugin.
    
    
## 2. `BController.php` - Back-end controller

This is the controller used in the back-end

```
<?php
namespace OmegaPlugin\TwitterFeed;

use Omega\Utils\Plugin\BController;

class BControllerTwitterFeed extends BController {

    public function __construct() {
        parent::__construct('twitter_feed');
    }

    public function install() {
        $this->migrate();
        return true;
    }

    public function uninstall() {
        $this->reset();
        return true;
    }

    public function index()
    {
        return $this->meta_view();
    }
}
```

Each method is discribed below

Method  | Description
--- | ---
`__construct` | The constructor. **Warning**, the parent constructor MUST be called with the `pluginName` in parameter
`install` | Code used to install the plugin. You can add some code under the `parent::install()`
`uninstall` | Code used to uninstall the plugin. You can add some code under the `parent::uninstall()`
`index` | Default action in the back-end GUI
    
Usefull methods :

Method | Parameters | Description
--- | --- | ---
`runSql` | `$sqlFileAbsPath` | Execute a SQL file given in parameter. Path must be given in absolute. You can use the property `$this->root` that contain the path to the root directory of the plugin.
`isInstalled` | &nbsp; | Return true if the plugin is installed
`getId` | &nbsp; | Return the if of the plugin in the database
`getMeta` | &nbsp; | Return the meta information about the plugin (`PluginMeta`) (it's the content of the `plugin.json` file)
`redirect` | `$action`, `$param = []` | Redirect to the given action (a method of this controller) with the $param as $_GET
`json` | `$array` | Return `$array` as JSON
`view` | `$name` | Return the view of the current action
`meta_view` |  | Return a view that display informations about the plugin    
`migrate` |  | Run all migrations files
`reset` |  | Rollback all migrations files
    
    
    
You can add more action just by adding method in this file.
    
    
    
    
    
## 3. `FController.php` - Front-end controller
    
    
This is the controller used in the front-end.
    
```
<?php
namespace OmegaPlugin\TwitterFeed;

use Omega\Utils\Plugin\FController;

class FControllerTwitterFeed extends  FController {


   public function __construct() {
       parent::__construct('twitter_feed');
   }

   public function registerDependencies() {
       return array(
           'css' => array(
                $this->asset('css/styles.css')
           ),
           'js' => array(
           )
       );
   }

   public function display( $args, $data ) {
        return $this->view('display')->with(['data' => $data]);
   }
}
```
    
Each method is discribed below :

Method | Parameters | Description
--- | --- | ---
`__construct` | &nbsp; | The constructor. **Warning**, the parent constructor MUST be called with the `pluginName` in parameter
`registerDependencies` | &nbsp; | Code used to register js and css files. You must write absulute path from the omega root directory
`display` | `$args`, `$data` | This method is called when the plugin is displayed in the front-end
    
    
Usefull methods :

Method | Parameters | Description
--- | --- | ---
`json` | `$array` | Return `$array` as JSON
`view` | `$name` | Return the view of the current action
`asset` | `$path` | Return the URL to the given asset

## 4. View

> Views work the same way in the front and in the back.

Views are placed in the `view` directory in the root of the plugin.

Views use Blade template engine.

Views filename end with `.blade.php`.

#### Exemple 

You can create a view called `index.blade.php` in the `/view` directory.

And in the **index** method in the **back-end** controller you can display the index view by doing this:

```
public function index()
{
    return $this->view('index');
}
```

# Components & Modules

> How to create components and modules.

## 1. Introduction

Components and modules are basically the same thing. The only difference is where they are used :
 - Components are used to build the content of a page.
 - Modules are used in ModuleArea.
 
They're stored in the same table in the database : `modules`. And they're all directly linked to a plugin.
To create a component/module, you need to create a plugin. One component/module per plugin. Can be component and module at the same time.

## 2. Creation

The creation is done in a migration file at the installation of the plugin.

> During dev you can manually migrate and rollback migration files.

You can create a migration file by calling the following command:

```
php artisan omega:plugin:make:migration plugin_name create_form
```

Then, in the `BController` of your plugin, check that the `migrate()` method is called in the `install()` method.

```
public function install() {
    $this->migrate();
    return true;
}
```

### Create the `form`

```
FormFactory::newForm('form_name', 'plugin_name', false, true, 'Quotation request form');
```

Parameters :

Name | Description | Value
--- | --- | ---
`$formName` | The name of the component/module | `string` : The same value as the plugin name
`$pluginName` | The name of the plugin | `string` : The name of your plugin
`$isModule` | Will be a module | `boolean` : 0 or 1
`$isComponent` | Will be a component | `boolean` : 0 or 1
`$title` | The title of the component/module | `string`


### Create the `entry` (one or many)
```
FormFactory::newFormEntry('form_name', 'entry_name', 1, \Omega\Utils\Plugin\Type\Alert::class, $infoParam, '', '', false);
```

Parameters :

Name | Description | Value
--- | --- | ---
`$formName` | The name of the component/module | `string` : The same value than the one in the `om_CreateForm` 
`$entryName` | The name of the `entry` | `string`
`$order` | The order | `integer`
`$type` | The class of the type of the entry | `class` : All available types are listed in the back-end under Developper > Datatypes. If you need to create your own datatyoe. Check out the  [Data Types](datatypes.md) page
`$param` | The datatype's parameter | `array` : Param of the type as an array
`$title` | The title | `string`
`$description` | The description | `string`
`$mandatory` | Will be a mandatory field | `boolean` : 0 or 1

## 3. Display

> How to display component/module in the front-end.

In the `FController`, you should have a `display` method with two parameters `$args` and `$data` :

```
public function display( $args, $data ) {
    return $this->view( $data );
}
```

- `$args` contain the parameters of the component/module.
- `$data` contain the value all entries as a key/value array.

> Note : If your plugin does not have component/module, then `$data` will be `null`.

### Tips

You could use the method `Util::printR()` to display the content of the `$data` variable. In this way, you can see 
how entry's values are set.

Exemple : 
```
public function display( $args, $data ) {
    Util::printR($data);
    return $this->view( $data );
}
```

> You can also do it on each variables to know what does it contain.

Now that we have passed the `$data` to the view, we just need to use them in the view by directly calling variable by `$[entry_name]`.

> Replace `[entry_name]` by the name of the entry you set in the migration file

# Migrations


1. Create a migration file

Create a migration file for the given plugin under `omega/plugin/[plugin_name]/database/migrations`

```
php artisan omega:plugin:make:migration [plugin_name] [migration_name]
```

2. Execute migration

Execute migration for the given plugin

```
php artisan omega:plugin:migrate [plugin_name]
```

3. Rollback migration

Rollback the last migrations for the given plugin

```
php artisan omega:plugin:migrate:rollback [plugin_name]
```

4. Reset migration

Rollback all migrations for the given plugin

```
php artisan omega:plugin:reset [plugin_name]
```
