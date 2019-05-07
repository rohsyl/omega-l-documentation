[Back](../../classes.md)

# The `Entity` facade

This class is usefull in the font-end part of the CMS.

It allow you to get instance of the following classes.

## Site
An instance of the class  [Site](./site.md). To get informations about the site.
```
$site = Entity::Site();
```

## Page
An instance of the class  [Page](./page.md). To get content of the page.
```
$page = Entity::Page();
```

## Menu
An instance of the class  [Menu](./menu.md). To configure and render the menu.
```
$menu = Entity::Menu();
```
