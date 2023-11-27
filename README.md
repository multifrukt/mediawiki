# mediawiki

[MediaWiki](https://en.wikipedia.org/wiki/MediaWiki) is the engine that powers Wikipedia. It is open-source software and can be deployed locally using Docker

## Initial setup:

Clone repository, run inside the repository root:

```bash
docker-compose up
```

Open http://localhost/ to configure mediawiki instance:
```agsl
database type: MariaDB
database host: db
database name: my_wiki
Database table prefix: [not required]
Database username: admin
Database password: admin
Database account for web access: Use the same account as for installation
```

Download `LocalSettings.php` file, put it into web container (created from `mediawiki` image) under path `/var/www/html` and set read permissions:

```agsl
docker ps
docker cp LocalSettings.php [mediawiki_container_ID]:/var/www/html/LocalSettings.php
docker exec [mediawiki_container_ID] chmod +r /var/www/html/LocalSettings.php
```

Open http://localhost/, log in, create new page

## Adminer

To connect directly to the database using Adminer, open:  
http://localhost:8080/  
Log in using:
```agsl
System: MySQL
Server: db
Username: admin
Password: admin
Database: my_wiki
```

## Troubleshooting:

After initial setup is complete, MediaWiki will always redirect to its original server name used during setup process. For example, if setup was done locally using http://localhost/ page and later some user tries to access the server remotely using http://server-network-name/, the engine will automatically redirect to http://localhost/. To access MediaWiki using any hostname change `$wgServer` in `LocalSettings.php` file:
```agsl
$wgServer = (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == 'on' ? 'https' : 'http') . '://' . $_SERVER['HTTP_HOST'];
```
