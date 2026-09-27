# Atributos de calidad de APU GO

Los atributos de calidad describen cómo debe comportarse APU GO, además de las funciones que ofrece.

| ID | Atributo | Escenario de calidad |
|---|---|---|
| AC01 | Rendimiento | Cuando hasta 200 usuarios utilicen lecciones y actividades al mismo tiempo, la plataforma debe mantener tiempos de respuesta adecuados. Se realizarán pruebas con picos de hasta 300 usuarios concurrentes para identificar límites y cuellos de botella. |
| AC02 | Disponibilidad | Durante una clase o evaluación, los estudiantes y docentes deben poder acceder a la plataforma. El monitoreo debe ayudar a detectar fallos y los respaldos deben permitir recuperar los datos. |
| AC03 | Escalabilidad | Si aumenta la demanda y las métricas muestran saturación, se deben poder agregar réplicas del backend detrás de un balanceador sin modificar el cliente Flutter. |
| AC04 | Seguridad | Al iniciar sesión o consultar información, cada usuario debe acceder únicamente a las funciones permitidas por su rol. Las comunicaciones deben utilizar HTTPS y las contraseñas deben almacenarse de forma segura. |
| AC05 | Mantenibilidad | Al modificar una función, por ejemplo la gamificación, los módulos separados del backend deben facilitar el cambio sin afectar innecesariamente contenidos, actividades o reportes. |
| AC06 | Privacidad | Al tratar datos de estudiantes, especialmente menores de edad, el sistema debe limitar la información recolectada y evitar enviar datos identificables a servicios externos de IA. |