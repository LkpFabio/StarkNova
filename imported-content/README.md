# ERS\_IEEE 830\_JARVIS\_

## FASE DE ANÁLISIS IEEE 830

**Juan Arias**\
**Juan Bustamente**\
**Nicolas Muñoz**\
**Fabio Morales**

**Universidad Santiago de Cali**\
**Área de Administración de la Información**\
**Santiago de Cali**\
**2026**

### Ficha del documento

| **Fecha**  | **Revisión** | **Autor**                                                         | **Verificado**   |
| ---------- | ------------ | ----------------------------------------------------------------- | ---------------- |
| 13/10/2026 | 1.0          | Fabio Morales (líder), Juan Arias, Juan Bustamante, Nicolás Muñoz | Jair Sanclemente |

## 1 Introducción

En este documento se describe detalladamente la Especificación de Requisitos Software (ERS) del proyecto J.A.R.V.I.S., un asistente virtual inteligente y autónomo para equipos de escritorio que combina un modelo de lenguaje con un conjunto de herramientas para controlar el equipo, gestionar información personal y comunicarse por distintos canales.

Esta especificación se ha estructurado basándose en las directrices dadas por el estándar IEEE Práctica Recomendada para Especificaciones de Requisitos Software ANSI/IEEE 830, 1998.

### 1.1 Propósito

El propósito de este documento es definir las especificaciones funcionales y no funcionales del sistema J.A.R.V.I.S., un asistente personal que se ejecuta en el equipo del usuario y le permite, mediante texto o voz, obtener información, automatizar tareas de varios pasos, controlar el PC y gestionar su correo, calendario y mensajería. Va dirigido al equipo de desarrollo, a los evaluadores del proyecto y al usuario propietario del sistema.

### 1.2 Alcance

El sistema comprende un servidor local (Python y FastAPI), una interfaz web (Next.js), un agente autónomo con planificador, ejecutor y cola de tareas, un conjunto de acciones sobre el equipo y servicios externos, subsistemas de memoria y de conciencia del contexto, y canales de acceso remoto por Telegram y WhatsApp.

Quedan fuera del alcance de esta versión: el uso por varios usuarios simultáneos con perfiles distintos, el acceso desde otros equipos de la red o desde internet, la ejecución de comandos de terminal sin autorización explícita, la transcripción de audios recibidos por WhatsApp y la administración de cuentas de Google Workspace. Esta especificación servirá de base para el mejoramiento continuo del sistema en próximas versiones.

### 1.3 Personal involucrado

El proyecto lo desarrolla un equipo de cuatro integrantes. Sus roles y responsabilidades son los siguientes:

| **Nombre**                  | Fabio Morales                                                                                                                                                                                                                                                                                      |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol**                     | Líder del proyecto y arquitecto de software                                                                                                                                                                                                                                                        |
| **Categoría profesional**   | Estudiante de Tecnologia en Desarrollo de Sistemas de Informacion y de Software                                                                                                                                                                                                                    |
| **Responsabilidad**         | Planificar y coordinar el proyecto, repartir el trabajo y hacer seguimiento de los entregables; definir la arquitectura del sistema (núcleo, agente autónomo, cola de tareas y motor de conciencia); integrar los módulos, revisar el código y validar que el producto cumple esta especificación. |
| **Información de contacto** | fabio.morales00@usc.edu.co                                                                                                                                                                                                                                                                         |

| **Nombre**                  | Juan Arias                                                                                                                                                                                                                                                              |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol**                     | analista de datos, desarrollador fullstack e integraciones                                                                                                                                                                                                              |
| **Categoría profesional**   | Estudiante de Tecnologia en Desarrollo de Sistemas de Informacion y de Software                                                                                                                                                                                         |
| **Responsabilidad**         | Desarrollar el servidor y la API (FastAPI), la base de datos y sus migraciones; implementar la autenticación, la verificación en dos pasos y los límites de uso; integrar los servicios externos: Groq, Google Workspace, Gmail, WhatsApp (Wazend), Telegram y Spotify. |
| **Información de contacto** | juan.arias23@usc.edu.co                                                                                                                                                                                                                                                 |

| **Nombre**                  | Juan Bustamante                                                                                                                                                                                                                               |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rol**                     | Desarrollador fullstack y experiencia de usuario                                                                                                                                                                                              |
| **Categoría profesional**   | Estudiante de Tecnologia en Desarrollo de Sistemas de Informacion y de Software                                                                                                                                                               |
| **Responsabilidad**         | Diseñar y desarrollar la interfaz web (Next.js): asistente, tareas, consola, reactor y ajustes; implementar la interacción por voz (reconocimiento y síntesis); cuidar la usabilidad, la accesibilidad y la adaptación a distintas pantallas. |
| **Información de contacto** | juan.bustamante05@usc.edu.co                                                                                                                                                                                                                  |

| **Nombre**                  | Nicolás Muñoz                                                                                                                                                                                                                                    |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Rol**                     | Analista de requisitos y tester                                                                                                                                                                                                                  |
| **Categoría profesional**   | Estudiante de Tecnologia en Desarrollo de Sistemas de Informacion y de Software                                                                                                                                                                  |
| **Responsabilidad**         | Levantar y analizar los requisitos; diseñar y ejecutar las pruebas automatizadas y la verificación de requisitos; registrar y dar seguimiento a los errores encontrados; elaborar y mantener la documentación (ERS, manual de usuario y README). |
| **Información de contacto** | nicolas.munoz02@usc.edu.co                                                                                                                                                                                                                       |

### 1.4 Definiciones, acrónimos y abreviaturas

| **Nombre**          | **Descripción**                                                                                                                                      |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Usuario             | Persona propietaria del equipo que usa y administra el sistema                                                                                       |
| ERS                 | Especificación de Requisitos Software                                                                                                                |
| RF                  | Requerimiento Funcional                                                                                                                              |
| RNF                 | Requerimiento No Funcional                                                                                                                           |
| IA                  | Inteligencia Artificial                                                                                                                              |
| LLM                 | Modelo de lenguaje de gran tamaño (Large Language Model) que interpreta las peticiones y genera las respuestas                                       |
| Herramienta         | Función del sistema que el modelo de lenguaje puede invocar para actuar (buscar, abrir una aplicación, enviar un correo, etc.)                       |
| Agente autónomo     | Componente que planifica y ejecuta por sí mismo los pasos necesarios para cumplir un objetivo                                                        |
| Motor de conciencia | Subsistema que observa el estado del equipo y del usuario para dar contexto al asistente                                                             |
| API                 | Interfaz de programación de aplicaciones                                                                                                             |
| REST                | Estilo de API basado en peticiones HTTP                                                                                                              |
| WebSocket           | Protocolo de comunicación bidireccional en tiempo real entre la interfaz y el servidor                                                               |
| JWT                 | JSON Web Token: credencial firmada que identifica la sesión del usuario                                                                              |
| OAuth               | Protocolo de autorización usado para acceder a las cuentas de Google                                                                                 |
| 2FA                 | Verificación en dos pasos: además de la contraseña se pide un segundo factor (un código del móvil)                                                   |
| TOTP                | Contraseña de un solo uso basada en el tiempo (RFC 6238): código de 6 dígitos que genera una app como Google Authenticator y cambia cada 30 segundos |
| Código de respaldo  | Código de un solo uso que sustituye al de la app si el usuario pierde el móvil                                                                       |
| TTS / STT           | Texto a voz (Text To Speech) / Voz a texto (Speech To Text)                                                                                          |
| Groq                | Servicio en la nube que ejecuta los modelos de lenguaje utilizados                                                                                   |
| Wazend              | Servicio externo que conecta una cuenta de WhatsApp mediante API                                                                                     |
| Playwright          | Biblioteca para automatizar navegadores web                                                                                                          |

### 1.5 Referencias

| **Título del documento**                                                     | **Referencia**                     |
| ---------------------------------------------------------------------------- | ---------------------------------- |
| Standard IEEE 830 - 1998                                                     | IEEE                               |
| README del proyecto J.A.R.V.I.S.                                             | Documentación interna del proyecto |
| Documentación de FastAPI                                                     | fastapi.tiangolo.com               |
| Documentación de Next.js                                                     | nextjs.org/docs                    |
| Documentación de la API de Groq                                              | console.groq.com/docs              |
| Google Workspace APIs (Gmail, Drive, Docs, Sheets, Slides, Calendar, People) | developers.google.com              |
| Documentación de Playwright para Python                                      | playwright.dev/python              |
| RFC 6238 - TOTP: Time-Based One-Time Password Algorithm                      | IETF                               |

### 1.6 Resumen

Este documento se compone de tres secciones. En la primera sección se realiza una introducción al mismo y se proporciona una visión general de la especificación de requisitos del sistema.

En la segunda sección se realiza una descripción general del sistema, con el fin de conocer las principales funciones que debe realizar, los usuarios a los que va dirigido y los factores, restricciones, supuestos y dependencias que afectan a su desarrollo, sin detallar de manera excesiva los diferentes aspectos del mismo.

En la tercera sección se definen los distintos requisitos funcionales y no funcionales que debe cumplir el sistema, así como los requisitos de sus interfaces.

## 2 Descripción general

### 2.1 Perspectiva del producto

J.A.R.V.I.S. es un sistema independiente que se instala y ejecuta en el propio equipo del usuario. Funciona bajo un modelo cliente/servidor local: un servidor en Python atiende únicamente peticiones del propio equipo y la interfaz web se usa desde el navegador del mismo equipo. El razonamiento del asistente lo aporta un modelo de lenguaje alojado en Groq, y el sistema se integra con servicios externos (Google, Spotify, Telegram, WhatsApp mediante Wazend). Su arquitectura se organiza en los siguientes módulos:

| **Módulo**           | **Responsabilidad**                                                                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Núcleo (core)        | Base de datos, bus de eventos en tiempo real, confirmaciones, caché y arranque de los subsistemas                                                            |
| Agente (agent)       | Planificador, ejecutor, cola de tareas en segundo plano e historial de tareas                                                                                |
| Acciones (tools)     | Herramientas: control del PC, ajustes del sistema, navegador, búsqueda, documentos, presentaciones, correo, Workspace, multimedia, programación y mensajería |
| API (api)            | Autenticación, límites de uso, chat, tareas, estado y WebSocket                                                                                              |
| Interfaz web (web)   | Aplicación Next.js: inicio de sesión, Asistente, Tareas, Consola, Reactor, Gmail, Calendario y Programación                                                  |
| Memoria y conciencia | Memoria a largo plazo, perfil, historial de conversaciones y motor de conciencia del contexto operativo                                                      |
| Canales remotos      | Bot de Telegram y puente de WhatsApp (Wazend)                                                                                                                |

### 2.2 Funcionalidad del producto

A grandes rasgos, el sistema ofrece las siguientes funciones:

### 2.3 Características de los usuarios

| **Tipo de usuario** | Usuario propietario                                                                                                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Formación**       | No requiere formación técnica para el uso diario; conocimientos básicos de informática para la instalación                                                                                                                |
| **Actividades**     | Conversar con el asistente, lanzar y supervisar tareas, confirmar acciones, gestionar memoria, perfil y conversaciones, configurar las integraciones y la seguridad de su cuenta (contraseña y verificación en dos pasos) |

| **Tipo de usuario** | Usuario remoto autorizado                                                                                               |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Formación**       | N/A                                                                                                                     |
| **Actividades**     | Enviar peticiones al asistente desde Telegram o WhatsApp con un identificador previamente autorizado por el propietario |

| **Tipo de usuario** | Desarrollador / mantenedor                                                      |
| ------------------- | ------------------------------------------------------------------------------- |
| **Formación**       | Ingeniero de sistemas o técnico en desarrollo de software (Python y TypeScript) |
| **Actividades**     | Instalar, actualizar, ampliar y dar mantenimiento al sistema y a sus pruebas    |

### 2.4 Restricciones

* El servidor solo atiende peticiones desde el propio equipo; no se publica en la red ni en internet.
* Lenguajes y tecnologías: Python 3.10+ (FastAPI, SQLAlchemy, Alembic, Playwright), TypeScript con Next.js y React, SQLite.
* Las funciones de IA dependen de una clave de Groq y de los límites de uso de su plan.
* Las integraciones de Google requieren autorización OAuth de la cuenta del usuario; WhatsApp requiere una sesión activa en Wazend.
* El reconocimiento de voz requiere un navegador compatible (Google Chrome o Microsoft Edge).
* El control del equipo y los ajustes del sistema están diseñados para Windows 10/11.
* El sistema se diseña según un modelo cliente/servidor y para un único usuario propietario.

### 2.5 Suposiciones y dependencias

* Se asume que los requisitos aquí descritos son estables.
* El equipo dispone de conexión a internet para los servicios de IA, búsqueda e integraciones.
* Los servicios externos (Groq, Google, Wazend, Telegram, Spotify) mantienen disponibles sus API y condiciones de uso.
* El usuario mantiene el equipo encendido y el sistema en ejecución para atender peticiones remotas.
* Los equipos en los que se ejecute el sistema deben cumplir los requisitos indicados para garantizar su correcto funcionamiento.

## 3 Requisitos específicos

### Requerimientos funcionales

#### RF01 — Autenticación de usuarios

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El acceso a la interfaz web y a la API exige un usuario con contraseña y, opcionalmente, verificación en dos pasos.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **Descripción del requerimiento** | En el primer arranque el sistema solicita crear el usuario propietario (usuario de 3 a 40 caracteres y contraseña de mínimo 8). Después, toda petición a la API y a los archivos requiere una sesión iniciada. El usuario puede cerrar sesión y cambiar su contraseña desde Ajustes → Seguridad; al cambiarla, las demás sesiones abiertas quedan invalidadas. Desde esa misma sección puede activar la verificación en dos pasos escaneando un código QR con Google Authenticator (u otra app compatible con TOTP): a partir de ese momento el inicio de sesión exige además el código de 6 dígitos de la app, y el sistema entrega 8 códigos de respaldo de un solo uso. Si el usuario olvida la contraseña, puede restablecerla ejecutando el script reset\_password.py en el equipo, que exige un código de la app o de respaldo; sin la verificación en dos pasos activa, el script no permite cambiarla. |
| **Requerimiento NO funcional**    | RNF05RNF06RNF07                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

#### RF02 — Conversación con el asistente

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                         |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Chat de texto con respuestas generadas por un modelo de lenguaje y uso de herramientas.                                                                                                                                                                                                                             |
| **Descripción del requerimiento** | El usuario escribe mensajes y JARVIS responde en español, con frases breves pensadas para ser leídas en voz alta. Para responder, el asistente puede invocar herramientas (búsqueda web, control del PC, correo, etc.). Durante el procesamiento la interfaz muestra en tiempo real qué herramienta se está usando. |
| **Requerimiento NO funcional**    | RNF01RNF04RNF08                                                                                                                                                                                                                                                                                                     |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                |

#### RF03 — Historial de conversaciones

| **Campo**                         | **Detalle**                                                                                                                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Gestión de las conversaciones guardadas.                                                                                                                                                        |
| **Descripción del requerimiento** | El sistema conserva cada conversación con su título y mensajes. El usuario puede abrir una conversación anterior, crear una nueva, borrar una conversación o borrar todas, previa confirmación. |
| **Requerimiento NO funcional**    | RNF01RNF07                                                                                                                                                                                      |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                           |

#### RF04 — Interacción por voz

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Reconocimiento de voz con palabra de activación y respuesta hablada.                                                                                                                                                                                                                                                                                                             |
| **Descripción del requerimiento** | Con el micrófono activo, el sistema escucha y reacciona cuando el usuario dice «Jarvis» (palabra de activación configurable). Tras una pregunta del asistente, el usuario puede responder durante 15 segundos sin repetir la palabra. Las respuestas se leen con voz sintetizada; el usuario elige voz, velocidad, tono e idioma de escucha, y puede interrumpir la voz con Esc. |
| **Requerimiento NO funcional**    | RNF01RNF09                                                                                                                                                                                                                                                                                                                                                                       |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                                                                                                                                            |

#### RF05 — Memoria a largo plazo y perfil

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                    |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Recordar datos duraderos del usuario.                                                                                                                                                                                                                                                          |
| **Descripción del requerimiento** | El asistente guarda automáticamente datos duraderos que el usuario comparte (gustos, personas, proyectos, rutinas) y los consulta antes de responder. El usuario puede ver y borrar los recuerdos, y editar su perfil (nombre, forma de trato, ciudad, ocupación, idiomas, intereses y notas). |
| **Requerimiento NO funcional**    | RNF07                                                                                                                                                                                                                                                                                          |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                                                          |

#### RF06 — Tareas autónomas en segundo plano

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Descomposición de un objetivo en pasos y ejecución autónoma.                                                                                                                                                                                                                                                                                                                                                           |
| **Descripción del requerimiento** | El usuario describe un objetivo (desde la sección Tareas, la consola o el chat). Un planificador lo divide en un máximo de 8 pasos; un ejecutor realiza cada paso con las herramientas, reintenta un paso fallido y, si vuelve a fallar, solicita un plan alternativo. Al terminar se genera un resumen. El usuario puede seguir el avance, cancelar, repetir o borrar tareas, que quedan registradas en un historial. |
| **Requerimiento NO funcional**    | RNF04RNF08RNF10                                                                                                                                                                                                                                                                                                                                                                                                        |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                                                                                                                   |

#### RF07 — Confirmación de acciones sensibles

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Ninguna acción con efectos externos se realiza sin aprobación explícita.                                                                                                                                                                                                                                                                                                                                                          |
| **Descripción del requerimiento** | Enviar correos o mensajes, modificar documentos o eventos de Google, aplicar cambios de código, pulsar botones de enviar/comprar/borrar en el navegador y apagar o reiniciar el equipo se preparan primero y se muestran al usuario. Solo se ejecutan cuando responde «Confirmar» (o un sinónimo como «sí»); con «Cancelar» se descartan y caducan a los 10 minutos. En las tareas autónomas la tarea se pausa hasta la decisión. |
| **Requerimiento NO funcional**    | RNF05RNF07RNF10                                                                                                                                                                                                                                                                                                                                                                                                                   |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                                                                                                                              |

#### RF08 — Control del equipo

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                     |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Manejo de aplicaciones y periféricos del PC.                                                                                                                                                                                                                                    |
| **Descripción del requerimiento** | El sistema puede abrir aplicaciones por su nombre (buscándolas en el menú Inicio), cerrar programas, abrir direcciones web, ajustar el volumen, controlar la reproducción multimedia, escribir texto, pulsar atajos de teclado, tomar capturas de pantalla y explorar carpetas. |
| **Requerimiento NO funcional**    | RNF05RNF07                                                                                                                                                                                                                                                                      |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                            |

#### RF09 — Ajustes del sistema operativo

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                          |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Modificación de configuraciones de Windows.                                                                                                                                                                                                                          |
| **Descripción del requerimiento** | El sistema puede ajustar el brillo de la pantalla, cambiar entre tema claro y oscuro, abrir páginas concretas de Configuración, informar del estado de la red y suspender, reiniciar, apagar o cerrar la sesión del equipo (estas últimas siempre con confirmación). |
| **Requerimiento NO funcional**    | RNF05RNF09                                                                                                                                                                                                                                                           |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                                |

#### RF10 — Navegador controlado

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                           |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Automatización de páginas web en una ventana propia.                                                                                                                                                                                                                                                                                  |
| **Descripción del requerimiento** | El asistente abre una ventana de Microsoft Edge con un perfil independiente, navega, lee el contenido y los controles de la página, hace clic, rellena campos, retrocede, toma capturas y cierra el navegador. Nunca escribe contraseñas ni datos de pago, y los clics que envían, compran, publican o borran requieren confirmación. |
| **Requerimiento NO funcional**    | RNF05RNF07                                                                                                                                                                                                                                                                                                                            |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                                                                                                 |

#### RF11 — Búsqueda web e investigación profunda

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                              |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Consulta de información actualizada en internet.                                                                                                                                                                                                         |
| **Descripción del requerimiento** | El sistema realiza búsquedas web, lee páginas y consulta el clima. En la investigación profunda genera varias búsquedas complementarias, lee las mejores fuentes y redacta un informe con citas numeradas y lista de fuentes, que guarda como documento. |
| **Requerimiento NO funcional**    | RNF04RNF07                                                                                                                                                                                                                                               |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                     |

#### RF12 — Generación de documentos y presentaciones

| **Campo**                         | **Detalle**                                                                                                                                                                                                                 |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Creación de archivos a partir de las peticiones del usuario.                                                                                                                                                                |
| **Descripción del requerimiento** | El sistema genera documentos PDF, DOCX o texto plano y presentaciones PowerPoint (.pptx) con portada, diapositivas de viñetas y notas del orador. Los archivos quedan disponibles en la interfaz y pueden abrirse en el PC. |
| **Requerimiento NO funcional**    | RNF01                                                                                                                                                                                                                       |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                       |

#### RF13 — Archivos adjuntos

| **Campo**                         | **Detalle**                                                                                                                                                                             |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Carga de archivos para que el asistente los analice.                                                                                                                                    |
| **Descripción del requerimiento** | El usuario puede adjuntar archivos de hasta 50 MB (documentos, hojas de cálculo, imágenes). El asistente lee su contenido o describe las imágenes para responder preguntas sobre ellos. |
| **Requerimiento NO funcional**    | RNF05RNF07                                                                                                                                                                              |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                   |

#### RF14 — Integración con Gmail

| **Campo**                         | **Detalle**                                                                                                                                                                                                                               |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Lectura y envío de correo electrónico.                                                                                                                                                                                                    |
| **Descripción del requerimiento** | Tras autorizar la cuenta mediante OAuth, el sistema busca, lee y resume correos, y muestra la bandeja en la sección Gmail. Para enviar un correo prepara destinatario, asunto y cuerpo, y solo lo envía tras la confirmación del usuario. |
| **Requerimiento NO funcional**    | RNF05RNF07                                                                                                                                                                                                                                |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                     |

#### RF15 — Integración con Google Workspace

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                  |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Características**               | Drive, Docs, Sheets, Slides, Calendar y Contactos.                                                                                                                                                                                                           |
| **Descripción del requerimiento** | El sistema busca archivos en Drive, lee documentos, hojas y presentaciones, consulta los próximos eventos del calendario y busca contactos. También crea eventos, documentos, hojas y presentaciones, y edita su contenido, siempre con confirmación previa. |
| **Requerimiento NO funcional**    | RNF05RNF07                                                                                                                                                                                                                                                   |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                        |

#### RF16 — Mensajería por WhatsApp

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Recepción de órdenes y envío de mensajes mediante el servicio Wazend.                                                                                                                                                                                                                                                                             |
| **Descripción del requerimiento** | El sistema recibe órdenes de texto desde los números autorizados (solo chats privados) y responde por el mismo medio. Para escribir a otra persona busca el contacto por nombre entre todos los contactos sincronizados (sin distinguir tildes, mayúsculas ni orden de las palabras), muestra destinatario y texto, y envía tras la confirmación. |
| **Requerimiento NO funcional**    | RNF05RNF06RNF07                                                                                                                                                                                                                                                                                                                                   |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                                                                                                             |

#### RF17 — Mensajería por Telegram

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                      |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Uso del asistente desde un bot de Telegram.                                                                                                                                                                                                                      |
| **Descripción del requerimiento** | Un bot de Telegram recibe mensajes de texto y notas de voz de los usuarios autorizados; las notas de voz se transcriben y se procesan como cualquier petición. Los usuarios no autorizados reciben su identificador para que el propietario los pueda habilitar. |
| **Requerimiento NO funcional**    | RNF05RNF07                                                                                                                                                                                                                                                       |
| **Prioridad del requerimiento**   | Baja                                                                                                                                                                                                                                                             |

#### RF18 — Control multimedia

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                  |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Características**               | Reproducción de música y video.                                                                                                                                                                                                                              |
| **Descripción del requerimiento** | El sistema busca y reproduce videos de YouTube en un reproductor integrado (con pausa, reanudación, volumen y cierre) y controla Spotify (reproducir, pausar, siguiente, anterior y canción actual). Mientras el asistente habla, baja el volumen del video. |
| **Requerimiento NO funcional**    | RNF01                                                                                                                                                                                                                                                        |
| **Prioridad del requerimiento**   | Baja                                                                                                                                                                                                                                                         |

#### RF19 — Asistente de programación

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                             |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Revisión y modificación de código de un proyecto local.                                                                                                                                                                                                                                                                 |
| **Descripción del requerimiento** | El usuario elige una carpeta de proyecto; el sistema lista y lee sus archivos de código, explica errores y prepara cambios mostrando un diff. Solo modifica archivos de texto dentro de la carpeta elegida, tras la confirmación, y bloquea archivos de claves y tokens. No ejecuta comandos de terminal por su cuenta. |
| **Requerimiento NO funcional**    | RNF05RNF07RNF10                                                                                                                                                                                                                                                                                                         |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                                                                                   |

#### RF20 — Motor de conciencia y alertas

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Seguimiento del estado operativo del equipo y del usuario.                                                                                                                                                                                                                                                                                                       |
| **Descripción del requerimiento** | Cada 15 segundos el sistema registra uso de CPU, RAM, disco y batería, la ventana activa y el tiempo de inactividad; cada 5 minutos consulta el próximo evento y los correos sin leer. Con esa información genera alertas (batería baja, CPU saturada, evento en 15 minutos), alimenta la vista Reactor y añade un resumen del estado al contexto del asistente. |
| **Requerimiento NO funcional**    | RNF04RNF07                                                                                                                                                                                                                                                                                                                                                       |
| **Prioridad del requerimiento**   | Media                                                                                                                                                                                                                                                                                                                                                            |

#### RF21 — Consola de eventos en tiempo real

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                 |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Registro en vivo de la actividad del sistema.                                                                                                                                                                                                               |
| **Descripción del requerimiento** | La sección Consola muestra en tiempo real el uso de herramientas, el avance de las tareas, las alertas y las confirmaciones pendientes, con filtros por tipo. Acepta comandos (/tarea, /estado, /cache, /limpiar, /ayuda) y mensajes directos al asistente. |
| **Requerimiento NO funcional**    | RNF01RNF04                                                                                                                                                                                                                                                  |
| **Prioridad del requerimiento**   | Baja                                                                                                                                                                                                                                                        |

### Requerimientos no funcionales

#### RNF01 — Interfaz del sistema

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                       |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El sistema presentará una interfaz moderna, sencilla e intuitiva.                                                                                                                                                                                                                                                 |
| **Descripción del requerimiento** | La interfaz web debe organizarse en secciones (Asistente, Tareas, Consola, Reactor, Gmail, Calendario y Programación), adaptarse a pantallas de escritorio y móviles, indicar visualmente el estado del asistente (en espera, escuchando, procesando, hablando) y respetar la preferencia de movimiento reducido. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                              |

#### RNF02 — Ayuda en el uso del sistema

| **Campo**                         | **Detalle**                                                                                                                                                                                                                      |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El sistema contará con documentación para instalarlo, configurarlo y usarlo.                                                                                                                                                     |
| **Descripción del requerimiento** | El proyecto debe incluir un manual (README) con requisitos, instalación, configuración de cada integración, arquitectura y resolución de problemas, además de mensajes de error comprensibles que indiquen al usuario qué hacer. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                             |

#### RNF03 — Mantenimiento

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                    |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El código será modular y verificable.                                                                                                                                                                                                                                                          |
| **Descripción del requerimiento** | El sistema debe estar dividido en módulos independientes (núcleo, agente, acciones, API, interfaz, memoria y conciencia), versionar el esquema de la base de datos con migraciones automáticas (Alembic) y disponer de pruebas automatizadas que se ejecuten sin acceder a servicios externos. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                           |

#### RNF04 — Desempeño

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Características**               | El sistema responderá con agilidad y aprovechará los límites del servicio de IA.                                                                                                                                                                                                                                                           |
| **Descripción del requerimiento** | Las respuestas de herramientas de solo lectura (búsquedas, páginas, clima, investigaciones) deben almacenarse en caché con vigencia limitada. Solo deben enviarse al modelo las herramientas relevantes para cada petición, reduciendo el consumo de tokens. Las tareas largas deben ejecutarse en segundo plano sin bloquear la interfaz. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                                       |

#### RNF05 — Seguridad de acceso

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Características**               | Solo el usuario autorizado podrá controlar el sistema.                                                                                                                                                                                                                                                                                                                                                                                                 |
| **Descripción del requerimiento** | El servidor debe escuchar únicamente en la dirección local y rechazar peticiones de otros orígenes. Las contraseñas se guardan cifradas con scrypt y la sesión viaja en una cookie httpOnly y SameSite=Strict firmada (JWT). El sistema ofrece verificación en dos pasos con códigos TOTP (RFC 6238) que no pueden reutilizarse, y códigos de respaldo almacenados solo como hash. Los canales de mensajería solo aceptan identificadores autorizados. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

#### RNF06 — Límites de uso

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                      |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El sistema limitará la cantidad de peticiones por minuto.                                                                                                                                                                                                                        |
| **Descripción del requerimiento** | Deben aplicarse límites configurables por ventana deslizante de 60 segundos: intentos de inicio de sesión (5 por defecto), mensajes y tareas por usuario (30) y peticiones generales por dirección (240). Al superarlos, el sistema responde indicando cuántos segundos esperar. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                             |

#### RNF07 — Seguridad y privacidad de la información

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                      |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | Los datos personales y las credenciales se mantendrán en el equipo del usuario.                                                                                                                                                                                                                                                  |
| **Descripción del requerimiento** | Claves y tokens se guardan en archivos locales excluidos del control de versiones. El contenido de páginas web, correos y archivos se trata como datos y nunca como instrucciones. El sistema no escribe contraseñas ni datos de pago y el envío de información a servicios externos se limita a lo necesario para cada función. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                             |

#### RNF08 — Disponibilidad y tolerancia a fallos

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                             |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El sistema seguirá funcionando ante fallos parciales.                                                                                                                                                                                                                                                                                   |
| **Descripción del requerimiento** | Si un modelo de IA alcanza su límite o no está disponible, el sistema debe probar automáticamente otros modelos. La conexión en tiempo real debe reconectarse sola y, sin ella, el chat debe funcionar por la API REST. Al reiniciarse, las tareas interrumpidas deben quedar marcadas como fallidas y las pendientes deben reanudarse. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                                    |

#### RNF09 — Portabilidad

| **Campo**                         | **Detalle**                                                                                                                                                                                           |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El sistema se ejecutará en equipos Windows de escritorio.                                                                                                                                             |
| **Descripción del requerimiento** | Debe funcionar en Windows 10/11 con Python 3.10 o superior; la interfaz se compila con Node.js 20 o superior y se usa desde Google Chrome o Microsoft Edge, necesarios para el reconocimiento de voz. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                  |

#### RNF10 — Fiabilidad de las acciones

| **Campo**                         | **Detalle**                                                                                                                                                                                                                                                                                                                             |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Características**               | El asistente no afirmará haber realizado acciones que no ejecutó.                                                                                                                                                                                                                                                                       |
| **Descripción del requerimiento** | Si el modelo de lenguaje afirma haber preparado o enviado algo sin haber usado la herramienta correspondiente, el sistema debe detectarlo, pedir al modelo que lo haga correctamente y, si insiste, reemplazar la respuesta por un aviso veraz. Toda acción con efectos externos depende de una confirmación registrada por el sistema. |
| **Prioridad del requerimiento**   | Alta                                                                                                                                                                                                                                                                                                                                    |

### 3.1 Requisitos comunes de las interfaces

#### 3.1.1 Interfaces de usuario

La interfaz web cuenta con una pantalla de inicio de sesión —y de creación del usuario en el primer arranque— que, si la verificación en dos pasos está activa, solicita a continuación el código de Google Authenticator e incluye la ayuda «¿Olvidaste tu contraseña?».

Una vez dentro, en la parte superior se encuentran:

* El nombre del sistema.
* Un indicador del estado del asistente y de la conexión en tiempo real.
* El menú de secciones: Asistente, Tareas, Consola, Reactor, Gmail, Calendario y Programación.
* Los botones de alertas, silenciar voz, memoria, ajustes y cierre de sesión.

El cuerpo se divide en tres columnas:

* A la izquierda, el historial de conversaciones, con opciones de crear y borrar chats.
* En el centro, la sección activa. En Asistente incluye un orbe animado que refleja el estado, el registro de mensajes y la barra de entrada con adjuntos y micrófono.
* A la derecha, tarjetas de acceso rápido para tareas activas, Gmail, Calendar, YouTube, Spotify y archivos generados.

En pantallas pequeñas, las columnas se apilan.

#### 3.1.2 Interfaces de hardware

Se requiere un equipo de cómputo con Windows 10/11 en buen estado y conexión a internet. Para la interacción por voz se necesitan micrófono y altavoces o auriculares. No se requiere hardware especializado.

#### 3.1.3 Interfaces de software

* **Sistema operativo:** Windows 10 o superior.
* **Entorno de ejecución:** Python 3.10 o superior; Node.js 20 o superior para compilar la interfaz.
* **Navegador:** Google Chrome o Microsoft Edge. Edge también es usado por el navegador controlado.
* **Autenticación:** app de verificación compatible con TOTP en el móvil, como Google Authenticator, Microsoft Authenticator u otra; opcional.
* **Servicios externos:** API de Groq; APIs de Google (Gmail, Drive, Docs, Sheets, Slides, Calendar, People); Spotify Web API; Telegram Bot API; Wazend.
* **Almacenamiento:** bases de datos SQLite locales para memoria y plataforma.

#### 3.1.4 Interfaces de comunicación

La interfaz y el servidor se comunican mediante HTTP —API REST— y WebSocket sobre la dirección local del equipo. La comunicación con los servicios externos se realiza mediante HTTPS y conexiones WebSocket seguras salientes, por lo que no es necesario abrir puertos en el router. Para transferir archivos se usan los protocolos estándar existentes.

### 3.2 Requisitos funcionales

{% stepper %}
{% step %}
#### Autenticación de usuarios (RF01)

El acceso a la interfaz web y a la API exige un usuario con contraseña y, opcionalmente, verificación en dos pasos.

En el primer arranque, el sistema solicita crear el usuario propietario, con un usuario de 3 a 40 caracteres y una contraseña de mínimo 8 caracteres. Después, toda petición a la API y a los archivos requiere una sesión iniciada.

El usuario puede cerrar sesión y cambiar su contraseña desde **Ajustes → Seguridad**. Al cambiarla, las demás sesiones abiertas quedan invalidadas.

Desde esa misma sección puede activar la verificación en dos pasos escaneando un código QR con Google Authenticator u otra app compatible con TOTP. A partir de ese momento, el inicio de sesión exige además el código de 6 dígitos de la app y el sistema entrega 8 códigos de respaldo de un solo uso.

Si el usuario olvida la contraseña, puede restablecerla ejecutando el script `reset_password.py` en el equipo. El script exige un código de la app o de respaldo; sin la verificación en dos pasos activa, no permite cambiarla.
{% endstep %}

{% step %}
#### Conversación con el asistente (RF02)

Chat de texto con respuestas generadas por un modelo de lenguaje y uso de herramientas.

El usuario escribe mensajes y JARVIS responde en español, con frases breves pensadas para ser leídas en voz alta. Para responder, el asistente puede invocar herramientas, como búsqueda web, control del PC o correo. Durante el procesamiento, la interfaz muestra en tiempo real qué herramienta se está usando.
{% endstep %}

{% step %}
#### Historial de conversaciones (RF03)

Gestión de las conversaciones guardadas.

El sistema conserva cada conversación con su título y mensajes. El usuario puede abrir una conversación anterior, crear una nueva, borrar una conversación o borrar todas, previa confirmación.
{% endstep %}

{% step %}
#### Interacción por voz (RF04)

Reconocimiento de voz con palabra de activación y respuesta hablada.

Con el micrófono activo, el sistema escucha y reacciona cuando el usuario dice «Jarvis», palabra de activación configurable. Tras una pregunta del asistente, el usuario puede responder durante 15 segundos sin repetir la palabra.

Las respuestas se leen con voz sintetizada. El usuario elige voz, velocidad, tono e idioma de escucha, y puede interrumpir la voz con Esc.
{% endstep %}

{% step %}
#### Memoria a largo plazo y perfil (RF05)

Recordar datos duraderos del usuario.

El asistente guarda automáticamente datos duraderos que el usuario comparte, como gustos, personas, proyectos y rutinas, y los consulta antes de responder.

El usuario puede ver y borrar los recuerdos, y editar su perfil: nombre, forma de trato, ciudad, ocupación, idiomas, intereses y notas.
{% endstep %}

{% step %}
#### Tareas autónomas en segundo plano (RF06)

Descomposición de un objetivo en pasos y ejecución autónoma.

El usuario describe un objetivo desde la sección Tareas, la consola o el chat. Un planificador lo divide en un máximo de 8 pasos; un ejecutor realiza cada paso con las herramientas, reintenta un paso fallido y, si vuelve a fallar, solicita un plan alternativo.

Al terminar se genera un resumen. El usuario puede seguir el avance, cancelar, repetir o borrar tareas, que quedan registradas en un historial.
{% endstep %}

{% step %}
#### Confirmación de acciones sensibles (RF07)

Ninguna acción con efectos externos se realiza sin aprobación explícita.

Enviar correos o mensajes, modificar documentos o eventos de Google, aplicar cambios de código, pulsar botones de enviar, comprar o borrar en el navegador y apagar o reiniciar el equipo se preparan primero y se muestran al usuario.

Solo se ejecutan cuando responde «Confirmar» o un sinónimo como «sí». Con «Cancelar» se descartan y caducan a los 10 minutos. En las tareas autónomas, la tarea se pausa hasta la decisión.
{% endstep %}

{% step %}
#### Control del equipo (RF08)

Manejo de aplicaciones y periféricos del PC.

El sistema puede abrir aplicaciones por su nombre, buscándolas en el menú Inicio; cerrar programas; abrir direcciones web; ajustar el volumen; controlar la reproducción multimedia; escribir texto; pulsar atajos de teclado; tomar capturas de pantalla y explorar carpetas.
{% endstep %}

{% step %}
#### Ajustes del sistema operativo (RF09)

Modificación de configuraciones de Windows.

El sistema puede ajustar el brillo de la pantalla, cambiar entre tema claro y oscuro, abrir páginas concretas de Configuración, informar del estado de la red y suspender, reiniciar, apagar o cerrar la sesión del equipo.

Las acciones de suspender, reiniciar, apagar o cerrar sesión siempre requieren confirmación.
{% endstep %}

{% step %}
#### Navegador controlado (RF10)

Automatización de páginas web en una ventana propia.

El asistente abre una ventana de Microsoft Edge con un perfil independiente, navega, lee el contenido y los controles de la página, hace clic, rellena campos, retrocede, toma capturas y cierra el navegador.

Nunca escribe contraseñas ni datos de pago. Los clics que envían, compran, publican o borran requieren confirmación.
{% endstep %}

{% step %}
#### Búsqueda web e investigación profunda (RF11)

Consulta de información actualizada en internet.

El sistema realiza búsquedas web, lee páginas y consulta el clima.

En la investigación profunda genera varias búsquedas complementarias, lee las mejores fuentes y redacta un informe con citas numeradas y lista de fuentes, que guarda como documento.
{% endstep %}

{% step %}
#### Generación de documentos y presentaciones (RF12)

Creación de archivos a partir de las peticiones del usuario.

El sistema genera documentos PDF, DOCX o texto plano y presentaciones PowerPoint (`.pptx`) con portada, diapositivas de viñetas y notas del orador.

Los archivos quedan disponibles en la interfaz y pueden abrirse en el PC.
{% endstep %}

{% step %}
#### Archivos adjuntos (RF13)

Carga de archivos para que el asistente los analice.

El usuario puede adjuntar archivos de hasta 50 MB, incluidos documentos, hojas de cálculo e imágenes. El asistente lee su contenido o describe las imágenes para responder preguntas sobre ellos.
{% endstep %}

{% step %}
#### Integración con Gmail (RF14)

Lectura y envío de correo electrónico.

Tras autorizar la cuenta mediante OAuth, el sistema busca, lee y resume correos, y muestra la bandeja en la sección Gmail.

Para enviar un correo prepara destinatario, asunto y cuerpo, y solo lo envía tras la confirmación del usuario.
{% endstep %}

{% step %}
#### Integración con Google Workspace (RF15)

Drive, Docs, Sheets, Slides, Calendar y Contactos.

El sistema busca archivos en Drive, lee documentos, hojas y presentaciones, consulta los próximos eventos del calendario y busca contactos.

También crea eventos, documentos, hojas y presentaciones, y edita su contenido, siempre con confirmación previa.
{% endstep %}

{% step %}
#### Mensajería por WhatsApp (RF16)

Recepción de órdenes y envío de mensajes mediante el servicio Wazend.

El sistema recibe órdenes de texto desde los números autorizados, únicamente en chats privados, y responde por el mismo medio.

Para escribir a otra persona, busca el contacto por nombre entre todos los contactos sincronizados, sin distinguir tildes, mayúsculas ni orden de las palabras; muestra destinatario y texto, y envía tras la confirmación.
{% endstep %}

{% step %}
#### Mensajería por Telegram (RF17)

Uso del asistente desde un bot de Telegram.

Un bot de Telegram recibe mensajes de texto y notas de voz de los usuarios autorizados. Las notas de voz se transcriben y se procesan como cualquier petición.

Los usuarios no autorizados reciben su identificador para que el propietario los pueda habilitar.
{% endstep %}

{% step %}
#### Control multimedia (RF18)

Reproducción de música y video.

El sistema busca y reproduce videos de YouTube en un reproductor integrado, con pausa, reanudación, volumen y cierre, y controla Spotify: reproducir, pausar, siguiente, anterior y canción actual.

Mientras el asistente habla, baja el volumen del video.
{% endstep %}

{% step %}
#### Asistente de programación (RF19)

Revisión y modificación de código de un proyecto local.

El usuario elige una carpeta de proyecto. El sistema lista y lee sus archivos de código, explica errores y prepara cambios mostrando un diff.

Solo modifica archivos de texto dentro de la carpeta elegida, tras la confirmación, y bloquea archivos de claves y tokens. No ejecuta comandos de terminal por su cuenta.
{% endstep %}

{% step %}
#### Motor de conciencia y alertas (RF20)

Seguimiento del estado operativo del equipo y del usuario.

Cada 15 segundos, el sistema registra uso de CPU, RAM, disco y batería, la ventana activa y el tiempo de inactividad. Cada 5 minutos consulta el próximo evento y los correos sin leer.

Con esa información genera alertas, como batería baja, CPU saturada o evento en 15 minutos, alimenta la vista Reactor y añade un resumen del estado al contexto del asistente.
{% endstep %}

{% step %}
#### Consola de eventos en tiempo real (RF21)

Registro en vivo de la actividad del sistema.

La sección Consola muestra en tiempo real el uso de herramientas, el avance de las tareas, las alertas y las confirmaciones pendientes, con filtros por tipo.

Acepta comandos `/tarea`, `/estado`, `/cache`, `/limpiar` y `/ayuda`, además de mensajes directos al asistente.
{% endstep %}
{% endstepper %}

### 3.3 Requisitos no funcionales

#### 3.3.1 Requisitos de rendimiento

* Las respuestas de herramientas de solo lectura se reutilizan desde una caché con vigencia limitada, por ejemplo, 10 minutos para búsquedas y 6 horas para investigaciones.
* En cada petición solo se envían al modelo las herramientas relevantes, lo que reduce el consumo de tokens a menos de la mitad.
* Las tareas largas se ejecutan en segundo plano y la interfaz recibe su avance en tiempo real sin bloquearse.

#### 3.3.2 Seguridad

* El servidor solo atiende peticiones locales y rechaza peticiones con origen distinto al propio sistema.
* Acceso con usuario y contraseña cifrada mediante scrypt; sesión en cookie httpOnly y `SameSite=Strict` con token firmado; cambio de contraseña con cierre de las demás sesiones.
* Verificación en dos pasos opcional con Google Authenticator mediante TOTP: cada código vale 30 segundos y una sola vez; 8 códigos de respaldo de un solo uso para recuperar el acceso si se pierde el móvil.
* El restablecimiento de una contraseña olvidada solo es posible desde el propio equipo mediante `reset_password.py` y exige un código de verificación; sin él no se cambia nada.
* Límites de peticiones por minuto para inicio de sesión, mensajes y API general.
* Confirmación obligatoria antes de cualquier acción con efectos externos; el navegador controlado nunca escribe contraseñas ni datos de pago.
* El contenido externo, como páginas, correos y archivos, se trata como datos y nunca como instrucciones.

#### 3.3.3 Fiabilidad

* El asistente no puede afirmar que preparó o envió algo sin haber usado la herramienta correspondiente; el sistema detecta y corrige esas respuestas.
* Los pasos fallidos de una tarea se reintentan y, si es necesario, se replanifican.
* Las pruebas automatizadas verifican autenticación, límites de uso, tareas, confirmaciones, caché y WebSocket.

#### 3.3.4 Disponibilidad

* El sistema está disponible mientras el equipo esté encendido y el servidor en ejecución.
* Ante la indisponibilidad o el límite de un modelo de IA, se usa automáticamente otro modelo disponible.
* La conexión en tiempo real se restablece sola; si no está disponible, el chat funciona mediante la API REST.

#### 3.3.5 Mantenibilidad

* El sistema dispone de un manual README con instalación, configuración, arquitectura y resolución de problemas.
* El código está dividido en módulos independientes y el esquema de datos se actualiza con migraciones automáticas.
* Los errores se muestran con mensajes comprensibles que indican al usuario cómo resolverlos.

#### 3.3.6 Portabilidad

El sistema será implantado bajo la plataforma Windows 10/11. El núcleo en Python y la interfaz web son independientes de la plataforma, mientras que el control del equipo y los ajustes del sistema son específicos de Windows.
