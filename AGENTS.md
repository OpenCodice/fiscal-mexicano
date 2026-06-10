# fiscal-mexicano — guía para agentes

Texto vigente de la legislación fiscal federal de México —leyes, reglamentos,
la Resolución Miscelánea Fiscal, sus anexos y los criterios del SAT—, un archivo
por artículo, regla o criterio, versionado para que el historial de git sea el
registro de sus cambios. Es un repo de datos: el único código es la validación
de los PR (`revisar-pr.yml`).

**No edites a mano nada de las carpetas de documentos ni de `metadata/`.** Todo
se genera con
[`fiscal-extractor`](https://github.com/OpenCodice/fiscal-extractor) a partir de
los PDF oficiales. Si una unidad está mal, el arreglo va en el extractor.

## Estructura

- `{documento}/NNN.md` — el texto (`cff/027.md`, `rmf-2026/2.7.1.21.md`). La
  `clave` del documento viene del registro del extractor.
- `metadata/{documento}/` — derivada y regenerable: índice, fechas de reforma y
  pasajes citables con su página del PDF.
- `metadata/documentos.json` — índice del corpus con la versión de cada documento.
- `README.md` — la tabla entre los marcadores `DOCUMENTOS:INICIO` / `FIN` la
  regenera la vigilancia (`scripts/readme_fiscal.py` del extractor); el resto
  se edita a mano.

## Cómo cambia este repo

La vigilancia no vive aquí: es `vigilar-fiscal.yml` de
[`fiscal-extractor`](https://github.com/OpenCodice/fiscal-extractor), y la lanza
cada lunes 09:00 UTC el Worker `scheduler` de Cloudflare (o a mano:
`gh workflow run vigilar-fiscal.yml -R OpenCodice/fiscal-extractor`). Así el
historial de este repo solo tiene cambios de texto legal. Descarga los PDF
oficiales, regenera con el extractor y, si cambió texto, lanza un agente
(Claude Code) que compara cada unidad cambiada contra el PDF y repara el
extractor si hace falta. Después abre aquí un PR con **un commit por documento**
(y uno aparte para el índice y el README):

- Con la etiqueta `verificado`, `revisar-pr` corre los invariantes (`validar`)
  y lo fusiona solo, con rebase, conservando esos commits: `git log -- cff/` es
  la historia del CFF.
- Si no quedó verificado, no se abre PR: el texto vigente se queda como está,
  lo no publicado se anota en el resumen de la corrida y la siguiente vigilancia
  lo reintenta. Nada se asigna a una persona.

El título de cada commit distingue una **reforma** (cambió la ley:
`Reforma CFF (2026-11-10): art. 27`) de una **corrección de extracción** (misma
ley, mejor lectura del PDF). La bitácora dependerá de él.

Si la vigilancia falla, se relanza una vez; si el reintento también falla, deja
un issue (sin asignar) en el extractor como constancia, y el vigilante del
`scheduler` la relanza si pasan más de 8 días sin correr.

La vigilancia descarga los cerca de 30 PDF y tarda unos 25 minutos. Si una
fuente no se puede descargar, el resto se procesa igual y el job termina en
rojo para que no pase inadvertido.

## Lo único que se edita aquí

`revisar-pr.yml` (la validación de los PR) y la documentación. Trabaja en `main`, con `git pull --ff-only` antes; `main` exige
el check `validar`.
