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
- >: Mayor que
- <=: Menor o igual que
- >=: Mayor o igual que
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
