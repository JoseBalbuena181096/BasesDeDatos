## Master-Detail en SQL

El patrón Master-Detail (Maestro-Detalle) es una relación común en bases de datos relacionales donde:

### Concepto Básico

- Tabla Maestra (Master): Contiene la información principal o de encabezado.
- Tabla de Detalle (Detail): Contiene elementos que pertenecen o están relacionados con un registro de la tabla maestra.

### Características Principales

1. Relación 1 a Muchos: Un registro maestro puede tener múltiples registros de detalle.
2. Integridad Referencial: Los registros de detalle dependen lógicamente del registro maestro.
3. Eliminación en Cascada: Al eliminar un registro maestro, normalmente se eliminan sus registros de detalle asociados.
   Ejemplo Común
   Maestro: Una orden de compra
   Detalle: Los artículos que componen esa orden

### Ejemplo Práctico

```sql
USE store;

CREATE TABLE sale(
    id INT  AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP(),
    FOREIGN KEY (customer_id) REFERENCES customer(id)
);

CREATE TABLE sale_detail(
    id INT AUTO_INCREMENT PRIMARY KEY,
    sale_id INT NOT NULL,
    product_id INT NOT NULL,
    price DECIMAL(8, 2) NOT NULL,
    quantity INT NOT NULL,
    FOREIGN KEY (sale_id) REFERENCES sale(id),
    FOREIGN KEY (product_id) REFERENCES product(id)
);
```

### Insertar datos

Insertar una venta:

```sql
INSERT INTO sale (customer_id) VALUES (1);
```

Traer la venta recien insertada:

```sql
SELECT *
FROM sale
WHERE customer_id = 1
ORDER BY id DESC
LIMIT 0, 1;
```

Insertar un detalle de venta:

```sql
INSERT INTO sale_detail (sale_id, product_id, price, quantity)
VALUES (1, 1, 15, 5),
(1, 2, 20, 2);
```

La venta insertada tiene dos detalles. De la compra de dos productos.

### Multiples joins

```sql
SELECT customer.name as 'customer',
	product.name AS 'product',
	sale.created_at,
    quantity,
    sale_detail.price,
    (quantity * sale_detail.price) AS 'total'
FROM sale_detail
INNER JOIN product ON product.id = sale_detail.product_id
INNER JOIN sale ON sale.id = sale_detail.sale_id
INNER JOIN customer ON sale.customer_id = customer.id;
```

La consulta anterior nos traera los detalles de la venta, el nombre del producto y el nombre del cliente.

### Agrupados con múltiples JOINS

Insertar otra venta:

```sql
INSERT INTO sale (customer_id) VALUES (1);
```

Traer la ultima venta insertada, para conocer su id:

```sql
SELECT *
FROM sale
WHERE customer_id = 1
ORDER BY id DESC
LIMIT 0, 1;
```

Insertar un detalle de venta:

```sql
INSERT INTO sale_detail (sale_id, product_id, price, quantity)
VALUES (2, 1, 15, 3),
(2, 1, 10, 2);
```

La venta insertada tiene dos detalles. De la compra de dos productos.

```sql
SELECT p.name as 'product',
SUM(quantity) as 'productos vendidos',
SUM(quantity * sd.price ) as 'total'
FROM sale_detail as sd
INNER JOIN product as p ON p.id = sd.product_id
GROUP BY p.id;
```

La consulta anterior nos traera los detalles de la venta, el nombre del producto y el nombre del cliente.

### Vistas

Las vistas son tablas virtuales que se crean a partir de una consulta SQL. Que suelen usarse para simplificar consultas complejas. Y sirven para ocultar la complejidad de las consultas.

```SQL
USE store;

CREATE VIEW vwsale AS
SELECT customer.name as 'customer',
	product.name AS 'product',
	sale.created_at,
    quantity,
    sale_detail.price,
    (quantity * sale_detail.price) AS 'total'
FROM sale_detail
INNER JOIN product ON product.id = sale_detail.product_id
INNER JOIN sale ON sale.id = sale_detail.sale_id
INNER JOIN customer ON customer.id = sale.customer_id;
```

- Para usar la vista, es como usar una tabla normal.

```sql
SELECT * FROM vwsale
ORDER BY total DESC;
```

Para modificar la vista, muy similar a modificar una tabla:

```sql
ALTER VIEW vwsale AS
SELECT customer.name as 'customers',
	product.name AS 'products',
	sale.created_at,
    quantity,
    sale_detail.price,
    (quantity * sale_detail.price) AS 'total'
FROM sale_detail
INNER JOIN product ON product.id = sale_detail.product_id
INNER JOIN sale ON sale.id = sale_detail.sale_id
INNER JOIN customer ON customer.id = sale.customer_id;
```
