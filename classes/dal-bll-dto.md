[Retour](../classes.md)


# Requête à la base de données avec `DAL BLL DTO`

## DTO - Data Transfer Object

Un DTO réprésente une entité, soit une table dans votre base de données.

Exemple : 
```
namespace ...;

use Omega\Library\DTO\Entity;

class ExempleEntity extends Entity
{
    public $primaryKey;
    public $field1 = 'Bonjour';
    public $field2 = null;
    public $field3 = 12;
    public $field3;

    public function __construct() {
        parent::__construct('exemple_table', array('field1', 'field2', 'field3'), 'primaryKey');
    }
}
```

Pour créer une entité, il faut créer une classe qui hérite de la classe `Omega\Library\DTO\Entity` et utiliser le constructeur comme l'exemple ci-dessus.

- Le premier paramètre correspond au nom de la table.
- Le deuxième paramètre est un `array` contenant le nom de tous les champs sauf la clé primaire.
- Le dernier paramètre est le nom de la clé primaire. (Si il n'y a pas de clé primaire, passer `null`)

Il faut aussi créer des propriétés dans la classe portant le même nom que les champs.

Il est aussi possible de définir les valeurs par défaut en donnant une valeur à l'initialisation de la propriété.
```
public $field1 = 'Bonjour';
```


## DAL - Data Access Layer

Utilise la classe `Omega\Library\DAL\Db` pour toute requête à la base de données

Créer une classe qui va `extends` de la classe `Db`

```
namespace ...;

use Omega\Library\DAL\Db;

class ExempleDb extends Db{
    
    
}
```


### Méthodes disponibles

> Pour plus de détails sur ces méthodes, voir la documentation phpDoc directement dans les fichiers sources.

#### `GetPDO()` | Retourne un instance de PDO
> Cette méthode `GetPDO()` permet de récupérer une connexion à la base de données pour pouvoir utiliser directement `PDO` pour faire des requêtes


Exemple d'une méthode à placer dans notre classe ExempleDb:

```
public function queryWithPDO() {
    $dbh = parent::GetPDO();
    $entity = new ExempleEntity();
    $tableName = $entity->getTableName();
    $dbh = self::GetPDO();
    $stmt = $dbh->prepare(sprintf("SELECT * FROM %s", $tableName));
    $stmt->execute();
    $entities = array();
    while($row = $stmt->fetch(PDO::FETCH_ASSOC)){
        $e = new ExempleEntity();
        EntityFiller::Fill($e, $row);
        $entities[] = $e;
    }
    return $entities;
}
```

> Que fait la classe `EntityFiller` ? Voir la section `EntityFiller` plus bas dans ce document.

Il faut utiliser la méthode `getTableName()` pour récupérer le nom de la table correspondant à l'entité. Il est fortement conseillé de faire ça pour éviter d'écrire le nom de la table en dur.

Mais ça fait beaucoup de code juste pour un simple `SELECT`... Et bien il existe des méthodes dans la classe `Db` pour vous simplifier la vie !

####  `GetOne()` | Sélectionner une entité par sa clé primaire

```
public funciton querySelectPk($id){
    return parent::GetOne(new ExempleEntity(), $id);
}
```

####  `GetOneWhere()` | Sélectionner une entité avec une condition

```
public funciton querySelectWhere($field1Value){
    return parent::GetOneWhere(new ExempleEntity(), $field1Value, 'field1');
}
```

####  `GetManyWhere()` | Sélectionner plusieurs lignes avec une condition
Récupère plusieurs lignes correspondant à la condition
```
public function querySelectManyWhere($field2Value){
    return parent::GetManyWhere(new ExempleEntity(), $field2Value, 'field2');
}
```
####  `GetManyWhereOrdered()` | Sélectionner plusieurs lignes avec une condition et ordée
Récupère plusieurs lignes correspondant à la condition (field2) et ordrée (field1)
```
public function querySelectManyWhereOrdered($field2Value){
    return parent::GetManyWhere(new ExempleEntity(), $field2Value, 'field2', 'field1', self::ORDER_ASC);
}
```

####  `GetAll()` | Récupérer toutes les lignes d'une table
```
public function querySelectAll(){
    return parent::GetAll(new ExempleEntity());
}
```

####  `GetAllOrdered($entity, $column, $orderType = self::ORDER_ASC)` | Récupérer toutes les lignes ordrée d'une table
```
public function querySelectAllOrdered(){
    return parent::GetAllOrdered(new ExempleEntity(), 'field1, self::ORDER_ASC);
}
```


####  `Count($entity)` | Compte le nombre de ligne d'une table
####  `CountWhere($entity, $value, $key)` | Compte le nombre de ligne repondant à une condition
####  `GetScalar($entity, $column)` | Récupère un scalaire
####  `GetScalarWhere($entity, $column, $whereValue, $whereColumn)` | Récupère un scalaire correspondant à une condition

####  `Insert($entity)` | Insère une `Entity` dans la base de données (INSERT)
####  `Update($entity)` | Modifie une `Entity` dans la base de données (UPDATE)
####  `UpdateWhere($entity, $values, $whereValue, $whereKey)` | 
####  `Delete($entity)` | 
####  `DeleteWhere($entity, $whereValue, $whereKey = null)` | 

## EntityFiller

La classe `Omega\Library\DAL\Filler\EntityFiller`