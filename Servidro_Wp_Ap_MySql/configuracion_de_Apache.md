---

### 1. **Acceder a la configuración de Apache**

La configuración principal de Apache se encuentra en el directorio `/etc/apache2/`. Dentro de este directorio, encontrarás varios archivos y subdirectorios importantes:

*   `apache2.conf`: Archivo de configuración principal de Apache.
*   `ports.conf`: Configuración de los puertos que Apache escucha (normalmente 80 para HTTP y 443 para HTTPS).
*   `sites-available/`: Directorio que contiene los archivos de configuración de los sitios web disponibles.
*   `sites-enabled/`: Directorio que contiene enlaces simbólicos a los archivos de configuración de los sitios web que están habilitados.
*   `mods-available/`: Directorio que contiene los módulos disponibles de Apache.
*   `mods-enabled/`: Directorio que contiene enlaces simbólicos a los módulos que están habilitados.

---

### 2. **Configurar un Virtual Host para WordPress**

Un Virtual Host permite que Apache sirva múltiples sitios web desde un mismo servidor. Vamos a crear un archivo de configuración para WordPress.

#### a. Crea un nuevo archivo de configuración

Primero, crea un nuevo archivo de configuración en el directorio `sites-available/`. Puedes usar `nano` o tu editor de texto preferido:

```bash
sudo nano /etc/apache2/sites-available/wordpress.conf
```

#### b. Añade la configuración del Virtual Host

Dentro del archivo, añade la siguiente configuración. Asegúrate de cambiar `tu_dominio` por `localhost` o la IP de tu servidor si estás trabajando localmente:

```apache
<VirtualHost *:80>
    ServerAdmin tu_correo@example.com
    DocumentRoot /var/www/html
    ServerName localhost

    <Directory /var/www/html/>
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Aquí te explico cada línea:

- `<VirtualHost *:80>`: Define un Virtual Host que escucha en el puerto 80 (HTTP).
- `ServerAdmin tu_correo@example.com`: Tu correo electrónico de administrador.
- `DocumentRoot /var/www/html`: La carpeta donde están los archivos de WordPress.
- `ServerName localhost`: El nombre del servidor (puede ser `localhost` o la IP de tu servidor).
- `<Directory /var/www/html/>`: Define las opciones para el directorio de WordPress.
- `AllowOverride All`: Permite que el archivo `.htaccess` de WordPress funcione correctamente.
- `ErrorLog ${APACHE_LOG_DIR}/error.log`: Define dónde se guardan los errores.
- `CustomLog ${APACHE_LOG_DIR}/access.log combined`: Define dónde se guardan los registros de acceso.

Guarda y cierra el archivo.

#### c. Habilita el Virtual Host

Para habilitar el Virtual Host, crea un enlace simbólico desde `sites-available/` a `sites-enabled/`:

```bash
sudo a2ensite wordpress.conf
```

#### d. Deshabilita el Virtual Host por defecto

Si tienes el Virtual Host por defecto habilitado, desactívalo para evitar conflictos:

```bash
sudo a2dissite 000-default.conf
```

#### e. Reinicia Apache

Para que los cambios surtan efecto, reinicia Apache:

```bash
sudo systemctl restart apache2
```

---

### 3. **Habilitar el módulo `rewrite`**

El módulo `rewrite` es necesario para que los enlaces permanentes de WordPress funcionen correctamente.

```bash
sudo a2enmod rewrite
```

Reinicia Apache de nuevo:

```bash
sudo systemctl restart apache2
```

---

### 4. **Verificar la configuración**

Abre tu navegador y entra a `http://localhost` o la IP de tu servidor. Deberías ver la página de inicio de WordPress. Si ves algún error, revisa los archivos de registro de Apache (`/var/log/apache2/error.log` y `/var/log/apache2/access.log`) para identificar el problema.

---

### 5. **Configurar el archivo `.htaccess` (si es necesario)**

WordPress utiliza el archivo `.htaccess` para configurar los enlaces permanentes. Si no tienes uno, puedes crearlo en el directorio de WordPress (`/var/www/html/`).

```bash
sudo nano /var/www/html/.htaccess
```

Añade la siguiente configuración:

```apache
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
```

Guarda y cierra el archivo.

---

### 6. **Finalizar la instalación de WordPress**

Si aún no lo has hecho, abre tu navegador y entra a `http://localhost` o la IP de tu servidor. Sigue los pasos para finalizar la instalación de WordPress:

- Elige un idioma.
- Pon el nombre del sitio, usuario administrador, contraseña, etc.
