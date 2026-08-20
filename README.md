# placapi-openapi

Especificación **OpenAPI 3.1** y colección de **Postman** de [PlacApi](https://placapi.com),
la API REST de consulta vehicular y de personas de Colombia.

PlacApi resuelve en una llamada lo que hoy exige captchas y portales separados: consulta el
**RUNT** por placa, las multas del **SIMIT**, el **SOAT** y la **revisión tecnomecánica**, el
impuesto vehicular, el avalúo **FASECOLDA**, el **pico y placa** de 30 ciudades, las licencias
de conducción, los antecedentes de una persona y los registros **RUES**, **SISBEN** y **RUAF**.
Devuelve JSON. Sin scraping del lado del cliente y sin captchas.

| | |
|---|---|
| Endpoints | **35** |
| Servidor | `https://placapi.com/api` |
| Autenticación | cabecera `x-api-key` |
| Formato | JSON, `POST` en todos los endpoints |
| Límite | 1.000 consultas por minuto por API key |
| Cobro | 1 crédito por consulta **con datos** (`consulta-full` cuesta 2) |

## Archivos

| Archivo | Qué es |
|---|---|
| [`openapi.json`](openapi.json) | Especificación OpenAPI 3.1 completa, con esquemas de petición y respuesta de los 35 endpoints |
| [`postman.json`](postman.json) | Colección de Postman lista para importar |

Los dos se **generan desde el catálogo de la API**, así que nunca se desincronizan del servicio
real. La versión viva siempre está en `https://placapi.com/openapi.json` y
`https://placapi.com/postman.json`.

## Empezar

```bash
curl -X POST 'https://placapi.com/api/consulta' \
  -H 'x-api-key: pk_live_TU_CLAVE' \
  -H 'content-type: application/json' \
  -d '{"placa":"ABC123","docType":"CC","docNumber":"1010111935"}'
```

La API key se genera en <https://placapi.com/integracion>. El registro incluye una consulta de
cortesía para probar sin comprar.

## Generar un cliente

Al ser OpenAPI 3.1 estándar, cualquier generador funciona:

```bash
# TypeScript
npx openapi-typescript https://placapi.com/openapi.json -o placapi.d.ts

# Cualquier lenguaje
npx @openapitools/openapi-generator-cli generate \
  -i https://placapi.com/openapi.json -g python -o ./placapi-python
```

Para Node y TypeScript hay un cliente ya hecho: **[placapi-node](https://github.com/pipe0919/placapi-node)**.

## Cómo se cobra

- Se cobra **solo cuando la consulta devuelve datos**.
- Una respuesta "no registra antecedentes" **sí cobra**: es el dato que se vino a comprar.
- Las consultas sin resultado (404) tienen 10 gratis al mes por cada tipo de respuesta sin
  datos, y después cobran igual.
- Los créditos se compran por adelantado, **no vencen** y no hay mensualidad.
- Un 502 (`source_error`) **no cobra**: reintentar sí sirve.

Precios vigentes en <https://placapi.com/comprar>.

## Documentación

- Referencia y guías: <https://placapi.com/docs>
- Errores y códigos: <https://placapi.com/docs/errores>
- Fuentes y metodología: <https://placapi.com/fuentes-y-metodologia>
- SLA: <https://placapi.com/sla>
- Para modelos de lenguaje: <https://placapi.com/llms.txt> y <https://placapi.com/llms-full.txt>

## Licencia

Los archivos de especificación de este repositorio se publican bajo **CC0 1.0** (dominio
público): úsalos, genera clientes, publícalos. El servicio que describen tiene sus propios
[términos](https://placapi.com/terminos).

---

Este repositorio contiene **únicamente la especificación pública** de la API. El código del
servicio no es abierto.
