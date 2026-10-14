# APU GO: aprendizaje gamificado del quechua

## Integrante
Juan Josue Huamán Soto

## Descripción
APU GO es una propuesta de aplicación multiplataforma para apoyar el aprendizaje del idioma quechua mediante lecciones, actividades, gamificación y un tutor de inteligencia artificial integrado obligatoriamente con la API de Groq.

## Caso de estudio
Análisis del sistema y diseño inicial de la arquitectura de APU GO, desarrollado a partir del proyecto final del curso.

## Curso
Arquitectura de Software (IS-488) — Semestre 2026-II

## Organización del repositorio

| Carpeta | Contenido |
|---|---|
| docs/01-analisis-de-sistema/ | Necesidad, actores, historias de usuario, requisitos, atributos de calidad, restricciones, drivers, decisiones y escenarios medibles. |
| docs/02-arquitectura-software/ | Arquitectura inicial, estilo, enfoque Clean Architecture y componentes. |
| docs/03-diseño-de-software/diseño-interno/ | Patrones, principios SOLID y diseño interno del módulo de actividades. |
| docs/04-modelo-c4/ | Contexto, contenedores, componentes y código. |
| img/ | Diagramas exportados como PNG. |
| img/fuentes-svg/ | Fuentes editables de los diagramas. |
| tecnologia/ | Ejemplo de código para ilustrar Clean Architecture. |

## Alcance del primer sprint

El primer sprint documenta el análisis, la arquitectura y el diseño
propuesto de APU GO.

Las metas de calidad están pendientes de validación. Los ejemplos
tecnológicos ilustran la organización del código y no constituyen
la aplicación completa.

## Arquitectura objetivo y punto de partida

Este repositorio documenta la **arquitectura objetivo** del Sprint 1: Flutter Web y Android, API REST, backend de nueve módulos con Clean Architecture, PostgreSQL y almacenamiento multimedia separado. El [MVP existente](https://github.com/Juanjo166/apugo-quechua) utiliza Flutter, Firebase y un proxy serverless de Vercel para su tutor con Groq; se adaptará progresivamente a esta propuesta.

**Groq es una integración obligatoria**, encapsulada mediante un contrato de aplicación y un adaptador de infraestructura. El módulo **Tutor IA y recomendaciones** comprende la asistencia conversacional (RF14) y las recomendaciones de refuerzo previstas (RF07). No constituye un décimo módulo.

La indisponibilidad temporal de Groq debe producir un error controlado del tutor, conservar el aprendizaje disponible y registrarse en monitoreo. La integración de Groq sigue siendo obligatoria.

Redis conserva su carácter opcional, condicionado por las mediciones.

La especificación del tutor y su migración están en [Integración obligatoria con Groq](docs/02-arquitectura-software/integracion-groq.md).
