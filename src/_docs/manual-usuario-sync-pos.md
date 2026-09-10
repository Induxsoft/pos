# POS Sync — Manual del usuario

Para el técnico que instala y opera la sincronización de un punto de
venta con la central.

Este manual no explica cómo está construido el sistema; eso está en
`LEEME.md`. Aquí sólo está lo que hay que hacer y lo que hay que mirar
cuando algo no cuadra.

---

## 1. Qué hace

El punto vende contra su propia base de datos y **no depende del
internet para operar**. Cada pocos minutos sube al servidor central lo
que vendió, y cada cierto tiempo baja de la central los catálogos, los
precios, las existencias y los saldos de clientes.

Si el enlace se cae, la caja sigue cobrando. Cuando vuelve, todo se
pone al corriente solo.

**Lo que se administra en la central:** productos, clientes, precios,
compras, movimientos de inventario, facturación y cancelaciones.

**Lo que ocurre en el punto:** vender y cobrar. Nada más.

---

## 2. Antes de instalar

### La base local debe estar vacía

Es un requisito, no una recomendación. Toda la sincronización funciona
porque los códigos de productos, clientes, cajas y almacenes son los
mismos en los dos lados, y eso sólo se garantiza si todos nacen en la
central.

Si el punto ya venía operando con su propia base, **haga un respaldo y
consérvelo como consulta**. No intente sincronizar sobre ella: los
códigos no van a coincidir y las ventas se van a atorar, o peor, van a
entrar apuntando al producto equivocado.

### Lo que hay que tener listo en la central

- La **ubicación** dada de alta, con su código.
- Su **token**, generado en el catálogo de ubicaciones.
- Los **almacenes, centros de consumo, cajas y cajeros** de esa sucursal,
  asignados a esa ubicación.
- Una **serie de folios propia** para el punto, que ninguna otra
  sucursal use.

Esto último es importante: si dos sucursales comparten serie, la
segunda que suba se va a quedar atorada.

---

## 3. Instalación

### 3.1 El archivo de configuración

Cree `sync.json` junto a los programas:

```json
{
  "qname": "Local@MaxiComercio.R5",
  "endpoint": "https://central.suempresa.mx/pos/_services/sync-v12/",
  "token": "el-token-de-esta-ubicacion",
  "ubicacion": "SUC01",
  "agente": "1.0.0",

  "trabajo": "./trabajo",
  "log": "./log/sync.log",
  "max_int": 10,

  "tx": { "cortes": 5, "docs": 50, "max": 0 }
}
```

| Clave | Qué es |
|---|---|
| `qname` | Conexión a la base local |
| `endpoint` | Dirección del servicio en la central |
| `token` | El de esta ubicación. **Nunca se pone en la línea de comandos** |
| `ubicacion` | El código de la sucursal. Debe coincidir con el del token |
| `trabajo` | Carpeta de las bandejas |
| `max_int` | Reintentos antes de dar un paquete por detenido |
| `tx.cortes` | Cortes por paquete |
| `tx.docs` | Documentos por corte y por paquete |

El archivo lleva el token, así que **protéjalo con permisos de lectura
sólo para la cuenta que corre las tareas**.

### 3.2 Verificar la conexión

Antes de bajar nada:

```
dkl ./sync/obtener-cat-tcambio.dkl "config=./sync.json"
```

Si responde con la marca del lote y aplica, el token es correcto y la
ubicación coincide. Si no, vea la sección 7.

### 3.3 Carga inicial

**El orden es obligatorio.** Cada lote necesita que el anterior ya esté.

```
dkl ./sync/obtener-cat-base.dkl        "config=sync/sync.json"
dkl ./sync/obtener-cat-estructura.dkl  "config=sync/sync.json"
dkl ./sync/obtener-cat-composicion.dkl "config=sync/sync.json"
dkl ./sync/obtener-cat-cliente.dkl     "config=sync/sync.json"
dkl ./sync/obtener-cat-operacion.dkl   "config=sync/sync.json"
dkl ./sync/obtener-precios.dkl         "config=sync/sync.json"
dkl ./sync/obtener-exis.dkl            "config=sync/sync.json"
dkl ./sync/obtener-saldos.dkl          "config=sync/sync.json"
```

Con eso el punto ya puede vender.

Si alguno falla, **deténgase y resuélvalo** antes de seguir. Correr
`composicion` sin `estructura` deja recetas colgando de productos que no
existen.

---

## 4. Las tareas programadas

Ocho tareas en el Programador de tareas de Windows. En todas marque
**"No iniciar una nueva instancia si ya está en ejecución"**.

| Programa | Cada | Qué hace |
|---|---|---|
| `enviar-t` | 5 min | Sube las ventas |
| `reintentar-t` | 15 min | Reintenta lo que no salió |
| `obtener-exis` | 30 min | Baja existencias |
| `obtener-saldos` | 30 min | Baja saldos de clientes |
| `obtener-precios` | 15 min | Baja precios |
| `obtener-cat-tcambio` | 1 hora | Baja tipos de cambio |
| `obtener-cat-estructura` | diario | Baja productos |
| `obtener-cat-*` (resto) | diario | Baja los demás catálogos |

Ajuste según el negocio. Si repricia varias veces al día, suba la
frecuencia de `obtener-precios`. Si no maneja moneda extranjera,
`obtener-cat-tcambio` puede ser diario. Si no vende a crédito,
`obtener-saldos` no hace falta.

**`obtener-cat-composicion` va siempre después de
`obtener-cat-estructura`**, con separación suficiente para que el
primero termine.

Todas admiten los mismos parámetros:

```
dkl ./sync/enviar-t.dkl "config=./sync.json"
```

---

## 5. Cómo saber que está bien

### La bandeja de salida

```
trabajo/salida
```

**Vacía es lo normal.** Cada archivo ahí es un paquete de ventas que
todavía no llegó a la central.

Uno o dos archivos entre corridas de `reintentar-t` no es problema. Una
docena que no baja significa que el enlace está caído o que la central
está rechazando algo.

```
trabajo/enviados     ya confirmados. Se pueden borrar después de una semana
trabajo/detenidos    agotaron los reintentos. Requieren atención
```

**Cualquier archivo en `detenidos` necesita que alguien lo mire.** No se
va a resolver solo.

### La bitácora

```sql
SELECT status, COUNT(*) FROM log_sync_paqs GROUP BY status;
```

| Estatus | Significa |
|---|---|
| `enviado` | Confirmado por la central |
| `pendiente` | Generado pero sin confirmar |
| `error` | La central lo rechazó. El motivo está en `mensaje` |
| `detenido` | Agotó los reintentos |

Para ver qué falló:

```sql
SELECT idpaq, reintentos, mensaje
FROM log_sync_paqs
WHERE status IN ('error','detenido')
ORDER BY fechagen DESC;
```

### La salida de los programas

Cada corrida imprime lo que hizo. En `obtener-*`, la línea que más
importa es ésta:

```
   ignorados: 14 (catálogo atrasado)
```

Significa que la central mandó existencias o precios de productos que el
punto todavía no tiene. **Cero es lo normal.** Un número que crece
significa que la bajada de catálogos dejó de correr.

---

## 6. Situaciones normales que parecen problemas

**"Se cayó el internet toda la tarde."** No pasa nada. La caja siguió
vendiendo. Cuando vuelva, `reintentar-t` sube todo lo acumulado en la
siguiente corrida.

**"La existencia que muestra la caja no es exacta."** Es normal y
esperado. La existencia del punto es una copia con unos minutos de
retraso; la verdad está en la central. Si necesita el dato exacto,
consúltelo allá.

**"Un cliente compró a crédito en dos sucursales el mismo día y se pasó
del límite."** Cada punto autoriza contra el saldo que bajó, que puede
tener minutos de antigüedad. Es una consecuencia conocida de operar sin
depender del enlace. Si el negocio no lo tolera, ponga en la central un
límite más conservador que el real.

**"Cancelé una venta y la caja sigue mostrándola."** Las cancelaciones
se hacen en la central. El punto no se entera, y su corte queda como se
cerró. La contabilidad correcta está en la central.

**"El corte de caja no cuadra con lo que veo en la central."** Si hubo
cancelaciones posteriores al corte, es lo esperado. El corte del punto
es un documento histórico: refleja lo que el cajero entregó.

**"Cambié un precio en la central y la caja sigue con el viejo."** Espere
a la siguiente corrida de `obtener-precios`, o dispárela a mano. Si
urge:

```
dkl ./sync/obtener-precios.dkl "config=./sync.json"
```

**"Di de alta un producto y no lo puedo vender."** El producto llega con
la siguiente bajada de catálogo. Para que llegue ya:

```
dkl ./sync/obtener-cat-estructura.dkl "config=./sync.json"
dkl ./sync/obtener-exis.dkl           "config=./sync.json"
```

Los dos, en ese orden. El primero trae el producto; el segundo, su
existencia.

---

## 7. Cuando algo falla

### "El token no corresponde a ninguna ubicación"

El token del archivo no existe en la central o fue revocado. Pida uno
nuevo y actualice `sync.json`.

### "El sobre declara la ubicación SUC02 pero el token corresponde a SUC01"

**Éste es el error más común al levantar una sucursal nueva**, y casi
siempre significa lo mismo: alguien copió la instalación de otro punto y
no cambió el `sync.json`.

Corrija `ubicacion` y `token` para que correspondan a esta sucursal.

### "Column 'xxx' cannot be null" al bajar un catálogo

Una llave foránea no resolvió: el catálogo del que depende no ha bajado
todavía, o la llave no empata.

Casi siempre es lo primero, y casi siempre es el orden. Verifique que el
lote anterior ya corrió:

```
base  →  estructura  →  composicion  →  cliente  →  operacion
```

Si el error menciona `impuestos`, falta `base`. Si menciona `iproducto`
o `ilinea`, falta `estructura`.

Para comprobar que un catálogo llegó:

```sql
SELECT COUNT(*) FROM cfgimpuesto;
SELECT COUNT(*) FROM linea;
SELECT COUNT(*) FROM producto;
```

Cero en cualquiera de ellos con el punto ya instalado significa que esa
bajada nunca corrió o falló sin que nadie lo notara.

### Un paquete se queda en `error` corrida tras corrida

Mire el mensaje:

```sql
SELECT mensaje FROM log_sync_paqs WHERE idpaq = '…';
```

Los tres motivos habituales:

**Un código que la central no conoce.** Un producto, cliente o cajero
que existe en el punto pero no allá. Con la base limpia no debería
ocurrir; si ocurre, alguien dio de alta algo localmente. Dé de alta ese
registro en la central con el mismo código y el paquete entrará solo.

**Serie de folios compartida.** Dos sucursales usando la misma. Se
resuelve en la central asignando una serie propia a cada punto, y los
paquetes atorados entran después.

**La central no responde.** Verifique el `endpoint` y que el servidor
esté arriba.

### Después de arreglar la causa

No hay que hacer nada especial. `reintentar-t` vuelve a intentar en su
siguiente corrida.

Si el paquete ya llegó a `detenidos`, muévalo de vuelta a `salida` a
mano y déjelo correr.

### Un paquete se perdió y hay ventas que no subieron

Último recurso, cuando el archivo ya no existe y las ventas siguen
marcadas como enviadas:

```
dkl ./sync/desmarcar.dkl "config=./sync.json" "arch=./trabajo/detenidos/xxx.fttjson"
```

Devuelve los documentos de ese paquete a pendientes para que `enviar-t`
los vuelva a tomar.

Si tampoco tiene el archivo, la reconciliación es contra el reporte de
la central: qué folios tiene ella y cuáles no.

---

## 8. Mantenimiento

**Semanal.** Borre `trabajo/enviados`. Sólo tiene valor forense.

**Mensual.** Purgue `log_sync_paqs` de los `enviado` con más de un mes.

**Al mover el punto de servidor.** Copie `sync.json`, la carpeta
`trabajo` completa y la base local. Si deja atrás la carpeta `trabajo`,
pierde los paquetes que no habían subido.

**Al rotar el token.** Actualícelo en la central y en `sync.json`. Entre
una cosa y la otra el punto no sincroniza, pero sigue vendiendo.

---

## 9. Lo que el punto no puede hacer

Por diseño, y conviene decirlo antes de que alguien lo intente:

- **Dar de alta productos o clientes.** Se hacen en la central.
- **Facturar.** Se factura en la central o en el kiosco, unos minutos
  después de la venta.
- **Cancelar.** Se cancela en la central.
- **Comprar o mover inventario.** Se hace en la central.
- **Ver costos o utilidad.** El punto no valúa; la valuación vive en la
  central.

Si un negocio necesita alguna de estas cosas en el punto, no es
candidato para este esquema.

---

## 10. Resumen para pegar en la pared

```
Todo bien             trabajo/salida vacía
                      log_sync_paqs sin 'error' ni 'detenido'
                      ignorados: 0 en los obtener-*

Enlace caído          archivos en salida que suben al volver. No hacer nada.

Requiere atención     algo en trabajo/detenidos
                      ignorados creciendo
                      salida que no baja entre corridas

Falta un producto     obtener-cat-estructura, luego obtener-exis

Precio viejo          obtener-precios
```
