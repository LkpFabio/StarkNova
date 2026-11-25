# Manual\_de\_Usuario\_JARVIS

**UNIVERSIDAD SANTIAGO DE CALI**

Tecnología en Desarrollo de Sistemas de Información y de Software

**MANUAL DE USUARIO**

**J.A.R.V.I.S.**

Asistente personal inteligente para equipos de escritorio

**Equipo StarkNova**

Fabio Morales (líder) · Juan Arias · Juan Bustamante · Nicolás Muñoz

Versión 1.0 · Noviembre de 2026

Santiago de Cali

## Control del documento

| **Versión** | **Fecha**  | **Descripción**                        | **Autor**        |
| ----------- | ---------- | -------------------------------------- | ---------------- |
| 1.0         | 24/11/2026 | Primera versión del manual de usuario. | Equipo StarkNova |

| **Dato**                | **Valor**                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| Sistema                 | J.A.R.V.I.S. – Asistente personal inteligente                                                    |
| Dirigido a              | Usuario propietario del equipo donde se instala J.A.R.V.I.S.                                     |
| Documentos relacionados | Especificación de requisitos (ERS, IEEE 830), matriz de pruebas unitarias y README del proyecto. |

## Contenido

[Control del documento 2](./#%5FToc246377195)

[Contenido 3](./#%5FToc246377196)

[1. Introducción 5](./#%5FToc246377197)

[1.1 Objetivo del manual 5](./#%5FToc246377198)

[1.2 A quién va dirigido 5](./#%5FToc246377199)

[1.3 Cómo leer este manual 5](./#%5FToc246377200)

[2. Requisitos del sistema 5](./#%5FToc246377201)

[3. Instalación y puesta en marcha 5](./#%5FToc246377202)

[4. Primer ingreso y seguridad 6](./#%5FToc246377203)

[4.1 Crear el usuario propietario 6](./#%5FToc246377204)

[4.2 Iniciar y cerrar sesión 7](./#%5FToc246377205)

[4.3 Activar la verificación en dos pasos (recomendado) 7](./#%5FToc246377206)

[4.4 Cambiar u olvidar la contraseña 8](./#%5FToc246377207)

[5. Conoce la interfaz 8](./#%5FToc246377208)

[6. Conversar con J.A.R.V.I.S. 9](./#%5FToc246377209)

[6.1 Por escrito 9](./#%5FToc246377210)

[6.2 Por voz 10](./#%5FToc246377211)

[6.3 Adjuntar archivos 10](./#%5FToc246377212)

[6.4 Historial de conversaciones 10](./#%5FToc246377213)

[6.5 Órdenes rápidas 10](./#%5FToc246377214)

[7. Acciones sensibles: siempre con tu permiso 11](./#%5FToc246377215)

[8. Secciones de la aplicación 12](./#%5FToc246377216)

[8.1 Tareas 12](./#%5FToc246377217)

[8.2 Consola 12](./#%5FToc246377218)

[8.3 Reactor 13](./#%5FToc246377219)

[8.4 Gmail y Calendario 14](./#%5FToc246377220)

[9. Perfil, memoria y rutinas 16](./#%5FToc246377221)

[9.1 Perfil 16](./#%5FToc246377222)

[9.2 Memoria 16](./#%5FToc246377223)

[9.3 Rutinas 17](./#%5FToc246377224)

[10. Conectar tus servicios 17](./#%5FToc246377225)

[11. Qué le puedes pedir 18](./#%5FToc246377226)

[12. Solución de problemas 18](./#%5FToc246377227)

[13. Glosario 19](./#%5FToc246377228)

[14. Soporte 19](./#%5FToc246377229)

1\. Introducción

Este manual explica, paso a paso, cómo instalar y usar **J.A.R.V.I.S.**, un asistente personal de inteligencia artificial que funciona en tu propio computador. Con él puedes conversar por texto o por voz, controlar aplicaciones y ajustes de Windows, consultar y enviar correos, revisar tu calendario, reproducir música y encargar tareas largas que el asistente realiza por su cuenta.

### 1.1 Objetivo del manual

Que cualquier persona, aunque no tenga conocimientos técnicos, pueda poner en marcha J.A.R.V.I.S., entender cada pantalla y aprovechar sus funciones de forma segura.

### 1.2 A quién va dirigido

Al **usuario propietario** del equipo donde se instala el sistema. J.A.R.V.I.S. es personal: tiene un único usuario, protegido con contraseña y, de forma opcional, con un código de Google Authenticator.

### 1.3 Cómo leer este manual

* Los textos en **negrita** son botones, secciones o nombres que verás en pantalla.
* Los textos en este formato son comandos o frases que puedes escribir tal cual.
* Los recuadros naranjas son avisos importantes; los azules, consejos.

## 2. Requisitos del sistema

| **Elemento**      | **Requisito**                                                                                                     |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- |
| Sistema operativo | Windows 10 u 11.                                                                                                  |
| Programas         | Python 3.10 o superior y Node.js 20 o superior (solo para la instalación).                                        |
| Navegador         | Google Chrome o Microsoft Edge (necesarios para el reconocimiento de voz).                                        |
| Internet          | Conexión estable: la inteligencia artificial, la búsqueda web y las integraciones la necesitan.                   |
| Clave de IA       | Una clave gratuita de Groq (console.groq.com).                                                                    |
| Opcional          | Micrófono y altavoces para hablar con Jarvis; cuentas de Gmail/Google, Spotify y Telegram para sus integraciones. |

## 3. Instalación y puesta en marcha

La instalación se hace una sola vez. Después basta con un doble clic para abrir J.A.R.V.I.S.

1. Copia la carpeta del proyecto (por ejemplo, Jarvis-VSCode) en tu computador.
2. Abre una terminal en esa carpeta y crea el entorno de Python: python -m venv .venv
3. Instala las dependencias: .venv\Scripts\python.exe -m pip install -r requirements.txt
4. Copia el archivo .env.example con el nombre .env, ábrelo con el Bloc de notas y escribe tu clave de Groq después de GROQ\_API\_KEY=.
5. Haz doble clic en **run.bat**. La primera vez prepara la interfaz (tarda un par de minutos); luego abre J.A.R.V.I.S. en el navegador.

| **Importante:** el archivo .env contiene tus claves. No lo compartas ni lo envíes a nadie. |
| ------------------------------------------------------------------------------------------ |

Para abrirlo en adelante, haz doble clic en **run.bat** o en el acceso directo **J.A.R.V.I.S.** del escritorio. La ventana negra que aparece es el servidor del asistente: mientras esté abierta, J.A.R.V.I.S. funciona. Para apagarlo, ciérrala.

## 4. Primer ingreso y seguridad

### 4.1 Crear el usuario propietario

La primera vez que abres J.A.R.V.I.S. aparece la pantalla para crear tu usuario. Solo se pide una vez.

1. Escribe un **Usuario** de 3 a 40 caracteres.
2. Escribe una **Contraseña** de al menos 8 caracteres y repítela en **Repite la contraseña**.
3. Pulsa **Crear usuario y entrar**.

_Figura 1. Pantalla para crear el usuario propietario (primer arranque)._

### 4.2 Iniciar y cerrar sesión

Las siguientes veces, J.A.R.V.I.S. pide tu usuario y contraseña. Si te equivocas cinco veces seguidas en un minuto, deberás esperar unos segundos antes de volver a intentarlo. Para salir, pulsa **Cerrar sesión** en la esquina superior derecha.

_Figura 2. Pantalla de inicio de sesión._

### 4.3 Activar la verificación en dos pasos (recomendado)

Con la verificación en dos pasos, además de la contraseña se pide un código de 6 dígitos de la app **Google Authenticator** del celular.

1. Pulsa el botón de **Ajustes y perfil** (icono de controles, arriba a la derecha) y baja hasta **Seguridad**.
2. En **Verificación en dos pasos (Google Authenticator)** pulsa **Activar**.
3. En Google Authenticator pulsa **+** → **Escanear un código QR** y escanea el código que aparece en pantalla.
4. Escribe el código de 6 dígitos que muestra la app y confirma.
5. Guarda los **códigos de respaldo** (puedes copiarlos o descargarlos en .txt). Sirven si pierdes el celular.

_Figura 3. Ajustes: rutinas, cambio de contraseña y verificación en dos pasos._

### 4.4 Cambiar u olvidar la contraseña

* **Cambiarla:** en **Ajustes y perfil → Seguridad**, escribe la contraseña actual y la nueva, y pulsa **Cambiar contraseña**.
* **Si la olvidaste y tienes la verificación en dos pasos activa:** abre una terminal en la carpeta de Jarvis y ejecuta .venv\Scripts\python.exe reset\_password.py. Te pedirá un código de Google Authenticator (o uno de respaldo) y la contraseña nueva. Conservas todos tus datos.
* **Si la olvidaste y no la tenías activa:** cierra J.A.R.V.I.S. y borra el archivo data\jarvis.db; al abrirlo de nuevo podrás crear el usuario otra vez (se pierde el historial de tareas, no tus conversaciones).

## 5. Conoce la interfaz

Después de iniciar sesión verás la pantalla principal del **Asistente**:

_Figura 4. Pantalla principal del Asistente._

| **Zona**                   | **Para qué sirve**                                                                                                                                                                                                                                                                           |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Barra superior             | Estado del asistente (en espera, escuchando, procesando, hablando), las secciones (**Asistente, Tareas, Consola, Reactor, Gmail, Calendario**), el interruptor **Decir «Jarvis»**, las alertas, el botón para callar a Jarvis, la **Memoria**, los **Ajustes y perfil** y **Cerrar sesión**. |
| Conversaciones (izquierda) | Historial de chats. **＋ Nuevo chat** crea uno nuevo; el icono de papelera borra uno. **Ocultar** pliega el panel.                                                                                                                                                                            |
| Centro                     | El **orbe** muestra lo que hace Jarvis; debajo aparece la conversación y el campo para escribir, con los botones de adjuntar, micrófono y enviar.                                                                                                                                            |
| Panel derecho              | Resúmenes de **Gmail** y **Google Calendar**, el reproductor de **YouTube**, los controles de **Spotify** y los **Archivos generados**.                                                                                                                                                      |

## 6. Conversar con J.A.R.V.I.S.

### 6.1 Por escrito

Escribe tu petición en el campo **Escribe o di «Jarvis, ...»** y pulsa Enter o el botón de enviar. La respuesta aparece mientras se genera y, además, Jarvis la lee en voz alta. Si usa una herramienta (por ejemplo, buscar en internet), verás un aviso de lo que está haciendo.

_Figura 5. Una conversación: el usuario pregunta y Jarvis responde._

### 6.2 Por voz

1. Pulsa el botón del **micrófono** y acepta el permiso del navegador la primera vez.
2. Di **«Jarvis»** seguido de tu petición, por ejemplo: Jarvis, ¿qué clima hace hoy?
3. Si Jarvis te hace una pregunta, puedes contestar durante 15 segundos sin repetir su nombre.

| **Consejo:** si desactivas **Decir «Jarvis»** en la barra superior, Jarvis responde a todo lo que escuche. Para que deje de hablar, pulsa el botón de callar o la tecla Esc. |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### 6.3 Adjuntar archivos

Pulsa el **clip** para adjuntar documentos, hojas de cálculo o imágenes (hasta 50 MB por archivo) y luego pregunta sobre ellos: ¿De qué trata este documento? o Describe esta imagen.

### 6.4 Historial de conversaciones

Cada chat se guarda con su título. Ábrelo desde el panel **Conversaciones** para continuar donde lo dejaste, o bórralo con la papelera (se pide confirmación).

### 6.5 Órdenes rápidas

Algunas órdenes se ejecutan al instante, sin consultar a la inteligencia artificial:

| **Di o escribe**                     | **Qué hace**                             |
| ------------------------------------ | ---------------------------------------- |
| volumen al 40 · silencia             | Ajusta o silencia el volumen del equipo. |
| pausa · siguiente canción · anterior | Controla la música que esté sonando.     |
| modo oscuro · modo claro             | Cambia el tema de Windows.               |
| brillo al 70                         | Ajusta el brillo (solo en portátiles).   |
| abre Spotify · abre la calculadora   | Abre una aplicación instalada.           |

## 7. Acciones sensibles: siempre con tu permiso

J.A.R.V.I.S. **nunca** hace algo delicado sin preguntarte. Enviar un correo, crear o cambiar eventos y documentos de Google, cerrar programas, pulsar botones de comprar o enviar en el navegador, y apagar o reiniciar el equipo se **preparan primero** y se te muestran con el texto «todavía NO se ha realizado».

_Figura 6. Jarvis prepara el apagado del equipo y espera la confirmación._

* Escribe o di **Confirmar** (o «sí», «dale», «envíalo») para realizar la acción.
* Escribe o di **Cancelar** (o «no», «mejor no») para descartarla.

| **Aviso:** lee bien el resumen antes de confirmar: muestra exactamente el destinatario y el texto que se enviará. Jarvis tampoco escribe contraseñas ni datos de pago por ti. |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

## 8. Secciones de la aplicación

### 8.1 Tareas

Para encargos largos o de varios pasos. Escribe el objetivo (por ejemplo, Investiga 3 portátiles para estudiantes y hazme un documento comparativo) y pulsa **Lanzar tarea**. Jarvis lo divide en un plan de hasta 8 pasos y lo ejecuta en segundo plano mientras sigues usando el equipo. Selecciona una tarea para ver su plan y su resultado, o pulsa **Cancelar tarea** para detenerla.

_Figura 7. Sección Tareas: el agente autónomo._

### 8.2 Consola

Muestra en tiempo real todo lo que hace Jarvis: herramientas usadas, tiempos de respuesta, avance de tareas, alertas y confirmaciones pendientes. Puedes filtrar por tipo y escribir comandos:

| **Comando**             | **Resultado**                                                  |
| ----------------------- | -------------------------------------------------------------- |
| /ayuda                  | Lista los comandos disponibles.                                |
| /tarea                  | Crea una tarea en segundo plano.                               |
| /estado                 | Estado de Jarvis y de sus integraciones.                       |
| /cache · /cache limpiar | Estadísticas de la memoria temporal de resultados, o vaciarla. |
| /limpiar                | Limpia la pantalla de la consola.                              |

_Figura 8. Sección Consola._

### 8.3 Reactor

Es el «motor de conciencia» de Jarvis: muestra el uso de CPU, memoria, disco y batería, la ventana activa, tu tiempo de inactividad, la agenda, las tareas activas, el estado de las integraciones y las alertas (por ejemplo, batería por debajo del 20 % o una reunión a punto de empezar).

_Figura 9. Sección Reactor._

### 8.4 Gmail y Calendario

Muestran tu bandeja de entrada y tus próximos eventos dentro de Jarvis. Necesitan que conectes tu cuenta de Google (ver el capítulo 10). Mientras no esté conectada, verás un aviso con el paso a seguir, como en las figuras.

_Figura 10. Sección Gmail (sin cuenta conectada)._

_Figura 11. Sección Calendario (sin cuenta conectada)._

## 9. Perfil, memoria y rutinas

### 9.1 Perfil

En **Ajustes y perfil** puedes indicar tu nombre, cómo quieres que te llame Jarvis (por ejemplo, «señor» o tu nombre), tu ciudad, ocupación, idiomas, intereses y notas personales. También eliges la **voz**, el **idioma de escucha**, la **velocidad** y el **tono**; pulsa **Probar voz** para escucharla y **Guardar** para aplicar los cambios.

_Figura 12. Ajustes: perfil y voz._

### 9.2 Memoria

Jarvis recuerda lo que le cuentas de ti (gustos, personas, proyectos) para tenerlo en cuenta después. Pulsa el botón **Memoria** de la barra superior para ver lo que recuerda y borrar cualquier recuerdo.

_Figura 13. Memoria a largo plazo._

### 9.3 Rutinas

Una rutina hace varias cosas con una sola frase. Por ejemplo, la rutina **Llegada** se activa con he llegado, ya llegué o estoy en casa y te da el resumen del día. Para crear o cambiar una rutina, pídelo en el chat:

Crea una rutina «modo estudio» que abra Chrome y Word y ponga mi playlist de Spotify

Las rutinas guardadas aparecen en **Ajustes y perfil → Rutinas**, con los botones **Probar** y **Borrar**.

## 10. Conectar tus servicios

Las integraciones son opcionales. Cada una se configura una vez desde la carpeta de Jarvis; el README del proyecto tiene el detalle completo.

| **Servicio**                                             | **Cómo conectarlo**                                                                                                                                                                                |
| -------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Gmail                                                    | Guarda credentials.json (cliente OAuth de Google) en la carpeta de Jarvis y ejecuta .venv\Scripts\python.exe gmail\_login.py. Acepta los permisos en el navegador.                                 |
| Google Drive, Docs, Sheets, Slides, Calendar y Contactos | Con el mismo credentials.json, ejecuta .venv\Scripts\python.exe workspace\_login.py.                                                                                                               |
| Spotify                                                  | Escribe tu SPOTIFY\_CLIENT\_ID en .env y ejecuta .venv\Scripts\python.exe spotify\_login.py. Deja abierta la app de Spotify. Sin cuenta Premium, Jarvis abre las canciones directamente en la app. |
| Telegram                                                 | Crea un bot con @BotFather, escribe el token en .env (TELEGRAM\_BOT\_TOKEN) y autoriza tu ID de usuario. run.bat lo inicia automáticamente.                                                        |

| **Aviso:** mientras la app de Google esté en modo de prueba, la autorización de Gmail y Google vence cada 7 días. Cuando Jarvis lo indique, vuelve a ejecutar gmail\_login.py y workspace\_login.py. |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

## 11. Qué le puedes pedir

| **Categoría**   | **Ejemplos**                                                                                                                                   |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Información     | ¿Qué clima hace en Cali? · Busca noticias de tecnología · Investiga a fondo la energía solar en Colombia                                       |
| Documentos      | Hazme un PDF con un resumen de la fotosíntesis · Crea una presentación de 5 diapositivas sobre J.A.R.V.I.S.                                    |
| Correo y agenda | Resume mis correos sin leer · Mándale un correo a Sara diciendo que voy en camino · ¿Qué tengo esta semana? · Crea un evento mañana a las 3 pm |
| Equipo          | Abre Valorant · Toma una captura de pantalla · Cierra el Bloc de notas · Suspende el equipo                                                    |
| Navegador       | Abre la página de la universidad y dime las noticias                                                                                           |
| Música y video  | Pon Back in Black en Spotify · Pon el tráiler de Iron Man en YouTube                                                                           |
| Memoria         | Recuerda que los viernes estudio con mi grupo · ¿Qué sabes de mí?                                                                              |

## 12. Solución de problemas

| **Problema**                                         | **Qué hacer**                                                                                                    |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| La página no abre.                                   | Comprueba que la ventana negra de run.bat sigue abierta. Si la cerraste, vuelve a abrir run.bat.                 |
| «Falta la clave de Groq».                            | Escribe tu clave en .env (GROQ\_API\_KEY=) y reinicia Jarvis.                                                    |
| «Límite de uso de Groq alcanzado; reintento en X s». | Es normal con la cuenta gratuita: Jarvis espera unos segundos y lo reintenta solo.                               |
| Jarvis no me escucha.                                | Usa Chrome o Edge, revisa el permiso de micrófono (candado de la barra de direcciones) y di «Jarvis» al empezar. |
| «La autorización de Google venció».                  | Ejecuta gmail\_login.py y workspace\_login.py en la carpeta de Jarvis.                                           |
| Spotify no reproduce.                                | Abre la app de Spotify con tu sesión iniciada y vuelve a pedirlo.                                                |
| «No encontré la aplicación».                         | Revisa el nombre o di el nombre exacto con el que aparece en el menú Inicio.                                     |
| Olvidé la contraseña.                                | Sigue el apartado 4.4.                                                                                           |

## 13. Glosario

| **Término**                     | **Significado**                                                                          |
| ------------------------------- | ---------------------------------------------------------------------------------------- |
| Asistente / Jarvis              | El programa con el que conversas y que realiza acciones por ti.                          |
| Inteligencia artificial (IA)    | El «cerebro» que entiende tus peticiones y redacta las respuestas.                       |
| Herramienta                     | Una acción concreta que Jarvis sabe hacer: buscar, abrir una app, enviar un correo…      |
| Tarea autónoma                  | Un encargo de varios pasos que Jarvis planifica y realiza en segundo plano.              |
| Confirmación                    | El paso en el que apruebas («Confirmar») o descartas («Cancelar») una acción sensible.   |
| Rutina                          | Una frase que dispara varias acciones a la vez.                                          |
| Verificación en dos pasos (2FA) | Seguridad extra: además de la contraseña, un código del celular.                         |
| Código de respaldo              | Código de un solo uso que reemplaza al de Google Authenticator si pierdes el celular.    |
| Integración                     | Conexión de Jarvis con un servicio externo (Gmail, Google Calendar, Spotify, Telegram…). |

## 14. Soporte

Si tienes dudas o encuentras un error, contacta al equipo StarkNova:

| **Integrante**  | **Rol**                            | **Correo**                   |
| --------------- | ---------------------------------- | ---------------------------- |
| Fabio Morales   | Líder del proyecto                 | fabio.morales00@usc.edu.co   |
| Juan Arias      | Datos, fullstack e integraciones   | juan.arias23@usc.edu.co      |
| Juan Bustamante | Fullstack y experiencia de usuario | juan.bustamante05@usc.edu.co |
| Nicolás Muñoz   | Requisitos y pruebas               | nicolas.munoz02@usc.edu.co   |
