# Práctica de MongoDB

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
