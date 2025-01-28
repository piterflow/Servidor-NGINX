# Introducción a Nginx

En está práctica detallaremos los pasos a seguir para la instalación de Nginx, un servidor web de código abierto que desde su exito también es usado como proxy inverso, cache de HTTP,    balanceador de carga.

  Haremos un caso práctico donde iremos haciendo unas comprobaciones y aplicandole restricciones de acceso a nuestro servidor. De esta forma comprobaremos que tan práctico es tener un servidor web con Ngnix.
  
   Debido a que sus raíces yacen en la optimización del rendimiento bajo escala, Nginx a menudo supera a otros populares servidores web en pruebas de rendimiento (Benchmarks), especialmente en situaciones con contenido estático y/o un elevado número de solicitudes concurrentes, es por eso que Kinsta usa Nginx para impulsar su hosting.

**¿Qué es Nginx?**
--
Nginx (pronunciado "engine-x") es un servidor web de código abierto que también puede funcionar como proxy inverso, balanceador de carga y caché HTTP. Fue creado para resolver el problema del C10K, que se refiere a la capacidad de manejar 10,000 conexiones simultáneas de manera eficiente. Es ampliamente utilizado en entornos de alto tráfico, como sitios web grandes y aplicaciones en la nube.

## Características principales de Nginx

**Alto rendimiento:** Utiliza un modelo de arquitectura asíncrono y orientado a eventos, lo que lo hace ideal para manejar múltiples conexiones simultáneas.

**Proxy inverso:** Puede redirigir solicitudes a otros servidores, lo que es útil para distribuir carga o servir contenido dinámico.

**Balanceador de carga:** Distribuye el tráfico entre varios servidores para mejorar la disponibilidad y el rendimiento.

**Caché HTTP:** Almacena contenido estático para reducir el tiempo de respuesta y la carga en los servidores backend.

**Configuración flexible:** Su archivo de configuración es modular y permite personalizar el comportamiento del servidor.
