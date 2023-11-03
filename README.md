# NGINX Web Server with PHP
---

This container is created to combine the NGINX Web server and PHP-FPM into a single container. This helps reduce sharing volumes between containers
and keeps all binaries in a single image. 

Currently Supported Versions:
- PHP 7.4
- PHP 8.0
- PHP 8.1
- PHP 8.2

All images are made using Almalinux, and includes PHP packages from Remi's RPM repository (https://rpms.remirepo.net/)

Important Information:

**Runtime User:** `www-user`
**Runtime UID:** `500:500`
**Runtime Volume:** `/var/lib/www-user`
**PHP-FPM Socket:** `/var/lib/www-user/php-www.sock`

Since the container is meant to be read-only, the Runtime Volume is defined in the image and should be generated upon creation.
All runtime/temp data is meant to be placed into there.