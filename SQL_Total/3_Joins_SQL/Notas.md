# JOIN SQL

Son una forma de combinar filas de dos o mas tablas en una sola consulta.
![alt text](image.png)

Poder unir informacion de una o mas tablas, para dar respuesta a preguntas complejas.

Por ejemplo tenemos las siguientes tablas Series y Episodios :

![alt text](image-1.png)

Por ejemplo buscar todos los episodios de una serie en particular.

![alt text](image-2.png)

```sql
SELECT
    E.episodio_id,
    E.serie_id,
    S.titulo AS 'Nombre de la Serie'
FROM episodios E
JOIN series S ON E.serie_id = S.serie_id
WHERE
    S.titulo = 'Stranger Things';
```

El dato en comun entre las dos tablas, se le denomina clave foranea, y es el que une las tablas.
![alt text](image-3.png)

Existen diferentes tipos de JOINs, los cuales se diferencian por la forma en que se unen las tablas.

Los JOINs mas comunes son:

- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL JOIN
- CROSS JOIN

![alt text](image-4.png)

## INNER JOIN

El INNER JOIN es el JOIN mas comun, y es el que se usa por defecto. Une las tablas solo si hay coincidencias en la clave foranea.

En el siguiente diagrama la operación del INNER JOIN se representa como la aparte donde estos dos conjuntos de datos se intersectan. Registros que coincidan en ambas tablas.

![alt text](image-5.png)

Unir la tabla de series con la tabla de actuaciones:

```sql
SELECT * FROM series
INNER JOIN Actuaciones ON Series.serie_id = Actuaciones.serie_id;
```

Si solo quisieramos el titulo y personaje de la union de la tabla series y actuaciones:

```sql
SELECT
    S.titulo,
    A.personaje
FROM series S
INNER JOIN Actuaciones A ON S.serie_id = A.serie_id;
```

EL INNER JOIN es el JOIN mas comun, y es el que se usa por defecto. Une las tablas solo si hay coincidencias en la clave foranea.

Con el INNER JOIN tambien podemos usar el WHERE para filtrar los resultados. Por ejemplo buscar todos los actores de una serie en particular como Stranger Things.

```sql
SELECT
    S.titulo,
    A.personaje
FROM series S
INNER JOIN Actuaciones A ON S.serie_id = A.serie_id
WHERE
    S.titulo = 'Stranger Things';
```

### Práctica INNER JOIN 1

#### Enunciado:

Escribe una consulta SQL que seleccione todos los campos de las tablas Series y Episodios donde el serie_id coincida entre ambas tablas, utiliza un INNER JOIN para realizar el join entre ambas tablas.

#### Aclaración Importante

Debido a una limitación técnica de la plataforma de codificación de Udemy, debes limitar el resultado a 10 filas (porque excede la cantidad de datos disponibles a imprimir en pantalla). Debes utilizar LIMIT 10 al final de tu consulta.

#### Sugerencias:

Aplica INNER JOIN para unir las tablas Series y Episodios

Asegúrate de que el join utilice como clave el campo serie_id que es común en ambas tablas, en este caso, ON Series.serie_id = Episodios.serie_id

Syntaxis de ayuda:

SELECT \* FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;

```SQL
SELECT *
FROM Series S
INNER JOIN Episodios E
ON  S.serie_id = E.serie_id
LIMIT 10;
```

## Práctica INNER JOIN 2

### Enunciado:

Escribe una consulta SQL que te permita obtener el título de la serie, el título de cada episodio y su duración de la serie 'Stranger Things'.

El resultado final debe contar con los siguientes nombres de columnas: titulo_serie, titulo_episodio, duracion.

### Sugerencias:

Para lograr esto, necesitarás hacer uso de INNER JOIN para combinar la tabla Series con Episodios basándote en la clave común serie_id

Utiliza la cláusula WHERE para filtrar los resultados solo para Serie.titulo = 'Stranger Things'

Recuerda que puedes utilizar AS para asignar un alias a una columna. Asegúrate de asignar alias apropiados a los títulos para diferenciarlos.

### Base de datos NetflixDB

#### Tabla: Actores

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| actor_id | int |
| nombre | text |
| fecha_nacimiento | date |
+-------------------+----------+
actor_id es la clave primaria (columna con valores únicos) para esta tabla.
Cada fila de esta tabla indica el ID, nombre y fecha de nacimiento de un actor.

#### Tabla: Series

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| serie_id | int |
| titulo | text |
| descripcion | text |
| año_lanzamiento | int |
| genero | text |
+-------------------+----------+
serie_id es la clave primaria (columna con valores únicos) para esta tabla.
Cada fila de esta tabla indica el ID, título, descripción, año de lanzamiento y género de una serie.

#### Tabla: Episodios

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| episodio_id | int |
| serie_id | int |
| titulo | text |
| duracion | int |
| rating_imdb | int |
| temporada | int |
| descripcion | text |
| fecha_estreno | date |
+-------------------+----------+
episodio_id es la clave primaria (columna con valores únicos) para esta tabla.
serie_id es una clave foránea (columnas de referencia) de la serie_id de la tabla Series.
Cada fila de esta tabla indica el ID de un episodio, el ID de la serie a la que pertenece, título, duración, rating en IMDb, temporada, descripción y fecha de estreno.

#### Tabla: Actuaciones

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| actor_id | int |
| serie_id | int |
| personaje | text |
+-------------------+----------+
actor_id y serie_id son claves primarias compuestas para esta tabla, y cada una es también una clave foránea que referencia las tablas Actores y Series, respectivamente.
Cada fila de esta tabla indica el ID del actor, el ID de la serie y el personaje interpretado por el actor en la serie.

```SQL
SELECT S.titulo AS titulo_serie, E.titulo AS titulo_episodio,  duracion
FROM Series S
INNER JOIN Episodios E
ON S.serie_id = E.serie_id
WHERE  S.titulo = 'Stranger Things';
```

## LEFT JOIN

El LEFT JOIN une las tablas solo si hay coincidencias en la clave foranea, pero si no hay coincidencias, se devuelven NULL. La diferencia con el INNER JOIN es que el LEFT JOIN devuelve todas las filas de la tabla izquierda, y las filas de la tabla derecha que coincidan con la clave foranea.

En el siguiente diagrama la operación del LEFT JOIN se representa como los datos de la tabla izquierda, y los datos de la tabla derecha que coincidan con la clave foranea. Lo de color rojo.

![alt text](image-6.png)

LEFT JOIN = LEFT OUTER JOIN

Ejemplo, tener una lista de todas las series, y cualquier episodio que tenga.

```SQL
SELECT series.titulo AS 'Titulo de la serie', episodios.titulo AS 'Titulo del episodio'
FROM series
LEFT JOIN episodios ON series.serie_id = episodios.serie_id
ORDER BY series.titulo;
```

## Práctica LEFT JOIN 1

### Enunciado:

Escribe una consulta SQL que devuelva, para cada serie, su título, el título de cada episodio asociado (si hay alguno), y el rating de IMDb.

Los alias exactos que debes aplicar son: Título de la Serie, Título del Episodio, Rating IMDB

Ordena los resultados por el título de la serie de forma ascendente

### Sugerencias:

Aplica LEFT JOIN para unir la tabla Series con Episodios, utilizando serie_id como la llave común.

La tabla de la izquierda debe ser Series y la de la derecha Episodios

Asegúrate de que el join utilice como clave el campo serie_id que es común en ambas tablas, en este caso, ON Series.serie_id = Episodios.serie_id

Syntaxis de ayuda:

SELECT \* FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;

Base de datos NetflixDB

### Tabla: Actores

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| actor_id | int |
| nombre | text |
| fecha_nacimiento | date |
+-------------------+----------+
actor_id es la clave primaria (columna con valores únicos) para esta tabla.
Cada fila de esta tabla indica el ID, nombre y fecha de nacimiento de un actor.

### Tabla: Series

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| serie_id | int |
| titulo | text |
| descripcion | text |
| año_lanzamiento | int |
| genero | text |
+-------------------+----------+
serie_id es la clave primaria (columna con valores únicos) para esta tabla.
Cada fila de esta tabla indica el ID, título, descripción, año de lanzamiento y género de una serie.

### Tabla: Episodios

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| episodio_id | int |
| serie_id | int |
| titulo | text |
| duracion | int |
| rating_imdb | int |
| temporada | int |
| descripcion | text |
| fecha_estreno | date |
+-------------------+----------+
episodio_id es la clave primaria (columna con valores únicos) para esta tabla.
serie_id es una clave foránea (columnas de referencia) de la serie_id de la tabla Series.
Cada fila de esta tabla indica el ID de un episodio, el ID de la serie a la que pertenece, título, duración, rating en IMDb, temporada, descripción y fecha de estreno.

### Tabla: Actuaciones

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| actor_id | int |
| serie_id | int |
| personaje | text |
+-------------------+----------+
actor_id y serie_id son claves primarias compuestas para esta tabla, y cada una es también una clave foránea que referencia las tablas Actores y Series, respectivamente.
Cada fila de esta tabla indica el ID del actor, el ID de la serie y el personaje interpretado por el actor en la serie.

```sql
SELECT S.titulo AS 'Título de la Serie', E.titulo AS 'Título del Episodio', E.rating_imdb  'Rating IMDB'
FROM Series S
LEFT JOIN episodios E ON S.serie_id = E.serie_id
ORDER BY S.titulo ASC;
```

## Práctica LEFT JOIN 2

### Enunciado:

Escribe una consulta SQL que muestre el título de la serie, el título de cada episodio, y el rating de IMDb para todos los episodios de la serie 'Stranger Things'

Ordena los resultados por Episodios.rating_imdb de forma descendente (de mayor a menor) según rating de imdb

Los alias exactos que debes aplicar sobre las columnas son: Título de la Serie, Título del Episodio, Rating IMDB

### Sugerencias:

Aplica LEFT JOIN para unir la tabla Series con Episodios, utilizando serie_id como la llave común

La tabla de la izquierda debe ser Series y la de la derecha Episodios

Asegúrate de que el join utilice como clave el campo serie_id que es común en ambas tablas, en este caso, ON Series.serie_id = Episodios.serie_id

Utiliza la cláusula WHERE para filtrar Series.titulo = 'Stranger Things'

### Base de datos NetflixDB

#### Tabla: Actores

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| actor_id | int |
| nombre | text |
| fecha_nacimiento | date |
+-------------------+----------+
actor_id es la clave primaria (columna con valores únicos) para esta tabla.
Cada fila de esta tabla indica el ID, nombre y fecha de nacimiento de un actor.

#### Tabla: Series

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| serie_id | int |
| titulo | text |
| descripcion | text |
| año_lanzamiento | int |
| genero | text |
+-------------------+----------+
serie_id es la clave primaria (columna con valores únicos) para esta tabla.
Cada fila de esta tabla indica el ID, título, descripción, año de lanzamiento y género de una serie.

#### Tabla: Episodios

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| episodio_id | int |
| serie_id | int |
| titulo | text |
| duracion | int |
| rating_imdb | int |
| temporada | int |
| descripcion | text |
| fecha_estreno | date |
+-------------------+----------+
episodio_id es la clave primaria (columna con valores únicos) para esta tabla.
serie_id es una clave foránea (columnas de referencia) de la serie_id de la tabla Series.
Cada fila de esta tabla indica el ID de un episodio, el ID de la serie a la que pertenece, título, duración, rating en IMDb, temporada, descripción y fecha de estreno.

#### Tabla: Actuaciones

+-------------------+----------+
| Nombre de Columna | Tipo |
+-------------------+----------+
| actor_id | int |
| serie_id | int |
| personaje | text |
+-------------------+----------+
actor_id y serie_id son claves primarias compuestas para esta tabla, y cada una es también una clave foránea que referencia las tablas Actores y Series, respectivamente.
Cada fila de esta tabla indica el ID del actor, el ID de la serie y el personaje interpretado por el actor en la serie.

```SQL
SELECT S.titulo AS 'Título de la Serie', E.titulo AS 'Título del Episodio', E.rating_imdb  'Rating IMDB'
FROM Series S
LEFT JOIN episodios E ON S.serie_id = E.serie_id
WHERE S.titulo = 'Stranger Things'
ORDER BY E.rating_imdb DESC;
```
