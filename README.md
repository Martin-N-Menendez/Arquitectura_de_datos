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

Lista de comandos útiles

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

Click derecho sobre la DB -> Disconnect database

```SQL
CREATE TABLE persona(
  ID int NOT NULL,
  nombre varchar(20),
  DNI varchar(10)
  );
```
