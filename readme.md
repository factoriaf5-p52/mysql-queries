# Consultas

### Consultas a una tabla
La instrucción básica de las consultas es la selección **`SELECT`**.

```sql
mysql> help select
Name: 'SELECT'
Description:
Syntax:
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
    [HIGH_PRIORITY]
    [STRAIGHT_JOIN]
    [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
    [SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr] ...
    [into_option]
    [FROM table_references
      [PARTITION partition_list]]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}, ... [WITH ROLLUP]]
    [HAVING where_condition]
    [WINDOW window_name AS (window_spec)
        [, window_name AS (window_spec)] ...]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
    [into_option]
    [FOR {UPDATE | SHARE}
        [OF tbl_name [, tbl_name] ...]
        [NOWAIT | SKIP LOCKED]
      | LOCK IN SHARE MODE]
    [into_option]

into_option: {
    INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
  | INTO DUMPFILE 'file_name'
  | INTO var_name [, var_name] ...
}
```
Ejemplo:

```sql
SELECT * FROM ACTOR;

# Especifica los atributos a recuperar
SELECT nombre, apellido FROM actor;
```

#### Concatenar campos de tablas

Usa la función `concat()`.

Ejemplo:

```sql
# Selecciona los atributos nombre y apellido de la tabla actor y los retorna en la misma columna.
	SELECT concat(nombre,apellido) FROM actor;

# Da formato a los registros resultantes, añadiendo un espacio y una coma entre el apellido y el apellido:

	SELECT concat(apellidos,', ',nombre) FROM actor;

# Renombra el nombre de la columna:

	SELECT concat(apellidos,', ',nombre) AS ‘nombre_completo’ FROM 	actor;
```

#### Registros únicos

Usa **`SELECT DISTINCT`**

```sql
#Selecciona el atributos first_name de los registros de la tabla actor, pero solo los que no se repiten.

	SELECT DISTINCT first_name  FROM actor;
```

#### Contar Registros
La instrucción `count()` retorna el número de registros encontrados.

```sql
#La selección del número de registros de la tabla actor es.

	SELECT COUNT(*) FROM actor;

#Selección del número de registros con first_name diferente.

	SELECT COUNT(distinct first_name) FROM actor;
```

Selección del valor medio del amount de la tabla payment.

```sql
	SELECT AVG(amount) FROM payment;
```

Selección del valor maximo y mínimo del amount de la tabla payment.

```sql
	SELECT MAX(amount), MIN(amount) FROM payment;
```
### Condiciones con cláusula WHERE

```sql
SELECT condicion FROM tabla WHERE condicion_where
```

Selecciona los datos de la tabla que cumplen la condición y los registros que cumplen la condición where.

Ejemplos:

Selecciona todos los atributos de los registros de la tabla actor cuyo first_name sea “Penelope”.

```sql
	SELECT * FROM actor WHERE first_name='penelope';
```

Selecciona los atributos first_name y last_name de la tabla actor cuyo first_name sea ”Penelope”.

```sql
	SELECT first_name, last_name FROM actor WHERE 	first_name='penelope';
```

Selecciona el número registros con amount menores de 1 de la tabla payment.

```sql
	SELECT count(*) FROM payment WHERE amount<1;
```

Selecciona el número registros con amount entre 1 y 2 de la tabla payment.

```sql
SELECT count(*) FROM payment WHERE amount>1 AND amount<2;
```


### Cláusula LIKE

```sql
SELECT condicion FROM tabla WHERE campo LIKE valor
```

Selecciona los datos de la tabla que cumplen la condición y los registros que cumplen valores de “campo” parecidos a “valor”.

Ejemplos:

Selecciona todos los atributos de los registros de la tabla actor cuyo first_name comience por ‘p’.

```sql
	SELECT * FROM actor WHERE first_name LIKE ‘p%’;
```

Selecciona todos los atributos de la tabla actor cuyo first_name contenga una ‘p’.

```sql
	SELECT first_name, last_name FROM actor WHERE first_name 	LIKE ‘%p%’;
```

Selecciona los atributos first_name y last_name de la tabla actor cuyo first_name acabe  en ‘a’.

```sql
	SELECT first_name, last_name FROM actor WHERE 	first_name LIKE ‘%a’;
```

### Cláusula GROUP BY

```sql
SELECT condicion FROM tabla WHERE condicion_where
GROUP BY {col_name | expr | position}
```

Selecciona los datos de la tabla que cumplen la condición y los registros que cumplen la condición where. El resultado lo agrupa según se indica en la expresión GROUP BY.

Ejemplos:

Agrupa todos los amount y muestra los grupos (selecciona los diferentes amount) .

```sql
	SELECT amount FROM payment GROUP BY amount;
```

Cuenta los registros que hay en cada grupo de amount.

```sql
	SELECT count(amount) FROM payment GROUP BY amount;
```

Agrupa todos los amount y muestra los grupos (selecciona los diferentes amount) en ascendente.

```sql
	SELECT amount FROM payment GROUP BY amount ASC;
```

Agrupa todos los amount y muestra los grupos (selecciona los diferentes amount) en descendente.

```sql
	SELECT amount FROM payment GROUP BY amount DESC;
```


### Cláusula ORDER BY

```sql
SELECT condicion FROM tabla WHERE condicion_where
ORDER BY {col_name | expr | position}
```

Selecciona los datos de la tabla que cumplen la condición y los registros que cumplen la condición where. El resultado lo ordena según se indica en la expresión ORDER BY.

Ejemplos:

Selecciona los first_name de la tabla actor y los ordena por first_name.

```sql
	SELECT first_name, last_name FROM actor ORDER BY first_name;
```

Selecciona los first_name y last_name de los actores con nombre 'Penelope'  y los ordena por last_name.

```sql
	SELECT last_name, first_name FROM actor WHERE first_name 	= 'penelope' ORDER BY last_name;
```

### Salida a fichero

```sql
SELECT condicion FROM tabla AS INTO OUTFILE fichero
```

Selecciona los datos de la tabla que cumplen la condición y los almacena en un fichero de tipo texto.

Ejemplo:

Selecciona todos los elementos de la tabla language y los almacena en el fichero idioma.txt .

```sql
	SELECT  * FROM language INTO OUTFILE 'c:\\idioma.txt';
```
### Consultas sobre varias tablas

```sql
SELECT condicion FROM tabla1, tabla2 WHERE condicion_where
```

La selección sobre varias tablas realiza el producto cartesiano de ambas tablas, la condición_where elimina los datos que no interesan.

Ejemplos:

1. Realiza la multiplicación de los registros de las tablas country y city.

```sql
	SELECT * FROM country, city
```

1. Obtener los datos de la ciudad (tabla city) con su país correspondiente (tabla country). El atributo que relaciona la ciudad con el país es country_id.

```sql
	SELECT * FROM country, city WHERE country.country_id = 	city.country_id;
```

1. Obtener la ciudad (tabla city) con su país correspondiente (tabla country) utilizando alias.

```sql
	SELECT ci.city, co.country FROM city AS ci, country AS co WHERE 	ci.country_id=co.country_id;
```

1. Obtener el nombre y apellidos del responsable (tabla staff) de las todas las tiendas (tabla store).

```sql
	SELECT first_name, last_name, store.store_id FROM store, staff  	WHERE 	store.store_id=staff.store_id;
```

### JOINS

![](https://imgur.com/SFNzM6H.png)

Otra manera de mostrar información de varias tablas (mucho más habitual y lógicamente) es uniendo filas de las dos, por eso hace falta que las columnas que se van a unir entre las dos tablas sean las mismas y contengan los mismos tipos de datos, es decir, mediante una clave externa.

La **operación JOIN o combinación** permite mostrar columnas de varias tablas como si se tratara de una sola tabla, combinando entre si los registros relacionados usando para lo cual claves externas.

Las tablas relacionadas se especifican en la cláusula **FROM**, además hay que hacer coincidir los valores que relacionan las columnas de las tablas.

```sql
SELECT OrderID, C.CustomerID, CompanyName, OrderDate
  FROM Customers C, Orders O WHERE C.CustomerID = O.CustomerID
```

Para evitar que se produzca como resultado el producto cartesiano entre las dos tablas, expresamos el vínculo que se establece entre las dos tablas en la cláusula **WHERE**. En este caso relacionamos las dos tablas mediante el identificador del cliente, clave existente en ambas.

Utilizamos alias paara cada tabla (C y O respectivamente) con el fin de no tener que escribir su nombre completo cada vez que necesitamos usarlos.

Hay que tener en cuenta que si el nombre de una columna existe en más de una de las tablas indicadas en la cláusula FROM, hay que poner, obligatoriamente, el nombre o alias de la tabla de la cual queremos obtener este valor. En caso contrario nos dará un error de ejecución, indicando que hay un nombre ambiguo.

Hay otra manera adicional, que es más explícita y clara a la hora de realizar este tipo de combinaciones, incorporada a partir de ANSI SQL92, que permite utilizar una nueva cláusula llamada **JOIN** en la cláusula FROM, la sintaxis es el siguiente: En el caso del ejemplo anterior quedaría de la siguiente manera:

```sql
SELECT [ALL / DISTINCT] [*] / [*ListaColumnas_*Expresiones]
FROM NombreTabla1 JOIN NombreTabla2 WHERE Condiciones_Vinculos_Tablas
```

De este modo relacionamos de manera explícita las dos tablas sin necesidad de involucrar la clave externa en las condiciones del SELECT (o sea, en el WHERE). Es una manera más clara y limpia de llevar a cabo la relación. Esto se puede ir aplicando a todas las tablas necesitamos combinar en nuestras consultas. Veamos un ejemplo en ambos formatos que involucra más tablas, en este caso las tablas de empleados, clientes y ventas:

```sql
SELECT OrderID, C.CustomerID, CompanyName, OrderDate
FROM Customers C, Orders O, Employees I
WHERE C.CustomerID = O.CustomerID
AND O.EmployeeID = E.EmployeeID
```
El segundo formato permite distinguir las condiciones que utilizamos para combinar las tablas y evitar el producto cartesiano, de las condiciones de filtro que tengamos que establecer.

Veamos un ejemplo como el anterior, pero ahora además necesitamos que el cliente sea de España o el vendedor sea el número 5.

En el primer formato tendríamos algo como esto:

```sql
SELECT OrderID, C.CustomerID, CompanyName, OrderDate
    FROM Customers C, Orders O, Employees I
    WHERE (C.CustomerID = O.CustomerID AND O.EmployeeID =E.EmployeeID)
      AND (C.Country = 'Spain' ORO E.EmployeeID = 5)
```

Es decir, estamos mezclando al WHERE las uniones de tablas, y las condiciones concretas de filtro de la consulta, quedando todo mucho más envuelto.

Sin embargo usando el segundo formato con JOIN, la consulta es mucho más clara:

```sql
SELECT OrderID, C.CustomerID, CompanyName, OrderDate
    FROM Customers C
      JOIN Orders O    ON C.CustomerID =O.CustomerID
      JOIN Employees I ON O.EmployeeID =E.EmployeeID
WHERE C.Country = 'Spain' OR E.EmployeeID = 5
```

También podemos utilizar una misma tabla (self-join) con dos alias diferentes para distinguirlos. Veamos un ejemplo, suponemos que tenemos una columna _salary_ en la tabla de empleados, y queremos saber los empleados que tienen un sueldo superior al del empleado 5:

```sql
SELECT EmployeeID
  FROM Employees E1 JOIN Employees E2 WHERE E1.salary > E2.salary
WHERE E2.EmployeeID = 5
```

#### Inner Join

Devuelve las filas que comparte al menos una coincidencia en ambas tablas.

Admite distintos operadores: Equi-join ("="),non-equi join(">", "<", "<=", ">=").

![](https://i.imgur.com/N2gqR9h.png)

```sql
/* INNER JOIN */
-- Create table 1
CREATE TABLE table1
(ID INT, Value VARCHAR(10));
INSERT INTO Table1 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 4,'Fourth'
UNION ALL
SELECT 5,'Fifth';

-- Create table 2
CREATE TABLE table2
(ID INT, Value VARCHAR(10));
INSERT INTO Table2 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 6,'Sixth'
UNION ALL
SELECT 7,'Seventh'
UNION ALL
SELECT 8,'Eighth';

SELECT *
FROM Table1;
SELECT *
FROM Table2;

/* INNER JOIN */
SELECT t1.*,t2.*
FROM Table1 t1
INNER JOIN Table2 t2 ON t1.ID = t2.ID;

/* INNER JOIN */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
INNER JOIN Table2 t2 ON t1.ID = t2.ID;

-- Clean up
DROP TABLE table1;
DROP TABLE table2;
```

#### Outer Join

- **Left outer join** devuelve todas las filas de la tabla izquerda junto a las filas con coincidencias de la tabla derecha.
- Si no hay columnas coincidentes en el lado derecho, devuelve valores NULL

![](https://i.imgur.com/NYaT5io.png)

```sql
-- Create table 1
CREATE TABLE table1
(ID INT, Value VARCHAR(10));
INSERT INTO Table1 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 4,'Fourth'
UNION ALL
SELECT 5,'Fifth';

-- Create table 2
CREATE TABLE table2
(ID INT, Value VARCHAR(10));
INSERT INTO Table2 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 6,'Sixth'
UNION ALL
SELECT 7,'Seventh'
UNION ALL
SELECT 8,'Eighth';

SELECT *
FROM Table1;
SELECT *
FROM Table2;

/* LEFT JOIN */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
LEFT OUTER JOIN Table2 t2 ON t1.ID = t2.ID;

-- Clean up
DROP TABLE table1;
DROP TABLE table2;
```


- **Right outer join** devuelve todas las filas de la tabla derecha junto a las filas con coincidencias de la tabla izquierda.
- Si no hay columnas coincidentes en el lado izquierdo, devuelve valores NULL

![](https://i.imgur.com/ZcESPbv.png)

```sql
-- Create table 1
CREATE TABLE table1
(ID INT, Value VARCHAR(10));
INSERT INTO Table1 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 4,'Fourth'
UNION ALL
SELECT 5,'Fifth';

-- Create table 2
CREATE TABLE table2
(ID INT, Value VARCHAR(10));
INSERT INTO Table2 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 6,'Sixth'
UNION ALL
SELECT 7,'Seventh'
UNION ALL
SELECT 8,'Eighth';

SELECT *
FROM Table1;
SELECT *
FROM Table2;

/* RIGHT JOIN */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
RIGHT OUTER JOIN Table2 t2 ON t1.ID = t2.ID;

/* LEFT JOIN */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table2 t2
LEFT JOIN Table1 t1 ON t1.ID = t2.ID;

-- Clean up
DROP TABLE table1;
DROP TABLE table2;
```

#### Natural join

Join que une dos o más tablas mediante las columnas que tengan el mismo nombre. Pueden ser INNER o OUTER

```sql
/* INNER JOIN */
SELECT 	t1.ID AS T1ID, t1.Value1 AS T1Value,
		t2.ID T2ID, t2.Value2 AS T2Value
FROM Table1 t1
INNER JOIN Table2 t2 ON t1.ID = t2.ID;

/* INNER JOIN */
SELECT 	t1.ID AS T1ID, t1.Value1 AS T1Value,
		t2.ID T2ID, t2.Value2 AS T2Value
FROM Table1 t1
NATURAL JOIN Table2 t2;

/* LEFT JOIN */
SELECT 	t1.ID AS T1ID, t1.Value1 AS T1Value,
		t2.ID T2ID, t2.Value2 AS T2Value
FROM Table1 t1
LEFT JOIN Table2 t2 ON t1.ID = t2.ID;

/* LEFT JOIN */
SELECT 	t1.ID AS T1ID, t1.Value1 AS T1Value,
		t2.ID T2ID, t2.Value2 AS T2Value
FROM Table1 t1
NATURAL LEFT JOIN Table2 t2;
```

#### Joins con USING

Simplifica la sintaxis donde las columnas tienen el mismo nombre, pueden utilizarse varias columnas

```sql

/* INNER JOIN */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
INNER JOIN Table2 t2 ON t1.ID = t2.ID;

/* INNER JOIN - USING*/
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
INNER JOIN Table2 t2 USING (ID);

/* INNER JOIN */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
INNER JOIN Table2 t2 ON t1.ID = t2.ID AND t1.Value = t2.Value;

/* INNER JOIN - USING*/
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
INNER JOIN Table2 t2 USING (ID, Value);

/* LEFT JOIN */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
LEFT JOIN Table2 t2 ON t1.ID = t2.ID;

/* LEFT JOIN - USING */
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
LEFT JOIN Table2 t2 USING (ID);
```

#### UNION

- UNION combina en una única salida la ejecución de dos o más sentencias SELECT
- Cada SELECT del operador UNION debe tener el mismo número de columnas
- UNION elimina las filas duplicadas
- UNION ALL no elimina los duplicados
- Con un único ORDER BY ordenamos todos los registros
- Simula un FULL OUTER JOIN.

```sql
-- Create table 1
CREATE TABLE table1
(ID INT, Value VARCHAR(10));
INSERT INTO Table1 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 4,'Fourth'
UNION ALL
SELECT 5,'Fifth';

-- Create table 2
CREATE TABLE table2
(ID INT, Value VARCHAR(10));
INSERT INTO Table2 (ID, Value)
SELECT 1,'First'
UNION ALL
SELECT 2,'Second - 2'
UNION ALL
SELECT 3,'Third'
UNION ALL
SELECT 6,'Sixth'
UNION ALL
SELECT 7,'Seventh'
UNION ALL
SELECT 8,'Eighth';

SELECT *
FROM Table1;
SELECT *
FROM Table2;

/* UNION ALL */
SELECT t1.ID T1ID, t1.Value AS T1Value
FROM Table1 t1
UNION ALL
SELECT  t2.ID AS T2ID, t2.Value AS T2Value
FROM Table2 t2;

/* UNION ALL */
-- Error
/* UNION ALL */
SELECT t1.ID T1ID
FROM Table1 t1
UNION ALL
SELECT  t2.ID AS T2ID, t2.Value AS T2Value
FROM Table2 t2;

/* UNION */
SELECT t1.ID T1ID, t1.Value AS T1Value
FROM Table1 t1
UNION
SELECT  t2.ID AS T2ID, t2.Value AS T2Value
FROM Table2 t2;

-- ----------------------------------
-- Order By
-- ----------------------------------
/* UNION ALL */
SELECT t1.ID T1ID, t1.Value AS T1Value
FROM Table1 t1
UNION ALL
SELECT  t2.ID AS T2ID, t2.Value AS T2Value
FROM Table2 t2
ORDER BY T1Value DESC;

-- ----------------------------------
-- FULL OUTER JOIN
-- ----------------------------------
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
LEFT JOIN Table2 t2 ON t1.ID = t2.ID
UNION
SELECT 	t1.ID AS T1ID, t1.Value AS T1Value,
		t2.ID T2ID, t2.Value AS T2Value
FROM Table1 t1
RIGHT OUTER JOIN Table2 t2 ON t1.ID = t2.ID;


-- Clean up
DROP TABLE table1;
DROP TABLE table2;
```
#### SUBQUERIES

- Una subquery es una query anidada donde el resultado de una de las queries puede ser utilizado por otra query mediante un operador relacional o una función de agregación
- Una subquery debe estar incluida entre paréntesis
- Una subquery puede tener una única columna en la cláusula SELECT si se usa en la cláusula WHERE
- Subqueries se pueden anidar en otras subqueries
- Subqueries se utilizan en WHERE, HAVING, FROM y SELECT.

```sql
/*
Problem Statement:
Find customers who like to watch action movies?
*/

USE sakila;
-- Subquery
SELECT cust.customer_id, cust.first_name, cust.last_name
FROM customer cust
WHERE cust.customer_id IN
(
SELECT ren.customer_id
FROM rental ren
WHERE ren.inventory_id IN
(
SELECT inv.inventory_id
FROM inventory inv
WHERE inv.film_id IN
(
SELECT fl.film_id
FROM film fl
WHERE fl.film_id IN
(
SELECT fc.film_id
FROM film_category fc
WHERE fc.category_id IN
(
SELECT cat.category_id
FROM category cat
WHERE cat.name = 'Action'
)))))
ORDER BY cust.customer_id, cust.first_name, cust.last_name;

-- Joins
SELECT DISTINCT cust.customer_id, cust.first_name, cust.last_name
FROM customer cust
INNER JOIN rental ren ON ren.customer_id = cust.customer_id
INNER JOIN inventory inv ON inv.inventory_id = ren.inventory_id
INNER JOIN film fl ON fl.film_id = inv.film_id
INNER JOIN film_category fc ON fc.film_id = fl.film_id
INNER JOIN category cat ON cat.category_id = fc.category_id
WHERE cat.name = 'Action'
ORDER BY cust.customer_id, cust.first_name, cust.last_name;
```

#### Joins vs Subqueries

- Joins pueden incluir columnas de las tablas conectadas en la cláusula SELECT. Son más fáciles de leer y más intuitivas
- Subqueries pueden pasar valores agregados a la sentencia principal
- Simplifica queries largas y complejas

#### Subqueries correlacionadas

- Son subqueries que se ejecutan una vez por cada fila

- Devuelven resultados basados en la columna de la query principal

```sql
SELECT t1.* FROM Table1 t1
WHERE t1.ID IN
(SELECT t2.ID FROM Table2 t2
WHERE t2.value = t1.value)

```

#### Outer Join with NULL

```sql
SELECT t1.* FROM Table1 t1
WHERE t1.ID NOT IN
(SELECT t2.ID FROM Table2 t2)
```


# Data Modification Language

Las sentencias dml las constituyen **INSERT**, **UPDATE** y **DELETE**

Creamos la tabla **crazynames** dentro del esquema de Sakila:

```sql
CREATE TABLE crazynames (
  actor_id smallint(5) unsigned NOT NULL AUTO_INCREMENT,
  first_name varchar(45) NOT NULL,
  last_name varchar(45) NULL,
  last_update timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP
		ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (actor_id)
);
```
### INSERT

#### Single row insert

```sql
-- Insert Single Row
INSERT INTO sakila.crazynames (first_name,last_name)
VALUES ('Polypharmacy','Scootching');

-- Insert Single Row, just values
INSERT INTO sakila.crazynames
VALUES (DEFAULT,'Handywork','Enamorations',DEFAULT);

-- Insert Single Row with null values
INSERT INTO sakila.crazynames (first_name,last_name,last_update)
VALUES ('Cervelas',NULL,DEFAULT);
```

#### Multi Row Insert

```sql
-- --------------------------------------
-- Insert Multiple Values
-- --------------------------------------
INSERT INTO sakila.crazynames (first_name,last_name)
VALUES 	('Thalli','Spermatophytes'),
		    ('Latexes','Suboptimise'),
		    ('Amusingnesses','Subceptions');

-- Subquery
INSERT INTO sakila.crazynames (first_name,last_name,last_update)
SELECT first_name,last_name,last_update
FROM actor
WHERE first_name = 'Nick';
```

### UPDATE

#### Single Row Update

```sql

-- Update Single Row Single Column
UPDATE 	sakila.crazynames
SET 	first_name = 'Tarpons'
WHERE 	actor_id = 1;

UPDATE 	sakila.crazynames
SET 	last_name = 'Socialites'
WHERE 	actor_id = 1;

-- Update Single Row Multiple Column
UPDATE 	sakila.crazynames
SET 	first_name = 'Unconfining',last_name = 'Visiters'
WHERE 	actor_id = 2;
```
#### Multi Rows Update

```sql
-- Update Multiple Rows Single Column
UPDATE 	sakila.crazynames
SET 	first_name = 'Paddings'
WHERE 	actor_id IN (3,4,5);

-- Update Multiple Rows Multiple Columns
UPDATE 	sakila.crazynames
SET 	first_name = 'Ophiurans',
		last_name = 'Bazazz',
		last_update = DEFAULT
WHERE 	actor_id IN (6,7,8);


-- Update using SELECT Statement
UPDATE sakila.crazynames
SET first_name = 'Reentered'
WHERE actor_id IN (SELECT actor_id
					FROM film_actor
					WHERE film_id = 1);

```
### DELETE

#### Single Row Delete

```sql
DELETE
FROM	sakila.crazynames
WHERE 	actor_id = 1;
```
#### Multiple Rows Delete

```sql
DELETE
FROM	sakila.crazynames
WHERE 	actor_id IN (3,4,5);
```

**NO OLVIDAR ESPECIFICAR LA CLAUSULA WHERE EN UPDATE Y DELETE PARA NO ACTUALIZAR/BORRAR TODOS LOS REGISTROS DE UNA TABLA**

#### Delete All Rows

```sql
TRUNCATE TABLE crazynames;
```



### Inserción a través de fichero csv

**Ejemplo #1:**
```sql
LOAD DATA
LOCAL INFILE 'C:\\location.csv'
INTO TABLE location
FIELDS TERMINATED BY ';'
LINES TERMINATED BY '\r\n'
IGNORE 1 LINES;
```

Se procesa el archivo "C:\location.csv", que se encuentra en directorio local y se graban los datos en la tabla "location", indicando que los campos están separados por punto y coma (CSV para Excel) y las líneas acaban con "CRLF" (Windows). Además se salta la línea de cabecera (IGNORE).

```sql
LOAD DATA
INFILE 'location.csv'
INTO TABLE location
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n‘
(uniqName, uniqCity);
```

En este segundo ejemplo se procesa el archivo "location.csv", que se encuentra en el servidor, indicando que los campos están separados por coma y delimitados por comillas (CSV estándar) y las líneas acaban con "LF" (Linux). Especifica los campos uniqName y uniqCity para insertar la información.


#### Ejercicios

1. Realizar una consulta que retorne solo los diferentes amounts de la tabla payment menores de 10.
2. Realizar una consulta que retorne los diferentes first_name de la tabla actor y cuantos registros hay en cada first_name.(es decir, cuantas 'Penelope', ‘John’, etc. existen en la tabla).
3. Realizar un consulta que muestre cuantas 'Penelope' hay en la tabla actor
4. Seleccionar el número de clientes (customer) que tiene  cada tienda (store).
5. Obtener el número de alquileres (rental) que ha hecho el cliente (customer) Freddie Duggan.
6. Obtener los títulos de películas que ha alquilado el cliente Freddie Duggan
7. Obtener el nombre, apellido y país de todos los componentes del staff (tabla staff)
8. Obtener el nombre, apellido y ciudad de todos los clientes (tabla customer) que hay en  España (country = ‘Spain’)
9. Obtener el nombre, apellido, ciudad y número de alquileres (rental) de todos los usuarios en España.
10. Obtener el acumulado, la media y el número de pagos recibidos (amount) por  tienda (store) a lo largo del año 2005
11. Obtener el titulo de las películas que ha alquilado el cliente ‘Carrie Porter’ y las veces que ha alquilado cada película
12. Obtener un listado de todos los clientes (nombre y apellidos) con el acumulado de pagos realizados
13. Obtener el número de películas de cada categoria (category) alquiladas en España.
14. Tenemos tres tablas:
    - Students
    - Classes
    - StudentClass

- Un estudiante se puede matricular en un máximo de tres clases.
- En verano los estudiantes pueden decidir no matricularse en ninguna clase

Cuestiones:
- Recupera todos los estudiantes que se han matriculado en clases.
- Recupera todos los estudiantes que no se han matriculado en ninguna clase

Tablas:

```sql
CREATE TABLE Students
(StudentId INT, StudentName VARCHAR(10));
INSERT INTO Students (StudentId, StudentName)
SELECT 1,'John'
UNION ALL
SELECT 2,'Matt'
UNION ALL
SELECT 3,'James';

-- Classes
CREATE TABLE Classes
(ClassId INT, ClassName VARCHAR(10));
INSERT INTO Classes (ClassId, ClassName)
SELECT 1,'Maths'
UNION ALL
SELECT 2,'Arts'
UNION ALL
SELECT 3,'History';

-- StudentClass
CREATE TABLE StudentClass
(StudentId INT, ClassId INT);
INSERT INTO StudentClass (StudentId, ClassId)
SELECT 1,1
UNION ALL
SELECT 1,2
UNION ALL
SELECT 3,1
UNION ALL
SELECT 3,2
UNION ALL
SELECT 3,3;
```

15. Encuentra todos los pagos de clientes (customer) que están por encima de la media de pagos.
16. Reescribe la subquery anterior usando SQL Joins
