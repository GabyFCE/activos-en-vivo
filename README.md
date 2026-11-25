# activos-en-vivo

Panel web de página única para consultar cotizaciones del día de dólares, criptomonedas, acciones argentinas y bonos soberanos sin registro ni recarga manual.

![Vista de la página](preview.png)

## Qué es y para qué sirve

Seguir el mercado local y cripto suele requerir mantener cinco pestañas abiertas en sitios distintos, cada una con publicidad, tiempos de carga dispares y recargas manuales.

Este proyecto reúne esas cotizaciones en una sola vista pensada como panel de escritorio o pantalla fija para un segundo monitor. Realiza las consultas desde el navegador cada 30 segundos, muestra una cuenta regresiva en el pie de página e informa la hora exacta de la última carga para verificar la vigencia del dato. También incluye un botón **Actualizar** para forzar la consulta en el momento sin esperar a que termine el ciclo.

## Qué incluye

El panel se divide en cuatro bloques independientes y consume cuatro endpoints públicos y gratuitos que no requieren credenciales:

| Bloque | Contenido | Fuente |
| --- | --- | --- |
| Dólares | Oficial, blue, bolsa, contado con liquidación, tarjeta, mayorista y cripto (compra y venta) | `dolarapi.com/v1/dolares` |
| Cripto | 6 activos en dólares con variación: BTC, ETH, SOL, BNB, USDT y XRP | `api.coingecko.com/api/v3/simple/price` |
| Acciones argentinas | 12 papeles con la variación del día | `data912.com/live/arg_stocks` |
| Bonos | 8 bonos soberanos: AL30, GD30, AE38, AL35, GD35, AL41, GD41 y AL29 | `data912.com/live/arg_bonds` |

## Decisiones técnicas

- **Archivo único (`index.html`) en lugar de empaquetadores o frameworks con paso de compilación:** todo el maquetado, los estilos y la lógica residen en un solo documento. Esto elimina la instalación de gestores de paquetes, pasos de compilación y servidores locales; el archivo se abre directamente en el navegador mediante el protocolo `file://`.
- **Consultas directas desde el cliente en lugar de un servidor intermediario (backend):** el navegador ejecuta los pedidos HTTP hacia las APIs públicas. Esto evita el costo y mantenimiento de infraestructura propia y la gestión de claves de API, con la condición de depender de que cada proveedor mantenga habilitado el acceso por CORS y conserve su formato de respuesta.
- **Carga en paralelo con aislamiento de errores en lugar de una consulta unificada bloqueante:** los cuatro pedidos se disparan de forma independiente y cada uno maneja sus propias excepciones antes de dibujar en el DOM. Si una fuente falla o tarda en responder, solo ese bloque informa el error mientras los otros tres muestran sus datos con normalidad.
- **Valores instantáneos sin almacenamiento en lugar de base de datos con series históricas:** la interfaz renderiza únicamente el último precio disponible sin guardar estados previos, gráficos ni alertas. Esto evita el consumo creciente de memoria en sesiones largas de segundo monitor y mantiene el código sin dependencias externas.

## Estructura

```text
LICENSE       Texto de la licencia MIT
index.html    Página completa: maquetado HTML, estilos CSS y lógica de consulta
preview.png   Captura de pantalla de referencia de la interfaz
```

## Cómo se usa

No requiere instalación de dependencias, compilación ni servidor web local. Se necesita un navegador web con conexión a internet.

Abrir el archivo `index.html` en el navegador:

```bash
# En Linux
xdg-open index.html

# En macOS
open index.html

# En Windows
start index.html
```

También se puede abrir arrastrando el archivo `index.html` hacia una ventana abierta del navegador o mediante doble clic desde el explorador de archivos del sistema.

## Cómo se publica

Al consistir en un único archivo estático sin procesamiento en servidor, se despliega subiendo `index.html` a cualquier servicio de alojamiento estático (como GitHub Pages) o copiándolo en el directorio público de un servidor web estándar sin configuración adicional.

## Estado del contenido

Los precios mostrados provienen en tiempo real de las APIs externas listadas, tienen carácter de referencia y pueden presentar demoras de origen; el pie de la página incluye esta aclaración de forma permanente. Si alguno de los proveedores externos modifica la estructura de su JSON o restringe los encabezados CORS, el bloque correspondiente dejará de mostrar cotizaciones hasta que se ajuste el código.

## Licencia

Distribuido bajo la licencia MIT. Consultar el archivo `LICENSE` para más detalles.
