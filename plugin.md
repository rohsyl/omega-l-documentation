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
Create a plugin with `omega-cli` tools.

> `omega-cli` is a tools writed in C# and need `mono` to run on linux.

1. Open a terminal

2. Go to the `tools/omega-cli` directory.
    
3. Configure `omega-cli` [(see this guide)](omega-cli/configure.md)

4. Run the command `omega-cli plugin:create [pluginName]`

    - The tool will normalize your pluginName to avoid error and will ask for confirmation.
    ```
    Output directory will be : D:\git\omegav3\plugin
    The plugin_name you entered will be normalized : twitterFeed
    The name of your plugin will be : twitterfeed
    Are you okay with this name ? (y/n)
    ```
    
    > Here you can see the the destination were the generated plugin will be placed (Output directory ).

    - Type `y` and hit [Enter]
    
    - Then it will ask you information about the plugin, like the display name, the author, ...
    ```
    Please give all informations about the plugin :
    Plugin title [] :
    Twitter Feed
    Plugin version [0.0.1] :
    
    Author name [Sylvain Roh] :
    
    Author mail [syzin12@gmail.com] :
    
    Author website [http://rohs.ch] :
    
    Plugin description [] :
    Display a feed of tweets in a modulearea
    Option - Display in menu (0, 1) [0] :
    1
    ```
    > If you keep empty, the default value will be used. The default value is shown between `[]`
    
    - When it's done, a confirmation will be asked. Just answer with `y`
    ```
    Are you okay with these informations ? (y/n)
    y
    ```
    
    - Finally, the plugin will be generated.
    ```
    Plugin creation started ...
    Create plugin directories under "D:\git\omegav3\plugin" ...
    Created : D:\git\omegav3\plugin\twitterfeed
    Created : D:\git\omegav3\plugin\twitterfeed\sql
    Created : D:\git\omegav3\plugin\twitterfeed\view
    Created : D:\git\omegav3\plugin\twitterfeed\image
    Created : D:\git\omegav3\plugin\twitterfeed\css
    Created : D:\git\omegav3\plugin\twitterfeed\js
    Ok
    
    Create json file ...
    Created : D:\git\omegav3\plugin\twitterfeed\plugin.json
    Ok
    
    Create plugin files...
    Created : D:\git\omegav3\plugin\twitterfeed\FControllerTwitterfeed.php
    Created : D:\git\omegav3\plugin\twitterfeed\BControllerTwitterfeed.php
    Created : D:\git\omegav3\plugin\twitterfeed\sql\install.sql
    Created : D:\git\omegav3\plugin\twitterfeed\sql\uninstall.sql
    Created : D:\git\omegav3\plugin\twitterfeed\view\view-display.sql
    Created : D:\git\omegav3\plugin\twitterfeed\view\view-index.sql
    Ok
    
    Plugin created successfully !
    Plugin location : D:\git\omegav3\plugin
    
    Success!
    ```
    
    - You've created successfully a plugin !
    
# How Omega plugin works

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
/*
 *  Generated with omega-cli.
 *  Date : lundi, 23 juillet 2018
 */

namespace Omega\Plugin\Twitterfeed;

use Omega\Library\Plugin\BController;

class BControllerTwitterfeed extends BController {

    public function __construct() {
        parent::__construct('twitterfeed');
    }

    public function install() {
        parent::runSql($this->root. '/sql/install.sql');
        return true;
    }

    public function uninstall() {
        parent::runSql($this->root. '/sql/uninstall.sql');
        return true;
    }

    public function index()
    {
        return $this->view();
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
`getAdminLink` | `$action`, `[$param]` | Return the URL to the given action. Exemple `$this->getAdminLink('index')` will return the URL to the index action. You can pass `GET` parameters by giving key/value array as second argument.
`isInstalled` | &nbsp; | Return true if the plugin is installed
`getId` | &nbsp; | Return the if of the plugin in the database
`getMeta` | &nbsp; | Return the meta information about the plugin (`PluginMeta`) (it's the content of the `plugin.json` file)
`partialView` | `$name`, `$m = array()` | Return a partial view by his name and optionnaly pass parameters
`json` | `$array` | Return `$array` as JSON
`view` | `$m = array()` | Return the view of the current action
    
    
    
    
You can add more action just by adding method in this file.
    
    
    
    
    
## 3. `FController.php` - Front-end controller
    
    
This is the controller used in the front-end.
    
```
<?php
/*
*  Generated with omega-cli.
*  Date : lundi, 23 juillet 2018
*/

namespace Omega\Plugin\Twitterfeed;

use Omega\Library\Plugin\FController;

class FControllerTwitterfeed extends  FController {


   public function __construct() {
       parent::__construct('twitterfeed');
   }

   public function registerDependencies() {
       return array(
           'css' => array(
           ),
           'js' => array(
           )
       );
   }

   public function display( $args, $data ) {
       return $this->view();
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
`partialView` | `$name`, `$m = array()` | Return a partial view by his name and optionnaly pass parameters
`json` | `$array` | Return `$array` as JSON
`view` | `$m = array()` | Return the view of the current action


## 4. View

> Views work the same way in the front and in the back.

Views are placed in the `view` directory in the root of the plugin.

Files are always prefixed with `view-`.

#### Exemple 

> with the **index** method in the **back-end** controller.

If you have the method called `index` then the view file will be called `view-index.php`.

```
public function index()
{
    return $this->view();
}
```

> The method must return `$this->view();`.

# Components & Modules

> How to create components and modules.

## 1. Introduction

Components and modules are basically the same thing. The only difference is where they are used :
 - Components are used to build the content of a page.
 - Modules are used to be placed in ModuleArea.
 
They're stored in the same table in the database : `om_module`. And they're all directly linked to a plugin.
To create a component/module, you need to create a plugin. One component/module per plugin. Can be compoent and module at the same time.

## 2. Creation

The creation is done in SQL at the installation of the plugin.

If you have used the `omega-cli` to create your plugin, you normally have a `sql/install.sql` file that is executed when installing the plugin.

Check inside the `BController`, the `install` method must look like this :

```
public function install() {
    parent::runSql($this->root. '/sql/install.sql');
    return true;
}
```

The line `parent::runSql($this->root. '/sql/install.sql');` will execute the sql file.


> If the `sql/install.sql` file and the `install` method doesn't exists, please create them.

All the following SQL query must be placed in this file.

### Create the `form`

```
CALL `om_CreateForm`('[comp_name]', '[plugin_name]', [is_module], [is_componant], [title]);
```

Parameters :

Name | Description | Value
--- | --- | ---
`comp_name` | The name of the component/module | `string` : The same value as the plugin name
`plugin_name` | The name of the plugin | `string` : The name of your plugin
`is_module` | Will be a module | `boolean` : 0 or 1
`is_componant` |Will be a componant | `boolean` : 0 or 1
`title` | The title of the component/module | `string`


### Create the `entry` (one or many)
```
CALL `om_CreateFormEntry`('[comp_name]', '[entry_name]', [entry_order], [entry_type], [entry_type_param], [entry_title], [entry_descr], [is_mendatory]);
```

Parameters :

Name | Description | Value
--- | --- | ---
`comp_name` | The name of the component/module | `string` : The same value than the one in the `om_CreateForm` 
`entry_name` | The name of the `entry` | `string`
`entry_order` | The order | `integer`
`entry_type` | The class of the type of the entry | `string` : All available types are listed in the back-end under Developper > Datatypes. If you need to create your own datatyoe. Check out the  [Data Types](datatypes.md) page
`entry_type_param` | The datatype's parameter | `json ` : **Warning**, it must be valid JSON and in one line. (You can use this tool [jsonformatter](https://jsonformatter.curiousconcept.com/) to validate and format the JSON.
`entry_title` | The title | `string`
`entry_descr` | The description | `string`
`is_mendatory` | Sera un champs obligatoire | `boolean` : 0 or 1

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

> Replace `[entry_name]` by the name of the entry you set in the `install.sql`

# Form

> How to create a full form in the a back-end page.

# Database

> How to save data in the database.

# Multi-langue

> How to use multi-langue in your plugin.


There is a full exemple available [here](./assets/plugindemo.zip)

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

# Old documentation

[Plugin guide old](./plugin.old.md)
