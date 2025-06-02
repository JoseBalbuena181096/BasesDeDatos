## COUNT

```sql
SELECT COUNT(*) FROM customer;
```

La query anterior retorna la cantidad de clientes.

```sql
SELECT COUNT(city_id) FROM customer;
```

La query anterior retorna la cantidad de clientes por ciudad.

```sql
SELECT count(DISTINCT name)
FROM customer;
```

- La query anterior retorna la cantidad de clientes con un nombre distinto.

```sql
SELECT name FROM customer
GROUP BY name;
```

- La query anterior retorna el nombre de los clientes agrupados por nombre.

```sql
SELECT name, DATE(created_at) AS 'Fecha', COUNT(*) AS 'Cantidad'
FROM customer
GROUP BY name, DATE(created_at);
```

- La query anterior retorna el nombre de los clientes agrupados por nombre y fecha de creacion.

## HAVING

Trabaja con funciones de agregacion. Y se puede usar para filtrar los resultados de una consulta.

```sql
SELECT name, COUNT(*) as 'cantidad'
FROM customer
GROUP BY name
HAVING COUNT(*) > 1;
```

La query anterior retorna el nombre de los clientes agrupados por nombre y la cantidad de clientes con un nombre distinto.

```sql
SELECT name, COUNT(*) as 'cantidad'
FROM customer
WHERE name != 'ana'
GROUP BY name
HAVING COUNT(*) > 1;
```

La diferencia entre WHERE y HAVING es que WHERE se ejecuta antes de agrupar y HAVING se ejecuta despues de agrupar.

```sql
SELECT DATE(created_at) AS 'Fecha', SUM(balance)
FROM customer
GROUP BY DATE(created_at);
```

La query anterior retorna la fecha de creacion y la suma de los balances agrupados por fecha.

```sql
SELECT DATE(created_at) AS 'Fecha', AVG(balance)
FROM customer
GROUP BY DATE(created_at);
```

La query anterior retorna la fecha de creacion y el promedio de los balances agrupados por fecha.

```sql
SELECT DATE(created_at) AS 'Fecha', AVG(balance)
FROM customer
GROUP BY DATE(created_at)
HAVING AVG(balance) > 7;
```

La query anterior retorna la fecha de creacion y el promedio de los balances agrupados por fecha, pero solo para aquellas fechas donde el promedio sea mayor a 7.

## MIN MAX

```sql
SELECT MIN(balance)
FROM customer;
```

La query anterior retorna el minimo de los balances.

```sql
SELECT MAX(balance)
FROM customer;
```

La query anterior retorna el maximo de los balances.

```sql
SELECT name, MIN(balance) as 'Minimo',
MAX(balance) as 'Maximo'
FROM customer
GROUP BY name;
```

La query anterior retorna el nombre de los clientes agrupados por nombre y el minimo y maximo de los balances.

```sql
SELECT DATE(created_at), GROUP_CONCAT(DISTINCT name ORDER BY name SEPARATOR ' | ') as 'Nombres'
FROM customer
GROUP BY DATE(created_at);
```

La query anterior retorna la fecha de creacion y los nombres distintos de los clientes agrupados por fecha. Ordenados por nombre y separados por comas.

```sql
SELECT DATE(created_at),
    GROUP_CONCAT(name ORDER BY UPPER(name) SEPARATOR ' | ') as 'Nombres'
FROM customer
GROUP BY DATE(created_at);
```

La query anterior retorna la fecha de creacion y los nombres de los clientes agrupados por fecha. Ordenados por nombre en mayusculas y separados por |.

```sql
SELECT DATE(created_at),
    GROUP_CONCAT(name ORDER BY UPPER(name) SEPARATOR ' | ') as 'Nombres'
FROM customer
GROUP BY DATE(created_at)
HAVING COUNT(*) > 2;
```

La query anterior retorna la fecha de creacion y los nombres de los clientes agrupados por fecha. Ordenados por nombre en mayusculas y separados por |. Pero solo para aquellas fechas donde la cantidad de clientes sea mayor a 2.
