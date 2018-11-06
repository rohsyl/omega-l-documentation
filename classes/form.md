[Retour](../classes.md)

# La classe `Form`
Cette classe permet de récupérer les données envoyées via un `<form>` en `POST`.

Liste des méthodes disponibles :

Méthode | Description
--- | ---
`getName()` | Retourne le nom du formlaire
`getValue($field)` |  Retourne la valeur d'un champs du formulaire
`getCheckBoxBoolean($field)` | Retourne `true` si la checkbox est cochée, sinon retourne `false`
`checkAndGetValueOrNull($field)` | Retourne la valeur d'un champs si elle existe, sinon retourne `null`
`checkValue($field)` | Retourne `true` si le champs existe dans le formulaire, sinon retourne `false`
`isPosted()` | Retourne `true` si le formulaire à été envoyé, sinon retourne `false`
`getTokenInput($form_name)` | Affiche un champs caché dans le formulaire contenant un token unique. Protection contre les attaques [CSRF](http://shiflett.org/articles/cross-site-request-forgeries). 
`checkTokenInput()` | Vérifie le token. Si le token est ok, retourne `true`, sinon retourne `false`

Exemple:
- Code HTML :
    > Avant de commencer, il faut donner un nom `[form_name]` à ce formulaire. 
    
    ```
    <form class="form-inline" action="<?php echo Url::Action('setting', 'index') ?>" method="post" role="form">
        <?php Form::getTokenInput('[form_name]'); ?>
        <div class="form-group">
            <label for="flang_default">Default language :</label>
            <select class="form-control" name="flang_default">
                ...
            </select>
        </div>
        <div class="checkbox">
            <label>
                <input type="checkbox" name="checkbox"> Check me out
            </label>
        </div>
        <button type="submit" class="btn btn-primary" name="[form_name]">Save</button>
    </form>
    ```

- Code PHP :
   ```
   <?php
   $formFlang = new Form('[form_name]');
   if($formFlang->isPosted()){
       if($formFlang->checkTokenInput()){
           // récupérer une value
           $flang = $formFlang->getValue('flang_default');
           // si une checkbox est cochée ou non.
           $ischecked = $form->getCheckBoxBoolean();
           
           echo $flang;
           echo $ischecked;
       }
   }
   ```