### OPERADORES LÓGICOS AND, OR, NOT

Length en sql permite obtener la longitud de una cadena de texto.

```sql
SELECT id, name
FROM customer
WHERE LENGTH(name) > 5;
```

La query anterior sirve para mostrar el id y el nombre de los clientes cuyo nombre tenga mas de 5 caracteres.

```sql
SELECT id, name
FROM customer
WHERE LENGTH(name) > 5 AND city_id IS NOT NULL;
```

La query anterior sirve para mostrar el id y el nombre de los clientes cuyo nombre tenga mas de 5 caracteres y que Existan una ciudad asignada.

```sql
SELECT id, name
FROM customer
WHERE LENGTH(name) > 5 OR city_id IS NOT NULL;
```

La query anterior sirve para mostrar el id y el nombre de los clientes cuyo nombre tenga mas de 5 caracteres o que Existan una ciudad asignada.

```sql
SELECT * FROM customer
WHERE NOT(name ='ana');
```

La query anterior sirve para mostrar el id y el nombre de los clientes cuyo nombre no sea ana.

### Operadores de Comparación

Los operadores de comparación permiten comparar valores. En sql son:

- =: Igual
- <: Menor que
- > : Mayor que
- <=: Menor o igual que
- > =: Mayor o igual que
- <>: Diferente de

```sql
SELECT * FROM customer
WHERE LENGTH(name) <= 5;
```

La query anterior sirve para mostrar el id y el nombre de los clientes cuyo nombre tenga menos de 5 caracteres.

```sql
SELECT * FROM customer
WHERE LENGTH(name) <> 4;
```

-La query anterior sirve para mostrar el id y el nombre de los clientes cuyo nombre no tenga 4 caracteres.

Similar a la siguiente query:

```sql
SELECT * FROM customer
WHERE LENGTH(name) != 4;
```

```sql
SELECT * FROM customer
WHERE created_at > '2024-08-24 17:00:00';
```

La query anterior retorna los clientes que se crearon despues de la fecha indicada.

```sql
SELECT * FROM customer
WHERE created_at < '2024-08-24 17:00:00';
```

La query anterior retorna los clientes que se crearon antes de la fecha indicada.

```sql
SELECT * FROM customer
WHERE day(created_at) < 24;
```

La query anterior retorna los clientes que se crearon antes del dia 24.

### BETWEEN

El operador BETWEEN permite seleccionar un rango de valores. Muy usado en fechas.

```sql
SELECT * FROM customer
WHERE created_at BETWEEN '2025-01-01 17:00:00' AND '2025-08-24 17:00:00';
```

La query anterior retorna los clientes que se crearon entre las fechas indicadas.

```sql
SELECT * FROM customer
WHERE id
	BETWEEN 5 AND 15;
```

La query anterior retorna los clientes que se crearon entre los id indicados.

```sql
SELECT SUBSTRING(UPPER(name), 1, 1), name
FROM customer
WHERE SUBSTRING(UPPER(name), 1, 1)
    BETWEEN 'A' AND 'P';
```

La query anterior retorna los clientes que su nombre empieza con una letra entre A y P.

## IN

El operador IN permite seleccionar un rango de valores.

```sql
SELECT * FROM customer
WHERE id IN (1, 2, 3);
```

La query anterior retorna los clientes que su id sea 1, 2 o 3.

```sql
SELECT * FROM customer
WHERE id NOT IN (1, 2, 3);
```

La query anterior retorna los clientes que su id no sea 1, 2 o 3.

```sql
SELECT * FROM customer
WHERE name NOT IN("josesillo", "JOSE MANUEL");
```

La query anterior retorna los clientes que su nombre no sea josesillo o jose manuel.

```sql
SELECT * FROM city
WHERE id IN(
	SELECT DISTINCT city_id FROM customer
    WHERE city_id IS NOT NULL
);
```

La query anterior retorna las ciudades que el id de la ciudad no sea nulo.

```sql
SELECT name,
	CASE
		WHEN name IN("josesillo", "JOSE MANUEL") THEN 'No aprobo'
       ELSE 'Aprobados'
	END AS 'STATUS'
FROM customer;
```

La query anterior retorna los clientes que su nombre sea josesillo o jose manuel, y ademas agrega una columna STATUS que indica si aprobo o no.

## ANY

El operador ANY permite seleccionar un rango de valores.

Vamos a modificar la tabla customer añadiendo una columna llamada "balance", con decimales de 8 cifras y 2 decimales:

```sql
ALTER TABLE customer
ADD balance DECIMAL(8, 2);
```

Actalizamos los balances de los clientes:

```sql
UPDATE `store`.`customer` SET `balance` = '12' WHERE (`id` = '1');
UPDATE `store`.`customer` SET `balance` = '11' WHERE (`id` = '2');
UPDATE `store`.`customer` SET `balance` = '14' WHERE (`id` = '3');
UPDATE `store`.`customer` SET `balance` = '15' WHERE (`id` = '4');
UPDATE `store`.`customer` SET `balance` = '22' WHERE (`id` = '6');
UPDATE `store`.`customer` SET `balance` = '56' WHERE (`id` = '5');
UPDATE `store`.`customer` SET `balance` = '76' WHERE (`id` = '7');
UPDATE `store`.`customer` SET `balance` = '43' WHERE (`id` = '8');
UPDATE `store`.`customer` SET `balance` = '42' WHERE (`id` = '9');
UPDATE `store`.`customer` SET `balance` = '24' WHERE (`id` = '10');
UPDATE `store`.`customer` SET `balance` = '86' WHERE (`id` = '11');
UPDATE `store`.`customer` SET `balance` = '12' WHERE (`id` = '12');
UPDATE `store`.`customer` SET `balance` = '12' WHERE (`id` = '13');
UPDATE `store`.`customer` SET `balance` = '32' WHERE (`id` = '16');
UPDATE `store`.`customer` SET `balance` = '45' WHERE (`id` = '17');
UPDATE `store`.`customer` SET `balance` = '55' WHERE (`id` = '18');
UPDATE `store`.`customer` SET `balance` = '76' WHERE (`id` = '19');
UPDATE `store`.`customer` SET `balance` = '45' WHERE (`id` = '20');
UPDATE `store`.`customer` SET `balance` = '90' WHERE (`id` = '21');
UPDATE `store`.`customer` SET `balance` = '19' WHERE (`id` = '22');
UPDATE `store`.`customer` SET `balance` = '23' WHERE (`id` = '23');
UPDATE `store`.`customer` SET `balance` = '21' WHERE (`id` = '24');
UPDATE `store`.`customer` SET `balance` = '28' WHERE (`id` = '25');
```

Crear una nueva tabla product:

```sql
CREATE TABLE product(
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(50) NOT NULL,
    price DECIMAL(8, 2)
);
```

```sql
SELECT name, balance FROM customer
WHERE balance > ANY (SELECT price FROM product);
```

La query anterior retorna los clientes que su balance sea mayor a alguno de los precios de los productos.

```sql
SELECT name, balance FROM customer
WHERE balance != ANY (SELECT price FROM product);
```

La query anterior retorna los clientes que su balance sea distinto a alguno de los precios de los productos.

## ALL

El operador ALL permite seleccionar un rango de valores.

```sql
SELECT name, balance FROM customer
WHERE balance > ALL (SELECT price FROM product);
```

La query anterior retorna los clientes que su balance sea mayor a todos los precios de los productos.

## LIKE

El operador LIKE permite seleccionar un rango de valores.

```sql
SELECT name FROM customer
WHERE name LIKE '%a%';
```

La query anterior retorna los clientes que su nombre contenga la letra a.

```sql
SELECT name FROM customer
WHERE UPPER(name) LIKE '%A%A%';
```

La query anterior retorna los clientes que su nombre contenga dos letras a.

```sql
SELECT name, email FROM customer
WHERE email LIKE '%@gmail.com';
```

La query anterior retorna los clientes que su correo sea gmail.

```sql
SELECT name FROM customer
WHERE UPPER(name) LIKE '__A%';
```

La query anterior retorna los clientes que su nombre tenga dos caracteres seguidos de una a.
