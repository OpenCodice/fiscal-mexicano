# fiscal-mexicano 🧾🇲🇽

Texto vigente del **corpus fiscal federal mexicano** en Markdown, con su metadata
derivada. El historial de git es el registro **auditable de reformas**: cada
commit corresponde a una reforma publicada en el DOF e incorporada al PDF oficial
de la H. Cámara de Diputados.

Este repo es **generado**: los `.md` no se editan a mano. Los produce
[`fiscal-extractor`](../fiscal-extractor) a partir de los PDF oficiales.

## Estructura

```
<clave>/NNN.md              texto fiel por artículo (capa 1 — fuente de verdad)
                            p. ej. cff/027.md, cff/017-h-bis.md, liva/018-h-quater.md
metadata/
  documentos.json           índice maestro del corpus (qué documentos, versión)
  <clave>/articulos.json    metadata por artículo (cita, título, reformas, derogado)
  <clave>/reformas.json     reforma DOF → artículos afectados
```

La clave del artículo incluye sus sufijos: `027` (Art. 27), `017-h-bis`
(Art. 17-H Bis), `032-b-quater` (Art. 32-B Quáter).

## Documentos

| Clave | Documento | Tipo |
|-------|-----------|------|
| `cff`   | Código Fiscal de la Federación | ley |
| `lisr`  | Ley del Impuesto sobre la Renta | ley |
| `liva`  | Ley del Impuesto al Valor Agregado | ley |
| `lieps` | Ley del IEPS | ley |
| `rcff`  | Reglamento del Código Fiscal de la Federación | reglamento |

Pendientes (fases siguientes): Resolución Miscelánea Fiscal (RMF), criterios
normativos y no vinculativos.
