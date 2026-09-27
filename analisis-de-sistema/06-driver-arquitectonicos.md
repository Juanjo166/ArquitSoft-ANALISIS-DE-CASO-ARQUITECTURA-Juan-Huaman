# Drivers arquitectónicos de APU GO

Un driver arquitectónico es una necesidad o condición que influye de manera importante en el diseño del sistema.

| ID | Driver arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
|---|---|---|---|
| DA01 | Soportar la concurrencia prevista y permitir el crecimiento. | AC01, AC03 | Requiere medir el rendimiento y prever réplicas del backend detrás de un balanceador de carga. |
| DA02 | Proteger el acceso según el rol del usuario. | AC04, RC07, RF01, RF12 | Requiere autenticación, autorización por roles y comunicaciones mediante HTTPS. |
| DA03 | Proteger los datos personales de los estudiantes. | AC06, RC08, RF07 | Condiciona los datos almacenados y la información que podría enviarse a un servicio externo de IA. |
| DA04 | Ofrecer el sistema en Web y Android. | RC01, RC02 | Determina el uso de un cliente Flutter con una base de código compartida. |
| DA05 | Separar la interfaz de la lógica del sistema. | RC03 | Determina la comunicación entre Flutter y el backend mediante una API REST. |
| DA06 | Gestionar datos y archivos multimedia de forma adecuada. | RC04, RC05, RF02, RF03 | Requiere PostgreSQL para los datos transaccionales y almacenamiento de objetos con CDN para audios e imágenes. |
| DA07 | Facilitar el mantenimiento por un desarrollador individual. | AC05, RC09 | Favorece un backend organizado como monolito modular, con responsabilidades separadas. |
| DA08 | Mantener la continuidad del servicio y recuperar los datos. | AC02 | Requiere monitoreo, alertas, respaldos y un procedimiento de restauración. |