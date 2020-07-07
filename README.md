# Arquitectura de datos

- [ ] Definir diagrama UML
- [ ] Definir tablas
- [ ] Crear base de datos (DB)
- [ ] Cargas datos en la DB
- [ ] Consultar a la DB
- [ ] Agregar elementos a la DB

# Fuente

[![Watch the video](https://www.uncommunitymanager.es/wp-content/uploads/seo_google_youtube.jpg)](https://www.youtube.com/watch?v=jxIEDKzGrOs&list=PL8gxzfBmzgex2nuVanqvxoTXTPovVSwi2)

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
  ID int NOT NULL,
  nombre varchar(20),
  DNI varchar(10)
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
Click derecho en tabla "persona" > View/Edit Data > All rows y debería ver lo siguiente:

| ID | nombre | DNI |
| ------------- | ------------- | ------------- |
| 1  | pepe | 123.456 |
| 2  | [null] | [null] |
