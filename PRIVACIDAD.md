# Política de Privacidad — Matechat

**Última actualización:** 12 de noviembre de 2026
**Versión:** 1.0
**Bot:** Matechat — usuario de Discord `1062131587380170813`
**Repositorio:** https://github.com/Kevinmoth/MateWoWChat
**Contacto:** https://github.com/Kevinmoth/MateWoWChat/issues

---

## 1. Quién es el responsable

Matechat ("el bot") es un bot de Discord desarrollado y operado por **Kevinmoth** ("el operador").

Matechat es un proyecto independiente. **No está afiliado, asociado ni respaldado por Discord Inc., Blizzard Entertainment ni el servidor de World of Warcraft con el que se conecte** (por defecto, NaerZone / `comunidad.naerzone.com`).

Esta política describe qué datos trata el bot cuando se lo invita a un servidor de Discord y se lo conecta a una cuenta de World of Warcraft.

---

## 2. Qué hace el bot

Matechat funciona como puente entre canales de Discord y el chat dentro del juego:

- Retransmite mensajes de Discord hacia canales del chat de WoW (según la configuración del operador).
- Retransmite mensajes del chat de WoW hacia canales de Discord.
- Responde a comandos escritos en Discord (`?who`, `?online`, `?gmotd`) con información del juego.
- Publica un estado personalizado ("N guildies online") calculado a partir de la lista de miembros de la hermandad **en el juego**.

---

## 3. Qué datos trata

### 3.1 Datos provenientes de Discord

| Dato | Para qué |
|---|---|
| Contenido de los mensajes de texto (incluido el texto, emojis y las URL de archivos adjuntos) | Retransmitirlo al chat del juego y ejecutar comandos |
| ID y nombre visible del autor (apodo del servidor) | Identificar quién habla en el juego (`%user`) y resolver menciones |
| ID y nombre del canal y del servidor | Decidir a qué canal del juego corresponde cada mensaje |
| Lista de miembros del servidor (ID, nombre de usuario, apodo) | Traducir `@Nombre` escrito en el juego a una mención real de Discord |
| Lista de roles del servidor (ID y nombre) | Traducir `@Rol` escrito en el juego a una mención de rol |
| Lista de emojis personalizados del servidor (nombre e ID) | Convertir `:emoji:` en el emoji correspondiente |

**Alcance de la lectura de mensajes:** una vez invitado, el bot recibe los mensajes de **todos los canales de texto donde tenga permiso de lectura**, no únicamente de los canales configurados. El bot solo **retransmite** los canales configurados; el resto se descarta. Los canales privados y los que el bot no puede ver quedan fuera de su alcance.

**Qué NO hace el bot:**

- No lee mensajes directos (DMs).
- No accede al historial de mensajes anterior al momento en que se conectó: solo recibe mensajes en tiempo real mientras está en línea.
- No solicita ni recibe presencias de Discord: **no puede saber si un usuario está conectado, ausente o desconectado**.
- No accede a canales de voz ni a estados de voz.
- No lee correos, teléfonos ni datos de facturación de las cuentas de Discord.

### 3.2 Datos provenientes del juego

| Dato | Para qué |
|---|---|
| Nombre y clase del personaje, y credenciales de la cuenta de juego | Iniciar sesión en el servidor de WoW (las provee el propio operador en su configuración) |
| Mensajes del chat del juego (canal, autor y texto) | Retransmitirlos a Discord |
| Lista de miembros de la hermandad: nombre, nivel, clase, zona, estado en línea y última desconexión | Mostrar el estado "N guildies online" y responder `?who` |
| Mensaje del día de la hermandad (MOTD) y notificaciones del servidor | Retransmitirlas a Discord si están configuradas |

### 3.3 Registros (logs)

El bot escribe registros en la salida estándar (consola) del servidor donde se ejecuta. A nivel informativo, esos registros incluyen **el texto completo de los mensajes retransmitidos y el nombre del autor**, además de mensajes de diagnóstico y errores.

El bot **no** escribe los mensajes en archivos propios ni en una base de datos: es el sistema de hosting (por ejemplo, systemd/journald, Docker o el panel de alojamiento) el que conserva esa salida.

---

## 4. Base legal del tratamiento

El operador trata estos datos para:

- **Ejecutar el servicio** que solicita el administrador del servidor al invitar y configurar el bot.
- **Interés legítimo** en mantener el bot funcionando, seguro y libre de abuso (diagnóstico, prevención de spam, resolución de errores).
- **Consentimiento** del administrador que configura el bot y de los usuarios que participan en los canales habilitados.

Cuando el bot se instala en un servidor de terceros, el administrador de ese servidor decide qué canales se conectan y es responsable de informar a sus miembros. En ese caso, el operador actúa como **encargado del tratamiento** por cuenta del administrador.

---

## 5. Con quién se comparten los datos

Matechat **no vende, alquila ni cede datos a anunciantes**. Los datos solo pasan por los servicios necesarios para funcionar:

| Destino | Qué se envía | Cuándo |
|---|---|---|
| **Discord** (API y gateway) | Los mensajes y metadatos que el bot envía o recibe | Siempre que el bot está conectado |
| **Servidor de World of Warcraft** (NaerZone por defecto) | Los mensajes retransmitidos y las credenciales de la cuenta de juego | Siempre que hay una sesión activa |
| **GitHub** (`api.github.com`) | Una consulta `GET` para comprobar si hay una versión nueva del bot. No incluye datos de usuarios: solo IP, User-Agent y la versión del bot | Una vez, al arrancar |
| **comunidadnaer.com** | Nada de forma automática. El bot solo genera enlaces; la visita ocurre si un usuario hace clic | Al hacer clic en un enlace |

No hay servicios de analítica, publicidad, telemetría ni webhooks de terceros.

---

## 6. Dónde se guardan y por cuánto tiempo

El bot **no utiliza base de datos**. Todo el estado operativo vive en memoria y se pierde al reiniciar:

| Dato | Ubicación | Retención |
|---|---|---|
| Mensajes recientes para detectar duplicados | Memoria (RAM) | Ventana de 15 segundos, máximo 10 mensajes por canal |
| Lista de miembros de la hermandad (nombre, nivel, clase, zona, estado) | Memoria (RAM) | Se refresca cada 61 segundos y se descarta al desconectarse |
| Caché de nombres de jugadores vistos | Memoria (RAM) | Máximo 10.000 entradas (se descartan las más antiguas) |
| Mensajes en espera de resolución de nombre | Memoria (RAM) | Hasta que el servidor responde con el nombre (segundos) |
| Estado "N guildies online" | Memoria (RAM) | Hasta el siguiente refresco |
| Registros de la consola | Hosting del operador | Según la configuración del hosting (puede ser desde horas hasta meses) |
| Archivo de configuración con credenciales | Disco del servidor del operador | Hasta que el operador lo borre o rote |

Las credenciales de la cuenta de juego y el token de Discord se guardan **en texto plano** en el archivo de configuración del servidor donde corre el bot. No se transmiten a nadie más que al servicio correspondiente (Discord y el servidor de WoW).

---

## 7. Seguridad

- La comunicación con Discord viaja cifrada (HTTPS/WSS).
- La conexión con el servidor de WoW utiliza el protocolo del juego (no TLS); el operador debe confiar en la red y el servidor de destino.
- El bot solicita únicamente los permisos e intents que necesita para funcionar.
- El acceso al servidor donde corre el bot y a su archivo de configuración queda bajo responsabilidad del operador.

Ningún sistema es completamente seguro: si detectás una vulnerabilidad, reportala por el canal de contacto antes de hacerla pública.

---

## 8. Tus derechos

Podés solicitar:

- **Acceso** a los datos que el bot trata sobre vos.
- **Rectificación** de datos incorrectos.
- **Supresión** de tus datos.
- **Oposición** o **limitación** al tratamiento.
- **Portabilidad** de los datos que hayas aportado.

Para ejercerlos, abrí un issue en https://github.com/Kevinmoth/MateWoWChat/issues o contactá al operador en el servidor donde está instalado el bot.

Tené en cuenta que:

- Los datos en memoria desaparecen al reiniciar el bot.
- Los mensajes publicados en Discord se rigen además por la política de privacidad de Discord y deben borrarse desde Discord.
- Los registros del hosting pueden requerir una solicitud manual al operador.

Responderemos en un plazo razonable, no mayor a **30 días**.

---

## 9. Menores de edad

El bot no está dirigido a menores de **13 años** (o la edad mínima legal para usar Discord en tu país, si es mayor). No se recopilan datos de forma consciente de menores por debajo de esa edad. Si detectás que un menor usó el bot, contactá al operador para eliminarlo.

---

## 10. Cambios a esta política

Esta política puede actualizarse para reflejar cambios en el bot o en la normativa. La versión vigente es siempre la publicada en este repositorio, con su fecha de actualización. Los cambios relevantes se anunciarán en el repositorio y, si corresponde, en los servidores donde está instalado el bot.

---

## 11. Contacto

- **Issues:** https://github.com/Kevinmoth/MateWoWChat/issues
- **Repositorio:** https://github.com/Kevinmoth/MateWoWChat
- **Operador:** Kevinmoth

Para consultas sobre datos personales, indicá en el asunto "Privacidad — Matechat" y el servidor o usuario involucrado.
