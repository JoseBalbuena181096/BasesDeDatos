## SQL dinámico

La consultas dinámicas son aquellas que se crean en tiempo de ejecución.

```sql
USE store;

PREPARE statement FROM 'SELECT * FROM city';
EXECUTE statement;
```

La ventaja de las consultas dinámicas es que se pueden crear consultas que se ajusten a las necesidades del usuario. La anterior consulta es estática, ya que se sabe exactamente lo que se va a ejecutar.

```sql
USE store;

PREPARE statement FROM 'SELECT * FROM city WHERE id = ?';
SET @id = 1;
EXECUTE statement USING @id;
```

```sql
USE store;

PREPARE statement FROM 'SELECT * FROM city WHERE name = ? OR name = ?';
SET @name1 = 'Barcelona';
SET @name2 = 'CDMX';
EXECUTE statement USING @name1, @name2;
```

Armar una consulta dinámica que permita buscar por.

```sql
USE store;

DROP PROCEDURE IF EXISTS DinamicQuery;

DELIMITER $$
$$
CREATE PROCEDURE DinamicQuery(cols VARCHAR(100), nameTable VARCHAR(20))
BEGIN
    SET @qry = CONCAT('SELECT ', cols, ' FROM ', nameTable);
    PREPARE statement FROM @qry;
    EXECUTE statement;
    DEALLOCATE PREPARE statement;
END$$
DELIMITER ;
```

```sql
CALL DinamicQuery('id, name', 'city');
```

## BUSQUEDAS DINAMICAS

Las busquedas dinamicas son aquellas que se crean en tiempo de ejecución.

```sql
USE store;

DROP PROCEDURE IF EXISTS DinamicSearch;

DELIMITER $$
$$
CREATE PROCEDURE DinamicSearch(minBalance DECIMAL(8,2),
    currency VARCHAR(3),
    customerNames VARCHAR(100)
)
BEGIN
    SET @qry = CONCAT('SELECT name, balance, currency FROM customer WHERE 1 = 1 ');
    IF minBalance IS NOT NULL THEN
        SET @qry = CONCAT(@qry, ' AND balance >= ', minBalance);
    END IF;
    IF currency IS NOT NULL THEN
        SET @qry = CONCAT(@qry, ' AND currency = ''', currency, '''');
    END IF;
    IF customerNames IS NOT NULL THEN
        SET @qry = CONCAT(@qry, ' AND name IN (', customerNames, ')');
    END IF;
    PREPARE statement FROM @qry;
    EXECUTE statement;
    DEALLOCATE PREPARE statement;
END$$
DELIMITER ;
```

```sql
CALL DinamicSearch(4, 'MXN', '''jose'', ''pedro'', ''maria''');
```

El problema de las busquedas dinamicas es que se tornan inseguras a inyeccion sql.

## BUSQUEDAS SQL PROTEGIDAS

```sql
USE store;

DROP PROCEDURE IF EXISTS DinamicSearch2;

DELIMITER $$
$$
CREATE PROCEDURE DinamicSearch2(minBalance DECIMAL(8,2),
    currency VARCHAR(3)
)
BEGIN
    SET @qry = CONCAT('SELECT name, balance, currency FROM customer WHERE 1 = 1 ');
    IF minBalance IS NOT NULL THEN
        SET @qry = CONCAT(@qry, ' AND balance >= ?');
        SET @minBalance = minBalance;
    END IF;
    IF currency IS NOT NULL THEN
        SET @qry = CONCAT(@qry, ' AND currency = ?');
        SET @currency = currency;
    END IF;
    PREPARE statement FROM @qry;
    IF minBalance IS NULL AND currency IS NULL THEN
        EXECUTE statement;
    ELSEIF minBalance IS NOT NULL AND currency IS NULL THEN
        EXECUTE statement USING @minBalance;
    ELSEIF minBalance IS NULL AND currency IS NOT NULL THEN
        EXECUTE statement USING @currency;
    ELSEIF minBalance IS NOT NULL AND currency IS NOT NULL THEN
        EXECUTE statement USING @minBalance, @currency;
    END IF;
    DEALLOCATE PREPARE statement;
END$$
DELIMITER ;
```

```sql
CALL DinamicSearch2(NULL, NULL);
CALL DinamicSearch2(4, NULL);
CALL DinamicSearch2(NULL, 'MXN');
CALL DinamicSearch2(4, 'MXN');
```
