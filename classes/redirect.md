[Retour](../classes.md)

# La classe `Redirect`

## Vers un URL
```
Redirect::toUrl($url);
```

## Vers la page précédante
```
Redirect::toLastPage();
```

## Vers une action d'un controlleur (back-office)
```
Redirect::toAction($controller, $action, $param);
```
- `$controller` : le nom du controlleur
- `$action` : le nom de l'action
- `$param` : un `array` contenant les paramètres (querystring)

## Vers une action d'un controlleur (front-end)
```
Redirect::toPAction($controller, $action, $param);
```
- `$controller` : le nom du controlleur
- `$action` : le nom de l'action
- `$param` : un `array` contenant les paramètres (querystring)