# Módulo de Detección de NoSQL Injection (nosql-detection-module)

## Descripción General
Este repositorio forma parte del proyecto de titulación titulado *"Desarrollo de una herramienta para pruebas de penetración en aplicaciones móviles Android"*, desarrollado en la Facultad de Ingeniería de Sistemas de la **Escuela Politécnica Nacional (EPN)**[cite: 2]. El módulo está enfocado en la automatización de la identificación, análisis y explotación controlada de vulnerabilidades por Inyección NoSQL (NoSQLi) en APIs REST[cite: 2].

---

## Arquitectura y Flujo del Módulo

![Diagrama de Arquitectura del Módulo NoSQLi](docs/images/arquitectura_nosql.png)

*El diagrama superior detalla el flujo operativo: el interceptor extrae los parámetros de la solicitud HTTP, el motor aplica los vectores estructurados de operadores BSON y el sistema analiza la respuesta del servidor para determinar la vulnerabilidad.*

---

## Motor de Base de Datos Seleccionado y Justificación

* **Motor seleccionado:** MongoDB.
* **Justificación técnica:** Se seleccionó MongoDB debido a que es el SGBD NoSQL orientado a documentos más adoptado en el desarrollo de arquitecturas de microservicios y APIs REST modernas. Su modelo de almacenamiento basado en documentos BSON y su diseño flexible (*schemeless*) permiten evaluar de forma directa la ausencia de validación estricta de tipos de datos en la capa de aplicación. Esto posibilita la ejecución y el análisis sistemático de operadores específicos del motor (como `$ne`, `$gt`, `$regex` y `$where`), convirtiéndolo en el entorno estándar ideal para demostrar fallos de inyección NoSQL en las pruebas de penetración móviles[cite: 2].

---

## Taxonomía del Diccionario de Vectores

El motor utiliza un conjunto de vectores estructurados y validados con base en repositorios de referencia de la industria como *SecLists* y *PayloadsAllTheThings*[cite: 1]:

1. **Bypasses de Autenticación y Lógica:** Uso de operadores de comparación y desigualdad (`$ne`, `$nin`, `$gt`) para alterar condiciones lógicas en consultas de autenticación[cite: 1].
2. **Extracción Ciega (Blind NoSQLi):** Aplicación de expresiones regulares (`$regex`) y funciones de coincidencia (`match`) para la recuperación incremental de información carácter por carácter[cite: 1].
3. **Ejecución de Código en Servidor:** Inyección de expresiones JavaScript mediante el operador `$where` para evaluar condiciones del lado del servidor[cite: 1].
4. **Pruebas Basadas en Tiempo (Time-Based):** Inyección de bucles síncronos y rutinas de retardo (`sleep`) para verificar vulnerabilidades en escenarios ciegos[cite: 1].

---

## Evidencias y Pruebas del Prototipo

![Captura de Ejecución del Módulo](docs/images/ejecucion_modulo.png)

*Interfaz de configuración y resultados de la ejecución de payloads contra la API de prueba.*

---

## Información del Autor y Componente

* **Estudiante:** Juan Diego Suárez Barberán[cite: 2]
* **Componente Asignado:** Módulo de Detección de NoSQL Injection[cite: 2]
* **Carrera:** Ingeniería de Software[cite: 2]
* **Institución:** Escuela Politécnica Nacional (EPN)[cite: 2]
