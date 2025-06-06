## Store Procedure

Un store procedure es un conjunto de sentencias SQL que se ejecutan como una unidad. El cual puede ser invocado por su nombre. Ya esta compilado y optimizado por el motor de base de datos.

```sql
USE store;
DROP PROCEDURE IF EXISTS CustomerWithBalance;

DELIMITER $$

CREATE PROCEDURE CustomerWithBalance()
BEGIN
    SELECT name FROM customer
    WHERE balance > 0;
END;
$$

DELIMITER ;
```

El anterior store procedure retorna una tabla con los nombres de los clientes que tienen un balance mayor a 100.

El store procedure se invoca de la siguiente manera:

```sql
CALL CustomerWithBalance();
```

## Funciones vs Store Procedure

Las funciones retornan un valor y pueden ser invocadas como una expresion solo retornando un valor. Por lo tanto, no pueden tener sentencias DML (INSERT, UPDATE, DELETE). En cambio, los store procedure pueden tener sentencias DML y no retornan un valor. Pero un store procedure retorna una tabla completa o un conjunto de tablas y valores.
| | Función | Asignatura |
|----|-------------------|--------------|
| Return | 1, "Nombre", 2022-08-05 | fn(1, 5, tabla) |
| Efectos Secundarios | No hace cambios en la tabla | Si realiza cambios en la tabla |
| No resultados | 1 int date | int var SELECT SELECT CUST CI ||
| 5 | SELECT Fn() c1, c2,c3 FROM tabla o SELECT \* FROM tabla WHERE Fn(n) > 5 | Call Store Procedure |

### Retornar multiples valores

```SQL
USE store;
DROP PROCEDURE IF EXISTS GetCityData;

DELIMITER $$

CREATE PROCEDURE GetCityData(cityID INT)
BEGIN
    DECLARE CityName VARCHAR(50);

    SELECT name INTO CityName
    FROM city WHERE id = cityId;

    -- PRIMER RESULTADO
    SELECT CityName;

    -- SEGUNDO RESULTADO
    SELECT name FROM customer
    WHERE city_id = cityId;

    -- TERCER RESULTADO
    SELECT SUM(balance) as 'Total'
    FROM customer
    WHERE city_id = cityId;
END;
$$

DELIMITER ;
```

El store procedure anterior retorna 3 resultados:

1. El nombre de la ciudad
2. La lista de clientes de la ciudad
3. El total de balance de los clientes de la ciudad

La forma de invocar el store procedure es la siguiente:

```SQL
CALL GetCityData(1);
```

### Modificar valores con store procedure

```SQL
USE store;
DROP PROCEDURE IF EXISTS UpdateNameCity;

DELIMITER $$

CREATE PROCEDURE UpdateNameCity(cityId INT, newName VARCHAR(50))
BEGIN
    UPDATE city
    SET name = newName
    WHERE id = cityId;

    SELECT COUNT(*) AS 'Total Clientes'
    FROM customer
    WHERE city_id = cityId;
END;
$$

DELIMITER ;
```

El store procedure anterior actualiza el nombre de la ciudad y retorna el total de clientes de la ciudad.

La forma de invocar el store procedure es la siguiente:

```SQL
CALL UpdateNameCity(1, 'New Zealand');
```

### Crear paginados con store procedure

```SQL
USE store;
DROP PROCEDURE IF EXISTS GetCustomers;

DELIMITER $$

CREATE PROCEDURE GetCustomers(quantity INT, page INT)
BEGIN
    DECLARE rows_offset INT;
    SET rows_offset = (page - 1) * quantity;

    SELECT * FROM customer
    ORDER BY id
    LIMIT rows_offset, quantity;
END;
$$

DELIMITER ;
```

La forma de invocar el store procedure es la siguiente:

```SQL
CALL GetCustomers(4, 1);
```

## Busquedas dinámicas

Las busquedas dinamicas son aquellas que se pueden realizar desde el ORM tambien optimizan la consulta.

```sql
USE store;
DROP PROCEDURE IF EXISTS GetCustomersSearch;

DELIMITER $$

CREATE PROCEDURE GetCustomersSearch(customerName VARCHAR(50), cityId INT, dateMonth INT)
BEGIN
    SELECT * FROM customer
    WHERE (customerName IS NULL OR name LIKE customerName)
    AND (cityId IS NULL OR city_id = cityId)
    AND (dateMonth IS NULL OR MONTH(created_at) = dateMonth);
END$$

DELIMITER ;
```

El store procedure anterior retorna la lista de clientes que coinciden con los filtros. Los filtros son opcionales. Y son los siguientes:

1. customerName: nombre del cliente
2. cityId: id de la ciudad
3. dateMonth: mes de creacion

La forma de invocar el store procedure es la siguiente:

```SQL
CALL GetCustomersSearch(NULL, 1, NULL);
```

### Paginados dinamicos

```sql
USE store;
DROP PROCEDURE IF EXISTS GetCustomersSearch;

DELIMITER $$

CREATE PROCEDURE GetCustomersSearch(quantity INT, page INT, customerName VARCHAR(50), cityId INT, dateMonth INT)
BEGIN
    DECLARE rows_offset INT;
    SET rows_offset = (page - 1) * quantity;

    SELECT * FROM customer
    WHERE (customerName IS NULL OR name LIKE customerName)
    AND (cityId IS NULL OR city_id = cityId)
    AND (dateMonth IS NULL OR MONTH(created_at) = dateMonth)
    ORDER BY id
    LIMIT rows_offset, quantity;
END$$

DELIMITER ;
```

La forma de invocar el store procedure es la siguiente:

```SQL
CALL GetCustomersSearch(2, 1, NULL, 1, NULL);
```
