[Retour](./plugin.md)

# Création d'un datatype

Dans le dossier `library/plugin/type`, créez un nouveau fichier `class.TypeName.php` avec le contenu suivant:
```
<?php
namespace Omega\Library\Plugin\Type;

use Omega\Library\Plugin\ATypeEntry;

class TypeName extends ATypeEntry {

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
> Votre classe `TypeName` doit hériter de la classe `Omega\Library\Plugin\ATypeEntry`

# La classe `Omega\Library\Plugin\ATypeEntry`
C'est la classe a utiliser lorsque l'on veut créer un Type.

Elle offre les outils necessaires à la création d'un Type.

Il y a 4 méthodes abstraites à surcharger pour créer un nouveau Type correctement.
- `getHtml()` : Qui retourne le code HTML du champs de formulaire dans un `string`. 
    C'est cette méthode qui sera utilisée pour afficher le champs de formulaire
- `getPostedValue()` : Qui retourne ce qui va être enregirstré dans la base de données. Le retour **doit** être un `string`. Si vous voulez enregistrer un `array` dans la base de données, il faut le transformer en json avant avec la fonction `json_encode($array)`.
- `getObjectValue()` : Cette méthode doit retourner ce que vous avez enregistré grâce à `getPostedValue()`. 
    Mais si vous aviez enregistré un `array`, pensez donc à le passer dans la fonction `json_decode($json, true)`. Vous pouvez sans autre retourner des objets si vous le voulez.
- `getDoc()` : Qui retourne un explication au format HTML des paramètres qui doivent être passé à votre Type.

Vous avez aussi plusieurs méthodes disponibles pour créer votre Type:
- `getUniqId()` : Qui vous retourne un identifiant unique. A utiliser dans l'attribut `name` de votre `<input />`
- `getParam()` : Qui retourne les paramètres de votre Type dans un `array` ou `null` si il n'y as pas de paramètres. *La gestion de paramètres sera exploqué plus loin dans le document*
- `getValue()` : Qui retourne la valeur enregistrée dans la base de données
- `getIdPage()` : Qui retourne l'id de la page dans laquelle se trouve le Type
- `existsPost($key)` : Qui retourne `true` ou `false` si la `$key` se trouve dans la variable `$_POST`
- `getPost($key)` : Qui retourne la valeur qui se trouve dans la variable `$_POST[$key]` 
- `view($viewName, $viewData)` : Qui retourne la vue se trouvant dans `library/plugin/type/view/TypeName/view-[$viewName].php` en lui passant les paramètres se trouvant dans `$viewData`

> Il ne faut pas hésiter à aller voir les types déjà existant pour s'inspirer. Ils se trouvent tous dans le dossier `library/plugin/type`.

# Les paramètres de votre Type
- Où les paramètres sont-ils définis ?
    
    C'est le développeur qui va définir les valeurs de ces paramètres lorsque qu'il va créer un composant/module en utilisant la procédure stockeé SQL `om_CreateFormEntry` en s'inspirant de ce que vous aurez écrit dans votre `getDoc()`.
    
    Ils sont définis qu'une seule fois et jamais changés.
    
- Comment sont stockés les paramètres ?

    Les paramètres sont stockés au format JSON dans la base de données, vous travaillerez donc avec un `array`
    
    La méthode `getParam()` lit dans la base de données, décode le JSON et retourne un `array`. `null` est retourné si le JSON n'est pas valide.

- Exemple de paramètres :
    ```
    {
        "default" : 1,
        "options" : {
            "0" : "Left",
            "1" : "Right",
            "2" : "Top",
            "3" :"Bottom"
        }
    }
    ```
- Exemple de paramètres avec un namespace dans le json
    ```
    {
        "model" : "Omega\\Library\\Test\\ClassTest"
    }
    ```
    
    > Si l'on met un \ dans du JSON il faut en mettre un deuxième car le \ est un caractère d'echappement. Et comme on insère les paramètres via SQL, il faut mettre quatre !!!!
    
    ```
    {
        "model" : "Omega\\\\Library\\\\Test\\\\ClassTest"
    }
    ```

- Comment faire pour définir des paramètres par défaut ?

    Comme dans le Type `MediaChooser` dans `library/plugin/type/class.MediaChooser.php`