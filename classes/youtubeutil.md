[Retour](../classes.md)

# La classe `YoutubeUtil`

La méthode `GetVideoIdFromUrl($url)` retourne l'id de la vidéo Youtube à partir de son URL.
```
$vid = YoutubeUtil::GetVideoIdFromUrl('https://www.youtube.com/watch?v=dQw4w9WgXcQ');
```
> Dans ce cas, `$vid` sera égal à `dQw4w9WgXcQ`.