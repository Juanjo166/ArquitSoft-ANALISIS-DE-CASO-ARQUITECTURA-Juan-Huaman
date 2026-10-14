# Actores del sistema APU GO

Los actores son las personas o sistemas externos que interactúan con APU GO.

| Actor | Tipo | ¿Qué necesita realizar? |
|---|---|---|
| Estudiante | Usuario principal | Acceder a lecciones de quechua, resolver actividades, consultar su progreso, obtener logros y consultar al tutor IA. |
| Docente | Usuario | Gestionar contenidos y actividades, y consultar el avance de sus estudiantes. |
| Administrador | Usuario | Gestionar usuarios y roles, supervisar contenidos y revisar los registros de auditoría. |
| Groq: API externa de IA | Sistema externo obligatorio | Proporcionar las respuestas del tutor educativo y apoyar las recomendaciones previstas mediante el adaptador del backend. |

## Alcance

La integración con Groq es obligatoria para el tutor IA de APU GO (RF14). Las recomendaciones de actividades se mantienen como parte de la arquitectura objetivo (RF07). Las reglas internas pueden validar contexto y resultados, pero no sustituyen la integración obligatoria. PostgreSQL, Redis y los módulos del backend no se consideran actores porque forman parte de la solución.
