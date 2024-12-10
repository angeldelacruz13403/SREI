# Configuración de un Servidor Web para un Instituto

## Paso 1: Instalación de Apache
```
sudo apt update
sudo apt install apache2
```

## Paso 2: Configuración de dominios locales
Editar `/etc/hosts` y agregar:
```
127.0.0.1   centro.intranet
127.0.0.1   departamentos.centro.intranet
```

## Paso 3: Instalación y configuración de PHP y MySQL
```
sudo apt install php libapache2-mod-php php-mysql
sudo apt install mysql-server
sudo a2enmod php
sudo systemctl restart apache2
```

## Paso 4: Instalación de WordPress
Crear base de datos y usuario:
```
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
Descargar e instalar WordPress:
```
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
sudo mv wordpress /var/www/html/centro.intranet
sudo chown -R www-data:www-data /var/www/html/centro.intranet
```
Configurar Apache:
```
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```
Config:
```
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/html/centro.intranet
    <Directory /var/www/html/centro.intranet>
        AllowOverride All
    </Directory>
</VirtualHost>
```
Habilitar sitio:
```
sudo a2ensite centro.intranet.conf
sudo systemctl reload apache2
```

## Paso 5: Activación del módulo WSGI
```
sudo apt install libapache2-mod-wsgi-py3
sudo a2enmod wsgi
sudo systemctl restart apache2
```

## Paso 6: Creación de una aplicación Python
Crear directorio y archivo:
```
sudo mkdir /var/www/html/departamentos.centro.intranet
sudo nano /var/www/html/departamentos.centro.intranet/app.py
```
Contenido de `app.py`:
```
def application(environ, start_response):
    status = '200 OK'
    output = b'Hello World from Python!'
    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)
    return [output]
```
Configurar Apache:
```
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```
Config:
```
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    WSGIScriptAlias / /var/www/html/departamentos.centro.intranet/app.py
    <Directory /var/www/html/departamentos.centro.intranet>
        Require all granted
    </Directory>
</VirtualHost>
```
Habilitar sitio:
```
sudo a2ensite departamentos.centro.intranet.conf
sudo systemctl reload apache2
```

## Paso 7: Protección con autenticación básica
Crear usuario:
```
sudo htpasswd -c /etc/apache2/.htpasswd usuario
```
Modificar configuración:
```
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```
Agregar:
```
AuthType Basic
AuthName "Área Restringida"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```
Reiniciar Apache:
```
sudo systemctl restart apache2
```

## Paso 8: Configuración de AWStats
```
sudo apt install awstats
sudo a2enmod cgi
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
sudo nano /etc/awstats/awstats.centro.intranet.conf
```
Modificar:
```
LogFile="/var/log/apache2/access.log"
SiteDomain="centro.intranet"
```
Configurar cron:
```
sudo nano /etc/cron.d/awstats
```
Agregar:
```
*/10 * * * * www-data /usr/lib/cgi-bin/awstats.pl -config=centro.intranet -update > /dev/null
```

## Paso 9: Instalación de nginx
Instalar y configurar nginx:
```
sudo apt install nginx
sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
```
Config:
```
server {
    listen 8080;
    server_name servidor2.centro.intranet;
    root /var/www/html/servidor2.centro.intranet;
    index index.php index.html index.htm;
    location / {
        try_files $uri $uri/ =404;
    }
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }
}
```
Activar configuración:
```
sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```
Instalar PHP-FPM y phpMyAdmin:
```
sudo apt install php-fpm
sudo apt install phpmyadmin
```
Configurar phpMyAdmin:
```
sudo ln -s /usr/share/phpmyadmin /var/www/html/servidor2.centro.intranet/phpmyadmin
```
