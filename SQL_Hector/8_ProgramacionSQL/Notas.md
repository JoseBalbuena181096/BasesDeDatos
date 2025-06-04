## DELIMITER

Se usa para cambiar el delimitador de la consulta, por defecto es ;

```sql
DELIMITER $$
```

```sql
$$
CREATE FUNCTION  Hi(name VARCHAR(50))
RETURNS VARCHAR(200)
DETERMINISTIC
BEGIN
	DECLARE message VARCHAR(200);
	SET message = CONCAT('Hi ', name);
	RETURN message;
END;
$$
```

- La anterior query crea una funcion que recibe un nombre y retorna un saludo.
- Deterministic indica que la funcion es determinista, es decir, que siempre retorna el mismo resultado para los mismos argumentos.

```sql
DROP FUNCTION IF EXISTS Hi;
```

- La anterior query elimina la funcion Hi si existe.

```sql
SELECT Hi('Jose');
```

- Ejecuta la funcion Hi con el argumento 'Jose' y retorna el resultado.

## IF

Se usa para ejecutar una sentencia si se cumple una condicion.

```sql
USE store;

DROP FUNCTION IF EXISTS NumberDescription;

DELIMITER $$
$$
CREATE FUNCTION NumberDescription(number INT)
RETURNS VARCHAR(8)
DETERMINISTIC
BEGIN
	DECLARE description VARCHAR(8);
    IF number < 0 THEN
		SET description = 'Negative';
	ELSEIF number > 0 THEN
		SET description = 'Positive';
	ELSE
		SET description = 'Zero';
	END IF;
	RETURN description;
END;

$$
```

- La anterior query crea una funcion que recibe un numero y retorna una descripcion, si el numero es negativo retorna 'Negative', si es positivo retorna 'Positive' y si es 0 retorna 'Zero'.

```sql
SELECT NumberDescription(10);
```

- Ejecuta la funcion NumberDescription con el argumento 10 y retorna el resultado, 'Positive'.

## BUCLES

Se usan para ejecutar una sentencia varias veces.

```sql
USE store;

DROP FUNCTION IF EXISTS SequenceNumber;

DELIMITER $$
$$
CREATE FUNCTION SequenceNumber(number INT)
RETURNS VARCHAR(200)
DETERMINISTIC
BEGIN
	DECLARE i INT;
	DECLARE sequence VARCHAR(200);

    SET i = 0;
    SET sequence = '';
    WHILE i <= number DO
		SET sequence = CONCAT(sequence, i, ', ');
		SET i = i + 1;
	END WHILE;
	RETURN sequence;
END;
$$
```

- La anterior query crea una funcion que recibe un numero y retorna una secuencia de numeros desde 0 hasta el numero.

```sql
SELECT SequenceNumber(10);
```

- Ejecuta la funcion SequenceNumber con el argumento 10 y retorna el resultado, '0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10'.

## FUNCIONES NO DETERMINISTAS

Se usan para ejecutar una sentencia varias veces.

```SQL
USE store;

DROP FUNCTION IF EXISTS Tomorrow;

DELIMITER $$
$$
CREATE FUNCTION Tomorrow()
RETURNS DATETIME
NOT DETERMINISTIC
READS SQL DATA  -- <--- AÑADIR ESTO
BEGIN
    RETURN DATE_ADD(NOW(), INTERVAL 1 DAY);
END;
$$
DELIMITER ;
```

- La anterior query crea una funcion que retorna la fecha de mañana.

```sql
SELECT Tomorrow();
```

- Ejecuta la funcion Tomorrow y retorna el resultado.

```SQL
SELECT UUID();
```

- Ejecuta la funcion UUID y retorna el resultado. Es una funcion no determinista.

## Funciones y consultas

Se pueden usar funciones en consultas.

```sql
USE store;

DROP FUNCTION IF EXISTS QuantityDescription;

DELIMITER $$

CREATE FUNCTION QuantityDescription(cityId INT)
RETURNS INT
NOT DETERMINISTIC
READS SQL DATA  -- <--- AÑADE ESTO AQUÍ
BEGIN
    DECLARE quantity INT;
    IF cityId IS NULL THEN
        SELECT COUNT(*) INTO quantity
        FROM customer;
    ELSE
        SELECT COUNT(*) INTO quantity
        FROM customer
        WHERE city_id = cityId;
    END IF;
    RETURN quantity;
END$$

DELIMITER ;
```

La query anterior crea una funcion que recibe un id de ciudad y retorna la cantidad de clientes que pertenecen a esa ciudad.

```sql
SELECT QuantityDescription(1);
```

- Ejecuta la funcion QuantityDescription con el argumento 1 y retorna el resultado.
