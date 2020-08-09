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

```

### d. Obtener sólo los datos de cliente de las facturas donde se haya comprado “CORREA 10mm”. Ordenar por apellido del cliente.

```

```
### e. Obtener sólo nombre y apellido de cliente, de las facturas con número entre 2500 y 3000.

```

```
### f. Obtener sólo la fecha de vencimiento de las facturas 5000, 6000, 7000 y 8000.

```

```
### g. Obtener las facturas de los clientes cuyo apellido comience con Z. Ordenar por número de factura y devolver solo las primeras 5.

```

```
### h. Obtener sólo los números de factura en las que la región sea “CENTRO” o la condición de pago sea “CONTADO”. Ordenar descendentemente por número de factura y devolver de la 5 a la 10.

```

```
### i. Obtener las facturas de todos los clientes que no sean de apellido “Zavasi” ni “Malinez”.

```

```
### j. Obtener sólo el nombre del producto de las facturas donde se haya comprado 15 unidades de dicho producto.

```

```
### k. Obtener sólo una factura del cliente de cuit 2038373771, condición de pago “30 Ds FF” y fecha de vencimiento entre el 20/03/2014 y 24/03/2014.

```

```
