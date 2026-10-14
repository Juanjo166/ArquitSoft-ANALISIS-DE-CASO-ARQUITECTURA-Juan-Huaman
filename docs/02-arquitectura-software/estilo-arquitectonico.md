# Estilo arquitectónico de APU GO

## Estilo seleccionado

APU GO seguirá un estilo **cliente-servidor**, con una aplicación Flutter para Web y Android y un **backend de tipo monolito modular**. El cliente se comunicará con el backend mediante una API REST sobre HTTPS.

El backend se desplegará inicialmente como una sola aplicación. Sus funcionalidades estarán separadas en módulos con responsabilidades definidas. Si las pruebas y métricas muestran saturación, se podrán agregar réplicas del backend detrás de un balanceador de carga.

## Componentes principales

| Componente | Responsabilidad |
|---|---|
| Flutter Web y Android | Presentar lecciones, actividades, progreso y funciones según el rol del usuario. |
| API REST | Recibir solicitudes del cliente y dirigirlas a las funciones correspondientes. |
| Backend modular | Aplicar las reglas de usuarios, aprendizaje, gamificación, tutor IA y recomendaciones, reportes y auditoría. |
| PostgreSQL | Almacenar los datos transaccionales. |
| Almacenamiento de objetos y CDN | Guardar y distribuir audios, imágenes y otros recursos multimedia. |
| Redis | Reducir consultas repetitivas cuando las pruebas de rendimiento justifiquen incorporarlo. |
| Groq: API externa de IA obligatoria | Proporcionar asistencia contextualizada al tutor (RF14) y apoyar las recomendaciones previstas (RF07). |

## Diagrama del estilo arquitectónico

```mermaid
flowchart LR
    Usuarios["Estudiantes, docentes y administradores"]
    Flutter["Cliente Flutter<br/>Web y Android"]
    API["API REST<br/>Backend APU GO"]

    subgraph Backend["Monolito modular"]
        Aprendizaje["Contenidos y actividades"]
        Progreso["Progreso y gamificación"]
        Gestion["Usuarios, reportes y auditoría"]
        Recomendaciones["Tutor IA y recomendaciones"]
    end

    PostgreSQL[("PostgreSQL")]
    Objetos["Almacenamiento de objetos"]
    CDN["CDN"]
    Redis[("Redis<br/>según demanda")]
    IA["Groq: API externa de IA<br/>obligatoria"]

    Usuarios --> Flutter
    Flutter -->|"HTTPS / REST"| API
    API --> Aprendizaje
    API --> Progreso
    API --> Gestion
    API --> Recomendaciones

    Aprendizaje --> PostgreSQL
    Progreso --> PostgreSQL
    Gestion --> PostgreSQL
    Recomendaciones --> PostgreSQL

    Aprendizaje --> Objetos
    Objetos --> CDN
    CDN --> Flutter
    API -. "Caché según demanda" .-> Redis
    Recomendaciones -->|"HTTPS: tutor IA"| IA
```

## Diagrama del estilo arquitectónico

![Estilo arquitectónico de APU GO](../../img/Estilo_Arquitectonico.png)

*Figura 1. Estilo arquitectónico de APU GO. Fuente: elaboración propia.*

## Justificación

El monolito modular permite construir y desplegar una sola aplicación backend, manteniendo separadas las responsabilidades de cada módulo. Es una opción viable para el desarrollo individual de APU GO y responde al driver DA06 de mantenibilidad. La comunicación mediante API REST permite que los clientes Web y Android utilicen los mismos servicios.

La organización modular y la ausencia de sesiones locales en cada instancia permiten plantear réplicas del backend cuando la demanda lo requiera. Esta capacidad deberá comprobarse con pruebas de carga; no se considera validada solo por el diseño.

## Relación con las decisiones

- **ADR-001:** monolito modular.
- **ADR-003:** comunicación entre Flutter y el backend mediante API REST.
- **ADR-004:** caché incorporada según mediciones.
- **ADR-006:** almacenamiento multimedia separado de PostgreSQL.

## Dependencia obligatoria de Groq

El backend consume Groq mediante HTTPS, protege la clave en su entorno y controla tiempos de espera y cuotas. El módulo Tutor IA y recomendaciones responde a RF14 y RF07. La recuperación ante fallos y la privacidad se verifican en EQ-07 y EQ-06. Esta organización describe la arquitectura objetivo a la que se adaptará el MVP.
