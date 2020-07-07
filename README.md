# Arquitectura de datos

- [ ] Definir diagrama UML
- [ ] Definir tablas
- [ ] Crear base de datos (DB)
- [ ] Cargas datos en la DB
- [ ] Consultar a la DB
- [ ] Agregar elementos a la DB

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
