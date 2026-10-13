# 🚀 Sideproject — Ingesta multifuente

**Repositorio:** `sideproject-ingesta-multifuente` · **Tipo:** ETL (extracción) · **Tiempo:** 5–6 h · **Después de:** lecciones 1 a 4 de esta carpeta

> Reglas, README y lista de cierre: [`SIDEPROJECTS.md`](docs/REGLAS.md). **No hay solución: el diseño es tuyo.**

## El caso

Finanzas quiere ver las compras del banco en dólares. Los datos viven en tres sitios y nadie los ha juntado. Tu trabajo es traerlos a una zona *raw*, **tal como llegan**, de forma que se pueda repetir cada día.

| Fuente | Qué traer |
|---|---|
| Archivo | `datos/movimientos.csv` |
| PostgreSQL | Tablas `clientes` y `cuentas` de `banco_python` |
| API | `https://api.frankfurter.dev/v1/2026-01-01..2026-09-30?base=USD&symbols=MXN` |

## Qué practicas

| Lección | Dónde aparece |
|---|---|
| Excel y otros formatos | Guardar en Parquet |
| Bases de datos | Extraer de PostgreSQL con parámetros y credenciales por entorno |
| Web y JSON | Guardar la respuesta cruda de la API |
| APIs REST | Timeout, códigos de estado y reintentos |

## Hitos

**Hito 1 — Una fuente, de punta a punta.** Un `main.py` que lea el CSV y lo deje en `data/raw/archivo/fecha=AAAA-MM-DD/`.
*Comprueba:* existe el archivo y tiene 400 filas.

**Hito 2 — Las otras dos fuentes.** Un módulo por fuente con la misma forma: una función `extraer()`. La respuesta de la API se guarda en JSON, sin tocar.
*Comprueba:* tres carpetas bajo `data/raw/`; ninguna contraseña en el código.

**Hito 3 — Que un fallo no tumbe todo.** La API con timeout y 3 reintentos. Si una fuente falla, las otras se ingieren y el programa lo informa al final.
*Comprueba:* apaga el wifi y ejecútalo: dos fuentes llegan, una se reporta como fallida.

**Hito 4 — Saber qué pasó.** `logging` en lugar de `print`: fuente, filas y segundos.
*Comprueba:* con solo leer el log sabes cuántas filas trajo cada fuente.

**Hito 5 — Repetible.** Ejecutarlo dos veces el mismo día no duplica nada.
*Comprueba:* mismo número de archivos y de filas tras la segunda ejecución.

## Preguntas para defenderlo

- ¿Por qué guardas el dato crudo en lugar de limpiarlo al vuelo?
- ¿Qué diferencia hay entre «la fuente no tiene datos» y «la fuente falló»? ¿Cómo lo distingue tu código?
- La API no publica sábados ni domingos. ¿Dónde se resuelve eso y por qué no aquí?

## Súbelo de nivel

- Zona raw en S3 sobre **Floci** en lugar de en disco (`07_nube_con_floci/02`), eligiendo el destino con una variable de entorno.
- Una cuarta fuente: un Excel con una hoja por mes.
