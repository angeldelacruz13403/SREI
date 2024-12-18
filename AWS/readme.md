---

### Configuración de Autenticación y Cifrado SSL en Apache

---

## **🔍 Propósito**
Implementar autenticación y encriptación SSL en un servidor Apache, asegurando un entorno web protegido y cifrado.

---

### **🔐 Configuración de Autenticación**

#### **Paso 1: Instalar el servidor LAMP**

Primero, instalamos Apache junto con los módulos necesarios:

```bash
sudo apt install apache2 php libaprutil1-dbd-mysql -y
```

#### **Paso 2: Instalar y configurar MariaDB**

Actualizamos paquetes e instalamos MariaDB:

```bash
sudo apt update
sudo apt install mariadb-server mariadb-client -y
```

#### **Paso 3: Iniciar servicios**

Asegúrate de que Apache y MariaDB estén activos y configurados para iniciarse automáticamente:

```bash
sudo systemctl start apache2
sudo systemctl start mariadb
sudo systemctl enable apache2
sudo systemctl enable mariadb
```

#### **Paso 4: Crear y configurar la base de datos**

Inicia sesión como administrador de MariaDB:

```bash
sudo mysql -u root -p
```

Crea una base de datos y otorga privilegios al usuario que manejará la autenticación:

```sql
CREATE DATABASE auth_db;
GRANT ALL PRIVILEGES ON auth_db.* TO 'auth_user'@'localhost' IDENTIFIED BY 'secure_password';
FLUSH PRIVILEGES;
```

En esta base de datos, crearemos una tabla para los usuarios:

```sql
USE auth_db;
CREATE TABLE user_auth (
    username VARCHAR(255) NOT NULL PRIMARY KEY,
    passwd VARCHAR(255) NOT NULL,
    group_name VARCHAR(255)
);
```

Agregamos un usuario con contraseña encriptada:

```bash
htpasswd -nb user password | awk '{print $2}' 
```

El hash generado se almacena en la tabla:

```sql
INSERT INTO user_auth (username, passwd, group_name) VALUES ('user', '{SHA}hash_generado', 'group1');
```

#### **Paso 5: Configuración de Apache para autenticación**

Activa los módulos requeridos:

```bash
sudo a2enmod dbd authn_dbd authn_socache
```

Edita el archivo de configuración de Apache para proteger un directorio específico:

```bash
<VirtualHost *:80>
    DocumentRoot "/var/www/html"
    <Directory "/var/www/html/protecteddir">
        AuthType Basic
        AuthName "Restricted Area"
        AuthBasicProvider socache dbd
        AuthDBDUserPWQuery "SELECT passwd FROM user_auth WHERE username = %s"
        Require valid-user
    </Directory>
</VirtualHost>
```

Crea el directorio protegido y reinicia Apache:

```bash
sudo mkdir /var/www/html/protecteddir
sudo chown -R www-data:www-data /var/www/html/protecteddir
sudo systemctl restart apache2
```

---

### **🔒 Configuración de SSL**

#### **Paso 1: Activar módulo SSL**

Habilita SSL y reinicia el servicio:

```bash
sudo a2enmod ssl
sudo systemctl restart apache2
```

#### **Paso 2: Generar un certificado SSL**

Genera un certificado autofirmado:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```

#### **Paso 3: Configurar Apache para SSL**

Edita la configuración del sitio SSL predeterminado:

```bash
sudo nano /etc/apache2/sites-available/default-ssl.conf
```

Asegúrate de incluir las rutas al certificado y clave generados:

```bash
SSLEngine on
SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
```

#### **Paso 4: Activar configuración SSL**

Habilita el sitio SSL y reinicia Apache:

```bash
sudo a2ensite default-ssl.conf
sudo systemctl restart apache2
```

---

### **💡 Verificación**
1. Accede al sitio mediante HTTPS en tu navegador.
2. Si el navegador indica que el certificado no es de confianza, instálalo manualmente para evitar advertencias.

---
