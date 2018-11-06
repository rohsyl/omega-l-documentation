[Retour](../../classes.md)

# La classe `Media`
La classe `Media` permet d'accéder à un média de la bibliothèque.

Cette classe donne accès à plusieurs propiétés d'un média

Propiétés | Description
--- | ---
`$id` | L'id du média
`$parent` | L'id du média parent
`$name` | Le nom du média
`$type` | Le type du média (`Media::FOLDER`, `Media::MEDIA` ou `Media::EXTERNAL_VIDEO`)
`$ext` | L'extension du média
`$path` | L'URL vers le média
`$title` | Le titre du média
`$description` | La description du média

Cette classe donne accès à plusieurs méthode

Méthodes | Description
--- | ---
`save()` | Sauvegarde dans la base de données les propiétés du média
`GetThumbnail($w, $h, $returnUrl)` | Si le média est une image, créer une miniature de largeur `$w` et de hauteur `$h`. Si `$returnUrl` est à `true`, retourne l'URL vers la miniature, sinon retourne le chemin physique vers la miniature.
`getType()` | Retourne le type détaillé du média (`T_PICTURE`, `T_VIDEO`, `T_MUSIC`, `T_DOCUMENT`, `T_OTHER`, `T_FOLDER` ou `T_VIDEO_EXT`)
`getWidth()` | Si le média est une image, retourne sa largeur, sinon retourne 0
`getHeight()` | Si le média est une image, retourne sa hauteur, sinon retourne 0
`getFilesize()` | Retourne la taille du fichier
`getRealpath()` | Retourne le chemin physique du fichier
`getChildren($arrayFilterType)` | Retourne un `array` contenant tous les médias enfants. `$arrayFilterType` est un `array` des types à filtrer (`T_PICTURE`, `T_VIDEO`, `T_MUSIC`, `T_DOCUMENT`, `T_OTHER`, `T_FOLDER` ou `T_VIDEO_EXT`)

> Attention, pour les médias de type `T_VIDEO_EXT`, veuillez utiliser la classe [VideoExternal](./videoexternal.md).

## Récupérer un média
```
$media = new Media($id);
```

## Créer un dossier
```
$media = MediaManager::CreateFolder($name, $idParent);
```

## Téléverser un média (image, document, video, fichier, ...)
```
$media = MediaManager::UploadMedia($FILES, $idParent, $success)
```
> Remarque : Si le téléversement a échoué, `UploadMedia()` retourne `null`.

## Supprimer un média
```
$result = MediaManager::Delete($idMedia)
```
> Remarque : cette méthode supprime uniquement l'entrée dans la base de données et pas les fichiers