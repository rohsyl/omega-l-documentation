[Plugin](./plugin.md) -
[Install](./install.md) - 
[Classes](./classes.md) - 
[JavaScript](./js.md) - 
[Multi-langue](./mlang.md) - 
[Theme](./theme.md)

# Installation / Migration / Mise à jour de OmegaCMS
Dans cette page, vous allez apprendre comment installer OmegaCMS de deux manières. Via l'installation automatique ou via l'installation manuelle.

Vous allez aussi apprendre comment migrer OmegaCMS vers un nouveau serveur avec un nom de domaine différent.

Et finalement comment mettre à jour manuellement OmegaCMS.

## Installation
### Prérequis

- Activer l'URL rewriting
- Autoriser l'utilisation des fichiers `.htaccess`
- Tous les dossiers doivent être accesible en lecture/écriture.

### Installation automatique
1. Créer une base de données et un utilisateur
1. Configuration des informations de connections à la base de données
    - Renommer le fichier `config/omega.ini.default` en `config/omega.ini`
    - Editer le fichier `config/omega.ini`
    - Entrer les informations de connexion à la base de données :
    ```
    [database]
    host=localhost
    name=db_name
    user=username
    pass=password
    ```
2. Supprimer le fichier `.htaccess` dans le dossier install si existant
3. Lancer l'installation via `http://yourdomain.tld/install`
4. Renseigner les informations sur le site et le compte utilisateur
5. Se connecter à l'interface d'administration via `http://yourdomain.tld/admin`
6. Enjoy !

### Installation manuelle
1. Créer une base de données et un utilisateur
1. Configuration des informations de connections à la base de données
    - Renommer le fichier `config/omega.ini.default` en `config/omega.ini`
    - Editer le fichier `config/omega.ini`
    - Entrer les informations de connexion à la base de données :
    ```
    [database]
    host=localhost
    name=db_name
    user=username
    pass=password
    ```
2. Executer les scripts SQL
    - Exécuter le script `install/database.sql`
    - Exécuter les scripts se trouvant dans `install/data`
    - Exécuter les scripts se trouvant dans `install/patch` (**Respecter l’ordre des versions**)
3. Dans la table « om_config », définir la valeur de « om_web_adress » avec l’URL vers la racine de votre instance de OmegaCMS.
    Exemple : 
    - Si le CMS se trouve à la racine de votre hébergement, l'URL sera `http://domain.ch/`
    - Si le CMS se trouve dans un sous-dossier a la racine de l'hébergement, l'URL sera `http://domain.ch/dossier/`
        > Attention ! Ne pas oublier le `/` à la fin de l'URL
4. Création de l'utilisateur `root`
    - Exécuter le script `install/default/01.user.sql`
        > Ce script créer un utilisateur administrateur avec comme login `root` et mot de passe `pass`.  
        
        > Remarque : Le hash insérer dans la base de données correspond au sha1 du SALT et du mot de passe 
        ```
        sha1(SALT.$password)
        ```
5. Se connecter à l'interface d'administration via `http://yourdomain.tld/admin`
6. Changer le mot de passe de `root`
7. Enjoy !

## Migration
Comment déplacer OmegaCMS d'un serveur à un autre.

Dans cette exemple nous allons déplacer une instance de OmegaCMS de A:`http://localhost/omega/` à B:`http://rohs.ch/`
Omega est installé sur le serveur A, et nous voulons le déplacer sur le serveur B.

1. Effectuer un dump de la base de données depuis A
2. Utiliser un programme comme Notepad++ pour Rechercher/Remplacer tous les `http://localhost/omega/` par `http://rohs.ch/`
3. Restaurer le dump sur B
4. Télécharger tous les fichiers de OmegaCMS de A vers B
5. Adapter les informations de connections à la base de données
   - Editer le fichier `config/omega.ini`
   - Entrer les informations de connexion à la base de données :
   ```
   [database]
   host=localhost
   name=db_name
   user=username
   pass=password
   ```
6. OmegaCMS est maintenant disponible sur `http://rohs.ch/`



## Mise à jour manuelle
Comment mettre à jour manellement OmegaCMS

1. Détecter quelle est la version actuelle du CMS
    
    Se rendre sur le dashboard de l'administration et trouver la `CMS Version` et la `Database version`.
    
    Exemples: 
    
    > CMS version : 3.3.0 & Database version : 5
    
2. Effectuer un update du SVN

3. Executer les scripts `.sql` se trouvant dans `install/patch`. Ne pas executer les scripts qui on la version <= à la `Database version` trouvée dans le point 1.

    Page exemple : 
    
    > Si la `Database version` est à 3, n'éxecuter que les fichier `00004-...`, `00005-...` ...