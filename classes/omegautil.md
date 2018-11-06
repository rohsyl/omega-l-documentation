[Retour](../classes.md)

# La classe `OmegaUtil`

Cette classe donne accès à plusieurs méthodes statiques.

Méthode | Description
--- | ---
`getCurrentUserAvatar()` | Pour le back-office. Retourne le code HTML pour afficher l'avatar de l'utilisateur connecté
`getCurrentUserName()` | Pour le back-office. Retourne le nom de l'utilisateur connecté
`getInstalledPlugin()` | Retourne un `array` contenant les noms des plugins installés
`renderMeta()` | Affiche les balises `<meta>`, `keywords` et `description`
`isInstalled()` | Retourne `false` si OmegaCMS n'est pas installé, sinon retourne `true`
`member_IsInGroup($idMember, $idGroup)` | Retourne `true` si le membre `$idMember` se trouve dans le membergroup `$idGroup`, sinon retourne `false`