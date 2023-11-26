# mediawiki

## mediawiki initial setup:

Clone repository, run in repository root:

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

Download `LocalSettings.php` file, put it into web container under path `/var/www/html` and set read permissions:

```agsl
docker ps
docker cp LocalSettings.php [mediawiki_container_ID]:/var/www/html/LocalSettings.php
docker exec [mediawiki_container_ID] chmod +r /var/www/html/LocalSettings.php
```

Open http://localhost/, log in, create new page

## Adminer

To connect directly to the database using Adminer:
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

By default MediaWiki is redirecting to its original server name when accessed with a different hostname. To access MediaWiki using another hostname change `$wgServer` in `LocalSettings.php` file:
```agsl
$wgServer = (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == 'on' ? 'https' : 'http') . '://' . $_SERVER['HTTP_HOST'];
```
