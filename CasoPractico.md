# Caso prático

Relizaremos una serie de ejercicios para ver que nos ofrece Nginx y lo que podemos hacer.

## A) Versión de Nginx instalado.

`sudo nginx -v`

[Imagen](Imegenes/Imagenes/instalacion.png)

## B) Fichero de configuración

`ls /etc/nginx`

[Imagen](Imegenes/Imagenes/fichero_configuracion.png)

## C) Servicios asociados

Como hicimos en el apartado 2 de instalación.

`systemctl status nginx`

## D) Personalizamos la página web por defecto.

Para ello tenemos que acceder con un `nano` el archivo de configuración `sudo nano /var/www/html/index.nginx-debian.html`.

Modificas el fichero a tu gusto.

[Imagen](Imegenes/Imagenes/html_nginx.png)

Guardamos y comprobamos acceciendo a http://localhost o htt://ip_servidor (la ip debe ser la externa, **la publica**).

[Imagen](Imegenes/Imagenes/localhost_nginx.png)

## E) Virtual hosting

En este apartado crearemos dos sitios web distinto. Los llamaremos web1 y web2. Cada sitio web compartira la misma dirección IP y el mismo puerto (80).

1. Creamos los directorios.

  `sudo mkdir -p /var/www/web1`

  `sudo mkdir -p /var/www/web2`

2. Asignamos los permisos a los directorios creados.

  `sudo chown -R www-data:www-data /var/www/web1`
  
  `sudo chown -R www-data:www-data /var/www/web2`
  
  `sudo chmod -R 755 /var/www/`

3. Creamos la página html.

  Creamos el index.html de *web1*.
  
   Entraremos en el archivo index.html y le añadiremos el lenguage html para que nuestra página se haga visible.

   `sudo nano /var/www/web1/index.html`

  Código html:

   ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Bienvenidos a Web1</title>
    </head>
    <body>
        <h1>Bienvenidos a la página Web1</h1>
    </body>
    </html>

   ```

  Creamos el index.html de *web2*.

  Hacemos lo mismos paso que con web1 pero para web2.
  
  `sudo nano /var/www/web1/index.html`

  Código html:

  ```html
    html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Bienvenidos a Web2</title>
    </head>
    <body>
        <h1>Bienvenidos a la página Web2</h1>
    </body>
    </html>

  ```
4.  Configurar los bloques de servidor Nginx.

   - Web1:

     `sudo nano /etc/nginx/sites-available/web1`

     Contenido:
       ```
       server {
	            listen 80;
	            server_name www.web1.org;
 
	            root /var/www/web1;
	            index index.html;
 
	            location / {
              try_files $uri $uri/ =404;
	            }
       }

       ```

       [Imagen](Imegenes/Imagenes/configuracion_bloques1.png)

     - Web2:

       `sudo nano /etc/nginx/sites-available/web2`

        ```
       server {
	            listen 80;
	            server_name www.web2.org;
 
	            root /var/www/web2;
	            index index.html;
 
	            location / {
              try_files $uri $uri/ =404;
	            }
       }

       ```

        [Imagen](Imegenes/Imagenes/configuracion_bloques2.png)

5. Habilitar sitios.

   `sudo ln -s /etc/nginx/sites-available/web1 /etc/nginx/sites-enabled/`

   `sudo ln -s /etc/nginx/sites-available/web2 /etc/nginx/sites-enabled/`

6. Configurar el archivo host para pruebas locales.

   Si no tenemos un DNS configurado, añade está entrada en el archivo `/etc/hosts`.

   ` sudo nano /etc/hosts`

   Añadele estás líneas.

   **127.0.0.1 www.web1.org**
   
   **127.0.0.1 www.web2.org**

   [Imagen](Imegenes/Imagenes/configuracion_archivo_hosts.png)

7. Probar la configuración de Nginx.

   `sudo nginx -t`

   [Imagen web1](Imegenes/Imagenes/web1.png)

   [Imagen web2](Imegenes/Imagenes/web2.png)

## F) Configuración de acceso según red (interna y externa).

  - Actualizamos la configuración de **www.web1.org**.

    Edicamos el archivo de configuración de web1 en `/etc/nginx/sites-available/web1`.

    Añadimos las líneas marcadas con un recuadro en la imagen.

    [Imagen](Imegenes/Imagenes/configuracion_we1_sites_available.png)

  - Actualizamos la configuración de **www.web2.org**.

    Edicamos el archivo de configuración de web2 en `/etc/nginx/sites-available/web2`.

    Añadimos las líneas marcadas con un recuadro en la imagen.

    [Imagen](Imegenes/Imagenes/configuracion_we1_sites_available2.png)

   - Aplicamos los cambios.

      Después de guardar los cambios de ambos archivos, recargamos la configuración de Nginx.

      `sudo nginx -t`

      `sudo systemctl reload nginx`

   - Entramos en el archivo `/etc/host` y añadimos las siguientes líneas. web1 puede acceder a la red privada y externa. web2 puede acceder a la red interna pero no a la red externa.
        
      **192..168.100.2 www.web1.org**

      **192..168.100.2 www.web2.org**

      **10.0.2.15      www.web1.org**

     [Imagen](Imegenes/Imagenes/ip_hosts.png)

      A web1 le damos acceso para que acceda a través de la red interna y externa, y web2 solo puede acceder a la red interta.

      Guardamos los cambios.

      > Nota: En caso de que no nos deje visualizar la red interna tenemos que habilitar en el firewall para que permita el tráfico al puerto 80.

      `sudo ufw allow 80/tcp`

      Comprobaciones:

      Comprobamos la red interna.

      > Nota: El nombre de mi red interna es enp0s8. Comprueba como se llama la tuya con un **ip a** o **ifconfig**.

      Desde web1 podemos acceder a la red interna.

      > Comando: `curl --interface enp0s8 http;//www.web1.org`

      [Imagen](Imegenes/Imagenes/redinterna_web1.png)

      Desde web2 podemos acceder a la red interna.

      > Comando: `curl --interface enp0s8 http;//www.web2.org`

     [Imagen](Imegenes/Imagenes/redinterna_web2.png)

      Comprobación de red externa.

      Entramos en el navegador y ponemos **http://www.web1.org**. Comprobamos que tenemos acceso.

      [Imagen](Imegenes/Imagenes/redexteriorweb1.png)

      Entramos en el navegador y ponemos **http://www.web2.org**. Comprobamos que no podemos acceder.

      [Imagen](Imegenes/Imagenes/redexteriorweb2.png)

## G) Autenticación, Autorización y Control de acceso.

En www.web1.com crearemos un directorio llamado privado, que solo tendrá acceso desde la red externa y el usuario valido.

1. Creamos un directorio llamado *privado* en web1.

   `sudo mkdir -p /var/www/web1/privado`

   `nano /var/www/web1/privado/index.html` e introducir “Contenido privado de Web1”.

   [Imagen](Imegenes/Imagenes/privadoweb1.png)

2. Instalar el módulo de autenticación básica:

   El módulo para autenticación básica ya está incluido en Nginx. Solo necesitas configurar los archivos de credenciales.

   Saltamos al siguiente punto.

3. Crear un archivo de credenciales:

   Instalar *htpasswd* si no está instalado.

   `sudo htpasswd -c /etc/nginx/.htpasswd usuario1`

   Crear un archivo para las credenciales:

   `sudo apt install apache2-utils`

   `sudo htpasswd -c /etc/nginx/.htpasswd usuario1`

   > Nota: El sistema pedirá una contraseña para el usuario usuario1.

   [Imagen](Imegenes/Imagenes/credenciales.png)

5. Configurar la autenticación en Nginx:

   Edita el archivo de configuración de web1.

   `sudo nano /etc/nginx/sites-available/web1`

   Agregue este bloque dentro de server **{}** para el directorio **/privado**:

   Añadimos las siguientes líenas.

   [Imagen](Imegenes/Imagenes/configuracion_autenticacion.png)

   Comprobamos que nos pide el acceso.

   **http://www.web1.org/privado**
   
   [Imagen](Imegenes/Imagenes/accesoprivadoweb1.png)

   Comprobamos que tenemos acceso al contenido privado.

   [Imagen](Imegenes/Imagenes/contenidoprivadoweb1.png)

## H) Autentificación, Autorización y Control de acceso.

   www.web1.org contiene un directorio llamado privado. Desde la red externa pide autorización y desde la red interna NO.

   Para que nos pida autorizacion desde la externa y la interna no. En el archivo de configuración `nano /etc/nginx/sites_available/web1` añadimos lo que está en el recuadro en la imagen.

   [Imagen](Imegenes/Imagenes/configuracionAutorizacion.png)

## I) Seguridad.

   Configuramos el sitio virtual www.web1.org para que el acceso sea seguro.

   Creamos la clave key privada para que podamos acceder a web1.

   `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/selfigned.key -out /etc/ssl/certs/selfsigned.crt`

   Rellenamos los campos para completar los pasos.
   
   País/Provincia/Localidad/Organización/Server/Correo electrónico.

   [Imagen](Imegenes/Imagenes/comandoClave.png)

   Configuramos el archivo `/etc/nginx/sites-available/web1`

   [Imagen](Imegenes/Imagenes/configuracionclave.png)
