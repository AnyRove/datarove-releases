<p align="center"><img src="docs/images/icon.svg" width="96" alt="DataRove"></p>

<h1 align="center">DataRove</h1>

<p align="center"><b>Administra todas tus bases de datos desde el ordenador y consúltalas desde el móvil, con cifrado de extremo a extremo.</b></p>

<p align="center">
<a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a> · <a href="README.zh-TW.md">繁體中文</a> · <a href="README.ja.md">日本語</a> · <a href="README.ko.md">한국어</a> · <b>Español</b> · <a href="README.fr.md">Français</a> · <a href="README.de.md">Deutsch</a> · <a href="README.pt-BR.md">Português</a> · <a href="README.ru.md">Русский</a>
</p>

---

DataRove es un cliente de bases de datos de escritorio con copias de seguridad programadas y sincronización integradas. Se ejecuta en tu ordenador (como aplicación o como servicio en segundo plano) y el móvil se conecta a él por la red local o a través de un relay. La conexión está cifrada de extremo a extremo y las contraseñas de las bases de datos nunca salen del ordenador.

**Bases de datos:** MySQL · MariaDB · PostgreSQL · SQLite · SQL Server · MongoDB · Redis, con túneles SSH y TLS.

> **Estado:** 1.0.0. Los instaladores se descargan en la página de [Releases](../../releases); después, las aplicaciones se actualizan solas. Usa «Watch» en este repositorio para recibir avisos de nuevas versiones. Cambios en el [CHANGELOG](CHANGELOG.md).

## Recorrido de 4 minutos en vídeo

<p align="center"><a href="docs/video/DataRove-promo.mp4"><img src="docs/video/DataRove-promo-poster.jpg" width="720" alt="Recorrido por DataRove (vídeo, narración en inglés, subtítulos en inglés y chino)"></a></p>
<p align="center"><a href="docs/video/DataRove-promo.mp4">Ver el vídeo</a> (MP4, 1080p, narración en inglés con subtítulos en inglés y chino) · subtítulos SRT: <a href="docs/video/DataRove-promo.en.srt">English</a>, <a href="docs/video/DataRove-promo.zh-CN.srt">简体中文</a></p>

![Resumen](docs/images/overview.png)

## Funciones

### Explorar y editar datos
- Cuadrícula rápida con desplazamiento virtual, filtros, ordenación y paginación. Edita celdas en el sitio o abre una fila completa como formulario, con un campo adecuado para cada tipo de columna (booleano, enum, fecha, JSON, NULL / valor por defecto).
- En las columnas de clave foránea puedes buscar en la tabla referenciada y elegir una fila.
- Revisa el SQL exacto antes de guardar. Todos los cambios se confirman en una sola transacción y se deshacen juntos si hay un error. Los borrados y las conexiones de producción piden confirmación.
- Copiar como CSV / TSV / JSON / SQL / Markdown, exportar archivos, importar CSV / JSON.

![Cuadrícula de datos](docs/images/data.png)

### Escribir consultas
- Editor SQL con autocompletado según el contexto: tablas tras `FROM` / `JOIN`, columnas tras `alias.`, listas de columnas de `INSERT`, funciones con su firma y plantillas de sentencias.
- Al pasar el ratón por una tabla o columna se ven su estructura, tipos y comentarios. Los nombres de tabla incorrectos, los `alias.columna` inexistentes y los `DELETE` / `UPDATE` sin `WHERE` se subrayan antes de ejecutar.
- El resultado de un `SELECT` sobre una sola tabla que incluya la clave primaria se puede editar y guardar directamente.
- Sintaxis del shell de MongoDB (`db.users.find({...})`) y línea de comandos de Redis, ambas con autocompletado.

![Editor de consultas](docs/images/query.png)

### Entender el esquema
- Bases de datos → tablas / vistas / colecciones → columnas, índices, claves foráneas, triggers, DDL, número de filas y tamaño.
- Diagrama ER, comparación de esquemas entre dos bases con SQL de migración generado, explorador de claves de Redis y monitor del servidor (conexiones, QPS, consultas lentas).

![Diagrama ER](docs/images/er.png)

### Importar y exportar
- Exporta una tabla o un resultado filtrado como CSV, TSV, JSON, JSON Lines, SQL `INSERT` o Markdown; importa CSV / TSV / JSON / JSON Lines (reconoce UTF-8 y GBK).
- Las transferencias grandes se ejecutan en segundo plano en el ordenador, con progreso en vivo y botón de cancelar. Los archivos viajan en trozos pequeños, así que una conexión cortada se reanuda en vez de empezar de nuevo.
- Una importación se ejecuta en una sola transacción: si falla una fila, no se escribe nada y se indica qué fila falló.

### Copias de seguridad fiables
- Copia a archivos con las herramientas nativas (mysqldump / pg_dump / mongodump) o el exportador integrado, compresión gzip y cifrado AES-256-GCM, conservando los últimos N archivos o N días.
- Cada copia nueva se vuelve a leer justo después de escribirse (suma de verificación, descifrado y análisis), así que un archivo dañado se detecta esa misma noche.
- Sincronización con otra base de datos: reemplazo completo, vaciar y cargar, añadir, upsert por clave primaria o sincronización **incremental** mediante una columna de marca (`updated_at`, id autoincremental…).
- Programación: cada N minutos, a diario, semanal, mensual o con cron. Las ejecuciones perdidas se recuperan al arrancar. Avisos de escritorio y por webhook. Restaura cualquier copia en cualquier conexión con un clic.
- La sincronización también funciona entre tipos de base de datos (por ejemplo PostgreSQL → MySQL). Cada tabla se escribe completa o no se toca, y se comparan los recuentos de filas antes de confirmar. Los errores temporales se reintentan solos; si algo sigue fallando, **reintenta solo las partes fallidas**. Las herramientas que falten (pg_dump, etc.) se instalan desde la app con un clic.

![Copias de seguridad](docs/images/backups.png)

### Deshacer errores
- En las conexiones marcadas como *producción*, DataRove guarda las tablas afectadas antes de un `UPDATE`, `DELETE`, `DROP`, `TRUNCATE`, edición en la cuadrícula, importación que sobrescribe, sincronización o restauración.
- Deshazlo después con un clic: solo se revierten las filas que difieren, en una transacción. Las instantáneas se guardan cifradas en el ordenador.

### Tu móvil, con seguridad
- La misma interfaz en el móvil. Escanea un código QR para emparejarlo, sin necesidad de cuenta.
- Cada móvil tiene un rol (solo lectura, lectura y escritura o administrador) que puedes cambiar o retirar cuando quieras. Los permisos temporales caducan solos y un registro de actividad muestra qué ha hecho cada móvil.
- En la misma red Wi-Fi el móvil se conecta directamente; fuera, pasa por un relay que solo reenvía texto cifrado.

<p align="center"><img src="docs/images/phone-data.png" width="300" alt="DataRove en un móvil"></p>

### Escritorio
Se queda en la bandeja del sistema, arranca al iniciar sesión y puede instalarse como servicio en segundo plano (launchd / systemd / Programador de tareas de Windows), para que las copias programadas sigan con la ventana cerrada. 10 temas, claro y oscuro, 15 idiomas de interfaz.

## Seguridad en resumen

- Las contraseñas guardadas se cifran en el ordenador con una clave maestra local. La interfaz y los móviles emparejados solo ven `••••`.
- El emparejamiento por QR usa un secreto de 256 bits; el código corto escrito a mano se refuerza con scrypt. Cada parte autentica la clave de la otra, así que un relay no puede colar un dispositivo propio.
- Cada sesión usa X25519 + ChaCha20-Poly1305 con claves efímeras nuevas (secreto perfecto hacia adelante). Las tramas alteradas, repetidas o desordenadas se rechazan, igual que los intentos de degradar el protocolo.
- Un relay público debe usar `wss://`. Puedes montar tu propio relay, y el emparejamiento puede hacerse solo en la red local.
- Todo lo que va más allá de DataRove queda reservado al propio ordenador: rutas de herramientas, explorar archivos, acceder a archivos SQLite y enviar una contraseña guardada a un host modificado.

Para informar de una vulnerabilidad, consulta [SECURITY.md](SECURITY.md).

## Plataformas

| | |
|---|---|
| macOS | Apple silicon e Intel |
| Windows | 10 / 11, x64 |
| Linux | AppImage, deb |
| Android | App (compañera de un ordenador, o modo directo) |
| Móvil | Cualquier navegador móvil actual (instalable como aplicación web) |

## Instalación

Descarga el archivo para tu sistema desde la [última versión](../../releases/latest). Cada versión incluye las sumas SHA-256 en `SHA256SUMS.txt`. Los instaladores aún no están firmados con un certificado de desarrollador de Apple o Microsoft, así que el sistema pregunta una vez:

- **macOS**: abre el `.dmg` y arrastra DataRove a Aplicaciones. Si al abrirla por primera vez macOS dice que no puede verificar la app, abre **Ajustes del Sistema → Privacidad y seguridad**, desplázate hacia abajo y haz clic en **Abrir igualmente**. Si macOS dice que la app está dañada, ejecuta `xattr -cr /Applications/DataRove.app` en Terminal y vuelve a abrirla.
- **Windows**: ejecuta el `.exe`. Si aparece SmartScreen, haz clic en **Más información → Ejecutar de todas formas**.
- **Linux**: da permiso de ejecución al `.AppImage` y ejecútalo, o instala el `.deb`.
- **Android**: abre el `.apk` y permite la instalación desde tu navegador o gestor de archivos. La app se empareja con DataRove en tu ordenador o, en modo directo, se conecta ella misma a tus bases de datos (activa el teléfono una vez con tu cuenta de DataRove).

Después inicia sesión en la app con tu correo y activa el ordenador. DataRove es gratuito; no se necesitan datos de pago.

## Licencia, aviso legal y versiones

- DataRove es gratuito por ahora, con todas sus funciones. No se necesitan datos de pago.
- DataRove es software propietario, no de código abierto. El uso de la app se rige por el Contrato de licencia de usuario final, el Aviso de privacidad y las Condiciones de compra que la app muestra al iniciarse por primera vez y que aceptas allí. Consulta [LICENSE.md](LICENSE.md) (prevalece el texto en inglés).
- DataRove modifica bases de datos reales. Conserva copias de seguridad independientes y probadas, y lee el [aviso legal](DISCLAIMER.md).
- Las versiones siguen [Versionado Semántico](https://semver.org/lang/es/). Cada versión detalla sus cambios en [CHANGELOG.md](CHANGELOG.md) e incluye un `SHA256SUMS.txt`.

## Comentarios

Los informes de errores y las sugerencias son bienvenidos en [Issues](../../issues). Este repositorio contiene las notas de versión y los instaladores; el código fuente no es público.
