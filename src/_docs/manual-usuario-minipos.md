# Manual del punto de venta

---

## 1. Qué es esto

Un punto de venta para tablet o teléfono Android que **funciona sin internet**. Todo lo que necesitas para vender —el catálogo, las ventas, los cortes— vive en el aparato. Si se cae el internet, el negocio sigue vendiendo.

### Lo que hace

- Vender y cobrar en efectivo, tarjeta o vales
- Imprimir ticket
- Llevar el corte de caja con arqueo del efectivo
- Registrar entradas y salidas de dinero del cajón
- Manejar un catálogo de productos con fotos
- Sincronizarse con MaxiComercio o Déminus, si el negocio los tiene

### Lo que no hace

- **No factura.** No emite CFDI ni se conecta con el SAT. Solo imprime tickets.
- **No maneja crédito.** Todas las ventas se cobran en el momento.
- **No lleva inventario estricto.** Las existencias son una referencia, no un control.
- **No es multisucursal.** Cada terminal es independiente.

Si el negocio necesita facturar, llevar inventario de verdad o manejar varias sucursales, la ruta es MaxiComercio o Déminus. Este punto de venta puede seguir trabajando como terminal conectada a ellos.

### Sobre el internet

Solo dos cosas necesitan conexión, y las dos son de una sola vez:

- **Activar la licencia**, al instalar
- **Recuperar la contraseña de administración**, si se pierde

Vender, cobrar, imprimir y cortar funcionan sin conexión.

---

## 2. Antes de empezar a vender

Si compraste la tablet ya preparada, este paso ya está hecho y puedes saltar a la sección 3.

### Activar

Al abrir por primera vez, la aplicación pide la clave de licencia y los datos del negocio: nombre, correo y teléfono. Necesitas internet en este momento. Cada clave sirve **para un solo aparato**: si la tablet se pierde o se descompone, hay que comprar una clave nueva.

Si todavía no tienes clave, el botón **Comprar clave** te lleva a la página donde adquirirla.

### Dar de alta a los cajeros

En la pantalla de acceso, toca **Administrar terminal** y escribe la contraseña de administración. La de fábrica es `1234` y **hay que cambiarla**.

En Cajeros, toca **Agregar** y captura:

- **Número:** un identificador corto, por ejemplo `01`
- **Nombre:** como aparecerá en el ticket y en el corte
- **Contraseña:** la que usará para entrar

Debe haber al menos un cajero para poder vender.

### Cargar los productos

En **Productos** puedes capturarlos uno por uno con **Agregar**, o cargarlos todos de golpe con **Importar** desde un archivo de Excel. Ve la sección 7 para eso.

### Configurar el negocio

En **Configuración** captura el nombre, la dirección y el teléfono del negocio. Es lo que sale impreso en el encabezado del ticket. Ahí mismo elige el ancho de la impresora y cambia la contraseña de administración.

---

## 3. El día a día

### Abrir el turno

1. Elige tu nombre de la lista
2. Escribe tu contraseña
3. Captura el **fondo de apertura**: cuánto efectivo hay en el cajón para empezar

El fondo es importante. Sin él, al final del día el corte no puede decirte cuánto dinero debería haber.

**Cada cajero usa su propia tablet.** Mientras tu turno esté abierto, nadie más puede entrar en ese aparato. Si intentan, la aplicación avisa de quién es el turno.

### Vender

Desde la pantalla principal, toca **Venta rápida**. Cada venta empieza aquí.

**Para agregar productos** tienes dos formas:

- **Escanear el código de barras.** El lector escribe el código y agrega el producto solo.
- **Buscar.** En el campo de abajo escribe parte del código o de la descripción. Si solo queda un producto, se agrega al presionar Enter; si quedan varios, tócalo en la pantalla.

También puedes tocar directamente cualquier producto de la retícula.

**Productos que se venden por peso.** Si el producto está marcado para admitir decimales, al elegirlo pregunta la cantidad. Ahí escribes lo que marcó la báscula, por ejemplo `1.375`.

**Para corregir la cantidad** de algo que ya está en la lista:

- Los botones **−** y **+** suman o restan de uno en uno
- **Tocar el número** te deja escribir la cantidad exacta
- **Quitar** elimina el renglón completo

**En pantalla de teléfono**, la lista de lo que llevas se abre con el botón de abajo que muestra cuántos artículos hay. Para volver a los productos, toca **← Productos** arriba. El total siempre se ve, aunque la lista esté cerrada.

**El cliente** es "Público en general" salvo que el negocio tenga la lista sincronizada desde MaxiComercio o Déminus. En ese caso puedes elegir otro con **Cambiar**.

### Cobrar

Toca **Cobrar** y captura los montos. Verás cuatro números:

| | |
|---|---|
| **Total** | Lo que se debe |
| **Pagado** | Lo que llevas capturado |
| **Falta** | Lo que aún no se cubre |
| **Cambio** | Lo que hay que devolver |

Puedes combinar formas de pago: parte en efectivo y parte con tarjeta, por ejemplo. **El cambio se calcula solo sobre el efectivo**, así que en tarjeta y vales captura el monto exacto.

Los botones de importes rápidos debajo del efectivo son atajos para los billetes más probables.

El botón de cobrar se habilita cuando ya no falta nada. Al tocarlo se registra la venta y se imprime el ticket.

### Meter o sacar dinero del cajón

Cuando el dueño saca dinero, se paga a un proveedor o se mete cambio, hay que registrarlo con **Ingreso** o **Egreso** desde la pantalla principal. Si no se registra, el corte no va a cuadrar y va a parecer que falta dinero.

Escribe el monto y el concepto. Hay conceptos sugeridos para no tener que teclear.

### Cancelar una venta

Desde **Ventas del turno**, toca la venta y luego **Cancelar venta**.

- Solo se puede cancelar **la venta completa**, no un renglón
- Solo dentro del turno; una vez cerrado el corte ya no se puede
- La venta cancelada no se borra: queda marcada y visible en el corte

### Cerrar el turno

Al final del día, **Corte de caja**:

1. Revisa el resumen de ventas y el efectivo esperado
2. **Cuenta el dinero del cajón** y escribe el total
3. La aplicación te dice si sobra o falta
4. Toca **Cerrar turno e imprimir**

El corte imprime el arqueo, la lista de tickets del día y el resumen de productos vendidos. Después de cerrar, la sesión termina y la aplicación vuelve a la pantalla de acceso.

**El efectivo esperado se calcula así:**

```
fondo de apertura
+ ventas en efectivo
+ ingresos
− egresos
= lo que debería haber en el cajón
```

Tarjeta y vales se reportan aparte porque no afectan el cajón.

---

## 4. Consultar y reimprimir

### Durante el turno

**Ventas del turno** muestra los tickets del día, los productos vendidos acumulados y los movimientos de efectivo. Desde ahí puedes reimprimir un ticket o cancelar una venta.

El botón **Imprimir el turno** saca un reporte de cómo va el día. **No cierra el turno**, solo informa.

### Turnos anteriores

**Cortes anteriores** guarda todos los turnos cerrados en esa tablet. Pide la contraseña de administración, porque un cajero no tiene por qué ver los turnos de otro.

La lista dice de un vistazo si el turno cuadró o cuánto sobró o faltó. Al abrir uno ves el arqueo completo, los tickets y los productos, y puedes reimprimirlo.

**Las reimpresiones salen marcadas como COPIA**, para que no se confundan con el original.

---

## 5. Las contraseñas

Hay dos tipos y conviene entender la diferencia.

**La contraseña del cajero** solo sirve para entrar a vender.

**La contraseña de administración** protege lo delicado: la configuración, los catálogos, los cortes anteriores y el cierre de turnos ajenos. Es, en la práctica, la llave del dinero. Solo debería tenerla el dueño.

### Si un cajero olvida la suya

En la pantalla de acceso, **Administrar terminal** → contraseña de administración → **Cajeros** → toca al cajero → escribe una contraseña nueva.

Si dejas el campo de contraseña vacío, se conserva la que tenía. Eso sirve para cambiarle el nombre sin tocar su acceso.

### Si el dueño olvida la de administración

En el cuadro que la pide, toca **Olvidé la contraseña**. Se entra con la cuenta de Induxsoft con la que se compró la licencia y ahí se establece una nueva.

**Este paso necesita internet.** Es la única forma de recuperarla; guárdala bien.

### Si un cajero se fue sin cortar

Puede pasar que alguien termine su turno y se vaya sin cerrarlo. Otro cajero no puede entrar mientras eso siga así.

La salida aparece cuando el otro cajero intenta entrar y no puede: se ofrece **cerrar el turno** con la contraseña de administración. El corte queda a nombre del cajero que lo abrió, porque el faltante es suyo, pero se registra que lo cerró un administrador.

Ten en cuenta que el arqueo en ese caso es menos confiable: el dinero pudo moverse desde que esa persona se fue.

---

## 6. Productos

### Agregar o editar

En **Productos**, toca **Agregar** o toca uno existente.

| Campo | Para qué sirve |
|---|---|
| **Código** | El código de barras o una clave que tú inventes |
| **Descripción** | Como aparece en pantalla y en el ticket |
| **Unidad** | PZA, KG, LT, lo que aplique |
| **Precio** | El precio de venta, ya con impuesto incluido |
| **Existencia** | Cuántos hay. Es una referencia |
| **No contar existencias** | Para servicios y cosas que no se inventarían |
| **Admitir decimales** | Para lo que se vende por peso o medida |

### Sobre las existencias

**La existencia nunca impide vender.** Si llega a cero se marca en rojo, pero la venta procede. Esto es a propósito: es preferible vender y ajustar después que detener la caja con un cliente enfrente.

El descuento se aplica **al cerrar el turno**, no en cada venta.

Si marcas **No contar existencias**, el producto no muestra número en la esquina y nunca se descuenta. Úsalo para recargas de tiempo aire, servicios, o cualquier cosa que no tenga sentido inventariar.

### Admitir decimales

Marca esta opción para lo que se vende por peso o medida. Al elegir el producto, la aplicación pregunta la cantidad en vez de agregar una pieza.

### Fotos

Puedes tomarla con la cámara o elegir una imagen del aparato. La foto se reduce automáticamente para no llenar la memoria de la tablet.

Los productos sin foto se muestran con un recuadro de color y las iniciales de su descripción, que igual sirve para distinguirlos rápido.

---

## 7. Cargar productos desde Excel

Con **Importar** puedes subir muchos productos de una vez desde un archivo CSV. El enlace **Descargar ejemplo de CSV** te da un archivo listo para llenar.

### Las columnas

| Columna | Obligatoria | Qué acepta |
|---|---|---|
| `sku` | **Sí** | El código del producto |
| `descripcion` | No | Texto |
| `precio` | No | `22.50`, `$1,234.50` |
| `unidad` | No | PZA, KG, LT |
| `decimales` | No | `si` / `no` |
| `existencia` | No | Números, admite decimales |
| `no contar existencias` | No | `si` / `no` |
| `imagen` | No | Una dirección de internet |

### Las reglas importantes

**Solo el código es obligatorio.** Todas las demás columnas se pueden omitir.

**Lo que no venga en el archivo no se modifica.** Esto es lo que hace útil la importación, porque te deja usarla para cosas distintas:

- Un archivo con `sku` y `precio` actualiza solo precios, sin tocar existencias ni descripciones
- Un archivo con `sku` y `existencia` sirve para un reconteo, sin tocar precios
- Un archivo completo sirve para la carga inicial

Una celda vacía cuenta igual que una columna ausente: no borra nada.

**Los productos que no aparezcan en el archivo se quedan como están.** Importar nunca borra el catálogo.

**Antes de aplicar se te muestra un resumen** con cuántos son nuevos, cuántos se actualizan y qué columnas se reconocieron. Revísalo: si una columna no aparece en esa lista, es que el encabezado no se entendió y esos datos no se van a aplicar.

### Consejos prácticos

- Guarda desde Excel como **CSV**. La aplicación entiende tanto coma como punto y coma, y también los acentos aunque Excel los guarde a su manera.
- Puedes escribir los encabezados con acentos, mayúsculas o en cualquier orden: reconoce `Código`, `codigo`, `SKU`, `Descripción`, `Stock` y varias más.
- Si tu archivo **no trae encabezado**, las columnas deben ir en el orden de la tabla de arriba.
- Las imágenes se descargan después de importar y pueden fallar si el sitio no lo permite. Al terminar te dice cuántas se lograron.

---

## 8. Configuración

Pide la contraseña de administración.

**Negocio.** Nombre, dirección y teléfono para el encabezado del ticket.

**Terminal.** El identificador de la caja y el ancho de la impresora. El ancho debe coincidir con tu impresora o los tickets van a salir cortados o desalineados.

**Controlador de impresión.** Determina cómo se imprime:

- *Vista previa en pantalla* muestra el ticket sin imprimirlo. Útil para probar y capacitar sin impresora.
- *Impresión del navegador* manda al sistema de impresión del aparato.
- Puede haber otros instalados por tu proveedor.

Al elegir un controlador que hable con una impresora directamente, aparecen sus parámetros: dirección, puerto y demás. **Imprimir página de prueba** saca una hoja con una regla numérica para verificar de un vistazo que el ancho esté bien.

**Color.** Cambia el color de la aplicación. Puedes elegir uno de los sugeridos o el que quieras.

**Formas de pago.** Activa o desactiva tarjeta y vales. El efectivo siempre está disponible. Si tus vales dan cambio, actívalo aquí.

**Sincronización.** Solo si el negocio tiene MaxiComercio o Déminus. Ve la sección 10.

**Contraseña de administración.** Cámbiala apenas instales. La de fábrica es conocida.

---

## 9. Respaldo y limpieza

### Respaldar

**Descargar respaldo** genera un archivo con los catálogos, las ventas y los cortes. Guárdalo fuera de la tablet: en el correo, en una memoria, en donde sea.

Vale la pena hacerlo de vez en cuando. Si la tablet se pierde o se rompe, todo lo que tenía se va con ella.

La licencia **no** se incluye en el respaldo, así que restaurarlo en otra tablet no la activa. Esa sigue necesitando su propia clave.

### Restaurar

**Restaurar** reemplaza todo lo que hay en la tablet con el contenido del archivo. No mezcla ni combina: lo que estaba se pierde. Por eso pide confirmación y conviene respaldar antes.

### Limpiar el histórico

Con el tiempo la tablet acumula ventas y cortes. Si notas que va lenta, o simplemente quieres empezar un ejercicio limpio, elige una fecha y usa **Eliminar ventas y cortes**.

- Se borra todo lo anterior a esa fecha, de forma definitiva
- **Nunca toca el turno abierto**
- Si hay documentos que aún no se han subido a MaxiComercio o Déminus, te avisa antes de borrarlos

**Descarga un respaldo antes de limpiar.** Lo borrado no se recupera.

Nota: los folios de los tickets continúan desde el último corte que quede. Si borras absolutamente todo, la numeración vuelve a empezar.

---

## 10. Trabajar junto con MaxiComercio o Déminus

Si el negocio ya tiene MaxiComercio o Déminus, esta terminal puede trabajar conectada a ellos. Quien te lo instaló captura la dirección del servidor y el token en Configuración.

### Qué pasa al sincronizar

**Suben** al servidor todas las ventas, movimientos y cortes que aún no se hayan enviado, aunque sean de días anteriores.

**Bajan** los catálogos de productos, clientes y cajeros.

La sincronización ocurre automáticamente al cerrar cada corte, y puedes lanzarla a mano desde **Sincronizar** en la pantalla principal.

### Lo que hay que saber

**Los catálogos del servidor reemplazan a los de la tablet.** Todo lo que hayas capturado localmente se pierde en la siguiente sincronización. Cuando la terminal está conectada, el catálogo bueno es el del servidor.

**Si no hay internet, no pasa nada.** Las ventas se quedan guardadas y suben en el siguiente intento. Nunca se pierden.

**Si aparece "Terminal desactivada"**, es que dieron de baja esta tablet en el servidor. Hay que pedir que la reactiven.

---

## 11. Problemas comunes

**"El corte no cuadra y falta dinero."**
Casi siempre es un egreso que no se registró: se pagó al repartidor, se sacó cambio, se compró algo. Revisa los movimientos en Ventas del turno. Registrar cada salida de dinero en el momento es lo que hace que el corte sirva.

**"No encuentro un producto al buscar."**
Prueba con menos letras. La búsqueda es por código o por descripción, así que si está capturado con otro nombre no va a aparecer. Revísalo en el catálogo.

**"El ticket sale cortado o con las columnas encimadas."**
El ancho de impresión no coincide con la impresora. Cámbialo en Configuración y usa la página de prueba para verificar.

**"No puedo entrar, dice que hay un turno abierto de otra persona."**
Esa persona debe cerrar su turno. Si no está, ve la sección 5.

**"El producto dice cero pero sí hay."**
Las existencias son una referencia y solo se ajustan al cerrar el turno. Puedes venderlo de todos modos, y corregir el número en el catálogo cuando tengas tiempo.

**"Se me olvidó capturar el fondo de apertura."**
Se pide al iniciar el turno y no se puede cambiar después. Si pusiste mal el monto, regístralo como ingreso o egreso con un concepto que lo explique.

**"Perdí la tablet."**
Las ventas que no se hubieran sincronizado se perdieron. Si tenías respaldo, puedes restaurarlo en otra tablet, pero esa necesita su propia clave de licencia. Por eso conviene respaldar seguido.

---

## 12. Recomendaciones

- **Cambia la contraseña de administración** el primer día
- **Registra cada ingreso y egreso** en el momento; es lo que hace confiable el corte
- **Cierra el turno todos los días**, aunque haya sido flojo
- **Respalda una vez por semana** y guarda el archivo fuera de la tablet
- **Una tablet por cajero.** Si dos personas comparten aparato, el corte deja de decir de quién es el faltante
- **Cuenta el dinero antes de mirar el número esperado.** Si primero ves cuánto debería haber, dejas de contar y empiezas a confirmar
