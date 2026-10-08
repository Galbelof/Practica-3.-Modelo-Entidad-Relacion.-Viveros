# Modelo entidad/relación: Viveros

**Asignatura:** Administración y diseño de bases de datos, curso 2026-2027.  
**Escenario:** Tajinaste S.A.

## Modelo

![Modelo entidad/relación de Tajinaste S.A.](modelo-viveros.png)

El archivo [modelo-viveros.drawio](modelo-viveros.drawio) contiene el diagrama editable en Draw.io. El modelo utiliza notación de Chen: rectángulos para entidades, rombos para relaciones y óvalos para atributos. Las claves están subrayadas y los atributos derivados tienen un contorno discontinuo.

La pareja `(mínimo, máximo)` junto a una entidad indica cuántas veces puede participar **cada instancia de esa entidad** en la relación. `N` representa un número arbitrario de participaciones; un mínimo de `0` indica participación opcional y un mínimo de `1`, obligatoria.

## 1. Entidades

| Entidad | Descripción | Identificador |
| --- | --- | --- |
| **VIVERO** | Establecimiento de Tajinaste S.A. cuya ubicación geográfica se conoce y que contiene zonas. | `id_vivero` |
| **ZONA** | Área de un único vivero, como la zona exterior o el almacén. Tiene georreferenciación propia y puede albergar productos y destinos de empleados. | `id_zona`, único en toda la empresa |
| **PRODUCTO** | Artículo del catálogo: planta, producto de jardinería o artículo de decoración. Su disponibilidad se registra por zona. | `id_producto` |
| **EMPLEADO** | Trabajador cuyo historial de destinos y pedidos gestionados se desea conservar. | `id_empleado` |
| **DESTINO** | Periodo durante el cual un empleado trabaja en una zona y desempeña un puesto o tarea. Cada traslado o cambio de puesto genera otro destino, conservando el anterior. | `id_destino` |
| **CLIENTE_PLUS** | Cliente perteneciente a Tajinaste Plus. Se registra su fecha de ingreso y un correo para las campañas de fidelización. | `id_cliente` |
| **PEDIDO** | Compra realizada por un cliente Tajinaste Plus desde su ingreso en el programa, gestionada por un único empleado. | `id_pedido` |
| **BONIFICACION_MENSUAL** | Bonificación de un cliente para un mes concreto, calculada a partir del volumen de compras de ese mes. | `id_bonificacion`; además, el par cliente-mes es único |

## 2. Atributos de las entidades: descripción, dominio y ejemplos

Todos los identificadores tienen como dominio los enteros positivos y son únicos dentro de su entidad. Los atributos son obligatorios salvo `DESTINO.fecha_fin`. Las coordenadas se expresan en grados decimales del sistema WGS84.

Las fechas y horas representan instantes y se muestran con su desplazamiento horario en formato ISO 8601. Para delimitar los meses se utiliza una referencia común: la zona horaria `Atlantic/Canary`.

### VIVERO

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_vivero` | Identificador del vivero. | Entero positivo. | `1` |
| `nombre` | Nombre del establecimiento. | Texto de entre 1 y 100 caracteres. | `Vivero La Laguna` |
| `latitud` | Coordenada norte-sur del vivero. | Número real entre -90 y 90, ambos incluidos. | `28.4874` |
| `longitud` | Coordenada este-oeste del vivero. | Número real entre -180 y 180, ambos incluidos. | `-16.3159` |

### ZONA

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_zona` | Identificador global de la zona. | Entero positivo. | `101` |
| `nombre` | Nombre del área dentro del vivero. | Texto de entre 1 y 100 caracteres. | `Zona exterior` |
| `latitud` | Coordenada norte-sur de la zona. | Número real entre -90 y 90, ambos incluidos. | `28.4876` |
| `longitud` | Coordenada este-oeste de la zona. | Número real entre -180 y 180, ambos incluidos. | `-16.3157` |

### PRODUCTO

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_producto` | Identificador del producto. | Entero positivo. | `501` |
| `nombre` | Denominación del artículo. | Texto de entre 1 y 150 caracteres. | `Geranio rojo` |
| `tipo` | Clase de artículo comercializado. | Uno de los valores `planta`, `jardinería` o `decoración`. | `planta` |

### EMPLEADO

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_empleado` | Identificador del trabajador. | Entero positivo. | `12` |
| `nombre` | Nombre y apellidos del empleado. | Texto de entre 1 y 150 caracteres. | `Ana Pérez Díaz` |

### DESTINO

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_destino` | Identificador del periodo de trabajo. | Entero positivo. | `1001` |
| `fecha_inicio` | Instante de comienzo del destino. | Fecha y hora válidas. | `2026-09-01T09:00:00+01:00` |
| `fecha_fin` | Instante en que termina el destino. | Fecha y hora posteriores a `fecha_inicio`, o ausencia de valor si el destino no tiene final establecido. | `2026-10-01T09:00:00+01:00`, o sin valor |
| `puesto` | Puesto o tarea desempeñada durante el periodo. | Texto de entre 1 y 100 caracteres. | `Vendedor` |

### CLIENTE_PLUS

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_cliente` | Identificador del cliente adscrito al programa. | Entero positivo. | `300` |
| `nombre` | Nombre y apellidos del cliente. | Texto de entre 1 y 150 caracteres. | `Luis García Ramos` |
| `fecha_ingreso` | Instante de incorporación a Tajinaste Plus. | Fecha y hora válidas. | `2026-09-01T08:00:00+01:00` |
| `email` | Dirección de contacto para las campañas. | Texto de entre 3 y 254 caracteres con formato de dirección de correo electrónico. | `luis@example.com` |

### PEDIDO

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_pedido` | Identificador del pedido. | Entero positivo. | `2001` |
| `fecha_hora` | Instante en que se registra la compra y su responsable. | Fecha y hora válidas, iguales o posteriores al ingreso del cliente en Tajinaste Plus. | `2026-09-15T10:30:00+01:00` |
| `importe_total` | Total del pedido; atributo derivado de sus productos, cantidades y precios de venta. | Número decimal no negativo expresado en euros, con dos decimales. | `17.00` para dos unidades a `8.50` euros |

### BONIFICACION_MENSUAL

Se propone expresar la bonificación como un importe monetario obtenido al aplicar un porcentaje al volumen de compras. El porcentaje depende de la política comercial de la empresa. El enunciado no establece tramos ni porcentajes concretos; los valores siguientes son ejemplos del dominio, no tarifas de Tajinaste S.A.

| Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- |
| `id_bonificacion` | Identificador de la bonificación mensual. | Entero positivo. | `9001` |
| `mes` | Año y mes al que corresponde la bonificación. | Periodo mensual válido, expresado como `AAAA-MM`. | `2026-09` |
| `volumen_mensual` | Suma de los importes de los pedidos del cliente en ese mes; atributo derivado. | Número decimal no negativo expresado en euros, con dos decimales. | `200.00` |
| `porcentaje` | Porcentaje aplicable según el volumen mensual de compras. | Número decimal entre 0 y 100, ambos incluidos, con hasta dos decimales. | `5.00` |
| `importe` | Bonificación monetaria; atributo derivado del volumen mensual y del porcentaje. | Número decimal no negativo expresado en euros, con dos decimales. | `10.00` al aplicar el 5 % a `200.00` euros |

## 3. Relaciones y cardinalidades

### CONTIENE: VIVERO - ZONA

Un vivero contiene una o varias zonas. Cada zona pertenece a un solo vivero.

- **VIVERO `(1,N)`:** se considera que todo vivero registrado tiene al menos una zona y puede tener muchas.
- **ZONA `(1,1)`:** toda zona debe pertenecer exactamente a un vivero.
- **Cardinalidad global:** uno a muchos (`1:N`).
- **Atributos de la relación:** ninguno.

Ejemplo: el vivero `1` contiene las zonas `101` (exterior) y `102` (almacén). La zona `101` pertenece únicamente al vivero `1`.

### ALMACENA: ZONA - PRODUCTO

Registra la asignación de un producto a una zona y la cantidad disponible en esa ubicación.

- **ZONA `(0,N)`:** una zona puede estar vacía o tener varios productos asignados.
- **PRODUCTO `(0,N)`:** un producto del catálogo puede no estar asignado todavía o estar asignado a varias zonas, incluso de distintos viveros.
- **Cardinalidad global:** muchos a muchos (`N:M`).
- **Atributo de la relación:** `cantidad_disponible`.

Ejemplo: el producto `501` tiene 37 unidades disponibles en la zona `101` y 12 en la zona `102`. Una cantidad de `0` mantiene la asignación a la zona, aunque el producto esté agotado.

### TIENE_DESTINO: EMPLEADO - DESTINO

Relaciona a cada empleado con sus periodos históricos de trabajo.

- **EMPLEADO `(0,N)`:** un trabajador puede estar registrado antes de su primera asignación y acumular varios destinos a lo largo del tiempo.
- **DESTINO `(1,1)`:** cada destino corresponde obligatoriamente a un único empleado.
- **Cardinalidad global:** uno a muchos (`1:N`).
- **Atributos de la relación:** ninguno; las fechas y el puesto pertenecen a la entidad DESTINO.

Ejemplo: el empleado `12` tiene un destino en septiembre y otro a partir de octubre. Tener varios destinos históricos no implica tener varios destinos simultáneos.

### UBICA: ZONA - DESTINO

Asocia cada periodo de trabajo con la zona donde se desempeña el puesto. El vivero del destino se obtiene a través de CONTIENE.

- **ZONA `(0,N)`:** una zona puede no tener destinos registrados o tener muchos, correspondientes a uno o varios empleados y a distintos periodos.
- **DESTINO `(1,1)`:** un destino se desarrolla exactamente en una zona.
- **Cardinalidad global:** uno a muchos (`1:N`).
- **Atributos de la relación:** ninguno.

Ejemplo: el destino `1001` del empleado `12` se ubica en la zona `101`. Otro empleado puede trabajar en esa misma zona durante el mismo periodo.

### GESTIONA: EMPLEADO - PEDIDO

Identifica al empleado responsable de cada pedido de un cliente Tajinaste Plus.

- **EMPLEADO `(0,N)`:** un empleado puede no haber gestionado pedidos o ser responsable de muchos.
- **PEDIDO `(1,1)`:** todo pedido tiene exactamente un empleado responsable.
- **Cardinalidad global:** uno a muchos (`1:N`).
- **Atributos de la relación:** ninguno.

Ejemplo: el empleado `12` gestiona los pedidos `2001` y `2002`. Cada uno conserva un solo responsable.

### REALIZA: CLIENTE_PLUS - PEDIDO

Relaciona al cliente del programa con sus compras desde su ingreso.

- **CLIENTE_PLUS `(0,N)`:** un cliente puede incorporarse sin haber comprado todavía y posteriormente realizar muchos pedidos.
- **PEDIDO `(1,1)`:** cada pedido pertenece exactamente a un cliente Tajinaste Plus.
- **Cardinalidad global:** uno a muchos (`1:N`).
- **Atributos de la relación:** ninguno.

Ejemplo: el cliente `300` realiza el pedido `2001` el 15 de septiembre, después de ingresar en el programa el día 1.

### INCLUYE: PEDIDO - PRODUCTO

Describe los productos de cada pedido, la cantidad comprada y su precio unitario en esa venta.

- **PEDIDO `(1,N)`:** un pedido debe incluir al menos un producto y puede incluir varios.
- **PRODUCTO `(0,N)`:** un artículo puede no haberse vendido o aparecer en muchos pedidos.
- **Cardinalidad global:** muchos a muchos (`N:M`).
- **Atributos de la relación:** `cantidad` y `precio_unitario`.

Ejemplo: el pedido `2001` incluye dos unidades del producto `501` a `8.50` euros cada una; esa participación aporta `17.00` euros al total.

### RECIBE: CLIENTE_PLUS - BONIFICACION_MENSUAL

Asocia cada bonificación mensual con el cliente beneficiario.

- **CLIENTE_PLUS `(0,N)`:** un cliente puede no tener aún bonificaciones calculadas o acumular bonificaciones de varios meses.
- **BONIFICACION_MENSUAL `(1,1)`:** cada bonificación corresponde exactamente a un cliente.
- **Cardinalidad global:** uno a muchos (`1:N`).
- **Atributos de la relación:** ninguno; el mes, el porcentaje y los importes pertenecen a BONIFICACION_MENSUAL.

Ejemplo: el cliente `300` recibe una bonificación correspondiente a `2026-09` y otra a `2026-10`.

## 4. Atributos de las relaciones: descripción, dominio y ejemplos

| Relación | Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- | --- |
| ALMACENA | `cantidad_disponible` | Existencias disponibles de un producto en una zona concreta. | Entero mayor o igual que cero, expresado en unidades. | `37` |
| INCLUYE | `cantidad` | Unidades de un producto compradas en un pedido. | Entero positivo. | `2` |
| INCLUYE | `precio_unitario` | Precio por unidad aplicado en ese pedido. Conserva el valor de la venta para calcular importes históricos. | Decimal mayor o igual que cero, expresado en euros, con dos decimales. | `8.50` |

Se supone que los productos se controlan y venden por unidades. CONTIENE, TIENE_DESTINO, UBICA, GESTIONA, REALIZA y RECIBE no tienen atributos propios.

## 5. Restricciones semánticas

1. **Pertenencia y stock.** Cada zona pertenece a un único vivero. Para un mismo par zona-producto existe una sola participación en ALMACENA, con una cantidad no negativa. La relación registra tanto la asignación del producto como sus existencias actuales.

2. **Histórico de destinos sin solapamientos.** Un destino ocupa el intervalo `[fecha_inicio, fecha_fin)`: incluye el comienzo y excluye el final. Si no tiene fecha final, el intervalo se considera abierto hacia el futuro. Cuando existe, `fecha_fin` debe ser posterior a `fecha_inicio`. Los intervalos de un mismo empleado no pueden solaparse, aunque correspondan a zonas o viveros diferentes. Así, en cada instante el empleado tiene como máximo un destino. Dos destinos consecutivos pueden terminar y comenzar en el mismo instante.

   Por ejemplo, el empleado `12` puede estar en la zona `101` desde `2026-09-01T09:00:00+01:00` hasta `2026-10-01T09:00:00+01:00`, y comenzar en la zona `201` de otro vivero exactamente a esa última hora. Para conservar el histórico, se cierra el destino anterior y se crea uno nuevo cuando cambian la zona o el puesto.

3. **Responsable y ubicación histórica de los pedidos.** Cada pedido tiene un solo responsable. Este empleado debe tener exactamente un destino válido en `PEDIDO.fecha_hora`. Para atribuir la venta a una zona se selecciona el destino de ese responsable cuyo intervalo contiene el instante del pedido y se sigue UBICA. CONTIENE determina entonces el vivero. Los destinos históricos necesarios para interpretar pedidos deben conservarse.

4. **Pedidos desde el ingreso en Tajinaste Plus.** La fecha y hora de cada pedido deben ser iguales o posteriores a `CLIENTE_PLUS.fecha_ingreso`. Cada pedido corresponde a un único miembro del programa y contiene al menos un producto.

5. **Contenido e importe del pedido.** Para un mismo par pedido-producto existe una sola participación en INCLUYE; si se compran varias unidades del artículo, se acumulan en `cantidad`. Esta cantidad es positiva y el precio unitario no puede ser negativo. El precio registrado en la venta se conserva para mantener los importes históricos. El atributo derivado se calcula como:

   `importe_total = suma(cantidad × precio_unitario)`

   La suma recorre todas las participaciones en INCLUYE del pedido.

6. **Bonificación por cliente y mes.** No puede haber dos bonificaciones para el mismo cliente y el mismo mes. El mes de la bonificación debe ser igual o posterior al mes de ingreso del cliente. Si el cliente se incorpora a mitad de mes, solo se consideran sus pedidos a partir del instante de ingreso. El atributo derivado se calcula como:

   `volumen_mensual = suma(importe_total de los pedidos del cliente en el mes)`

   Si no existen pedidos en ese mes, el volumen es `0.00`. El porcentaje se determina mediante la política comercial aplicable al volumen mensual. El importe derivado es:

   `importe = redondear(volumen_mensual × porcentaje / 100, 2)`

   Se utiliza redondeo al céntimo, elevando el resultado cuando el siguiente decimal es 5 o superior. Las bonificaciones se calculan al cerrar cada mes, utilizando sus compras completas. Los atributos derivados del diagrama se obtienen de estos cálculos.

7. **Productividad a lo largo del tiempo.** Se propone medir la actividad comercial mediante el número de pedidos y la suma de sus importes en un periodo. Para cada empleado se utilizan los pedidos de GESTIONA. Para cada zona se utiliza la ubicación histórica del responsable en el instante de cada pedido, según la restricción 3. Por tanto, un traslado posterior no atribuye las ventas antiguas a la nueva zona. El mismo criterio permite obtener resultados por vivero y comparar periodos.

   Por ejemplo, si el empleado `12` gestiona un pedido de `17.00` euros el 15 de septiembre mientras su destino es la zona `101`, esa venta aporta un pedido y `17.00` euros a la productividad de dicho empleado y de la zona `101`, incluso después de su traslado en octubre.
