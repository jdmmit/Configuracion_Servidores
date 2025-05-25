---

### 1. **Actualiza el sistema**

Primero, siempre es buena práctica actualizar los paquetes:

```bash
sudo apt update
sudo apt upgrade -y
```

---

### 2. **Instala Apache (el servidor web)**

Apache es el servidor web más común y fácil de usar para WordPress.

```bash
sudo apt install apache2 -y
```

Para comprobar que funciona, abre tu navegador y entra a `http://localhost` o la IP de tu servidor. Deberías ver la página de bienvenida de Apache.

---

### 3. **Instala MySQL (la base de datos)**

WordPress necesita una base de datos para guardar la información.

```bash
sudo apt install mysql-server -y
```

Cuando termine, ejecuta el siguiente comando para asegurar la instalación y ponerle contraseña al usuario root de MySQL:

```bash
sudo mysql_secure_installation
```

Sigue las instrucciones (puedes decir que sí a todo para mayor seguridad).

---

### 4. **Crea la base de datos y usuario para WordPress**

Entra a MySQL:

```bash
sudo mysql -u root -p
```

Dentro de la consola de MySQL, ejecuta estos comandos (puedes cambiar los nombres por los que prefieras):

```sql
CREATE DATABASE wordpress_db DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'tu_contraseña_segura';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

### 5. **Instala PHP y módulos necesarios**

WordPress está hecho en PHP, así que necesitas instalarlo junto con algunos módulos:

```bash
sudo apt install php libapache2-mod-php php-mysql php-xml php-gd php-curl php-mbstring php-xmlrpc php-soap php-intl php-zip -y
```

Reinicia Apache para que reconozca PHP:

```bash
sudo systemctl restart apache2
```

---

### 6. **Descarga y configura WordPress**

Descarga la última versión de WordPress:

```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
```

Copia los archivos a la carpeta de tu sitio web (por defecto es `/var/www/html`):

```bash
sudo cp -R wordpress/* /var/www/html/
```

Dale permisos a los archivos para que Apache pueda usarlos:

```bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```

---

### 7. **Configura WordPress**

Cambia al directorio de tu sitio:

```bash
cd /var/www/html
```

Copia el archivo de configuración de ejemplo:

```bash
sudo cp wp-config-sample.php wp-config.php
```

Edita el archivo de configuración:

```bash
sudo nano wp-config.php
```

Busca las siguientes líneas y pon los datos de tu base de datos:

```php
define( 'DB_NAME', 'wordpress_db' );
define( 'DB_USER', 'wp_user' );
define( 'DB_PASSWORD', 'tu_contraseña_segura' );
define( 'DB_HOST', 'localhost' );
```

Guarda y cierra el archivo (en nano, es Ctrl+O, Enter, y luego Ctrl+X).

---

### 8. **Finaliza la instalación desde el navegador**

Abre tu navegador y entra a `http://localhost` o la IP de tu servidor. Deberías ver la pantalla de instalación de WordPress. Ahí solo sigue los pasos: elige idioma, nombre del sitio, usuario admin, etc.

---

### 9. **¡Listo!**

Ya tienes WordPress funcionando localmente en tu servidor Ubuntu.

---
