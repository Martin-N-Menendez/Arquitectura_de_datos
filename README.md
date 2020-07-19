# Arquitectura de datos

- [x] Definir diagrama UML
- [x] Definir tablas
- [x] Crear base de datos (DB)
- [x] Cargas datos en la DB
- [x] Consultar a la DB

# UML

![UML](uml.png)


# Topología


![bypass](bypass.jpg)

# Tablas

## Secciones

| ID | nombre | estado |
| :-: | :-: | :-: |
| 1 | 'T_1'  | 0 |
| 2 | 'T_2'  | 1 |
| 3 | 'T_3'  | 1 |
| 4 | 'T_4'  | 1 |
| 5 | 'T_5'  | 0 |
| 6 | 'T_6'  | 1 |
| 7 | 'T_7'  | 1 |
| 8 | 'T_8'  | 1 |

## SEC_a_MDC
 
| ID | MDC_ID |
| :-: | :-: |
| 2 | 1 |
| 5 | 2 |

## Adyacencias

| ID1 | ID2 | puerto |
| :-: | :-: | :-: |
| 1 | 2 | 2 |
| 2 | 1 | 1 |
| 2 | 3 | 2 |
| 2 | 4 | 3 |
| 3 | 2 | 1 |
| 3 | 5 | 3 |
| 4 | 2 | 1 |
| 4 | 5 | 2 |
| 5 | 3 | 1 |
| 5 | 6 | 2 |
| 6 | 5 | 1 |
| 6 | 7 | 2 |
| 7 | 6 | 1 |
| 7 | 8 | 2 |
| 8 | 7 | 2 |

## SEC_a_SEM

| ID | SEM_ID |
| :-: | :-: |
| 1 | 1 |
| 1 | 2 |
| 3 | 3 |
| 3 | 6 |
| 4 | 4 |
| 4 | 5 |
| 6 | 7 |
| 6 | 8 |
| 6 | 9 |
| 8 | 10 |

## SEC_a_PAN

| ID | PAN_ID |
| :-: | :-: |
| 7 | 1 |

## Maquina_de_cambios

| ID | nombre | anterior | posterior | desvio | estado |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 1 | 'MDC_1'  | 1 | 3 | 4 | 0 |
| 2 | 'MDC_2'  | 3 | 6 | 4 | 0 |

## Semaforos

| ID1 | ID2 | puerto |
| :-: | :-: | :-: |
| 1 | 1 | 0 |
| 2 | 2 | 0 |
| 3 | 1 | 0 |
| 4 | 1 | 0 |
| 5 | 2 | 0 |
| 6 | 2 | 0 |
| 7 | 1 | 0 |
| 8 | 2 | 0 |
| 9 | 2 | 0 |
| 10 | 2 | 0 |

## Paso_a_nivel

| ID | estado |
| :-: | :-: |
| 1 | 0 |

## Estado_SEC

| ID | estado |
| :-: | :-: |
| 0 | 'Ocupado' |
| 1 | 'Libre' |

## Tipo_puertos

| ID | puertos |
| :-: | :-: |
| 1 | 'Anterior' |
| 2 | 'Posterior' |
| 3 | 'Desvio' |

## Estado_MDC

| ID | estado |
| :-: | :-: |
| 0 | 'Normal' |
| 1 | 'Reverso' |

## Tipo_SEM

| ID | tipo |
| :-: | :-: |
| 1 | 'Circulacion' |
| 2 | 'Maniobra' |

## Estado_SEM

| ID | estado |
| :-: | :-: |
| 0 | 'Rojo' |
| 1 | 'Amarillo' |
| 2 | 'Verde' |

## Estado_PAN

| ID | estado |
| :-: | :-: |
| 0 | 'Barrera baja' |
| 1 | 'Barrera alta' |

# Consultas

## Contar cantidad de secciones

```SQL
SELECT COUNT(ID) FROM secciones
```
Resultado = 8

## Mostrar solo secciones ocupados

```SQL
SELECT ID,nombre, (SELECT estado FROM estado_sec WHERE ID = secciones.estado) FROM secciones
WHERE estado = 0
```

| ID | nombre | estado |
| :-: | :-: | :-: |
| 1 | 'T_1'  | Ocupado |
| 5 | 'T_5'  | Ocupado |

## Mostrar solo secciones libres

```SQL
SELECT ID,nombre, (SELECT estado FROM estado_sec WHERE ID = secciones.estado) FROM secciones
WHERE estado = 1
```

| ID | nombre | estado |
| :-: | :-: | :-: |
| 2 | 'T_2'  | Desocupado |
| 3 | 'T_3'  | Desocupado |
| 4 | 'T_4'  | Desocupado |
| 6 | 'T_6'  | Desocupado |
| 7 | 'T_7'  | Desocupado |
| 8 | 'T_8'  | Desocupado |

## Mostrar a que sección esta conectada cada una y en que puerto

```SQL
SELECT ID,nombre,ID2 AS conectado_con,puerto FROM secciones AS A1
FULL JOIN (SELECT ID1,ID2,(SELECT puerto FROM tipo_puertos WHERE ID = adyacencias.puerto) FROM adyacencias) AS B1
ON A1.ID = B1.ID1
```

| ID | nombre | conectado_con | puerto |
| :-: | :-: | :-: | :-: |
| 1 | "T_1" | 2 | "Posterior" |
| 2 | "T_2" | 1 | "Anterior" |
| 2 | "T_2" | 3 | "Posterior" |
| 2 | "T_2" | 4 | "Desvio" |
| 3 | "T_3" | 2 | "Anterior" |
| 3 | "T_3" | 5 | "Desvio" |
| 4 | "T_4" | 2 | "Anterior" |
| 4 | "T_4" | 5 | "Posterior" |
| 5 | "T_5" | 3 | "Anterior" |
| 5 | "T_5" | 6 | "Posterior" |
| 6 | "T_6" | 5 | "Anterior" |
| 6 | "T_6" | 7 | "Posterior" |
| 7 | "T_5" | 6 | "Anterior" |
| 7 | "T_7" | 8 | "Posterior" |
| 8 | "T_8" | 7 | "Anterior" |

## Mostrar tipo y estado de semáforos

```SQL
SELECT ID,
(SELECT tipo FROM tipo_sem WHERE ID = semaforos.tipo), 
(SELECT estado FROM estado_sem WHERE ID = semaforos.estado) FROM semaforos
```

| ID | tipo | estado |
| :-: | :-: | :-: |
| 1 | "Circulacion" | "Rojo" |
| 2 | "Maniobra" | "Rojo" |
| 3 | "Circulacion" | "Rojo" |
| 4 | "Circulacion" | "Rojo" |
| 5 | "Maniobra" | "Rojo" |
| 6 | "Maniobra" | "Rojo" |
| 7 | "Circulacion" | "Rojo" |
| 8 | "Maniobra" | "Rojo" |
| 9 | "Maniobra" | "Rojo" |
| 10 | "Maniobra" | "Rojo" |

## Mostrar estado de pasos a nivel

```SQL
SELECT ID,
(SELECT estado FROM estado_pan WHERE ID = paso_a_nivel.estado) FROM paso_a_nivel
```

| ID | estado |
| :-: | :-: |
| 1 | "Barrera baja" |

## Mostrar estado de máquinas de cambios

```SQL
SELECT ID,nombre,anterior,posterior,desvio,
(SELECT estado FROM estado_mdc WHERE ID = maquina_de_cambios.estado) FROM maquina_de_cambios
```

| ID | nombre | anterior | posterior | desvio | estado |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 1 | "MDC_1" | 1 | 3 | 4 | "Normal" |
| 2 | "MDC_2" | 3 | 6 | 4 | "Normal" |

