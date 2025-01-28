# Instalación del servidor Nginx

La instalación la haremos en Debian, sistema basado en GNU/Linux de código abierto.

La instalación de algo muy sencilla, en caso de que los comandos no funcionen en tu distribución, entra en la página oficial de tu sistema operativo y busca la documentación para comprobar que comandos debes utilizar a corde a la práctica.

## Instalación basado en Debian

1. Actualizamos nuestro sistema.
   
    Con estos comando actualizaremos la lista de paquete del sistema y actualizamos los paquetes que ya tengamos instalado.

    ``` apt update ```

    ``` apt upgrade ```
  
  2. Instalación de Nginx.

     Con el comando apt get install [paquete] se no instalará el servidor. Con la opción -y ya le indicamos que si se instale.

     ``` apt install nginx -y ```

4. Comprobar que Nginx se instalo correctamente.

      Iniciamos el servicio.
  
     ``` systemclt start nginx ```

      Permitimos el servicio.
  
     ``` systemctl enable nginx ```

      Comprobamos el estado.
  
     ```systemctl status nginx ```

5. Vericamos la instalación.

    Para comprobar que verificar que se ha instalado correctamente. Abrimos nuestro navegador, en la barra de busqueda ponemos **http://localhost** y se mostrará la página de bienvenida que   tiene Nginx.

6. Conclución.

   Con está simple instalación ya tenemos disponible nuestro servidor Nginx. Con el puedes prácticar el crear una página web estática en local o realizar un proxy inverso.
