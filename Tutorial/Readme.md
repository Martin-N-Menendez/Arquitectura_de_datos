# Fuente

[![Watch the video](https://www.uncommunitymanager.es/wp-content/uploads/seo_google_youtube.jpg)](https://www.youtube.com/watch?v=jxIEDKzGrOs&list=PL8gxzfBmzgex2nuVanqvxoTXTPovVSwi2)

# Tipos de datos

- Boolean = TRUE o FALSE
- Int, Integer = Entero
- Float = Flotante
- Decimal = Exacto
- Character(N) = Cadena de caracteres de tamaño fijo N
- Varchar(N) = Cadena de caracteres de tamaño variable hasta N
- Date = Fecha
- Time = Tiempo en HH:MM:SS

# Comandos

Lista de comandos útiles en TOOLS > QUERY TOOLS (sobre la DB que quiero, cada una tiene SU consola).

### Crear base de datos

```SQL
CREATE DATABASE prueba
```

### Eliminar base de datos

Click derecho sobre la DB -> Disconnect database

```SQL
DROP DATABASE IF EXISTS prueba
```

### Crear tabla

```SQL
CREATE TABLE persona(
  ID INTEGER NOT NULL,
  nombre VARCHAR(20),
  DNI VARCHAR(10)
  );
```
### Insertar datos en la tabla

Para ingresar TODOS los campos
```SQL
INSERT INTO persona VALUES('1','pepe','123.456')
```

Para ingresar  SOLO los campos obligatorios (los que puse NOT NULL)
```SQL
INSERT INTO persona (ID) VALUES('2')
```
### Ver datos de la tabla

Click derecho en tabla "persona" > View/Edit Data > All rows

o

```SQL
SELECT * FROM persona
```

y debería ver lo siguiente:

| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 |
| 2  | [null] | [null] |

### Actualizar valores de la tabla

Para ingresar TODOS los campos
```SQL
UPDATE persona SET (nombre,DNI) = ('maria','456.789')
WHERE DNI IS NULL
```
| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 |
| 2  | maria | 456.789 |

### Buscar datos en la tabla (con alias)

Ver solamente a maria y llamar a la columna "nombre" de otra forma
```SQL
SELECT nombre  AS objetivo FROM persona
WHERE nombre = 'maria'
```
| objetivo |
| ------------- | 
| maria  |

### Borrar elementos de la tabla

Puedo usar cualquier criterio en WHERE
```SQL
DELETE FROM persona
WHERE nombre = 'pepe'
```
| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 2  | maria | 456.789 |


### Agregar columna a la tabla

```SQL
ALTER TABLE persona
ADD COLUMN direccion VARCHAR(10)
```
| ID | nombre | DNI | direccion |
| ------------- | ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 | [null] |
| 2  | maria | 456.789 | [null] |

### Modificar nombre de columna de tabla

```SQL
ALTER TABLE persona
RENAME COLUMN direccion TO lugar
```
| ID | nombre | DNI | lugar |
| ------------- | ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 | [null] |
| 2  | maria | 456.789 | [null] |

### Eliminar columna de la tabla

```SQL
ALTER TABLE persona
DROP COLUMN lugar
```
| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 |
| 2  | maria | 456.789 |



### Actualizar un dato a toda la columna

```SQL
UPDATE persona SET direccion = 'Algo'
```

| ID | nombre | DNI | direccion |
| ------------- | ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 | Algo |
| 2  | maria | 456.789 | Algo |

### Cambiar una columna para que no admita datos nulos

```SQL
ALTER TABLE persona
ALTER COLUMN direccion SET NOT NULL
```
De esta forma al hacer un insert el dato "direccion" sera obligatorio.

### Cambiar una columna para que admita datos nulos

```SQL
ALTER TABLE persona
ALTER COLUMN direccion DROP NOT NULL
```
De esta forma al hacer un insert el dato "direccion"  ya no sera obligatorio.

### Cambiar tipo de dato de una columna

Para cambiar el tipo de "direccion" a un varchar de otro tamaño (no puedo pasarlo a integer sin castear los datos que ya tengo)

```SQL
ALTER TABLE persona
ALTER COLUMN direccion TYPE VARCHAR(15)
```

### Configurar clave primaria (para definir campos con valor único)

Si la tabla aun no existe

```SQL
CREATE TABLE persona(
  ID INTEGER NOT NULL,
  nombre VARCHAR(20),
  DNI VARCHAR(10),
  PRIMARI KEY(ID)
  );
```

Si la tabla ya existe y quiero modificar una columna

```SQL
ALTER TABLE persona
ADD PRIMARY KEY (ID)
```

### Configurar clave primaria autoincremental (para definir campos con valor único incremental)

Si la tabla aun no existe

```SQL
CREATE TABLE persona(
  ID SERIAL PRIMARY KEY NOT NULL,
  nombre VARCHAR(20),
  DNI VARCHAR(10)
  );
```
Si la tabla ya existe NO ENCONTRE LA SOLUCION!

### Reiniciar tabla

Sin reiniciar el ID (cuando agregue elementos nuevos va a seguir contando desde donde quedo antes)
```SQL
TRUNCATE TABLE persona
```
Reiniciando el ID (cuando agregue elementos nuevos va a contar desde el 1)
```SQL
TRUNCATE TABLE persona RESTART IDENTITY
```

### Crear tablas con valores por defecto

```SQL
CREATE TABLE persona(
  ID SERIAL PRIMARY KEY NOT NULL,
  nombre VARCHAR(20),
  DNI VARCHAR(10) DEFAULT "000.000"
  );
```
Si luego ejecuto
```SQL
INSERT INTO persona (nombre) VALUES ('luis')
```

| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 |
| 2  | maria | 456.789 |
| 3  | luis | 000.000 |

### Ordenar tabla

Para ordenar en sentido ascendente o descendente. 
```SQL
SELECT * FROM persona ORDER BY ID ASC/DESC
```
Puedo utilizar varios criterios de orden
```SQL
SELECT * FROM persona ORDER BY ID ASC , nombre DESC
```

### Buscar dato si no estas seguro del mismo

Para buscar nombres que tengan 'e' y puedan tener mas letras a izquierda o derecha.

```SQL
SELECT * FROM persona
WHERE nombre LIKE '%e%'
```
| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 |

### Contar elementos de la tabla

De la siguiente tabla:

| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 1  | jojo | 567 |
| 2  | jose | 123 |
| 3  | maria | 234 |
| 4  | pepe | 345 |
| 5  | raul | 456 |

Cuantos tienen la letra 'e' en su nombre?

```SQL
SELECT COUNT(nombre) FROM persona
WHERE nombre LIKE '%e%'
```
El resultado es 2 ('jose' y 'pepe')

### Sumar registros

Si 'salario' fuese un INTEGER.

```SQL
SELECT SUM(salario) FROM persona
```

### Agrupar registros con criterio

Si 'salario' fuese un INTEGER.

Para agrupar todos los nombres iguales, dejando el que tenga menor salario (se puede hacer con MAX y seria el mayor salario).

```SQL
SELECT nombre, MIN(salario) FROM persona
GROUP BY nombre
```
Para agrupar por promedio.

```SQL
SELECT AVG(salario) FROM persona
```

### Filtrado de grupos

De la siguiente tabla:

| ID | nombre | salario |
| ------------- | ------------- | ------------- |
| 1  | jose | 2500 |
| 2  | maria | 4500 |
| 3  | eduardo | 3000 |
| 4  | jose | 5500 |
| 5  | pepe | 200 |

```SQL
SELECT nombre,salario FROM persona
WHERE nombre = 'jose'             -- Actua sobre el SELECT
GROUP BY nombre,salario
HAVING salario > 3000             -- Actua sobre el GROUP
```

| nombre | salario |
| ------------- | ------------- |
| jose | 5500 |

### Mostrar elementos distintos

De la siguiente tabla:

| ID | nombre | salario |
| ------------- | ------------- | ------------- |
| 1  | jose | 2500 |
| 2  | maria | 4500 |
| 3  | eduardo | 3000 |
| 4  | jose | 5500 |
| 5  | pepe | 200 |

Al aplicar:
```SQL
SELECT DISTINCT nombre FROM persona
```

Obtenemos:
| nombre |
| ------------- | 
| jose |
| maria |
| eduardo |
| pepe |

Y si aplicamos:
```SQL
SELECT COUNT(DISTINCT nombre) FROM persona
```

El resultado será 4

### Mostrar rango de elementos

De la siguiente tabla:

| ID | nombre | salario |
| ------------- | ------------- | ------------- |
| 1  | jose | 2500 |
| 2  | maria | 4500 |
| 3  | eduardo | 3000 |
| 4  | jose | 5500 |
| 5  | pepe | 200 |

Al aplicar:
```SQL
SELECT * FROM persona
WHERE salario BETWEEN 2000 AND 5000
```

Obtenemos:

| ID | nombre | salario |
| ------------- | ------------- | ------------- |
| 1  | jose | 2500 |
| 2  | maria | 4500 |
| 3  | eduardo | 3000 |

Al aplicar:
```SQL
SELECT * FROM persona
WHERE salario NOT BETWEEN 2000 AND 5000
```

Obtenemos:

| ID | nombre | salario |
| ------------- | ------------- | ------------- |
| 4  | jose | 5500 |
| 5  | pepe | 200 |

### Restringir valores de la tabla

Si quiero añadir una restruccion para que 'salario' no se repita.

```SQL
ALTER TABLE persona
ADD CONSTRAINT salario_unico
UNIQUE(salario)
```

Si quiero elimianr dicha restruccion.

```SQL
ALTER TABLE persona
DROP CONSTRAINT salario_unico
```

### Clave foránea

Creo una tabla secundaria a la cual ligar los datos de la primer tabla

```SQL
CREATE TABLE empresa(
  ID SERIAL PRIMARY KEY NOT NULL,
  nombre VARCHAR(20)
  );
```
Lleno la tabla secundaria de datos
```SQL
INSERT INTO empresa (nombre) VALUES ('EMPRESA_A');
INSERT INTO empresa (nombre) VALUES ('EMPRESA_B');
INSERT INTO empresa (nombre) VALUES ('EMPRESA_C');
```

| ID | nombre | 
| ------------- | ------------- |
| 1 | EMPRESA_A |
| 2 | EMPRESA_B |
| 3 | EMPRESA_C |

Agrego a la tabla primaria una columna para el codigo de empresa

```SQL
ALTER TABLE persona
ADD CodigoEmpresa INTEGER  
```
Linkeo la tabla principal a la secundaria cargando los datos

```SQL
ALTER TABLE persona
ADD CONSTRAINT FK_test
FOREIGN KEY (nombre)
REFERENCES empresa(ID)
```

```SQL
UPDATE persona SET CodigoEmpresa = '2'
```
Si intento añadir un nuevo elemento a la tabla primaria con un CodigoEmpresa que no exista en la tabla secundaria dará un error.

### Crear funciones

Creo una funcion que reciba el nombre de la persona y devuelva el salario asociado.

```SQL
CREATE FUNCTION BuscarSalario(variable VARCHAR) RETURNS INTEGER
AS
$$

SELECT salario FROM persona
WHERE nombre = variable

$$
LANGUAGE SQL
```
Invocando con

```SQL
SELECT BuscarSalario('eduardo')
```

el resultado mostrado será 3000

Tambien puedo crear funciones que no pida ni devuelva nada

```SQL
CREATE FUNCTION InsertarPersonas() RETURNS VOID
AS
$$

INSERT INTO persona (nombre,salario) VALUES ('pepe',200);
INSERT INTO persona (nombre,salario) VALUES ('jose',4500);

$$
LANGUAGE SQL
```
Para poder sumar personas a la lista ejecutando

```SQL
SELECT InsertarPersonas()
```

### Vistas

Para poder ver solamente el id y el nombre de una tabla

```SQL
CREATE VIEW view_data
AS SELECT id,nombre FROM persona

SELECT * FROM view_data
```

Y obtenemos:

| ID | nombre |
| ------------- | ------------- |
| 1  | jose |
| 2  | maria |
| 3  | eduardo | 
| 4  | jose | 
| 5  | pepe | 

### Union

Para poder hacer vistas combinando tablas

```SQL
CREATE VIEW view_union
AS 
SELECT id,nombre, 'persona' AS ORIGEN FROM persona
UNION ALL
SELECT id,nombre, 'empresa' FROM empresa
ORDER BY ORIGEN

SELECT * FROM view_union
```
