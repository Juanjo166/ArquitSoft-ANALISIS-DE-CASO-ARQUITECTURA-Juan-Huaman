# Actores del sistema APU GO

Los actores son las personas o sistemas externos que interactúan con APU GO.

| Actor | Tipo | ¿Qué necesita realizar? |
|---|---|---|
| Estudiante | Usuario principal | Acceder a lecciones de quechua, resolver actividades, consultar su progreso y obtener logros. |
| Docente | Usuario | Gestionar contenidos y actividades, y consultar el avance de sus estudiantes. |
| Administrador | Usuario | Gestionar usuarios y roles, supervisar contenidos y revisar los registros de auditoría. |
| Servicio externo de IA | Sistema externo opcional | Proporcionar recomendaciones o retroalimentación cuando se configure una integración externa. |

## Alcance

El servicio externo de IA es opcional: APU GO también puede iniciar con recomendaciones basadas en reglas internas. PostgreSQL, Redis y los módulos del backend no se consideran actores porque forman parte de la solución.