[Retour](../classes.md)

# La classe `Path`

Cette classe donne accès aux méthodes statiques suivantes:

Méthode | Description
--- | ---
`Combine()` | Combine plusieurs partie de chemin.
`getExt($filepath)` | Retourne l'extension du fichier `$filepath`.
`getFullPath($path)` | Retourne le chemin absolu du chemin `$path`.
`getSize($filepath)` | Retourne la taille du fichier `$filepath`.
`Make($path){` | Crée une structure de dossier de manière récursive.
`MkDir($pathname , $mode = 0777 , $recursive = false)` | Crée un dossier, si il n'existe pas.
`GetFiles($pathToFolder)` | Retourne la liste de tous les fichiers/dossiers dans un dossier donné `$pathToFolder`.