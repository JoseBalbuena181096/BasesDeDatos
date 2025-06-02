## Insertar datos en una tabla

Insertar un solo registro, haciendo uso de los nombres de las columnas:

```sql
INSERT INTO customer (name, email) VALUES ('Jose Balbuena', 'jose.balbuena.palma@tec.mx');
```

Insertar múltiples registros:

```sql
INSERT INTO customer (name, email) VALUES 
	('Jose Manuel', 'jose.manuel@tec.mx'),
    ('Jorge Perez', 'jorge.perez@tec.mx'),
    ('Josue Marcos', 'josue.marcos@tec.mx');
```

Seleccionar información de la base de datos:

```sql
SELECT * FROM customer;
```
- Esta instrucción selecciona todos los registros de la tabla customer, se trae todas las columnas y todas las filas, el uso de * es un comodín que selecciona todo.

```sql
SELECT * FROM customer
LIMIT 0,2;
```
- Esta instrucción selecciona los primeros 2 registros de la tabla customer, comenzando desde el registro 0.

Si quisiera ver los registros desde el 2 hasta el 4:

```sql
SELECT * FROM customer
LIMIT 2,4;
```

Seleccionar una columna en particular:

```sql
SELECT name FROM customer;
```
Colocar elementos adicionales en la tabla, que originalmente no estan:

```sql
SELECT name, 'Holi desde SQL' FROM customer;
```

Funciones que tranforman el resultado de la consulta:

```sql
SELECT name, UPPER(name) FROM customer;
```
- UPPER(name) convierte el nombre a mayusculas.

```sql
SELECT name, LOWER(name) FROM customer;
```
- LOWER(name) convierte el nombre a minusculas.

```sql
SELECT name, LENGTH(name) FROM customer;
```
- LENGTH(name) devuelve la longitud del nombre.

Los alias son nombres alternativos para columnas o tablas, se utiliza as para crear un alias:

```sql
SELECT name, LENGTH(name) AS long_name FROM customer;
```
- AS long_name es un alias para la columna LENGTH(name).

### Ordenar la información al consultar

```sql
SELECT name FROM customer
ORDER BY id;
```
- Ordena los registros por el id en orden ascendente.

```sql
SELECT name FROM customer
ORDER BY id DESC;
```
- Ordena los registros por el id en orden descendente.

- Ordenar usando el nombre, de forma ascendente y por orden alfabetico:

```sql
SELECT name FROM customer
ORDER BY LOWER(name);
```

Combinar ordenamineto descendente, y descencendente:

```sql
SELECT name FROM customer
ORDER BY LOWER(name) DESC, email;
```

### Filtrar la información al consultar

```sql
SELECT name FROM customer
WHERE name = 'Jose Balbuena';
```

- Un WHERE es una clausula que filtra los registros, es decir, selecciona solo los registros que cumplen con la condicion. La condicion se compara con el valor de la columna. 

```sql
SELECT name FROM customer
WHERE LOWER(name) = 'jose balbuena';
```

- Busca registros que coincidan con la condicion, en este caso, que el nombre sea 'Jose Balbuena', no distingue entre mayusculas y minusculas.


```sql
SELECT name FROM customer
WHERE LOWER(name) = 'jose balbuena' OR LOWER(name) = 'jose manuel';
```

- Busca registros que coincidan con la condicion, en este caso, que el nombre sea 'Jose Balbuena' o 'Jose Manuel', no distingue entre mayusculas y minusculas. El operador OR es un operador lógico que permite buscar registros que cumplen con una de las condiciones.

```sql
SELECT name FROM customer
WHERE LOWER(name) = 'jose balbuena' AND email = 'jose.balbuena.palma@tec.mx';
```

- Busca registros que coincidan con la condicion, en este caso, que el nombre sea 'Jose Balbuena' y el email sea 'jose.balbuena.palma@tec.mx', no distingue entre mayusculas y minusculas. El operador AND es un operador lógico que permite buscar registros que cumplen con todas las condiciones.

Operadores logicos:

- AND: Permite buscar registros que cumplen con todas las condiciones.
- OR: Permite buscar registros que cumplen con una de las condiciones.
- NOT: Permite buscar registros que no cumplen con la condicion.

Operadores de comparacion:

- =: Igual
- !=: Distinto
- >: Mayor
- <: Menor
- >=: Mayor o igual
- <=: Menor o igual

```sql
SELECT name FROM customer
WHERE LENGTH(name) > 5;
```
- Busca registros que coincidan con la condicion, en este caso, que la longitud del nombre sea mayor a 5.

### Modificar la información de la base de datos

```sql
UPDATE customer
SET name = 'Jose Palma'
WHERE id = 1;
```
- Actualiza el nombre del registro con id = 1.

```sql
UPDATE customer SET name = UPPER(name)
WHERE name = 'Jose Palma';
```
- Actualiza el nombre del registro con el nombre 'Jose Palma', convirtiendo el nombre a mayusculas.

Desactivar el modo safe update, en mysql por defecto esta activado, lo que significa que no se puede modificar una fila sin especificar una condicion.

```sql
SET SQL_SAFE_UPDATES = 0;
```
- al desactivar el modo safe update, se puede modificar una fila sin especificar una condicion con el id.

```sql
UPDATE customer SET name = 'josesillo', email = 'josesillo@tec.mx'
WHERE id = 1;
```
- Actualiza el nombre y el email del registro con id = 1. Mas de un campo del registro puede ser modificado.

```sql
UPDATE customer SET name = UPPER(name)
WHERE id IN (2, 4, 5);
```
- Actualiza el nombre del registro con id = 2, 4 y 5, convirtiendo el nombre a mayusculas.

```sql
DELETE FROM customer
WHERE id = 1;
```
- Elimina el registro con id = 1. La condicion where es obligatoria, al momenr de eliminar un registro, se debe especificar una condicion.

### Agrupar la informacion al consultar

Agrupar los datos que son identicos, y poder hacer algo con ello, el maxiomo de ellos, el menor de todos, sumarlos, ect.

```sql

SELECT name, COUNT(*) AS 'cantidad' 
FROM customer
GROUP BY name;
```
- Agrupa los registros por el nombre, y cuenta la cantidad de registros por nombre.

