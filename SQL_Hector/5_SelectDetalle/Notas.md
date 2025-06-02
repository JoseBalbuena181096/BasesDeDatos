### CASE
 
Un case en SQL es una forma de hacer una validacion similar a un if, permite ejecutar cierto codigo dependiendo de una condicion.

```sql
SELECT name,
	CASE 
		WHEN currency = 'MXN' THEN 'Peso Mexicano'
        WHEN currency = 'USD' THEN 'Dólar americano'
        ELSE 'Otra moneda'
	END as 'Moneda'
FROM customer;
```
 
- La query anterior sirve para mostrar el nombre de los clientes y su moneda, si la moneda es MXN se muestra Peso Mexicano, si es USD se muestra Dólar americano y si es otra moneda se muestra Otra moneda.


### SUBQUERIES

Un subquery es una query que se encuentra dentro de otra query. Pueden tener problemas de rendimiento si no se usan correctamente. Y un problema de legibilidad.

```sql 
SELECT name,
	IFNULL((SELECT name FROM city
    WHERE city.id = customer.city_id), 'No tiene ciudad') as 'city'
FROM customer;
```

La query anterior sirve para mostrar el nombre de los clientes y su ciudad, la ciudad se obtiene de la tabla city.
El problema con las subconsutas es qle el sub query se ejecuta por cada fila de la tabla customer, por lo que si la tabla customer tiene 100 filas, el sub query se ejecutara 100 veces.

```sql
SELECT c.name
FROM (SELECT name FROM customer
	 WHERE city_id IS NOT NULL) as c
ORDER BY name ASC;
```

La query anterior sirve para mostrar el nombre de los clientes que tengan una ciudad asignada.

```sql
SELECT name,
	(SELECT COUNT(*)
    FROM customer
    WHERE city.id = customer.city_id) as 'Cantidad'
FROM  city;
```
La query anterior sirve para mostrar el nombre de la ciudad y la cantidad de clientes que tengan asignada esa ciudad.

```sql

  
SELECT name
FROM city
WHERE (SELECT COUNT(*)
	FROM customer 
    WHERE city.id = customer.city_id) > 0;
```
La query anterior sirve para mostrar el nombre de la ciudad que tenga al menos un cliente asignado.

### LIMIT

El limit es una clausula que se utiliza para limitar el numero de filas que se devuelven de una query.

```sql
SELECT name
FROM city
LIMIT 10;
```
La query anterior sirve para mostrar el nombre de las primeras 10 ciudades.

```sql
SELECT id, name
FROM customer
LIMIT 5, 5;
```
La query anterior sirve  mostrar el nombre de los clientes, desde el cliente 5 hasta los proximos 5 clientes.

```sql
SELECT id, name
FROM customer
ORDER BY id DESC
LIMIT 3, 5;
```
La query anterior sirve para mostrar el nombre de los clientes, desde el cliente 3 hasta los proximos 5 clientes, ordenados por id en orden descendente.


### INSERT CON SELECT

El insert con select es una forma de insertar datos en una tabla, utilizando los datos de otra tabla.

 
```sql
INSERT INTO customer(name, email)
SELECT  name, 'email@tec.mx' FROM city;
```
La query anterior sirve para insertar el nombre de las ciudades y un email en la tabla customer.

### ORDER BY

```sql
SELECT RAND();
```
La query anterior sirve para mostrar un numero aleatorio.

```sql
SELECT id,name
FROM customer
ORDER BY RAND();
```
La query anterior sirve para mostrar el nombre de los clientes ordenados aleatoriamente.

```sql
SELECT id,name
FROM customer
ORDER BY RAND()
LIMIT 3;
```
La query anterior sirve para mostrar el nombre de los clientes ordenados aleatoriamente, limitando el resultado a 3 filas.