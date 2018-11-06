[Retour](../../classes.md)

# La classe `VideoExternal`
La classe `VideoExternal` permet de gérer une vidéo externe (Youtube ou Vimeo).

Cette classe hérite de la classe [Media](./media.md), toutes les mêmes propriétés/méthodes sont donc accessibles.

La méthode suivante a été ajoutée:
```
getVideoThumbnail();
```
> Cette méthode retourne l'URL vers la miniature de cette vidéo ou `null` si il n'y en a pas.