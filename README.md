# Recomendaciones de interoperabilidad
Se entiende por interoperabilidad a la capacidad de diferentes sistemas de información de intercambiar, compartir y utilizar datos sin un esfuerzo especial. Habilita el flujo de datos entre diversos sistemas y organizaciones mediante el uso de estándares, protocolos y tecnologías comunes permitiendo comunicar, compartir y usar información de manera coordinada y útil. La interoperabilidad es un concepto clave de los **principios FAIR data**, según los cuales los datos deben ser:
- Encontrables (**F**indable), 
- **A**ccesibles, 
- **I**nteroperables y 
- **R**eutilizables

Para **un conjunto de datos publicado de series temporales en ubicaciones específicas** (por ejemplo, datos ambientales, meteorológicos, hidrológicos o de sensores), estos son los **requerimientos mínimos** para hacerlo *interoperable* — o sea, habilitado para acceso máquina-máquina y entendible por otras personas y otros sistemas.

---

## 1. Formato legible por máquina (machine-readable)

El conjunto de datos debe usar un **formato estándar y estructurado** que se pueda analizar (parsear) en forma programática — no PDFs ni archivos de Excel.

**Ejemplos de buenos formatos:**

* **CSV/TSV** (con encabezados y metadatos bien definidos)
* **JSON / GeoJSON** (para datos estructurados o geoespaciales)
* **NetCDF / HDF5** (para series temporales en grilla o multidimensionales)
* **Parquet** (para series temporales tabulares de gran tamaño)
* **XML (e.g. WaterML 2.0, O&M)** (para datos de sensores)

**Malos formatos:** Documentos de Word, texto plano sin encabezados, capturas de pantalla, etc.

---

## 2. Modelo de datos explícito (variables, unidades, coordenadas)

Cada variable y coordenada debe ser inequívoca.

| Elemento                       | Requerimiento                                                  | Ejemplo                              |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------ |
| **Tiempo**                      | Etiqueta de fecha ISO 8601                                           | `2025-10-20T12:00:00Z`               |
| **Ubicación**                  | Especificar sistema de coordenadas (CRS)                  | `EPSG:4326`, lat/lon                 |
| **Nombre de variable**             | Descriptivo o estandarizado (vocabularios CF, SWEET, INSPIRE) | `caudal`, `air_temperature`, `river_discharge` |
| **Unidades**                     | Usar unidades del SI o estandarizadas                                 | `m³/s`, `°C`                         |
| **Valores faltantes**            | Representar con una convención clara                          | `NaN`, `-9999`, o `null`   |
| **ID de observación** (opcional) | Identificador único persistente                  | `station_id`, `sensor_id`            |

---

## 3. Suficientes metadatos

Los metadatos deben describir lo que el conjunto de datos *significa* y *cómo fue creado*.

**Campos de metadatos mínimos:**

| Categoría              | Campos                                    | Ejemplo                                                     |
| --------------------- | --------------------------------------------- | ----------------------------------------------------------- |
| **Identificación**    | título, descripción, palabras clave                  | "Caudal horario en 10 estaciones de la cuenca del Paraná" |
| **Procedencia**        | creador, organización, contacto, fecha de creación | `contacto@ina.gob.ar`, "Instituto Nacional del Agua"                       |
| **Cobertura espacial**  | CRS, cuadro delimitador (bounding box), coordenadas de las estaciones        | EPSG:4326, -59.3–-57.1                                      |
| **Cobertura temporal** | fecha inicial y final                            | 2020-01-01 → 2025-10-01      |
| **Resolución temporal** | Paso temporal, método de interpolación o de agregación | "horario", "P1D", "Average" | 
| **Variables**         | nombre, unidades, método, precisión                  | `temperature`, `°C`, modelo del equipo                           |
| **Licencia**         | términos de uso                                   | CC-BY-4.0                                                   |
| **Versión**        | versión del conjunto de datos o DOI                        | `v1.1`, `10.5281/zenodo.12345`                              |

**Estándares de metadatos recomendados:**

* **Dublin Core (DCAT / DCAT-AP)** para catálogos de metadatos
* **ISO 19115 / 19139** para conjuntos de datos geoespaciales
* **Convenciones CF** (Climate and Forecast) para nombres de variables en archivos NetCDF/CSV
* **SensorML / O&M / WaterML / WMDR** para observaciones in-situ

---

## 4. Identificadores persistentes y referencias

Cada uno de los siguientes idealmente debería tener un **identificador estable y resoluble**, (o sea, que permita vincularse con un recurso persistente en la web):

* Conjunto de datos: DOI o URI
* Variables: usar vocabularios controlados (p.ej., nombres de CF)
* Ubicaciones/sensores: IDs únicos de estación (preferentemente URIs persistentes)
* CRS: código EPSG oficial

---

## 5. Estructura clara y acceso estandarizado mediante API (opcional pero muy recomendado)

Para hacer los datos interoperables **entre sistemas**, ofrecer acceso a través de un servicio o modelo estandarizado:

| Estándar                       | Propósito                           |
| ------------------------------ | ----------------------------------- |
| **OGC SensorThings API / O&M / WaterOneFlow** | series temporales de sensores       |
| **OGC WFS / WMS / WCS**        | capas espaciales                    |
| **THREDDS / ERDDAP**           | series temporales científicas basadas en NetCDF |
| **CSV on the Web (W3C CSVW)**  | datos tabulares vinculados a metadatos       |

En los casos en que sólo se publiquen archivos estáticos, o se publique una API no estándar (custom), se recomienda **documentar el esquema y método de acceso**.

