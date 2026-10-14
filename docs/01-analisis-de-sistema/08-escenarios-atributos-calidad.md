# Escenarios de atributos de calidad de APU GO

## Propósito

Definir situaciones verificables para evaluar los atributos de calidad de APU GO. Cada escenario identifica la fuente, el estímulo, el artefacto, el entorno, la respuesta y su medida.

Las metas numéricas son propuestas de diseño pendientes de validación; no representan resultados de pruebas realizadas.

## EQ-01. Rendimiento: consultar lecciones

| Elemento | Descripción |
|---|---|
| Atributo relacionado | AC01: Rendimiento. |
| Fuente del estímulo | Estudiantes. |
| Estímulo | Solicitan simultáneamente el listado o el detalle de las lecciones. |
| Artefacto | API REST, módulo de contenidos y acceso a PostgreSQL. |
| Entorno | Operación normal con datos representativos y 200 usuarios concurrentes. |
| Respuesta | El sistema devuelve las lecciones solicitadas sin errores técnicos inesperados. |
| Medida | Propuesta por validar: al menos el 95 % de las solicitudes responde en 2 segundos o menos y menos del 1 % presenta errores técnicos. La descarga de audios e imágenes se mide por separado. |
| Verificación | Ejecutar una prueba de carga y registrar tiempos de respuesta, errores y concurrencia. |
| Estado | Pendiente de validar la meta y documentar la infraestructura de prueba. |

## EQ-02. Disponibilidad: acceso al aprendizaje

| Elemento | Descripción |
|---|---|
| Atributo relacionado | AC02: Disponibilidad. |
| Fuente del estímulo | Estudiantes y docentes. |
| Estímulo | Intentan consultar lecciones, registrar respuestas o revisar el progreso durante el horario de servicio. |
| Artefacto | API REST y servicios de persistencia de APU GO. |
| Entorno | Sistema publicado con monitoreo de las operaciones críticas. |
| Respuesta | El sistema atiende las operaciones y comunica de forma controlada los fallos que impiden completarlas. |
| Medida | Propuesta por validar: disponibilidad mensual mínima del 99,5 % para consultar lecciones, registrar respuestas y consultar progreso. |
| Verificación | Revisar el monitoreo y calcular: disponibilidad = tiempo disponible / tiempo total observado × 100. |
| Estado | Pendiente de acordar la meta, el horario de servicio y el tratamiento de los mantenimientos programados. |

## EQ-03. Escalabilidad: incremento de estudiantes

| Elemento | Descripción |
|---|---|
| Atributo relacionado | AC03: Escalabilidad. |
| Fuente del estímulo | Incremento de estudiantes conectados. |
| Estímulo | La carga aumenta de 200 a un pico de 300 usuarios concurrentes. |
| Artefacto | Backend modular, balanceador y PostgreSQL. |
| Entorno | Prueba controlada con datos, recursos y mezcla de operaciones documentados. |
| Respuesta | Se incorporan réplicas del backend para atender la carga sin modificar el cliente Flutter. |
| Medida | Propuesta por validar: con la configuración ampliada, al menos el 95 % de las consultas de lecciones y progreso responde en 3 segundos o menos, con menos del 1 % de errores técnicos, durante un pico de 15 minutos. |
| Verificación | Comparar las métricas antes y después de agregar réplicas; revisar también la utilización y las conexiones de PostgreSQL. |
| Estado | Pendiente de validar mediante pruebas de carga. |

## EQ-04. Seguridad: acceso según el rol

| Elemento | Descripción |
|---|---|
| Atributo relacionado | AC04: Seguridad. |
| Fuente del estímulo | Usuario sin sesión o sin permisos suficientes. |
| Estímulo | Intenta gestionar contenidos, cambiar roles o consultar información de otro estudiante sin autorización. |
| Artefacto | API REST y controles de autenticación y autorización. |
| Entorno | Pruebas con cuentas de estudiante, docente y administrador, además de solicitudes sin sesión. |
| Respuesta | El sistema rechaza el acceso, no ejecuta cambios y no expone información protegida. |
| Medida | Rechazar el 100 % de los intentos no autorizados incluidos en la matriz de pruebas, sin modificaciones de datos ni exposición de información protegida. |
| Verificación | Probar los permisos por rol y por recurso; comprobar las respuestas y el estado de los datos. |
| Estado | Pendiente de elaborar la matriz de permisos y ejecutar las pruebas. |

## EQ-05. Mantenibilidad: modificar la gamificación

| Elemento | Descripción |
|---|---|
| Atributo relacionado | AC05: Mantenibilidad. |
| Fuente del estímulo | Desarrollador de APU GO. |
| Estímulo | Se solicita modificar la cantidad de puntos otorgados por completar una actividad. |
| Artefacto | Reglas de gamificación y casos de uso relacionados. |
| Entorno | Backend organizado como monolito modular con Clean Architecture y pruebas disponibles. |
| Respuesta | El cambio se realiza en las responsabilidades correspondientes, conservando los contratos que no necesitan modificarse. |
| Medida | Aprobar el 100 % de las pruebas acordadas de gamificación y regresión de registro de respuestas y progreso; mantener cero dependencias del dominio hacia frameworks o infraestructura. |
| Verificación | Revisar los archivos modificados, ejecutar las pruebas e inspeccionar las dependencias del código. |
| Estado | Pendiente de implementar las pruebas y los controles de dependencias. |

## EQ-06. Privacidad: mensajes y contexto enviados a Groq

| Elemento | Descripción |
|---|---|
| Atributo relacionado | AC06: Privacidad. |
| Fuente del estímulo | Estudiante que consulta al tutor y caso de uso de recomendaciones. |
| Estímulo | Solicita asistencia al tutor con Groq (RF14) o recomendaciones previstas (RF07). |
| Artefacto | Filtro de mensajes y contexto, ServicioTutorIA, ServicioRecomendaciones y AdaptadorGroq. |
| Entorno | Integración obligatoria con Groq, probada con datos sintéticos, campos permitidos y casos definidos de mensajes con información personal. |
| Respuesta | El sistema envía únicamente los datos de aprendizaje mínimos permitidos, sin información identificable del estudiante. |
| Medida | En el 100 % de las solicitudes de los casos de privacidad definidos, ausencia de los datos personales prohibidos introducidos en mensajes, historial y contexto; todos los campos estructurados enviados pertenecen a la lista permitida. |
| Verificación | Inspeccionar las solicitudes del adaptador y probar que los campos prohibidos se excluyan antes del envío. |
| Estado | Verificación obligatoria pendiente de definir e implementar la lista de campos y el filtro de mensajes. |

## Trazabilidad de los escenarios

| Escenario | Atributo | Driver | Decisiones relacionadas |
|---|---|---|---|
| EQ-01 | AC01: Rendimiento | DA01 | ADR-001, ADR-004 |
| EQ-02 | AC02: Disponibilidad | DA08 | ADR-007 |
| EQ-03 | AC03: Escalabilidad | DA01 | ADR-001, ADR-004 |
| EQ-04 | AC04: Seguridad | DA02 | ADR-003, ADR-005 |
| EQ-05 | AC05: Mantenibilidad | DA06 | ADR-001, ADR-002 |
| EQ-06 | AC06: Privacidad | DA03, DA09 | ADR-005, ADR-008 |
| EQ-07 | AC02: Disponibilidad | DA08, DA09 | ADR-007, ADR-008 |

## EQ-07. Disponibilidad: fallo de la API de Groq

| Elemento | Descripción |
|---|---|
| Atributo relacionado | AC02: Disponibilidad. |
| Fuente del estímulo | API de Groq o conexión con el proveedor. |
| Estímulo | Tiempo de espera agotado, cuota excedida o error del proveedor al atender al tutor. |
| Artefacto | AdaptadorGroq, caso de uso ConsultarTutorIA y controlador REST. |
| Entorno | Prueba controlada que simula errores 429, 5xx y ausencia de respuesta. |
| Respuesta | Cancelar la espera, mostrar un mensaje de indisponibilidad del tutor y registrar el incidente sin datos personales. Mantener lecciones, actividades y progreso disponibles. |
| Medida | Propuesta por validar: límite de espera al proveedor de 20 segundos y respuesta controlada al cliente en 25 segundos o menos en el 100 % de los casos de fallo definidos; cero escrituras de puntos o respuestas de actividades originadas por el fallo del tutor. |
| Verificación | Simular los fallos y comprobar tiempos, respuestas, registros y acceso a las funciones de aprendizaje. Medir la disponibilidad real del tutor por separado. |
| Estado | Pendiente de implementar y validar. Groq continúa siendo obligatorio para RF14. |
