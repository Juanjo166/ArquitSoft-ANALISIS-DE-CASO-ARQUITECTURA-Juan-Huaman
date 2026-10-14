# Patrones de diseño de APU GO

## Objetivo

Documentar los patrones propuestos para organizar el backend de APU GO, indicando dónde se utilizarán, qué problema resuelven y cómo respetan Clean Architecture.

## Contexto

- Estilo: cliente-servidor con backend de tipo monolito modular.
- Enfoque interno: Clean Architecture.
- Cliente: Flutter para Web y Android.
- Persistencia: PostgreSQL.
- Integración obligatoria: API de Groq para el tutor IA.
- Integración opcional: Redis, según mediciones.

Los patrones descritos son propuestas de diseño. Su implementación se verificará durante el desarrollo.

## Patrones propuestos

| Patrón | Dónde se utiliza | Problema que resuelve | Implementación propuesta | Elementos de código previstos |
|---|---|---|---|---|
| Repository | Contenidos, actividades y progreso → PostgreSQL | Evitar que los casos de uso dependan de SQL o de un ORM concreto. | Contratos de repositorio en aplicación e implementaciones en infraestructura. | RepositorioActividades, RepositorioProgreso, RepositorioProgresoPostgreSQL. |
| Adapter | Tutor IA y recomendaciones → Groq | Adaptar la API del proveedor al contrato interno y controlar los datos enviados. | Adaptador que transforma solicitudes y respuestas, excluyendo campos personales no permitidos. | ServicioTutorIA, ServicioRecomendaciones, AdaptadorGroq. |
| Strategy | Evaluación de respuestas | Aplicar reglas diferentes para actividades de selección y escritura sin concentrarlas en un único bloque de condiciones. | Una interfaz de evaluación con estrategias intercambiables según el tipo de actividad. | EstrategiaEvaluacion, EvaluacionSeleccion, EvaluacionEscritura. |
| Decorator | Consulta de contenidos → caché opcional | Agregar caché sin cambiar los casos de uso que consultan lecciones. | Un repositorio decorado consulta Redis y delega al repositorio original cuando no encuentra el dato. | RepositorioContenidosConCache. |

## Cómo funciona cada patrón

### Repository

El caso de uso RegistrarRespuesta utiliza un contrato para guardar la respuesta y actualizar el progreso.

La implementación PostgreSQL realiza las operaciones de persistencia. El caso de uso no necesita conocer las tablas ni las instrucciones SQL.

Las operaciones de respuesta, progreso y puntos deben coordinarse mediante una transacción cuando requieran guardarse de forma conjunta.

### Adapter

Los casos de uso ConsultarTutorIA (RF14) y ObtenerRecomendaciones (RF07) utilizan contratos internos de aplicación.

AdaptadorGroq es obligatorio: convierte los mensajes y el contexto educativo permitidos al formato de la API de Groq y transforma la respuesta al formato utilizado por APU GO. Las claves y el modelo se configuran en el servidor.

La integración debe excluir información identificable del estudiante y manejar de forma controlada los errores del proveedor.

### Strategy

El caso de uso de evaluación selecciona la estrategia correspondiente al tipo de actividad.

- EvaluacionSeleccion compara la opción enviada con la opción correcta.
- EvaluacionEscritura aplica las reglas de comparación y normalización definidas para el contenido educativo.

Las estrategias comparten un contrato y devuelven un resultado de evaluación común. Las reglas lingüísticas deben validarse con el responsable de los contenidos de quechua.

### Decorator

Si las pruebas justifican usar Redis, un repositorio de contenidos con caché envuelve al repositorio original.

1. Busca la lección en la caché.
2. Si existe una entrada vigente, devuelve el contenido.
3. Si no existe, consulta el repositorio original.
4. Guarda temporalmente el resultado y lo devuelve.

Cuando el docente modifica una lección, deben invalidarse las entradas afectadas. La caché no reemplaza a PostgreSQL como fuente de datos.

## Beneficios

- Separar las reglas de aprendizaje de los detalles de persistencia.
- Cambiar un proveedor externo mediante su adaptador.
- Incorporar nuevos tipos de actividades mediante estrategias de evaluación.
- Añadir o retirar caché sin modificar los casos de uso de consulta.
- Probar los casos de uso con implementaciones de prueba de sus contratos.

## Patrones previstos para siguientes iteraciones

| Patrón | Dónde se aplicaría | Condición para incorporarlo |
|---|---|---|
| Observer | Eventos de aprendizaje → notificaciones | Cuando varias acciones deban reaccionar a una actividad completada. Las notificaciones se emitirán después de confirmar la persistencia. |
| Factory | Creación de actividades | Cuando la construcción de distintos tipos de actividades requiera reglas suficientemente complejas. |
| Facade | Consulta de progreso y logros | Cuando sea conveniente ofrecer una interfaz estable que coordine consultas de varios módulos. |

No se incorporarán patrones adicionales sin una necesidad concreta.

## Relación con las decisiones arquitectónicas

| Patrón | Decisiones relacionadas |
|---|---|
| Repository | ADR-002: Clean Architecture. |
| Adapter | ADR-005: privacidad; ADR-008: contratos internos de tutoría y recomendaciones con Groq obligatorio. |
| Strategy | ADR-002: separación de reglas de negocio. |
| Decorator | ADR-004: caché incorporada según mediciones. |
