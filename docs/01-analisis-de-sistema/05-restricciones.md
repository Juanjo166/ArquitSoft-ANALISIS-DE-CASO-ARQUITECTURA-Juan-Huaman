# Restricciones de APU GO

Las restricciones son condiciones que deben respetarse al diseñar e implementar la solución.

| ID | Restricción | Descripción |
|---|---|---|
| RC01 | Plataforma multiplataforma | APU GO debe ofrecer acceso desde Web y Android. |
| RC02 | Cliente Flutter | La aplicación cliente se desarrollará con Flutter y Dart, utilizando una misma base de código. |
| RC03 | Comunicación mediante API REST | El cliente debe comunicarse con el backend mediante una API REST. |
| RC04 | Base de datos PostgreSQL | La información transaccional, como usuarios, actividades, respuestas y progreso, se almacenará en PostgreSQL. |
| RC05 | Recursos multimedia separados | Los audios e imágenes deben almacenarse fuera de las tablas de PostgreSQL y distribuirse mediante una CDN. |
| RC06 | Control de versiones | Los documentos y el código del proyecto deben gestionarse con Git y publicarse en GitHub. |
| RC07 | Comunicación y acceso seguros | Las comunicaciones deben usar HTTPS y las funciones deben limitarse según el rol del usuario. |
| RC08 | Protección de datos | Se debe minimizar la recopilación de datos personales y evitar el envío de información identificable de estudiantes a servicios externos de IA. |
| RC09 | Desarrollo individual | La propuesta arquitectónica debe ser viable para su implementación y mantenimiento por un solo desarrollador. |