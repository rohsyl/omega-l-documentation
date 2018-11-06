[Retour](../classes.md)

# La classe `FacebookSharer`

A mettre à un endroit dans la page:
```
 $ogParam = array(
    'url' => $currentUrl,
    'title' => 'title',
    'description' => 'description',
    'image' => 'url/to/image.jpg'
);

FacebookSharer::GetScript();
FacebookSharer::SetOg($ogParam);
```

Placer le bouton "Partager":
```
<?php FacebookSharer::GetButton($currentUrl) ?>
```

A mettre dans la balise `<head>`:
```
<?php FacebookSharer::GetMetaOg() ?>
```
> Si aucun `SetOg()` n'a été appelé, `GetMetaOg()` n'affichera rien.