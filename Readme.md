# Modelo entidad/relación: Viveros

## 1. Entidades

Los identificadores son atributos clave: enteros positivos y únicos dentro de cada entidad.

| Entidad | Descripción | Identificador | Ejemplo |
| --- | --- | --- | --- |
| VIVERO | Establecimiento georreferenciado de Tajinaste S.A. que contiene zonas. | `id_vivero` | `1` |
| ZONA | Área georreferenciada de un único vivero donde se ubican productos y empleados. | `id_zona` | `101` |
| PRODUCTO | Planta, producto de jardinería o artículo de decoración del catálogo. | `id_producto` | `501` |
| EMPLEADO | Trabajador cuyos destinos y pedidos gestionados se registran. | `id_empleado` | `12` |
| DESTINO | Periodo de trabajo de un empleado en una zona y un puesto. | `id_destino` | `1001` |
| CLIENTE_PLUS | Cliente inscrito en Tajinaste Plus. | `id_cliente` | `300` |
| PEDIDO | Compra de un cliente Plus gestionada por un único empleado. | `id_pedido` | `2001` |
| BONIFICACION_MENSUAL | Bonificación de un cliente correspondiente a un mes. | `id_bonificacion` | `9001` |

## 2. Atributos: descripción, dominio y ejemplos

Además de los identificadores anteriores, las entidades tienen estos atributos. Todos son obligatorios salvo `fecha_fin`.

| Entidad | Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- | --- |
| VIVERO | `nombre` | Nombre del establecimiento. | Texto de 1 a 100 caracteres. | `Vivero La Laguna` |
| VIVERO | `latitud` | Coordenada norte-sur en grados decimales. | Real entre -90 y 90. | `28.4874` |
| VIVERO | `longitud` | Coordenada este-oeste en grados decimales. | Real entre -180 y 180. | `-16.3159` |
| ZONA | `nombre` | Nombre del área. | Texto de 1 a 100 caracteres. | `Zona exterior` |
| ZONA | `latitud` | Coordenada norte-sur en grados decimales. | Real entre -90 y 90. | `28.4876` |
| ZONA | `longitud` | Coordenada este-oeste en grados decimales. | Real entre -180 y 180. | `-16.3157` |
| PRODUCTO | `nombre` | Denominación del artículo. | Texto de 1 a 150 caracteres. | `Geranio rojo` |
| PRODUCTO | `tipo` | Clase de producto. | `planta`, `jardinería` o `decoración`. | `planta` |
| EMPLEADO | `nombre` | Nombre y apellidos. | Texto de 1 a 150 caracteres. | `Ana Pérez Díaz` |
| DESTINO | `fecha_inicio` | Comienzo del destino. | Fecha y hora válidas. | `2026-09-01T09:00:00+01:00` |
| DESTINO | `fecha_fin` | Final del destino. | Fecha y hora posteriores al inicio, o sin valor. | `2026-10-01T09:00:00+01:00` |
| DESTINO | `puesto` | Puesto o tarea desempeñada. | Texto de 1 a 100 caracteres. | `Vendedor` |
| CLIENTE_PLUS | `nombre` | Nombre y apellidos. | Texto de 1 a 150 caracteres. | `Luis García Ramos` |
| CLIENTE_PLUS | `fecha_ingreso` | Incorporación al programa. | Fecha y hora válidas. | `2026-09-01T08:00:00+01:00` |
| CLIENTE_PLUS | `email` | Correo para las campañas. | Dirección de correo válida, de 3 a 254 caracteres. | `luis@example.com` |
| PEDIDO | `fecha_hora` | Instante de la compra. | Fecha y hora válidas. | `2026-09-15T10:30:00+01:00` |
| PEDIDO | `importe_total` | Suma de `cantidad × precio_unitario` de sus productos; derivado. | Decimal no negativo en euros, con dos decimales. | `17.00` |
| BONIFICACION_MENSUAL | `mes` | Periodo de la bonificación. | Año y mes válidos, en formato `AAAA-MM`. | `2026-09` |
| BONIFICACION_MENSUAL | `volumen_mensual` | Suma de los importes de los pedidos del cliente en ese mes; derivado. | Decimal no negativo en euros, con dos decimales. | `200.00` |
| BONIFICACION_MENSUAL | `porcentaje` | Porcentaje según el volumen de compras. | Decimal entre 0 y 100, con hasta dos decimales. | `5.00` |
| BONIFICACION_MENSUAL | `importe` | `volumen_mensual × porcentaje / 100`, redondeado al céntimo; derivado. | Decimal no negativo en euros, con dos decimales. | `10.00` |

Las relaciones tienen estos atributos:

| Relación | Atributo | Descripción | Dominio | Ejemplo |
| --- | --- | --- | --- | --- |
| ALMACENA | `cantidad_disponible` | Existencias del producto en la zona. | Entero mayor o igual que 0, en unidades. | `37` |
| INCLUYE | `cantidad` | Unidades compradas del producto. | Entero positivo. | `2` |
| INCLUYE | `precio_unitario` | Precio por unidad aplicado en la venta. | Decimal no negativo en euros, con dos decimales. | `8.50` |

Las demás relaciones no tienen atributos propios.

## 3. Relaciones y cardinalidades

La pareja `(mínimo, máximo)` indica las participaciones de cada instancia de la entidad. `N` significa muchas; el mínimo `0` permite no participar.

| Relación | Participación de las entidades | Descripción | Cardinalidad |
| --- | --- | --- | --- |
| CONTIENE | VIVERO `(1,N)`; ZONA `(1,1)` | Un vivero contiene una o varias zonas; cada zona pertenece exactamente a un vivero. | `1:N` |
| ALMACENA | ZONA `(0,N)`; PRODUCTO `(0,N)` | Una zona puede tener ninguno o varios productos asignados; un producto puede estar en ninguna o varias zonas. | `N:M` |
| TIENE_DESTINO | EMPLEADO `(0,N)`; DESTINO `(1,1)` | Un empleado puede tener ninguno o varios destinos históricos; cada destino corresponde exactamente a un empleado. | `1:N` |
| UBICA | ZONA `(0,N)`; DESTINO `(1,1)` | Una zona puede albergar ninguno o varios destinos; cada destino se desarrolla exactamente en una zona. | `1:N` |
| GESTIONA | EMPLEADO `(0,N)`; PEDIDO `(1,1)` | Un empleado puede gestionar ninguno o varios pedidos; cada pedido tiene exactamente un responsable. | `1:N` |
| REALIZA | CLIENTE_PLUS `(0,N)`; PEDIDO `(1,1)` | Un cliente puede realizar ninguno o varios pedidos; cada pedido pertenece exactamente a un cliente. | `1:N` |
| INCLUYE | PEDIDO `(1,N)`; PRODUCTO `(0,N)` | Un pedido incluye uno o varios productos; un producto puede aparecer en ninguno o varios pedidos. | `N:M` |
| RECIBE | CLIENTE_PLUS `(0,N)`; BONIFICACION_MENSUAL `(1,1)` | Un cliente puede tener ninguna o varias bonificaciones mensuales; cada bonificación corresponde exactamente a un cliente. | `1:N` |

## 4. Restricciones semánticas

1. Los destinos de un mismo empleado no pueden solaparse. Sus intervalos son `[fecha_inicio, fecha_fin)`: incluyen el comienzo y excluyen el final. Sin `fecha_fin`, el intervalo permanece abierto. Al cambiar de zona o puesto se conserva el destino anterior y se crea otro.
2. La fecha de cada pedido debe ser igual o posterior al ingreso del cliente en Tajinaste Plus. Su responsable debe tener un único destino válido en el instante del pedido.
3. Existe una sola participación por cada par zona-producto en ALMACENA y pedido-producto en INCLUYE. Las unidades repetidas de un producto en un pedido se acumulan en `cantidad`. El precio unitario de la venta se conserva.
4. Solo puede existir una bonificación por cliente y mes, desde el mes de ingreso. Los meses se delimitan en la zona horaria `Atlantic/Canary`. Se propone una bonificación monetaria mediante un porcentaje según las compras mensuales; el enunciado no fija porcentajes ni tramos y los valores mostrados son ejemplos.
5. La productividad comercial se obtiene del número y el importe de los pedidos por empleado y periodo. Para atribuirla a una zona se utiliza el destino del responsable en la fecha del pedido, conservando así la ubicación histórica de la venta.

