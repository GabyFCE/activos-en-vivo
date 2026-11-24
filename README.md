# activos-en-vivo

Página de una sola archivo que muestra cotizaciones de dólares, cripto, acciones
argentinas y bonos. Sin servidor, sin dependencias, sin credenciales.

Para usarla, abrir `index.html` en el navegador con conexión a internet.

## Qué muestra

- **Dólares**: oficial, blue, bolsa, CCL, tarjeta, mayorista y cripto, con compra y venta.
- **Cripto**: BTC, ETH, SOL, BNB, USDT y XRP en dólares, con variación.
- **Acciones argentinas**: doce papeles con variación del día.
- **Bonos**: AL30, GD30, AE38, AL35, GD35, AL41, GD41 y AL29.

Se refresca sola cada 30 segundos. El botón "Actualizar" hace la consulta en el momento.

Si una fuente no responde, esa sección muestra el error y el resto sigue funcionando.

## Fuentes

| Sección | API |
| --- | --- |
| Dólares | `dolarapi.com/v1/dolares` |
| Cripto | `api.coingecko.com/v3/simple/price` |
| Acciones y bonos | `data912.com/live/arg_stocks`, `data912.com/live/arg_bonds` |

Las tres son públicas y no piden clave. Los pedidos van directo desde el navegador,
así que la página depende de que cada API permita CORS.

## Límites

- Los precios son de referencia y pueden estar demorados. La página lo dice en el pie.
- No hay historial ni gráficos: muestra el último valor de cada activo.
- Si alguna de las APIs cambia su formato o deja de permitir CORS, la sección correspondiente deja de cargar.

## Archivos

```
index.html    la página completa: estilos, lógica y maquetado
preview.png   imagen de referencia del diseño
```

`preview.png` no es una captura del navegador: la genera un script aparte que
dibuja el mismo diseño con los datos del momento. Ese script no está en el repo.

## Licencia

MIT.
