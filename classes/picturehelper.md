[Retour](../classes.md)

# La classe `PictureHelper`

Cette classe donne accès à plusieurs méthodes statiques

Méthode | Description
--- | ---
`CreateThumbnail($in_filename, $out_filename, $width, $height)` | Crée une miniature dans `$out_filename` à partir de `$in_filename` de largeur `$width` et de hauteur `$height`
`Crop($in_filename, $out_filename, $max_width, $max_height, $quality = 80)` | Recadre une image `$in_filename` pour qu'elle ne dépasse pas une largeur maximum `$max_width` et une hauteur maximum `$max_height`. L'image sera enregistrée dans `$out_filename`
`GetImageWidth($filepath)` | Tout est dans le nom
`GetImageHeight($filepath)` | Tout est dans le nom
`Get($filepath, $width = null, $height = null)` | Recadre l'image `$filepath` à une largeur `$width` et une hauteur `$height` et retourne l'URL vers la nouvelle image. L'image est créée seulement une fois.