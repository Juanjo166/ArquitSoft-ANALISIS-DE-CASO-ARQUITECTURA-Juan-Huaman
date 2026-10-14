# Requisitos funcionales de APU GO

Los requisitos funcionales describen las acciones que debe realizar el sistema para atender las historias de usuario.

| ID | Requisito funcional |
|---|---|
| RF01 | El sistema debe permitir que los usuarios inicien sesión según su rol. |
| RF02 | El sistema debe permitir consultar lecciones y recursos de aprendizaje del quechua. |
| RF03 | El sistema debe presentar actividades y registrar las respuestas de los estudiantes. |
| RF04 | El sistema debe evaluar las respuestas y mostrar los resultados de las actividades. |
| RF05 | El sistema debe registrar y mostrar el progreso de aprendizaje de cada estudiante. |
| RF06 | El sistema debe asignar y mostrar puntos y logros según las actividades completadas. |
| RF07 | El sistema debe recomendar actividades de refuerzo según el progreso del estudiante, utilizando el módulo Tutor IA y recomendaciones integrado con Groq. |
| RF08 | El sistema debe generar y mostrar avisos relacionados con las actividades de aprendizaje. |
| RF09 | El sistema debe permitir al docente crear, actualizar y organizar lecciones y recursos. |
| RF10 | El sistema debe permitir al docente crear y gestionar actividades y evaluaciones. |
| RF11 | El sistema debe generar reportes de progreso para los docentes. |
| RF12 | El sistema debe permitir al administrador gestionar usuarios y asignar roles. |
| RF13 | El sistema debe registrar y permitir consultar acciones administrativas relevantes. |
| RF14 | El sistema debe ofrecer un tutor conversacional de quechua chanka mediante la API de Groq, con contexto de aprendizaje autorizado y explicaciones o pistas sin sustituir la evaluación de actividades. |

## Relación entre historias de usuario y requisitos

| Historia de usuario | Requisitos relacionados |
|---|---|
| HU01: Iniciar sesión | RF01 |
| HU02: Consultar lecciones | RF02 |
| HU03: Resolver actividades y evaluaciones | RF03, RF04 |
| HU04: Consultar progreso | RF05 |
| HU05: Obtener puntos y logros | RF06 |
| HU06: Recibir recomendaciones | RF07 |
| HU07: Recibir avisos | RF08 |
| HU08: Gestionar lecciones y recursos | RF09 |
| HU09: Gestionar actividades y evaluaciones | RF10 |
| HU10: Consultar reportes | RF11 |
| HU11: Gestionar usuarios y roles | RF12 |
| HU12: Consultar auditoría | RF13 |
| HU13: Consultar dudas con el tutor IA | RF14 |

## Alcance de la inteligencia artificial

RF14 es indispensable para la solución. RF07 corresponde a las recomendaciones de refuerzo previstas en la arquitectura objetivo y se diferencia del tutor conversacional ya presente en el MVP.

Las solicitudes pasan por el backend, que autentica al estudiante, filtra el contexto y conserva la clave de Groq en el servidor. Si Groq no responde, informa la indisponibilidad del tutor y conserva disponibles lecciones, actividades y progreso. La evaluación y la asignación de puntos mantienen sus reglas de dominio.
