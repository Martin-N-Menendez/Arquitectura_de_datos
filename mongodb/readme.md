# Práctica de MongoDB parte 1

## 1. Abrir una terminal en el directorio donde haya guardado el archivo “facturas.json” y usar el siguiente comando para importar la colección de facturas a la db finanzas:

```
menendez91@ubuntu:~$ cd docker-scripts/mongodb/
menendez91@ubuntu:~/docker-scripts/mongodb$ ./mongo_client.sh 
=====================================================================
Cliente mongodb
Ingrese el comando 'mongo' para gestionar
=====================================================================
root@b52d33afcb73:/# mongo
root@b52d33afcb73:/# ls   
bin   data  docker-entrypoint-initdb.d  home        lib    media  opt   root  sbin  sys  usr
boot  dev   etc                         js-yaml.js  lib64  mnt    proc  run   srv   tmp  var
root@b52d33afcb73:/# cd data
root@b52d33afcb73:/data# cd db
root@b52d33afcb73:/data/db# ls
WiredTiger         WiredTigerHS.wt                       collection-4--6158487284959031766.wt  index-3--6158487284959031766.wt  mongod.lock
WiredTiger.lock    _mdb_catalog.wt                       diagnostic.data                       index-5--6158487284959031766.wt  sizeStorer.wt
WiredTiger.turtle  collection-0--6158487284959031766.wt  facturas.json                         index-6--6158487284959031766.wt  storage.bson
WiredTiger.wt      collection-2--6158487284959031766.wt  index-1--6158487284959031766.wt       journal
root@b52d33afcb73:/data/db# mongoimport -d finanzas -c facturas --file facturas.json
2020-08-09T14:09:04.465+0000	connected to: mongodb://localhost/
2020-08-09T14:09:04.699+0000	100 document(s) imported successfully. 0 document(s) failed to import.
```
## 2. Abrir el shell de MongoDB (mongo) y pararse en la db finanzas:

```
root@b52d33afcb73:/data/db# mongo
MongoDB shell version v4.4.0
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("be018e10-1b70-4384-9936-2cf7946c7a48") }
MongoDB server version: 4.4.0
---
The server generated these startup warnings when booting: 
        2020-08-09T14:04:44.290+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
        2020-08-09T14:04:45.832+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
---
---
        Enable MongoDB's free cloud-based monitoring service, which will then receive and display
        metrics about your deployment (disk utilization, CPU, operation statistics, etc).

        The monitoring data will be available on a MongoDB website with a unique URL accessible to you
        and anyone you share the URL with. MongoDB may use this information to make product
        improvements and to suggest MongoDB products and deployment options to you.

        To enable free monitoring, run the following command: db.enableFreeMonitoring()
        To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
> show dbs
admin     0.000GB
config    0.000GB
finanzas  0.000GB
local     0.000GB
> use finanzas
switched to db finanzas
> db
finanzas
> show collections
facturas

```

## 3. Realizar las siguientes operaciones:

### a. Consultar la cantidad de documentos insertados.

```
> db.facturas.find().count()
100
```

### b. Obtener 1 sólo documento para ver el esquema y los nombres de los campos. Sin mostrar el _id.

```
> db.facturas.findOne({},{_id:0})
{
	"cliente" : {
		"apellido" : "Malinez",
		"cuit" : 2740488484,
		"nombre" : "Marina",
		"region" : "CENTRO"
	},
	"condPago" : "CONTADO",
	"fechaEmision" : ISODate("2014-02-20T00:00:00Z"),
	"fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"),
	"item" : [
		{
			"cantidad" : 11,
			"precio" : 18,
			"producto" : " CORREA 12mm"
		},
		{
			"cantidad" : 1,
			"precio" : 490,
			"producto" : "TALADRO 12mm"
		}
	],
	"nroFactura" : 1063
}

```

### c. Obtener las facturas con fecha de emisión posterior al 23/02/2014 y número menor a 1500. Ordenar por región y cuit del cliente

```
> db.facturas.find({$and:[{"fechaEmision":{$gt:new ISODate("2014-02-23")}},{"nroFactura":{$lt:1500}}]}).sort({"cliente.region":1, "cliente.cuit": 1})
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410a7"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1068 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410a8"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1069 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041068"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1005 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041066"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1003 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041069"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1006 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041070"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1013 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04106d"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1010 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04106f"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1012 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041074"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1017 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041077"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1020 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041076"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1019 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410a5"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1066 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041082"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1031 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04107e"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1027 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041085"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1034 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041084"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1033 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041089"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1038 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04107d"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1026 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a04108c"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1041 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a04108b"), "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1040 }
Type "it" for more
```

### d. Obtener sólo los datos de cliente de las facturas donde se haya comprado “CORREA 10mm”. Ordenar por apellido del cliente.

```
> db.facturas.find({"item.producto":"CORREA 10mm"},{"cliente":1, "_id":0}).sort({"cliente.apellido":1})
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" } }
Type "it" for more
```
### e. Obtener sólo nombre y apellido de cliente, de las facturas con número entre 2500 y 3000. (Entre 1000 y 3000 si hay)

```
> db.facturas.find({$and:[{"nroFactura":{$gt:1000}},{"nroFactura":{$lt:3000}}]},{"_id":0,"cliente.apellido":1,"cliente.nombre":1})
{ "cliente" : { "apellido" : "Malinez", "nombre" : "Marina" } }
{ "cliente" : { "apellido" : "Zavasi", "nombre" : "Martin" } }
{ "cliente" : { "apellido" : "Manoni", "nombre" : "Juan Manuel" } }
{ "cliente" : { "apellido" : "Zavasi", "nombre" : "Martin" } }
{ "cliente" : { "apellido" : "Lavagno", "nombre" : "Soledad" } }
{ "cliente" : { "apellido" : "Manoni", "nombre" : "Juan Manuel" } }
{ "cliente" : { "apellido" : "Zavasi", "nombre" : "Martin" } }
{ "cliente" : { "apellido" : "Lavagno", "nombre" : "Soledad" } }
{ "cliente" : { "apellido" : "Zavasi", "nombre" : "Martin" } }
{ "cliente" : { "apellido" : "Manoni", "nombre" : "Juan Manuel" } }
{ "cliente" : { "apellido" : "Zavasi", "nombre" : "Martin" } }
{ "cliente" : { "apellido" : "Manoni", "nombre" : "Juan Manuel" } }
{ "cliente" : { "apellido" : "Manoni", "nombre" : "Juan Manuel" } }
{ "cliente" : { "apellido" : "Zavasi", "nombre" : "Martin" } }
{ "cliente" : { "apellido" : "Malinez", "nombre" : "Marina" } }
{ "cliente" : { "apellido" : "Zavasi", "nombre" : "Martin" } }
{ "cliente" : { "apellido" : "Manoni", "nombre" : "Juan Manuel" } }
{ "cliente" : { "apellido" : "Malinez", "nombre" : "Marina" } }
{ "cliente" : { "apellido" : "Manoni", "nombre" : "Juan Manuel" } }
{ "cliente" : { "apellido" : "Malinez", "nombre" : "Marina" } }
Type "it" for more

```
### f. Obtener sólo la fecha de vencimiento de las facturas 5000, 6000, 7000 y 8000. (Sin resultados, pero de la 1000 si hay)

```
> db.facturas.find({$or:[{"nroFactura":1000},{"nroFactura":6000},{"nroFactura":7000},{"nroFactura":8000}]},{"fechaVencimiento":1, "_id":0})
{ "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z") }
```
### g. Obtener las facturas de los clientes cuyo apellido comience con Z. Ordenar por número de factura y devolver solo las primeras 5.

```
> db.facturas.find({"cliente.apellido":{$regex:"Z"}}).sort({"nroFactura":1}).limit(5)
{ "_id" : ObjectId("55e4a6fbbfc68c676a041064"), "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1001 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041065"), "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1002 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04106b"), "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1008 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04106c"), "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1009 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041072"), "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1015 }

```
### h. Obtener sólo los números de factura en las que la región sea “CENTRO” o la condición de pago sea “CONTADO”. Ordenar descendentemente por número de factura y devolver de la 5 a la 10.

```
> db.facturas.find({$or:[{"cliente.region":"CENTRO"},{"condPago":"CONTADO"}]},{"_id":0}).sort({"nroFactura":-1}).skip(5).limit(5)
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1090 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1087 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1086 }
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1084 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1083 }

```
### i. Obtener las facturas de todos los clientes que no sean de apellido “Zavasi” ni “Malinez”.

```
> db.facturas.find( { $nor: [{"cliente.apellido":"Zavasi"},{"cliente.apellido":"Malinez"}]}, {"_id":0})
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1068 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1067 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1069 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1004 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1005 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1003 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1006 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1013 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1010 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1012 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1018 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1017 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1020 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "60 Ds FF", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-04-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1019 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1011 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1066 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1025 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1031 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1032 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1027 }
Type "it" for more

```
### j. Obtener sólo el nombre del producto de las facturas donde se haya comprado 15 unidades de dicho producto.

```
> db.facturas.find({"item.cantidad":15},{"_id":0,"item.producto":1})
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }
{ "item" : [ { "producto" : "TUERCA 2mm" }, { "producto" : "TALADRO 12mm" }, { "producto" : "TUERCA 5mm" } ] }

```
### k. Obtener sólo una factura del cliente de cuit 2038373771, condición de pago “30 Ds FF” y fecha de vencimiento entre el 20/03/2014 y 24/03/2014.

```
> db.facturas.findOne({$and:[{"cliente.cuit":2038373771},{"condPago":"30 Ds FF"},{$and:[{"fechaVencimiento":{$gt:new ISODate("2014-03-20")}},{"fechaVencimiento":{$lt:new ISODate("2014-03-24")}}]}]})
{
	"_id" : ObjectId("55e4a6fcbfc68c676a0410aa"),
	"cliente" : {
		"apellido" : "Zavasi",
		"cuit" : 2038373771,
		"nombre" : "Martin",
		"region" : "CABA"
	},
	"condPago" : "30 Ds FF",
	"fechaEmision" : ISODate("2014-02-20T00:00:00Z"),
	"fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"),
	"item" : [
		{
			"cantidad" : 2,
			"precio" : 134,
			"producto" : "CORREA 10mm"
		}
	],
	"nroFactura" : 1071
}

```

# Práctica de MongoDB parte 2

## 1. Abrir el shell de MongoDB (mongo) y pararse en la db finanzas:


```
Codigo

```

## 2. Realizar las siguientes operaciones:

### a. Obtener los números de factura de los clientes que contengan una i (minúscula o mayúscula) pero que no sea Manoni.


```
> db.facturas.find({"cliente.apellido":{$regex:"i",$nin:['Manoni'],$options:"i"}},{nroFactura:1,"cliente.apellido":1,_id:0})
{ "cliente" : { "apellido" : "Malinez" }, "nroFactura" : 1063 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1071 }
{ "cliente" : { "apellido" : "Malinez" }, "nroFactura" : 1070 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1072 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1001 }
{ "cliente" : { "apellido" : "Malinez" }, "nroFactura" : 1000 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1002 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1065 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1009 }
{ "cliente" : { "apellido" : "Malinez" }, "nroFactura" : 1014 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1064 }
{ "cliente" : { "apellido" : "Malinez" }, "nroFactura" : 1007 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1008 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1015 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1016 }
{ "cliente" : { "apellido" : "Malinez" }, "nroFactura" : 1021 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1022 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1023 }
{ "cliente" : { "apellido" : "Zavasi" }, "nroFactura" : 1036 }
{ "cliente" : { "apellido" : "Malinez" }, "nroFactura" : 1035 }
Type "it" for more
> 
```

### b. Obtener las facturas de clientes con apellido terminado en i, cuya región sea CABA y donde hayan comprado una TUERCA, sin importar su medida.

```
> db.facturas.find({$and:[{"cliente.apellido":{$regex:".*i$",$options:"i"}},{"cliente.region":"CABA"},{"item.producto":{$regex:"TUERCA.*"}}]},{_id:0})
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1072 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1002 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1065 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1009 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1016 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1023 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1037 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1044 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1051 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1030 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1058 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1079 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1086 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1093 }
> 
```

### c. Obtener las facturas donde se haya comprado una correa, sin importar la medida.

```
> db.facturas.find({"item.producto":{$regex:"correa.*", $options:"i"}},{_id:0}).limit(1)
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1063 }
>
```

### d. Obtener los datos de cliente de las facturas donde se haya comprado 12 o más de cualquier ítem.

```
> db.facturas.find({"item.cantidad":{$gt:12}},{"cliente":1,_id:0})
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" } }
>
```

### e. Obtener las facturas donde no se hayan comprado "SET HERRAMIENTAS"


```
> db.facturas.find({"item.producto":{ $not:{$regex:"set herramientas", $options:"i"}}},{_id:0})
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1063 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1071 }
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1070 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1072 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1001 }
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1000 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1002 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1003 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1006 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1065 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1010 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1009 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1013 }
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1014 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1064 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-25T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-25T00:00:00Z"), "item" : [ { "cantidad" : 10, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1069 }
{ "cliente" : { "apellido" : "Manoni", "cuit" : 2029889382, "nombre" : "Juan Manuel", "region" : "NEA" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" }, { "cantidad" : 15, "precio" : 90, "producto" : "TUERCA 5mm" } ], "nroFactura" : 1066 }
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1007 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1008 }
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"), "item" : [ { "cantidad" : 2, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1015 }
Type "it" for more
>
```

### f. Armar una función en javascript que recorra un cursor con todos las facturas imprimiendo “Factura <nroFactura>: <cliente.apellido> compró <n> items”


```
> { 
... var cursor = db.facturas.find();
... while( cursor.hasNext() ){
...  var factura = cursor.next();
...  var cant = 0;
...  for(var x =0; x < factura.item.length; x++){
... cant = cant +factura.item[x].cantidad; 
... }
... print("Factura n: " + factura.nroFactura + " compró " + cant +" items");
... }
... }
Factura n: 1063 compró 12 items
Factura n: 1071 compró 2 items
Factura n: 1070 compró 12 items
Factura n: 1067 compró 1 items
Factura n: 1072 compró 18 items
Factura n: 1001 compró 2 items
Factura n: 1000 compró 12 items
Factura n: 1002 compró 18 items
Factura n: 1003 compró 18 items
Factura n: 1004 compró 1 items
Factura n: 1006 compró 10 items
Factura n: 1005 compró 2 items
Factura n: 1068 compró 2 items
Factura n: 1065 compró 18 items
Factura n: 1010 compró 18 items
Factura n: 1011 compró 1 items
Factura n: 1009 compró 18 items
Factura n: 1013 compró 10 items
Factura n: 1012 compró 2 items
Factura n: 1014 compró 12 items
Factura n: 1064 compró 2 items
Factura n: 1069 compró 10 items
Factura n: 1066 compró 18 items
Factura n: 1007 compró 12 items
Factura n: 1008 compró 2 items
Factura n: 1015 compró 2 items
Factura n: 1016 compró 18 items
Factura n: 1018 compró 1 items
Factura n: 1017 compró 18 items
Factura n: 1019 compró 2 items
Factura n: 1020 compró 10 items
Factura n: 1021 compró 12 items
Factura n: 1022 compró 2 items
Factura n: 1023 compró 18 items
Factura n: 1024 compró 18 items
Factura n: 1025 compró 1 items
Factura n: 1032 compró 1 items
Factura n: 1033 compró 2 items
Factura n: 1031 compró 18 items
Factura n: 1034 compró 10 items
Factura n: 1036 compró 2 items
Factura n: 1035 compró 12 items
Factura n: 1037 compró 18 items
Factura n: 1039 compró 1 items
Factura n: 1038 compró 18 items
Factura n: 1040 compró 2 items
Factura n: 1041 compró 10 items
Factura n: 1042 compró 12 items
Factura n: 1044 compró 18 items
Factura n: 1043 compró 2 items
Factura n: 1045 compró 18 items
Factura n: 1046 compró 1 items
Factura n: 1048 compró 10 items
Factura n: 1047 compró 2 items
Factura n: 1026 compró 2 items
Factura n: 1028 compró 12 items
Factura n: 1049 compró 12 items
Factura n: 1050 compró 2 items
Factura n: 1051 compró 18 items
Factura n: 1052 compró 18 items
Factura n: 1053 compró 1 items
Factura n: 1054 compró 2 items
Factura n: 1055 compró 10 items
Factura n: 1056 compró 12 items
Factura n: 1057 compró 2 items
Factura n: 1027 compró 10 items
Factura n: 1030 compró 18 items
Factura n: 1029 compró 2 items
Factura n: 1059 compró 18 items
Factura n: 1058 compró 18 items
Factura n: 1075 compró 2 items
Factura n: 1076 compró 10 items
Factura n: 1077 compró 12 items
Factura n: 1078 compró 2 items
Factura n: 1079 compró 18 items
Factura n: 1081 compró 1 items
Factura n: 1080 compró 18 items
Factura n: 1083 compró 10 items
Factura n: 1082 compró 2 items
Factura n: 1084 compró 12 items
Factura n: 1060 compró 1 items
Factura n: 1088 compró 1 items
Factura n: 1087 compró 18 items
Factura n: 1089 compró 2 items
Factura n: 1086 compró 18 items
Factura n: 1090 compró 10 items
Factura n: 1091 compró 12 items
Factura n: 1092 compró 2 items
Factura n: 1093 compró 18 items
Factura n: 1094 compró 18 items
Factura n: 1095 compró 1 items
Factura n: 1096 compró 2 items
Factura n: 1097 compró 10 items
Factura n: 1062 compró 10 items
Factura n: 1085 compró 2 items
Factura n: 1073 compró 18 items
Factura n: 1074 compró 1 items
Factura n: 1061 compró 2 items
Factura n: 1098 compró 12 items
Factura n: 1099 compró 2 items
> 
```

### g. Insertar una factura número 999 con usted como cliente, habiendo comprado un Destornillador.


```
> db.facturas.insert({nroFactura:999,item:{cantidad:1,precio:270,producto:"Destornillador"},cliente:{apellido:"Menendez",cuit:0000000001, nombre: "Martin", region:"CABA"}})
WriteResult({ "nInserted" : 1 })
> 

```

### h. ¿De qué forma insertará el siguiente array de documentos para que, sin importar si alguno de ellos falla, el resto igualmente se inserte? [{_id:1},{_id:1},{_id:2}]


```
> var bulk = db.collection.initializeUnorderedBulkOp();
> bulk.insert({_id:1});
> bulk.insert({_id:1});
> bulk.insert({_id:2});
> bulk.execute();
uncaught exception: BulkWriteError({
	"writeErrors" : [
		{
			"index" : 1,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: finanzas.collection index: _id_ dup key: { _id: 1.0 }",
			"op" : {
				"_id" : 1
			}
		}
	],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
}) :
BulkWriteError({
	"writeErrors" : [
		{
			"index" : 1,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: finanzas.collection index: _id_ dup key: { _id: 1.0 }",
			"op" : {
				"_id" : 1
			}
		}
	],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
BulkWriteError@src/mongo/shell/bulk_api.js:367:48
BulkWriteResult/this.toError@src/mongo/shell/bulk_api.js:332:24
Bulk/this.execute@src/mongo/shell/bulk_api.js:1186:23
@(shell):1:1
> 
```

### i. Mediante un Bulk agregarle a cada factura del cliente Lavagno agregarle el campo “tipo” con el valor “VIP”. Este deberá estar dentro del campo cliente. (cliente: {nombre:…, apellido:…, tipo:…, …}). Junto con esto, eliminar las facturas entre 2000 y 2222.

```
> {
... var bulk = db.facturas.initializeOrderedBulkOp();
... bulk.find({"cliente.apellido":"Lavagno"}).update({ $set: {"cliente.tipo":"VIP"}});
... bulk.find({$and:[{"nroFactura":{$gt:2000}},{"nroFactura":{$lt:2222}}]}).remove();
... bulk.execute();
... }
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 0,
	"nUpserted" : 0,
	"nMatched" : 14,
	"nModified" : 14,
	"nRemoved" : 0,
	"upserted" : [ ]
})
> 
> db.facturas.find({"cliente.apellido":"Lavagno"},{_id:0}).limit(5)
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA", "tipo" : "VIP" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1067 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA", "tipo" : "VIP" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1004 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA", "tipo" : "VIP" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1011 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA", "tipo" : "VIP" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1018 }
{ "cliente" : { "apellido" : "Lavagno", "cuit" : 2729887543, "nombre" : "Soledad", "region" : "NOA", "tipo" : "VIP" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-24T00:00:00Z"), "fechaVencimiento" : ISODate("2014-03-26T00:00:00Z"), "item" : [ { "cantidad" : 1, "precio" : 700, "producto" : "SET HERRAMIENTAS" } ], "nroFactura" : 1025 }
> 
```

### j. Eliminar todas las facturas de los clientes del CENTRO.

```
> db.facturas.find({"cliente.region":"CENTRO"},{_id:0}).limit(2)
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1063 }
{ "cliente" : { "apellido" : "Malinez", "cuit" : 2740488484, "nombre" : "Marina", "region" : "CENTRO" }, "condPago" : "CONTADO", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 11, "precio" : 18, "producto" : " CORREA 12mm" }, { "cantidad" : 1, "precio" : 490, "producto" : "TALADRO 12mm" } ], "nroFactura" : 1070 }
> 
> {
... var bulk = db.facturas.initializeOrderedBulkOp();
... bulk.find({"cliente.region":"CENTRO"}).remove();
... bulk.execute();
... }
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 0,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 15,
	"upserted" : [ ]
})
>
> db.facturas.find({"cliente.region":"CENTRO"},{_id:0}).limit(2)
> 
```

### k. Obtener la factura con el mayor nroFactura del cliente de apellido Lavagno donde haya comprado “SET HERRAMIENTAS” y luego asegurarse de borrar únicamenteesa factura.


```
> db.facturas.find({$and:[{"cliente.apellido":"Lavagno"},{"item.producto":"SET HERRAMIENTAS"}]},{"nroFactura":1}).sort({"nroFactura":-1})
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410c2"), "nroFactura" : 1095 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410bb"), "nroFactura" : 1088 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410b4"), "nroFactura" : 1081 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410ad"), "nroFactura" : 1074 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410a6"), "nroFactura" : 1067 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a04109f"), "nroFactura" : 1060 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041098"), "nroFactura" : 1053 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041091"), "nroFactura" : 1046 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a04108a"), "nroFactura" : 1039 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041083"), "nroFactura" : 1032 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04107c"), "nroFactura" : 1025 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041075"), "nroFactura" : 1018 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04106e"), "nroFactura" : 1011 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041067"), "nroFactura" : 1004 }
> 
> {
... var factura = db.facturas.find({$and:[{"cliente.apellido":"Lavagno"},{"item.producto":"SET HERRAMIENTAS"}]}).sort({"nroFactura":-1}).limit(1);
... db.facturas.remove({_id:factura.next()._id});
... }
WriteResult({ "nRemoved" : 1 })
> 
> db.facturas.find({$and:[{"cliente.apellido":"Lavagno"},{"item.producto":"SET HERRAMIENTAS"}]},{"nroFactura":1}).sort({"nroFactura":-1})
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410bb"), "nroFactura" : 1088 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410b4"), "nroFactura" : 1081 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410ad"), "nroFactura" : 1074 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410a6"), "nroFactura" : 1067 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a04109f"), "nroFactura" : 1060 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041098"), "nroFactura" : 1053 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041091"), "nroFactura" : 1046 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a04108a"), "nroFactura" : 1039 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a041083"), "nroFactura" : 1032 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04107c"), "nroFactura" : 1025 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041075"), "nroFactura" : 1018 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a04106e"), "nroFactura" : 1011 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041067"), "nroFactura" : 1004 }
> 
```

### l. Incrementar el número de factura de la 1501. (NO EXISTE! -> Usare 1050)


```
> db.facturas.findOne({"nroFactura":1050})
{
	"_id" : ObjectId("55e4a6fcbfc68c676a041095"),
	"cliente" : {
		"apellido" : "Zavasi",
		"cuit" : 2038373771,
		"nombre" : "Martin",
		"region" : "CABA"
	},
	"condPago" : "30 Ds FF",
	"fechaEmision" : ISODate("2014-02-20T00:00:00Z"),
	"fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"),
	"item" : [
		{
			"cantidad" : 2,
			"precio" : 134,
			"producto" : "CORREA 10mm"
		}
	],
	"nroFactura" : 1050
}
> db.facturas.update({"nroFactura":1050},{$inc:{"nroFactura":1}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> 
> db.facturas.findOne({"nroFactura":1050},{_id:0})
null
> 
> db.facturas.findOne({"nroFactura":1051},{_id:0})
{
	"cliente" : {
		"apellido" : "Zavasi",
		"cuit" : 2038373771,
		"nombre" : "Martin",
		"region" : "CABA"
	},
	"condPago" : "30 Ds FF",
	"fechaEmision" : ISODate("2014-02-20T00:00:00Z"),
	"fechaVencimiento" : ISODate("2014-03-22T00:00:00Z"),
	"item" : [
		{
			"cantidad" : 2,
			"precio" : 134,
			"producto" : "CORREA 10mm"
		}
	],
	"nroFactura" : 1051
}
> 
```

### m. A la factura número 1500 cambiarle la condición de pago a “30 Ds FF” -> (NO EXISTE 1500! usare la 1002)


```
> db.facturas.find({},{"nroFactura":1002,"condPago":1}).limit(5)
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410aa"), "condPago" : "30 Ds FF", "nroFactura" : 1071 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410a6"), "condPago" : "30 Ds FF", "nroFactura" : 1067 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410ab"), "condPago" : "CONTADO", "nroFactura" : 1072 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041064"), "condPago" : "30 Ds FF", "nroFactura" : 1001 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041065"), "condPago" : "CONTADO", "nroFactura" : 1002 }
>
> db.facturas.update({"nroFactura":1002},{$set:{"condPago":"30 Ds FF"}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
>
> db.facturas.find({},{"nroFactura":1002,"condPago":1}).limit(5)
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410aa"), "condPago" : "30 Ds FF", "nroFactura" : 1071 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410a6"), "condPago" : "30 Ds FF", "nroFactura" : 1067 }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410ab"), "condPago" : "CONTADO", "nroFactura" : 1072 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041064"), "condPago" : "30 Ds FF", "nroFactura" : 1001 }
{ "_id" : ObjectId("55e4a6fbbfc68c676a041065"), "condPago" : "30 Ds FF", "nroFactura" : 1002 }
>
```

### n. Restarle 9 al número de factura de la 1510. (NO EXISTE 1510! usare la 1071)

```
> db.facturas.update({"nroFactura":1071},{$inc:{"nroFactura":-9}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
>
> db.facturas.find({},{"nroFactura":1071,"condPago":1}).limit(1)
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410aa"), "condPago" : "30 Ds FF", "nroFactura" : 1062 }
>
```

### o. A la factura número 1515 incrementarle el campo “vistas” y decrementarle el campo “contador”. (NO EXISTE 1515! usare la 1002)

```
> db.facturas.update({"nroFactura":1002},{$inc:{"vistas":1,"contador":-1}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
>
> db.facturas.find({"nroFactura":1002},{_id:0})
{ "cliente" : { "apellido" : "Zavasi", "cuit" : 2038373771, "nombre" : "Martin", "region" : "CABA" }, "condPago" : "30 Ds FF", "fechaEmision" : ISODate("2014-02-20T00:00:00Z"), "fechaVencimiento" : ISODate("2014-02-20T00:00:00Z"), "item" : [ { "cantidad" : 6, "precio" : 60, "producto" : "TUERCA 2mm" }, { "cantidad" : 12, "precio" : 134, "producto" : "CORREA 10mm" } ], "nroFactura" : 1002, "contador" : -1, "vistas" : 1 }
> 
```

### p. Guarde el documento de la factura número 9000 en una variable. Luego haga un update total de la factura 9000 reemplazándola por el documento {x:Math.random()} (generará un número aleatorio para el campo x). ¿Hay forma de consultar el documento modificado? ¿De qué forma? (NO EXISTE 9000! usare la 1073)


```
> db.facturas.findOne({"nroFactura":1073})
{
	"_id" : ObjectId("55e4a6fcbfc68c676a0410ac"),
	"cliente" : {
		"apellido" : "Manoni",
		"cuit" : 2029889382,
		"nombre" : "Juan Manuel",
		"region" : "NEA"
	},
	"condPago" : "CONTADO",
	"fechaEmision" : ISODate("2014-02-24T00:00:00Z"),
	"fechaVencimiento" : ISODate("2014-02-24T00:00:00Z"),
	"item" : [
		{
			"cantidad" : 2,
			"precio" : 60,
			"producto" : "TUERCA 2mm"
		},
		{
			"cantidad" : 1,
			"precio" : 490,
			"producto" : "TALADRO 12mm"
		},
		{
			"cantidad" : 15,
			"precio" : 90,
			"producto" : "TUERCA 5mm"
		}
	],
	"nroFactura" : 1073
}
> 
> {
... var factura = db.facturas.findOne({"nroFactura":1073});
... var id = factura._id;
... db.facturas.update({"nroFactura":factura.nroFactura},{x:Math.random()});
... db.facturas.findOne({"_id":id});
... }
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410ac"), "x" : 0.40223124133015487 }
> 
> db.facturas.find({"x":{$exists:1}})
{ "_id" : ObjectId("55e4a6fcbfc68c676a0410ac"), "x" : 0.40223124133015487 }
> 
```

