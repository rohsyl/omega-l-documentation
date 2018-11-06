

# Création d'un plugin simple (manuellement)
## 1. Créer un dossier dans `/plugin`
(Le nom du dossier doit commencer par une minuscule, ne pas contenir de nombre, de caractères spécianx (sauf `_`), et ni d'espace)

Exemple ok: "video_gallery", "videogallery", "videoGallery"

Exemple pas ok : "VideoGallery", "video gallery", "gallery2video"

Pour l'exemple, nous allons prendre comme nom de plugin : "pluginTest"

> Il n'y a pas de vérification si le nom est correct, donc respecte!

Donc créez un dossier nommé `pluginTest` dans `/plugin`.

## 2. Le fichier `plugin.json`
Dans le dossier `/plugin/pluginTest`, créez un fichier `plugin.json` avec le contenu suivant :
```
{
    "pluginName" : "pluginTest",
    "pluginTitle" : "Plugin de test",
    "pluginVersion" : "0.0.1",
    "author" : "Roh Sylvain",
    "authorMail" : "support@omega-cms.org",
    "authorWebsite" : "http://omega-cms.org",
    "description" : "Comment créer un plugin omegacms",
    "options" : {
        "displayInMenu" : 0
    }
}
```
> Attention ! L'option `pluginName` doit avoir la même valeur que le nom du dossier : `pluginTest`.

> Remarque : définisser l'option `displayInMenu` à `1` ou `0` selon si vous voulez 
l'afficher ou non dans le menu comme enfant de l'onglet "Plugins" dans le back-office

## 3. Les controlleurs
> Pour notre plugin, nous allons devoir créer deux controlleurs, un pour la partie back-office et un pour la partie public

1. Controlleur back-office
    > Créer un fichier nommé `BControllerPluginTest.php`  avec le contenu suivant :

```
<?php
    namespace Omega\Plugin\PluginTest;
        
    use Omega\Library\Plugin\BController;
    
    class BControllerPluginTest extends BController {
        
        public function __construct() {
            parent::__construct('pluginTest');
        }
        
        public function index() {
            return 'Plugin test, partie back-office';
        }
    }
``` 
- C'est via ce controlleur que vous allez pouvoir créer des pages dans le back-office
- Vous pouvez accéder à l'action `index` via Plugins > Manage Plugins > PluginTest
    > Vous pouvez créer plusieur action dans votre controlleur pour avoir plusieurs pages dans le back-office
    
2. Controlleur public
    > Créer un fichier nommé `FControllerPluginTest.php`  avec le contenu suivant : 

```
<?php
     namespace Omega\Plugin\PluginTest;
         
     use Omega\Library\Plugin\FController;
     
     class FControllerPluginTest extends FController {
         
        public function __construct() {
            parent::__construct('pluginTest');
        }
        
        public function registerDependencies()
        {
            return array(
                'css' => array(
                ),
                'js' => array(
                )
            );
        }
        
        public function display( $args, $data ) {
            return 'Plugin test, partie front';
        }
        
     }
``` 
    
- L'action `display` du controlleur sera appelée lorsque le plugin sera affiché dans la partie public
- L'action `registerDependencies` permet d'inclure dans le `<head>` les fichiers css/js que vous allez utiliser dans votre plugin
    
    > Remarque, veuillez préciser le chemin complet depuis la racine de Omega
     - Exemple:
        - si le fichier se trouve a la racine du plugin, le chemin sera : "plugin/pluginTest/style.css"
        - si il se trouve dans un sous dossier le chemain sera : "plugin/pluginTest/sousdossier/style.css"

## 4. <a name="plugin-install">Action lors de l'installation et de la désinstallation</a>

Comment effectuer des actions lors de l'installation et de la désinstallation du plugin ?

- Dans le controlleur back-office il est possible de surcharger les functions `install` et `uninstall` pour par exemple lancer des scripts sql après l'installation.
	
	Dans cette exemple, nous avons un fichier `.sql` que nous voulons éxecuter lors de l'installation du plugin.
	
	Le fichier `.sql` sera donc placé dans `/plugin/pluginTest/sql/install.sql`.
	
	Exemple de code: 

	```
    public function install() {
        if (!$this->isInstalled()) {
            parent::install();
            parent::runSql($this->root . DS . 'sql/install.sql');
        }
    }
	```
	> Ce code est à ajouter dans le controlleur Back-office
	    
    > Remarque : `$this->root` contient le chemin vers le dossier du plugin
    
## 5 Utilisation des vues
Comment créer un vue basique.
    
> Les vues sont utilisées pour simplifier le code et pour eviter du HTML directement dans les controlleurs

Exemple : Nous allons créer la vue correspondant à l'action `index` du controlleur back-office de notre plugin

- Créer un dossier `view` à la racine de votre plugin
- Dans ce dossier, créer un fichier nommé `view-[votre_action].php` 
    > Remarque : Remplacer `[votre_action]` par le nom de votre action, ici `index` donc `view-index.php`

- Et dans ce fichier, placer votre code HTML
    ```
    <p>
        <strong>Plugin test</strong>, partie back-office
    </p>
    ```
      
- Modifier l'action index de votre controlleur back-office de cette manière :
    ```
    public function index() {
        return $this->view();
    }
    ```
        
- Passer des variables à notre vue
    > Pour passer des variables à notre vue, il suffit de passer un array en paramètre à la methode view()
    
    Dans le controlleur :
    ```
    public function index() {
        $m['ma_variable'] = "Coucou";
        return $this->view($m);
    }
    ```
 
    > Et pour l'afficher, simplement faire un echo
    
    Dans la vue:
    ```
    <p>
        <strong>Plugin test</strong>, partie back-office<br />
        <?php echo $ma_variable ?>
    </p>
    ```
    
- Afficher une vue depuis une autre vue
    
    Si nous voulons avoir plusieur fois le même contenu sur plusieurs vues de notre controlleur (par exemple un menu).
    - Il nous faut créer une nouvelle vue. Par exemple `view-menu.php`
    - Et pour l'afficher depuis la vue index il nous faut ajouter la ligne suivante dans le fichier `view-index.php` :
        ```
        <?php echo $this->partialView('menu') ?>
        ```
        
        > Remarque : On peut passer des variables a cette vue `menu` en lui passant en 2ème argument un array.
    
## 6. URL vers une   action:
```
$this->getAdminLink('nomdelaction')
```
Pour rediriger vers cette action:
```
Redirect::toUrl($this->getAdminLink(nomdelaction));
```

## 7 Les composants/modules
Les composants sont utilisés pour créer le contenu d'une page. Lorsque vous ajouter une page, elle est créée vide, sans composants.
Pour ajouter du texte dans votre page, il faut ajouter un composant "Text" dans votre page.

Quant à eux, les modules sont utilisés pour remplir les modulearea.


Dans cette section, nous allons voir comment créer un nouveau composant/module!

On peut créer un composant/module par plugin.

### Comment créer un composant/module ?
Les composants/modules sont créés en SQL, en général à l'installation du plugin. cf [Action lors de l'installation](#plugin-install)

En premier, il faut créer un nouveau `form`:
```
-- Create form
CALL `om_CreateForm`('[comp_name]', '[plugin_name]', [is_module], [is_componant], [title]);
```

Explication des paramètres :

Nom du paramètre | Description | Valeurs possibles
--- | --- | ---
`comp_name` | Le nom du composant | En général, le même nom que le plugin
`plugin_name` | Le nom du plugin | Bah le nom de ton plugin, bolosse
`is_module` | Sera un module | 1 ou 0
`is_componant` | Sera un composant | 1 ou 0
`title` | Le titre du composant | Comme tu veux 


Ensuite il faut créer une ou plusieurs `entry`:
```
-- Create form entry
CALL `om_CreateFormEntry`('[comp_name]', '[entry_name]', [entry_order], [entry_type], [entry_type_param], [entry_title], [entry_descr], [is_mendatory]);
```

Explication des paramètres :

Nom du paramètre | Description | Valeurs possibles
--- | --- | ---
`comp_name` | Le nom du composant | La même valeur que lors de la création du `form`
`entry_name` | Le nom de l'`entry` | Comme tu veux
`entry_order` | L'ordre | 1, 2, 3, ...
`entry_type` | Le nom du composant | Les différents type disponible sont listé dans le back-office via l'onglet Développeur > Datatypes. Si tu as besoin d'un type qui n'existe pas. Check la page [Data Types](datatypes.md)
`entry_type_param` | Les paramètres du type | Du JSON. **Attention**, le JSON doit être valide et sur une seule ligne. (Vous pouvez utiliser cet [outil](https://jsonformatter.curiousconcept.com/) pour valider et "compresser" votre JSON.
`entry_title` | Le titre du composant | Comme tu veux
`entry_descr` | La description du composant | Comme tu veux
`is_mendatory` | Sera un champs obligatoire | 1 ou 0

> Remarque, l'option `is_mendatory` n'est pas encore implémentée.

### Comment utiliser les valeurs dans la partie public
Dans le controlleur front, dans la méthode `display`, l'argument `$data` est un array qui contient toutes les valeurs des `entry`.
