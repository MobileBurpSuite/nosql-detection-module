# Módulo de Detección de NoSQL Injection (nosql-detection-module)

## Descripción General
Este repositorio forma parte del proyecto de titulación titulado *"Desarrollo de una herramienta para pruebas de penetración en aplicaciones móviles Android"*, desarrollado en la Facultad de Ingeniería de Sistemas de la Escuela Politécnica Nacional (EPN). El módulo está enfocado en la automatización de la identificación, análisis y explotación controlada de vulnerabilidades por Inyección NoSQL (NoSQLi) en APIs REST.

---

## Arquitectura y Flujo del Módulo

![Diagrama de Arquitectura del Módulo NoSQLi](docs/images/arquitectura_nosql.png)

*El diagrama superior detalla el flujo operativo: el interceptor extrae los parámetros de la solicitud HTTP, el motor aplica los vectores estructurados de operadores BSON y el sistema analiza la respuesta del servidor para determinar la vulnerabilidad.*

---

## Motor de Base de Datos Seleccionado y Justificación

* **Motor seleccionado:** MongoDB.
* **Justificación técnica:** Se seleccionó MongoDB debido a su posición dominante y sostenida como el sistema de gestión de bases de datos NoSQL orientado a documentos (*Document Stores*) más adoptado a nivel global. Su modelo de almacenamiento basado en documentos BSON y su diseño flexible (*schemeless*) permiten evaluar de forma directa la ausencia de validación estricta de tipos de datos en la capa de aplicación, facilitando la ejecución sistemática de vectores de ataque como operadores lógicos y de comparación (`$ne`, `$gt`, `$regex`, `$where`).

![Tendencia Histórica de Popularidad - DB-Engines](docs/images/db_engines_trend.png)
*Figura: Gráfica de tendencia histórica de popularidad (escala logarítmica) obtenida de [DB-Engines Ranking of Document Stores](https://db-engines.com/en/ranking_trend/document+store). La visualización evidencia el liderazgo absoluto y constante de MongoDB frente a otras alternativas del mercado a lo largo del tiempo, consolidándose como el estándar de la industria y el entorno objetivo ideal para el análisis de vulnerabilidades NoSQLi en aplicaciones móviles.*

---

## Taxonomía del Diccionario de Vectores

El motor utiliza un conjunto de vectores estructurados y validados con base en repositorios de referencia de la industria como *SecLists* y *PayloadsAllTheThings*:

1. **Bypasses de Autenticación y Lógica:** Uso de operadores de comparación y desigualdad (`$ne`, `$nin`, `$gt`) para alterar condiciones lógicas en consultas de autenticación.
2. **Extracción Ciega (Blind NoSQLi):** Aplicación de expresiones regulares (`$regex`) y funciones de coincidencia (`match`) para la recuperación incremental de información carácter por carácter.
3. **Ejecución de Código en Servidor:** Inyección de expresiones JavaScript mediante el operador `$where` para evaluar condiciones del lado del servidor.
4. **Pruebas Basadas en Tiempo (Time-Based):** Inyección de bucles síncronos y rutinas de retardo (`sleep`) para verificar vulnerabilidades en escenarios ciegos.

---

## Evidencias y Pruebas del Prototipo

![Captura de Ejecución del Módulo](docs/images/ejecucion_modulo.png)

*Interfaz de configuración y resultados de la ejecución de payloads contra la API de prueba.*

---

## Información del Autor y Componente

* **Estudiante:** Juan Diego Suárez Barberán
* **Componente Asignado:** Módulo de Detección de NoSQL Injection
* **Carrera:** Ingeniería de Software
* **Institución:** Escuela Politécnica Nacional (EPN)
