[Plugin](./plugin.md) -
[Install](./install.md) - 
[Classes](./classes.md) - 
[JavaScript](./js.md) - 
[Multi-langue](./mlang.md) - 
[Theme](./theme.md)

UPDATED FOR OMEGA-L

# Installing / Migrating / Updating OmegaCMS

In this section, you will find all informations about the installation, the migration and the update of Omega-L

## Installation
### Requirements

As Omega-L use Laravel 5.7, you must check out the requirement of [Laravel](https://laravel.com/docs/5.7/installation)

### Install
1. Configure the webserver
Check [Laravel](https://laravel.com/docs/5.7/installation) installations guide to configure the webserver.

2. Clone the repository
```
git clone git@github.com:rohsyl/omega-l.git
```

3. Install php dependencies
```
composer install
```

4. Install css/js dependencies
```
npm install
```

5. Compile css/js assets
	- For development
	```
	npm run dev
	```
	- For production
	```
	npm run prod
	```

6. Create a database

7. Configure ´.env´
	- Copy and rename the ´.env.exemple´ to ´.env´
	- Edit the ´.env´
	- Database settings
	Set database connection informations
	```
	DB_CONNECTION=mysql
	DB_HOST=127.0.0.1
	DB_PORT=3306
	DB_DATABASE=omega-l
	DB_USERNAME=user
	DB_PASSWORD=password
	```
	> Laravel documentations about [database](https://laravel.com/docs/5.7/database)
	- Save the file

8. Generate Laravel App Key
```
php artisan key:generate
```

9. Install Omega-L with the webinstall

As now everything about laravel is ready, we can install Omega-L

- Launch your browser and navigate to the URL that point to your Omega-L instance.
- You will be redirected to the installation page
- Follow the easy step and finally click on the "Do the installation" button.
- Wait...
- You will be redirected to the login page for the back-office
- Login with the informations you provided during the installation
- Now Omega-L


## First step after a fresh installation of Omega-L

TODO: more details about this

- Install a theme and define it as the "in use" theme.
- Install needed plugins
- Create some pages
- Set the homepage
- Enjoy!



## Migration

How to move an Omega-L instance from a server to another (Exemple: dev to prod)

In this exemple, we will move from DEV:`http://omega-l.local` to PROD:`http://rohs.ch/`
Omega-L is installed on DEV and we want to move it to PROD

1. Do a dump of the database on DEV
2. Restore the dump on PROD
3. Upload all file from DEV to PROD
	> If you have an SSH access with npm and composer, you can upload files without the npm_modules and vendor directories and then install them directly on the server. It will be faster.
	
	> Don't forget to upload `/public/media` that contains all your medias.
4. Compile assets for prod
5. Edit the `.env` file
	- Check the database connection informations
	- Set the `APP_ENV` to production
	-	Set the `APP_DEBUG` to false
	- Set the `APP_URL` to the PROD domain name
6. Omega-L is now available on PROD `http://rohs.ch/`



## Updating
How to update Omega-L

1. Update the files with git from doing a checkout from the last stable release

2. Update the database
```
php artisan migrate
```
