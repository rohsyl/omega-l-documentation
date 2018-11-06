[Retour](../classes.md)

# !!! deprecated !!!

> Utiliser la structure [DAL BLL DTO](dal-bll-dto.md)

# Requête à la base de données avec `Dbs`

## Sélection : `Dbs::select()`

### Un `SELECT` simple sur plusieurs lignes
```
$stmt = Dbs::select('*')
        ->from('table')
        ->run();
```
> Il y a deux options pour parcourire le résulat de la requête

1. En utilisant l'objet `SelectReturn`

```
for($i = 0; $i < $stmt->length(); $i++){
    echo $stmt->getInt($i, 'columnNamePk') . '<br />';
    echo $stmt->getString($i, 'columnNameString') . '<br />';
}
```
`getHtmlString()`, `getString()`, `getDouble()`, `getInt()`, `getBool()`, `getDate()` ou `getValue()`

2. En utilisant un `array`

```
$datas = $stmt->getAllArray();
   
foreach($datas as $row){
    echo $row['columnNamePk'] . '<br />';
    echo $row['columnNameString] . '<br />';
}
```
    
### Un `SELECT` avec un `WHERE` sur la clé primaire
```
$stmt = Dbs::select('*')
        ->from('table')
        ->where('id', '=', '?')
        ->prepare(array($id))
        ->run();
        
if($stmt->length() == 0){
    echo 'Aucune ligne correspondant à cette condition';
    return null;
}

echo $stmt->getInt(0, 'columnNamePk');
// ou
$row = $stmt->getRowArray(0);
echo $row['columnNamePk'];
```

### Un `SELECT` d'un scalaire
```
$count = Dbs::select('count(*)')
        ->from('table')
        ->runScalar();
```

### Un `SELECT` complet (presque)
```
$stmt = Dbs::select('id', 'A.column1 AS column1A', 'column2', 'columnB')
        ->from('tableA AS A')
        ->join('inner', 'tableB AS B', 'B.id', 'A.fkB')
        ->where('column1A', '>', ?)
        ->andwhere('column2', '<', ?)
        ->prepare(array($colum1, $column2))
        ->run();
```
> Si vous ne pouvez pas faire tout ce que vous voulez avec cette manière de faire, regardez directement la section 
"Requête à la base de données directement avec une instance de PDO"

## Insertion : `Dbs::insert()`
```
$result = Dbs::insert('table')
    ->field('column1', 'column2')
    ->value($value1, $value2)
    ->run();
```
> `$result` contient `false` si l'insertion n'a pas réussi, sinon il contient l'id de la ligne insérée.

## Modification : `Dbs::update()`
```
$result = Dbs::update('table')
    ->field('column1', 'column2')
    ->value($value1, $value2)
    ->where('id', '=', $id)
    ->run();
```
> `$result` contient `true` ou `false` selon si la modification est réussie ou non.

## Suppression : `Dbs::delete()`
```
$result = Dbs::delete('table')
    ->where('id', '=', $id)
    ->run();
```
> `$result` contient `true` ou `false` selon si la modification est réussie ou non.

## `Dbs::query()`
```
$datas = Dbs::query('SELECT * FROM table');
```
> Utilise la méthode `query` de PDO et retourne `$stmt->fetchAll();`

## `Dbs::exec()`
```
$datas = Dbs::exec('DELETE FROM table WHERE id = 2');
```
> Utilise la méthode `exec` de PDO et retourne son résultat


# Requête à la base de données directement avec une instance de `PDO`

## Avoir une instance de `PDO`
```
$pdo = Dbs::getConnexion();
```

## Se connecter à un autre base de données avec `PDO`
```
$pdo = Dbs::getConnexion($db_host, $db_name, $db_user, $db_pass);
```
> Que faire avec cet objet `$pdo` ? Voir directement la [Documentation de PDO](http://php.net/manual/fr/book.pdo.php)