# Arquitectura inicial de APU GO

## Descripción general

APU GO se organiza en tres capas: presentación, lógica de negocio y datos. La aplicación Flutter permite el acceso desde Web y Android. Se comunica mediante HTTPS con una API REST, que atiende las solicitudes y utiliza los módulos de un backend modular.

## Diagrama de arquitectura

```mermaid
flowchart TB
    subgraph ACTORES["ACTORES"]
        Estudiante["Estudiante"]
        Docente["Docente"]
        Administrador["Administrador"]
    end

    subgraph PRESENTACION["CAPA DE PRESENTACIÓN"]
        Flutter["Aplicación Flutter<br/>Web y Android"]
        API["API REST<br/>HTTPS"]
    end

    subgraph NEGOCIO["CAPA DE LÓGICA DE NEGOCIO · MONOLITO MODULAR"]
        Usuarios["Autenticación y usuarios"]
        Aprendizaje["Contenidos educativos<br/>Actividades y evaluaciones<br/>Progreso"]
        Motivacion["Gamificación<br/>Recomendaciones con IA<br/>Notificaciones"]
        Gestion["Reportes<br/>Auditoría"]
    end

    subgraph DATOS["CAPA DE DATOS"]
        Persistencia["Capa de persistencia común"]
        PostgreSQL["PostgreSQL"]
        Objetos["Almacenamiento de objetos<br/>Audios e imágenes"]
    end

    subgraph EXTERNOS["SERVICIOS EXTERNOS"]
        CDN["CDN"]
        IA["Servicio externo de IA<br/>(opcional)"]
    end

    Estudiante --> Flutter
    Docente --> Flutter
    Administrador --> Flutter
    Flutter --> API

    API --> Usuarios
    API --> Aprendizaje
    API --> Motivacion
    API --> Gestion

    Usuarios --> Persistencia
    Aprendizaje --> Persistencia
    Motivacion --> Persistencia
    Gestion --> Persistencia
    Persistencia --> PostgreSQL

    Aprendizaje --> Objetos
    Objetos --> CDN
    CDN --> Flutter
    Motivacion -. Integración opcional .-> IA
```

## Responsabilidades de las capas

| Capa | Responsabilidad | Componentes de APU GO |
|---|---|---|
| Presentación | Permitir la interacción con los usuarios y recibir las solicitudes. | Aplicación Flutter para Web y Android; entrada mediante API REST. |
| Lógica de negocio | Aplicar las reglas de aprendizaje y administrar las funciones del sistema. | Autenticación y usuarios, contenidos, actividades y evaluaciones, progreso, gamificación, recomendaciones, reportes, notificaciones y auditoría. |
| Datos | Almacenar y recuperar la información. | Capa de persistencia, PostgreSQL y almacenamiento de objetos para archivos multimedia. |

## Sistemas externos

- **CDN:** distribuye los audios, imágenes y recursos estáticos.
- **Servicio externo de IA:** puede apoyar las recomendaciones y la retroalimentación. Su integración es opcional y debe respetar la privacidad de los estudiantes.

## Decisiones iniciales

El backend se plantea como un monolito modular para separar responsabilidades y facilitar su mantenimiento por un desarrollador individual. Los módulos comparten una capa de persistencia. PostgreSQL guarda los datos transaccionales, mientras que los archivos multimedia se almacenan por separado.