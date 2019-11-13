[Retour](./plugin.md)

# Datatypes

**Datatypes** are used to create form for components and modules.

## Create a datatype

Create a new class that extend the abstract class `Omega\Utils\Plugin\ATypeEntry`.
```
<?php
/**
 * Created by PhpStorm.
 * User: sylvain
 * Date: 05.08.2017
 * Time: 19:12
 */

namespace Omega\Utils\Plugin\Type;

use Omega\Utils\Plugin\ATypeEntry;

class TextSimple extends ATypeEntry {

    public function getHtml()
    {
        $uid = $this->getUniqId();
        $value = $this->getObjectValue();
        return '<input type="text" id="'.$uid.'" name="'.$uid.'" value="'.$value.'" class="form-control" />';
    }

    public function getPostedValue()
    {
        return $this->getPost($this->getUniqId());
    }

    public  function getObjectValue() {
        $v = $this->getValue();
        return isset($v) ? $v : '';
    }

    public function getDoc(){
        return $this->view('Doc');
    }
}
```

# The class `Omega\Utils\Plugin\ATypeEntry`
This class is important to create new TypeEntry.

It will give you all usefull methods to create and new Type.

First, there is 4 abstract method to override.
- `getHtml()` : Must return the html of the form entry as a `string`. 
- `getPostedValue()` : Must return the value that will be stored in the database. The return type **MUST** be a `string`. If you need to store an array, just `json_encode()` it.
- `getObjectValue()` : Must return the value you saved in the database with `getPostedValue()`. But of you have saved an array encoded in json. Don't forget to decode it ! You can also return object if you what.
- `getDoc()` : Must return the explaination of the entry type parameters. This view will be displayed in the Developper > Datatyles section.

There is many usefull method that you will have to use to create your type.
- `getUniqId()` : Wil return an uniq identifier. You will have to use it, for exemple, in the `name` attribute of an `<input />`.
- `getParam()` : Will return parameters of your type as an `array`, or `null` if there is no parameters. Parameters will be explained later in this document.
- `getValue()` : Will return the value stored in the database (raw)
- `getIdPage()` : Will return the id of the current page (can be null)
- `existsPost($key)` : Will return `true` or `false` if the `$key` is present in the POST
- `getPost($key)` : Will return the value `$key` from the POST variable
- `view($viewName, $viewData, $viewParentPath = null)` : Will return the view located in the `$viewParentPath` directory and named **`$viewName`**`.blade.php` and will pass parameters passed in the `$viewData` directory

> Do not hesitate to take a look at existing Types. They are located in `app/Utils/Plugin/Type`.

# Parameter of your Tyoe
- Where do I define parameters of my entry 
    
    It's the developper that will set the parameters that will be passed to the type. You will have to explain which parameters you expect in the `getDoc()`. Parameters will be passed when the user create form entry.
    
    Parameters are set when the user create form entry and they can't be changed.
    
- How parameters are stored.

    They are stored in the database as JSON. Yon will have to work with an array.
    
    The `getParam()` method will read the value in the database, then will `json_decode` it and return the `array` or `null` of there is no parameters or if the json is not valid.

- Parameters exemple : 
    ```
    [
        "default" =>  1,
        "options" => [
            "0" => "Left",
            "1" => "Right",
            "2" => "Top",
            "3" => "Bottom"
        ]
    ]
    ```
- Exemple with an classname.
    ```
    [
        "model" => "Omega\\Library\\Test\\ClassTest"
    ]
    ```

- How to define default parameters ?

    Look how the `MediaChooser` type did it. The file is located in `app/Utils/Plugin/Type/MediaChooser.php`
