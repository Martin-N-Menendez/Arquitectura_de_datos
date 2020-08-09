1. Abrir una terminal en el directorio donde haya guardado el archivo “facturas.json” y usar el
siguiente comando para importar la colección de facturas a la db finanzas:
mongoimport -d finanzas -c facturas --file facturas.json
2. Abrir el shell de MongoDB (mongo) y pararse en la db finanzas:
3. Realizar las siguientes operaciones:
a. Consultar la cantidad de documentos insertados.
b. Obtener 1 sólo documento para ver el esquema y los nombres de los campos. Sin
mostrar el _id.
c. Obtener las facturas con fecha de emisión posterior al 23/02/2014 y número menor
a 1500. Ordenar por región y cuit del cliente
d. Obtener sólo los datos de cliente de las facturas donde se haya comprado
“CORREA 10mm”. Ordenar por apellido del cliente.
e. Obtener sólo nombre y apellido de cliente, de las facturas con número entre 2500
y 3000.
f. Obtener sólo la fecha de vencimiento de las facturas 5000, 6000, 7000 y 8000.
g. Obtener las facturas de los clientes cuyo apellido comience con Z. Ordenar por
número de factura y devolver solo las primeras 5.
h. Obtener sólo los números de factura en las que la región sea “CENTRO” o la
condición de pago sea “CONTADO”. Ordenar descendentemente por número de
factura y devolver de la 5 a la 10.
i. Obtener las facturas de todos los clientes que no sean de apellido “Zavasi” ni
“Malinez”.
j. Obtener sólo el nombre del producto de las facturas donde se haya comprado 15
unidades de dicho producto.
k. Obtener sólo una factura del cliente de cuit 2038373771, condición de pago “30 Ds
FF” y fecha de vencimiento entre el 20/03/2014 y 24/03/2014.
