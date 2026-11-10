# Arquitectura\_de\_datos\_JARVIS

## Arquitectura de datos

### Arquitectura de datos de J.A.R.V.I.S.

J.A.R.V.I.S. almacena la información directamente en el computador del usuario. Para organizar los datos utiliza dos bases de datos SQLite:

* **jarvis.db (plataforma):** guarda los usuarios, las contraseñas protegidas, la configuración del doble factor de autenticación, las tareas del agente y sus pasos. También almacena las respuestas que quedan guardadas en caché. Para manejar las tablas se utiliza SQLAlchemy y, con Alembic, se realizan los cambios en la estructura de la base de datos.
* **memory.db (asistente):** almacena las conversaciones, los mensajes, la información que el asistente debe recordar y las acciones que necesitan autorización antes de ejecutarse, como enviar un correo electrónico o un mensaje de WhatsApp.

## Diseño de la base de datos

El diseño de la base de datos permite organizar la información del sistema y establecer cómo se relacionan los datos entre sí. También define las reglas que se deben cumplir para mantener la información ordenada y evitar errores al guardarla o consultarla.

## Fases del diseño de la base de datos aplicadas a J.A.R.V.I.S.

{% stepper %}
{% step %}
### Fase 1. Recopilación y análisis de requisitos

A partir de la ERS se determinaron los datos que necesita manejar J.A.R.V.I.S. Entre ellos están los usuarios, las conversaciones, la información que el asistente recuerda, las tareas que realiza y el avance de cada una. También se tuvo en cuenta la confirmación de acciones importantes y el almacenamiento temporal de respuestas para facilitar consultas posteriores.
{% endstep %}

{% step %}
### Fase 2. Diseño conceptual

En esta fase se identificaron las entidades principales del sistema: Usuario, Tarea, Paso, Conversación, Mensaje, Recuerdo, Acción pendiente y Entrada de caché. También se establecieron algunas de sus relaciones:

* Un usuario puede tener varias tareas.
* Una tarea puede estar dividida en varios pasos.
* Una conversación puede contener varios mensajes.
{% endstep %}

{% step %}
### Fase 3. Diseño lógico

Después de definir las entidades, se organizaron los datos en tablas relacionadas. Cada tabla cuenta con una clave primaria para identificar sus registros y se utilizan claves foráneas para conectar la información. Por ejemplo, `tasks.user_id` relaciona las tareas con los usuarios, `task_steps.task_id` conecta los pasos con sus tareas y `chat_messages.conversation_id` relaciona los mensajes con las conversaciones.
{% endstep %}

{% step %}
### Fase 4. Diseño físico

Se eligió SQLite porque J.A.R.V.I.S. funciona directamente en el computador del usuario y no necesita un servidor de base de datos independiente.

Para su funcionamiento se tuvieron en cuenta los siguientes aspectos:

* Las fechas se almacenan como marcas de tiempo (`REAL`).
* Índices en campos que se consultan con frecuencia, como el estado de las tareas y la fecha de expiración de la caché.
* Las contraseñas y los códigos de recuperación se almacenan mediante hashes, en lugar de guardarse como texto legible.
* Cuando se elimina una tarea, también se eliminan los pasos asociados a ella.
{% endstep %}

{% step %}
### Fase 5. Implementación y optimización

Las tablas se crean y modifican mediante migraciones de Alembic, como `0001_inicial` y `0002_dos_pasos`. Esto permite actualizar la estructura de la base de datos de forma controlada.

Para mejorar el funcionamiento del sistema, se va a separar la información de la plataforma y la memoria del asistente en dos bases de datos. Además, se implementara una caché con tiempo de expiración y se cerraran las conexiones después de cada consulta para evitar mantenerlas abiertas innecesariamente.
{% endstep %}
{% endstepper %}
