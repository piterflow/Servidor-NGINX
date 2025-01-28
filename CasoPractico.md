# Caso prático

Relizaremos una serie de ejercicios para ver que nos ofrece Nginx y lo que podemos hacer.

## A) Versión de Nginx instalado.

`sudo nginx -v`

IMAGEN ...

## B) Fichero de configuración

`ls /etc/nginx`

IMAGEN ...

## C) Servicios asociados

Como hicimos en el apartado 2 de instalación.

`systemctl status nginx`

## D) Personalizamos la página web por defecto.

Para ello tenemos que acceder con un `nano` el archivo de configuración `sudo nano /var/www/html/index.nginx-debian.html`.

Modificas el fichero a tu gusto.

IMAGEN...

Guardamos y comprobamos acceciendo a http://localhost o htt://ip_servidor (la ip debe ser la externa, **la publica**).

IMAGEN...

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

       IMAGEN...

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

        IMAGEN...

5. Habilitar sitios.

   `sudo ln -s /etc/nginx/sites-available/web1 /etc/nginx/sites-enabled/`

   `sudo ln -s /etc/nginx/sites-available/web2 /etc/nginx/sites-enabled/`

6. Configurar el archivo host para pruebas locales.

   Si no tenemos un DNS configurado, añade está entrada en el archivo `/etc/hosts`.

   ` sudo nano /etc/hosts`

   Añadele estás líneas.

   **127.0.0.1 www.web1.org**
   
   **127.0.0.1 www.web2.org**

   IMAGEN...

7. Probar la configuración de Nginx.

   `sudo nginx -t`

   IMAGENweb1...

   IMAGENweb2...

## F) Configuración de acceso según red (interna y externa).

  - Actualizamos la configuración de **www.web1.org**.

    Edicamos el archivo de configuración de web2 en `/etc/nginx/sites-available/web1`.

    Añadimos las líneas marcadas con un recuadro en la imagen.

    IMAGEN...

  - Actualizamos la configuración de **www.web2.org**.

    Edicamos el archivo de configuración de web2 en `/etc/nginx/sites-available/web2`.

    Añadimos las líneas marcadas con un recuadro en la imagen.

    IMAGEN...

    - Aplicamos los cambios.

      Después de guardar los cambios de ambos archivos, recargamos la configuración de Nginx.

      `sudo nginx -t`

      `sudo systemctl reload nginx`

      - Entramos en el archivo `/etc/host` y añadimos las siguientes líneas. web1 puede acceder a la red privada y externa. web2 puede acceder a la red interna pero no a la red externa.
        
      **192..168.100.2 www.web1.org**

      **192..168.100.2 www.web2.org**

      **10.0.2.15      www.web1.org**

      A web1 le damos acceso para que acceda a través de la red interna y externa, y web2 solo puede acceder a la red interta.

      Guardamos los cambios.

      > Nota: En caso de que no nos deje visualizar la red interna tenemos que habilitar en el firewall para que permita el tráfico al puerto 80.

        `sudo ufw allow 80/tcp`

      Comprobaciones:

      Comprobamos la red interna.

      < Nota: El nombre de mi red interna es enp0s8. Comprueba como se llama la tuya con un **ip a** o **ifconfig**.

      Desde web1 podemos acceder a la red interna.

      > Comando: `curl --interface enp0s8 http;//www.web1.org`

      IMAGEN...

      Desde web2 podemos acceder a la red interna.

      > Comando: `curl --interface enp0s8 http;//www.web2.org`

      IMAGEN...

      Comprobación de red externa.

      Entramos en el navegador y ponemos **http://www.web1.org**. Comprobamos que tenemos acceso.

      IMAGEN...

       Entramos en el navegador y ponemos **http://www.web2.org**. Comprobamos que no podemos acceder.

      IMAGEN...
