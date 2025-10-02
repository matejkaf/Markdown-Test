


```
──(kali㉿kali)-[~/Markdown-Test]
└─$ systemctl status nginx
○ nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset>
     Active: inactive (dead)
       Docs: man:nginx(8)

                                                                             
┌──(kali㉿kali)-[~/Markdown-Test]
└─$ ss -tlnp | grep 80    
                                                                             
┌──(kali㉿kali)-[~/Markdown-Test]
└─$ nginx -v                                
nginx version: nginx/1.26.3
```

nginx ist also installiert, aber nicht gestartet

```sh
sudo systemctl start nginx   # starten
sudo systemctl enable nginx  # on startup
ss -tlnp | grep 80 # test
```

```sh
sudo apt update
sudo apt install php php-fpm
```

```sh
$ php -v           
PHP 8.4.11 (cli) (built: Aug 15 2025 23:34:18) (NTS)
Copyright (c) The PHP Group
Built by Debian
Zend Engine v4.4.11, Copyright (c) Zend Technologies
    with Zend OPcache v8.4.11, Copyright (c), by Zend Technologies
```

Also es ist php 8.4

```sh
sudo systemctl start php8.4-fpm
sudo systemctl status php8.4-fpm

sudo systemctl enable php8.4-fpm
```

Bezeichnung des sockets ermittel:

```sh
sudo find /run/php/ -name "*.sock"
/run/php/php-fpm.sock
/run/php/php8.4-fpm.sock
```

Den String `/run/php/php8.4-fpm.sock` brauchen wir für das nginx Konfigurationsfile:

```sh
sudo nano /etc/nginx/sites-available/default
```

Hier `index-php` hinzufügen und den `location ~ \.php$` anpassen (auskommentieren)

```
server {
    index index.php ...;

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
    }

...
}
```

Nginx-Konfiguration testen

```sh
sudo nginx -t
```

Nginx-Konfiguration neu starten

```sh
sudo systemctl reload nginx
```

Datei erstellen

```sh
sudo nano /var/www/html/info.php
```


```php
<?php
phpinfo();
?>
```

Und testen:

```url
http://localhost/info.php
```

# Access

Noch nicht getestet

User Kali den Zugriff auf das dir erlauben

User in die Gruppe aufnehmen:

```sh
sudo usermod -aG www-data kali
```

setze das Gruppenrecht auf das Verzeichnis. Ursprünglich `root:root`

```
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 775 /var/www/html
```

# MariaDB

Check ob aufgesetzt:

```sh
$ dpkg -l | grep mariadb
```

Check ob service gestartet:

```sh
$ systemctl status mariadb
```

Starten und dauerhaft enablen

```sh
$ sudo systemctl start mariadb    
$ sudo systemctl enable mariadb
```

```sh
$ mysql -u root -p 
Enter password: 
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```

Als root wird man nicht nach einem PW gefragt

```sh
$ sudo mysql
```

Nun ein PW für root festlegen

```sh
$ sudo mysql_secure_installation
```


PHP Fehlermeldungen anzeigen für debugging

```sh
$ sudo tail /var/log/nginx/error.log -f
```


