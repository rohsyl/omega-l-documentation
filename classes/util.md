[Retour](../classes.md)

# La classe `Util`

Cette classe donne accès à plusieurs méthodes statiques.

Méthode | Description
--- | ---
`htmlEntitiesArray($array)` | Exécute la fonction `htmlentities()` sur chaque entrée du tableau `$array`
`rrmdir($dir)` | Supprime un dossier `$dir` et tout son contenu
`toTextKey($string)` | Enlève les espaces, les accents et les caractères spéciaux du `$string`
`removeAccents($string)` | Enlève les accents du `$string`
`replaceSpaces($string, $char = '_')` | Remplacer les espaces de `$string` par `$char`
`removeSpecialChar($string)` | Enlève les caractères spéciaux de `$string`
`toTextClean($text)` | Enlève l'extension et remplace les `_` par des espaces. Utile sur un nom de fichier
`printR($d, $dieIt = false)` | Affiche dans une balise `<pre>` la variable `$d`. Si `$dieIt` est à `true`, la fonction `die()` sera appelée
`echoIfIsset($value)` | `echo` la `$value` si elle n'est pas `null`
`substrIfLonger($str, $len, $addDots = false)` | `substr` si le texte `$str` dépasse la longeur `$len`. Si `$addDots` est à `true`, `...` seront ajoutés à la fin.
`getMIME($filename)` | Un petit workaround pour récupérer le `MIME` d'un fichier selon son extension. C'est pas très catholique...