[Retour](../classes.md)

# La classe `TableBuilder`

Cette classe permet de générer un tableau facilement

## Exemple de code

```
<?php
$actions = new TableBuilderActions();
$actions->addHeaderAction(new TableBuilderActionHead(
    $this->getAdminLink('addEntity'),
    '<i class="fa fa-plus"></i> ' . __('Add', true),
    'btn btn-primary btn-xs')
);
$actions->addItemAction(new TableBuilderActionItem(
    $this->getAdminLink('editEntity', array('id' => ':id')),
    __('Edit', true),
    'id',
    '')
);
$actions->addItemAction(new TableBuilderActionItem(
    $this->getAdminLink('deleteEntity', array('id' => ':id')),
    __('Delete', true),
    'id',
    'delete')
);
$actions->addActionsOnColumn('title', new TableBuilderActionColumnItem(
    $this->getAdminLink('editEntity', array('id' => ':id')),
    'title',
    'id',
    '')
);

TableBuilder::Build($datas, array('Entity', 'Is Module'), array('title', 'isModule'), $actions);

?>
```

> `$datas` est un array d'objet.


## Resultat
![preview](../assets/images/tablebuilder-preview.png)