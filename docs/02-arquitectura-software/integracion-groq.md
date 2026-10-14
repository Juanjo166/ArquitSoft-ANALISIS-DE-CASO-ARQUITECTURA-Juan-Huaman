# Integración obligatoria con Groq

## Alcance y trazabilidad

El tutor IA es indispensable en APU GO. HU13 y RF14 requieren la API de Groq, indicada en RC10 y DA09 y justificada por ADR-008. El módulo Tutor IA y recomendaciones sigue siendo uno de los nueve módulos; incluye ConsultarTutorIA y ObtenerRecomendaciones (RF07).

El tutor explica vocabulario y reglas del quechua chanka con contexto educativo validado, ofrece pistas y evita resolver evaluaciones por el estudiante. Las recomendaciones de actividades forman parte de la arquitectura objetivo y deben validarse contra el catálogo autorizado antes de presentarlas.

## Arquitectura objetivo

Flutter Web y Android consumen la API REST del backend modular. La API autentica y autoriza. ConsultarTutorIA obtiene contexto por interfaces públicas de contenidos y progreso, aplica FiltroDatosIA y utiliza ServicioTutorIA. AdaptadorGroq implementa el contrato mediante HTTPS. Las reglas de dominio no dependen del SDK ni del formato HTTP de Groq.

La clave GROQ_API_KEY se guarda en el servidor. El modelo se configura mediante GROQ_MODEL; su elección y disponibilidad deben comprobarse en el despliegue. La obligatoriedad de la integración no depende de fijar un modelo concreto en los documentos.

## Datos y privacidad

- Contexto permitido: lección, unidad, vocabulario validado y progreso mínimo útil para la consulta.
- Mensaje e historial: límites de longitud y turnos, filtrado de datos personales y validación en el backend.
- No enviar nombres, correos, documentos de identidad, identificadores de cuenta ni otros campos personales prohibidos.
- La identidad autenticada se utiliza dentro del backend y no se incorpora al payload de Groq.
- No registrar claves, mensajes originales ni contenido personal en logs técnicos.
- Las pruebas de privacidad usan casos definidos con datos sintéticos. El filtrado de texto libre requiere diseño y validación; no se presume resuelto por excluir email y uid del contexto.

## Fallos y continuidad

El adaptador maneja errores de cuota, autenticación del proveedor, respuesta inválida y tiempo agotado. Se proponen 20 segundos como límite de espera al proveedor y 25 segundos para devolver una respuesta controlada al cliente en las pruebas de EQ-07. Son metas pendientes de validar.

Ante un fallo se comunica la indisponibilidad temporal del tutor. Lecciones, actividades y progreso siguen funcionando. Los errores del tutor no otorgan puntos ni revierten transacciones de actividades ya confirmadas. La disponibilidad del tutor se mide por separado de las operaciones críticas de aprendizaje de EQ-02.

Las respuestas educativas deben contrastarse con contenidos validados y comunicar incertidumbre cuando proceda. La IA no sustituye la estrategia de evaluación de RF04.

## Adaptación del MVP

El [MVP apugo-quechua](https://github.com/Juanjo166/apugo-quechua) ya contiene ChatbotService en Flutter y un proxy de Vercel en api/chatbot.js. Su integración de Groq se reutilizará como referencia para el adaptador del backend objetivo.

| Situación del MVP | Adaptación prevista |
|---|---|
| Proxy serverless de Groq en Vercel | Encapsular la llamada en AdaptadorGroq y ServicioTutorIA dentro del backend modular. |
| Cliente con acceso a Firebase | Migrar progresivamente al acceso por API REST y a PostgreSQL según la propuesta. |
| Contexto e historial construidos en cliente | Validar la identidad, permisos y datos en el backend antes de llamar a Groq. |
| Endpoint de tutor sin verificación de identidad | Incorporar autenticación, autorización y límites de consumo verificables. |
| Tutor conversacional existente | Conservar RF14 y desarrollar las recomendaciones de RF07 como mejora objetivo. |

Esta tabla describe trabajo de adaptación pendiente, no cambios ya implementados en el MVP.
