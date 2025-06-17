## Instalación de MySql y MySQL Workbench

Debemos ir a la pagina web de mysql:

```
https://www.mysql.com/
```

![alt text](image.png)

Y descargamos el instalador de MySQL.

![alt text](image-2.png)

Y seleccionamos el MySQL for Windows (x64),

![alt text](image-3.png)

Vamos a descargar el instalador que pesa cerca de 296 MB.
![alt text](image-4.png)

![alt text](image-5.png)

Para ejecutar el instalador, debemos darle permisos de administrador y decir que si a todo.

Una vez abierto el instalador, selccionamos la opcion de instalación personalizada.
![alt text](image-6.png)

Los productos que debemos seleccionar en la instalación es MySQL Server y MySQL Workbench, donde el primero es el motor de base de datos y el segundo es la herramienta para gestionar la base de datos, despues de seleccionarlos, damos siguiente.

![alt text](image-7.png)

Despues le damos click en ejecutar.
![alt text](image-8.png)

Presionamos siguiente y siguinete, hasta que aparezca la pantalla de Type and Network. Donde configuramos de la siguiente forma la conección de red y damos siguiente.
![alt text](image-9.png)

Utilizamos el tipo de encriptacion fuerte.
![alt text](image-10.png)

Despues configuramos el usuario root y la contraseña, en mi caso es **root** y damos siguiente.
![alt text](image-11.png)

Mantenemos la configuración estandar del servicio de windows y damos siguiente.
![alt text](image-12.png)

Garantizamos el full acceso al usuario root y damos siguiente.
![alt text](image-13.png)

Una vez completado el proceso de instalación, se muestra la siguiente pantalla.
![alt text](image-14.png)

Al dar click en finish, se muestra la siguiente pantalla, donde ya podemos iniciar la base de datos.
![alt text](image-15.png)

## Primeros pasos con MySQL Workbench

Se puede seleccionar la instancia local para conectarnos al motor de base de datos local.

En la coneccion de servidor, se muestra la siguiente pantalla, donde de lado derecho se encuentra la pestaña de administración, donde nos viene informacion del servidor.

![alt text](image-16.png)

El la opcion de Server Status se encuentra el estado del servidor, si esta corriendo o no.
![alt text](image-17.png)

Gestionar usuarios y permisos de los usuarios.
![alt text](image-18.png)

La opción de data export, nos ayuda a guardar la informacion de la base de dtos en un archivo .sql:
![alt text](image-19.png)

En la seccion de SCHEMAS, se encuentra la lista de las bases de datos que tenemos en el servidor. La base de datos es la coleccion de información, SYS es la base de datos por defecto.
![alt text](image-20.png)

La pestaña query es donde podemos escribir codigo sql.
![alt text](image-21.png)
