# Mapa de la investigación con ciencia de datos y machine learning

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FatimaMirandap/ML-Datostadamx2026/blob/main/mappingML.ipynb)

Material del evento DatostadaMx 2026, "Ciencia de Datos y Machine Learning en la investigación: lo que ya es posible y lo que viene". Creado por Fátima Miranda Pestaña.

El proyecto responde a una pregunta: qué instituciones del mundo publican trabajos con ciencia de datos o machine learning en Nature, Science, Scientific Data y arXiv, y en qué líneas de investigación. Consta de un cuaderno de Google Colab que descarga y agrega los datos desde OpenAlex, y de un mapa interactivo en HTML que los muestra.

## Aviso sobre versiones

Este repositorio contiene la **versión 1**. Hay una **versión 2** del cuaderno y del mapa en desarrollo, con filtro por año, animación de la evolución en el tiempo, descarga con reanudación y otras mejoras. Estará disponible en unos días en este mismo repositorio. La versión 1 seguirá funcionando.

## Contenido del repositorio

```
mappingML.ipynb    Cuaderno de Colab que descarga y agrega los datos
docs/index.html              Mapa interactivo (HTML, sin servidor)
docs/data/                   Aquí van instituciones.csv y meta.json, que genera el cuaderno
requirements.txt             Dependencias, solo si ejecutas el cuaderno fuera de Colab
.gitignore                   Archivos que no deben subirse
```

La carpeta se llama `docs` porque GitHub Pages solo puede publicar la raíz del repositorio o una carpeta con ese nombre.

## Qué necesitas

- Una cuenta de Google, para usar Colab.
- Una cuenta gratuita de OpenAlex, para obtener una clave de API.
- Una cuenta de GitHub, si quieres publicar tu propio mapa.

## Inicio rápido

1. Obtén una clave gratuita de OpenAlex (paso 1 de la sección siguiente).
2. Abre el cuaderno en Colab con el botón de arriba o con *Archivo, Abrir cuaderno, GitHub*.
3. Guarda la clave como secreto de Colab con el nombre `OPENALEX_API_KEY`.
4. Ejecuta las celdas en orden. Con `DRY_RUN = True` solo se estima el costo. Cámbialo a `False` para descargar los datos.
5. Descarga `instituciones.csv` y `meta.json`, colócalos en `docs/data/` y abre `docs/index.html` desde un servidor local o publícalo con GitHub Pages.

## Instrucciones paso a paso

### 1. Clave de OpenAlex

1. Crea una cuenta gratuita en openalex.org.
2. Copia tu clave desde openalex.org/settings/api.
3. En Colab, abre el panel izquierdo, pulsa el ícono de la llave (Secretos), agrega un secreto llamado `OPENALEX_API_KEY` con tu clave y activa el acceso del cuaderno a ese secreto.

La clave gratuita da un presupuesto de 1 USD al día, que se reinicia a medianoche UTC (18:00 en México). Una descarga completa de esta versión cuesta alrededor de 0.2 a 0.5 USD. No escribas nunca la clave dentro del cuaderno ni la subas al repositorio.

### 2. Ejecutar el cuaderno

El cuaderno está explicado en celdas de texto, sección por sección. En resumen:

1. En la sección 1 puedes cambiar el período (`PERIODO`), tu correo de contacto (`EMAIL`, opcional) y el modo de prueba (`DRY_RUN`).
2. Las secciones 3 a 6 identifican las fuentes, definen el criterio de machine learning, permiten revisar una muestra de títulos y cargan las líneas de investigación.
3. La sección 7 descarga los conteos por institución. Es la única que gasta presupuesto de forma importante.
4. Las secciones 8 a 10 agregan tipo, país y coordenadas de cada institución, muestran una vista previa y guardan los archivos.

### 3. Archivos que genera el cuaderno

`instituciones.csv`: una fila por institución, fuente y línea de investigación.

| Columna | Contenido |
| --- | --- |
| `institution_id` | Identificador de OpenAlex |
| `institution` | Nombre |
| `type` | Tipo: education, company, government, nonprofit, healthcare, facility, archive u other |
| `country_code` | País (código de dos letras) |
| `city` | Ciudad |
| `lat`, `lon` | Coordenadas |
| `source_group` | Fuente: Nature, Science, Scientific Data o arXiv |
| `field` | Línea de investigación (campo de OpenAlex) |
| `works` | Número de obras |

`meta.json`: fecha de extracción, período, fuentes, criterio de machine learning y resumen por fuente. El mapa lo usa para el pie de página.

### 4. Ver el mapa en tu computadora

Abrir `docs/index.html` con doble clic no funciona, porque el navegador bloquea la lectura de los archivos de datos. En una consola, entra a la carpeta `docs` y ejecuta:

```
python -m http.server 8000
```

Luego abre `http://localhost:8000` en el navegador. El mapa necesita conexión a internet para cargar el mapa base y las librerías.

## Qué puedes hacer en el mapa

- **Filtrar por fuente, línea de investigación y sector** de la institución (educación, empresa, gobierno, sin fines de lucro y otros), y fijar un mínimo de obras por institución.
- **Filtrar por institución**, escribiendo su nombre o eligiéndola de una lista. Puedes seleccionar varias.
- **Ver métricas por institución**: obras, porcentaje del total, lugar en el ranking, y desglose por línea y por fuente. Un panel muestra además las 10 instituciones con más obras.
- **Cambiar la vista**: círculos, hotspots (concentración de participaciones) o ambos.
- **Leer los porcentajes**: dos barras con su leyenda muestran cuántas obras corresponden a cada sector y a cada línea de investigación, con su porcentaje.
- **Cambiar el idioma** entre español e inglés.
- **Consultar el pie de página**, con la fecha de extracción, el período del estudio y la fuente de los datos.

Si no existe `docs/data/meta.json`, el pie de página usa una fecha y un período de respaldo, definidos en la constante `RESPALDO` al inicio del script de `docs/index.html`. Cámbialos si publicas tus propios datos sin ese archivo.

## Solución de problemas

| Síntoma | Qué hacer |
| --- | --- |
| El mapa dice que no encuentra los datos | Comprueba que `instituciones.csv` esté en `docs/data/` y que abras el mapa desde un servidor, no con doble clic |
| El cuaderno falla con "Falló works" tras muchas llamadas | Se agotó el presupuesto diario de OpenAlex. Espera al reinicio de medianoche UTC |
| "Clave de OpenAlex: no encontrada" | Revisa el nombre del secreto (`OPENALEX_API_KEY`) y que el acceso del cuaderno esté activado |
| La descarga se queda en cero filas | Verifica que `DRY_RUN` sea `False` |
| El mapa se ve vacío en GitHub Pages | Confirma que Pages apunte a `main` y `/docs`, y espera unos minutos tras publicar |
| Una institución no aparece | Puede no tener coordenadas en OpenAlex; el cuaderno imprime cuántas se excluyen |

## Límites del estudio

- La unidad es la publicación, no el proyecto de investigación: muchos proyectos no publican en estas revistas.
- Que una obra "tenga machine learning" se decide por palabras clave en el título y el resumen, así que hay falsos positivos y falsos negativos.
- Cada obra se cuenta una vez por institución. Una obra con autores de varias instituciones suma en cada una, por lo que el total son participaciones institucionales y no obras.
- El tipo, el país y la ubicación de cada institución provienen de OpenAlex y pueden tener errores.
- Los hotspots muestran concentración, no calidad.

## Fuentes de datos y acceso

Este proyecto usa únicamente OpenAlex, consultado mediante su API. La tabla resume cómo se puede acceder a cada catálogo, por si quieres ampliar el análisis.

| Fuente | Acceso | Uso en este proyecto |
| --- | --- | --- |
| OpenAlex | API con clave gratuita; presupuesto de 1 USD al día | Catálogo base: obras, instituciones, tipo, país y coordenadas |
| Science (AAAS) | Sin API pública; el texto completo requiere licencia | Solo sus metadatos abiertos, a través de OpenAlex |


Los precios y límites de las APIs pueden cambiar; consulta la documentación de cada una.

## Próximas versiones

- Versión 2 del cuaderno y del mapa: filtro por año, animación de la evolución, descarga con reanudación y mejoras del mapa. Disponible en unos días.
- Revisión de resúmenes de una muestra con la API de Springer Nature.
- Crossref como segundo catálogo para Science.


## Cómo citar

Miranda Pestaña, F. (2026). *Mapa de la investigación con ciencia de datos y machine learning* [Software y visualización]. Evento DatostadaMx 2026, "Ciencia de Datos y Machine Learning en la investigación: lo que ya es posible y lo que viene".

Para citar los datos: Priem, J., Piwowar, H. y Orr, R. (2022). OpenAlex: A fully-open index of scholarly works, authors, venues, institutions, and concepts. arXiv:2205.01833.