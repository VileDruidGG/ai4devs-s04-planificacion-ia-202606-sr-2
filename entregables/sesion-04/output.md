# Backlog FlowSync — MVP

> **Fuente**: PRD FlowSync v1.0 · Q3 2026
> **Alcance**: solo funcionalidades de la sección MVP del PRD.
> **Convención**: lo que no aparece literal en el PRD pero es necesario para que la historia sea coherente se marca con **(asumido)**.

**Notas de alcance (decisiones de PO):**
- No se generan historias para nada fuera del MVP (equipos/tareas compartidas, calendarios no-Google, notificaciones, app nativa, etiquetas/proyectos/subtareas, recordatorios configurables).
- La **sincronización inversa** (editar un evento en Google y que se refleje en la tarea) **no se descompone**: el PRD la deja pendiente de un spike técnico antes de comprometerla. Las historias de la Épica 5 cubren la dirección FlowSync → Google Calendar.
- Los requisitos no funcionales (rendimiento <1s, errores comprensibles, web responsive, logs de sincronización) se incorporan como criterios de aceptación dentro de las historias correspondientes, no como historias aparte.

---

## Épica 1 — Autenticación y gestión de cuenta

### Historia 1.1 — Registro
Como visitante, quiero registrarme con mi correo y contraseña, para tener una cuenta propia en FlowSync.

- GIVEN estoy en la pantalla de registro, WHEN ingreso un correo válido y una contraseña de al menos 8 caracteres, THEN mi cuenta se crea y accedo a la aplicación.
- GIVEN ingreso una contraseña de menos de 8 caracteres, WHEN intento registrarme, THEN veo un mensaje comprensible indicando que la contraseña debe tener al menos 8 caracteres y la cuenta no se crea.
- GIVEN ingreso un correo con formato inválido **(asumido: validación de formato de email)**, WHEN intento registrarme, THEN veo un error claro y la cuenta no se crea.
- GIVEN dejo vacío el correo o la contraseña, WHEN intento registrarme, THEN veo un mensaje indicando que el campo es obligatorio y no se crea la cuenta.

### Historia 1.2 — Correo ya registrado
Como visitante con un correo ya registrado, quiero que el sistema me avise, para ir directamente al inicio de sesión.

- GIVEN ingreso un correo que ya existe, WHEN intento crear la cuenta, THEN veo un mensaje indicando que el correo ya está en uso y no se crea una cuenta duplicada.
- GIVEN veo el aviso de correo ya registrado, WHEN el sistema me lo muestra, THEN se me ofrece una vía para ir al inicio de sesión.
- GIVEN voy al inicio de sesión desde ese aviso **(asumido: el correo se conserva en el campo)**, WHEN llego a la pantalla de login, THEN puedo autenticarme con ese correo.

### Historia 1.3 — Inicio de sesión
Como usuario registrado, quiero iniciar sesión con mi correo y contraseña, para acceder a mis tareas.

- GIVEN estoy en la pantalla de inicio de sesión, WHEN ingreso un correo y contraseña correctos, THEN se inicia mi sesión y accedo a mis tareas.
- GIVEN ingreso credenciales incorrectas, WHEN intento iniciar sesión, THEN veo un mensaje de error comprensible y no accedo.
- GIVEN inicio sesión correctamente, WHEN la sesión se establece, THEN se mantiene mediante un token de acceso sin pedirme credenciales en cada acción.

### Historia 1.4 — Cierre de sesión
Como usuario autenticado, quiero cerrar sesión, para proteger el acceso a mi cuenta.

- GIVEN estoy autenticado, WHEN selecciono cerrar sesión, THEN mi sesión termina y dejo de tener acceso a mis tareas.
- GIVEN cerré sesión, WHEN intento acceder a una pantalla que requiere autenticación **(asumido)**, THEN se me redirige al inicio de sesión.

### Historia 1.5 — Bienvenida / onboarding mínimo
Como usuario recién registrado, quiero ver una pantalla de bienvenida que explique qué hace FlowSync, para entender el producto y crear mi primera tarea.

- GIVEN completé el registro con éxito, WHEN entro por primera vez, THEN veo una pantalla de bienvenida que explica en una frase qué hace FlowSync.
- GIVEN estoy en la pantalla de bienvenida, WHEN la leo, THEN se me invita a crear mi primera tarea.
- GIVEN acepto la invitación, WHEN la selecciono, THEN llego al flujo de creación de tarea.

### Historia 1.6 — Aislamiento de datos por usuario
Como usuario, quiero que mis datos solo sean accesibles por mí, para mantener mi información privada.

- GIVEN estoy autenticado, WHEN consulto mis tareas, THEN solo veo mis propias tareas y nunca las de otro usuario.
- GIVEN no estoy autenticado, WHEN intento acceder a datos de tareas **(asumido)**, THEN el acceso se rechaza.
- GIVEN tengo una cuenta de Google conectada, WHEN se almacenan mis tokens, THEN se guardan de forma segura y no son accesibles por otros usuarios.

---

## Épica 2 — Gestión de tareas (CRUD)

### Historia 2.1 — Crear tarea
Como usuario, quiero crear una tarea indicando al menos un título, para registrar un pendiente.

- GIVEN estoy en el formulario de creación de tarea, WHEN ingreso un título y guardo, THEN la tarea se crea y aparece en mi listado.
- GIVEN dejo el título vacío, WHEN intento guardar, THEN veo un mensaje indicando que el título es obligatorio y la tarea no se crea.
- GIVEN creo una tarea sin descripción ni fecha límite, WHEN guardo, THEN la tarea se crea correctamente con esos campos vacíos.
- GIVEN creo una tarea, WHEN se guarda, THEN nace con estado `pending`.

### Historia 2.2 — Campos opcionales (descripción y fecha límite)
Como usuario, quiero añadir una descripción y una fecha límite opcionales a mi tarea, para darle más contexto y plazo.

- GIVEN estoy creando o editando una tarea, WHEN añado una descripción, THEN se guarda junto con la tarea.
- GIVEN estoy creando o editando una tarea, WHEN añado una fecha límite, THEN se guarda junto con la tarea.
- GIVEN no añado descripción ni fecha límite, WHEN guardo, THEN la tarea se guarda sin esos campos y sin error.

### Historia 2.3 — Ver listado de tareas
Como usuario, quiero ver el listado de mis tareas, para saber qué tengo pendiente.

- GIVEN tengo tareas creadas, WHEN entro al listado, THEN veo mis tareas con al menos su título y estado **(asumido)**.
- GIVEN tengo tareas con fecha límite, WHEN veo el listado, THEN la fecha límite es visible **(asumido)**.
- GIVEN tengo hasta 200 tareas, WHEN abro el listado, THEN carga en menos de 1 segundo.

### Historia 2.4 — Editar tarea
Como usuario, quiero editar cualquier campo de una tarea existente, para corregirla o actualizarla.

- GIVEN selecciono una tarea existente, WHEN edito su título, descripción o fecha límite y guardo, THEN los cambios se reflejan en el listado.
- GIVEN edito el título y lo dejo vacío, WHEN intento guardar, THEN veo el error de título obligatorio y el cambio no se guarda.
- GIVEN edito un campo de la tarea, WHEN guardo, THEN los demás campos no modificados permanecen intactos.

### Historia 2.5 — Borrar tarea
Como usuario, quiero borrar una tarea, para eliminar lo que ya no necesito.

- GIVEN selecciono una tarea, WHEN la borro, THEN desaparece de mi listado.
- GIVEN inicio el borrado **(asumido: con confirmación previa para evitar borrados accidentales)**, WHEN confirmo, THEN la tarea se elimina.
- GIVEN la tarea borrada tenía un evento sincronizado, WHEN se borra, THEN se dispara la eliminación del evento en Google Calendar (ver Historia 5.4).

### Historia 2.6 — Cambiar estado de la tarea
Como usuario, quiero cambiar el estado de una tarea entre pendiente, completada y archivada, para reflejar su avance.

- GIVEN una tarea en estado `pending`, WHEN la marco como completada, THEN su estado pasa a `completed`.
- GIVEN una tarea, WHEN la archivo, THEN su estado pasa a `archived`.
- GIVEN una tarea completada o archivada, WHEN cambio su estado de vuelta a `pending` **(asumido: el cambio de estado es reversible)**, THEN se actualiza correctamente.
- GIVEN cambio el estado de una tarea, WHEN el cambio se guarda, THEN se refleja de inmediato en el listado.

---

## Épica 3 — Organización y filtrado

### Historia 3.1 — Filtrar por estado
Como usuario, quiero filtrar mis tareas por estado, para ver solo las que me interesan en ese momento.

- GIVEN tengo tareas en distintos estados, WHEN filtro por `pending`, THEN solo veo las tareas pendientes.
- GIVEN aplico el filtro por `completed`, WHEN se aplica, THEN solo veo las tareas completadas.
- GIVEN aplico el filtro por `archived`, WHEN se aplica, THEN solo veo las tareas archivadas.
- GIVEN tengo un filtro activo, WHEN lo quito **(asumido)**, THEN vuelvo a ver todas mis tareas.

### Historia 3.2 — Orden por defecto priorizando "hoy"
Como usuario, quiero que el listado priorice por defecto las tareas más relevantes para "hoy", para enfocarme en lo inmediato.

- GIVEN tengo tareas con distintas fechas límite, WHEN abro el listado sin filtros, THEN las tareas más relevantes para hoy aparecen primero **(el criterio exacto de ordenación queda a definir en refinamiento, según el PRD)**.
- GIVEN el criterio de ordenación se define en refinamiento, WHEN se implementa, THEN el orden por defecto es consistente y predecible **(asumido)**.
- GIVEN ninguna de mis tareas tiene fecha límite **(asumido)**, WHEN abro el listado, THEN igualmente veo un orden por defecto estable.

### Historia 3.3 — Estado vacío
Como usuario sin tareas, quiero ver un estado vacío con una invitación a crear la primera, para saber cómo empezar.

- GIVEN mi cuenta es nueva y no tengo tareas, WHEN abro el listado, THEN veo un estado vacío con una invitación a crear mi primera tarea.
- GIVEN tenía tareas pero el listado por defecto no muestra ninguna (p. ej. todas archivadas) **(asumido)**, WHEN abro el listado por defecto, THEN veo el estado vacío con la invitación.
- GIVEN estoy en el estado vacío, WHEN acepto la invitación, THEN llego al formulario de creación de tarea.

---

## Épica 4 — Exportación

### Historia 4.1 — Exportar tareas a CSV
Como usuario, quiero exportar mis tareas a un archivo CSV, para poder llevarme mis datos.

- GIVEN tengo tareas creadas, WHEN selecciono exportar a CSV, THEN obtengo un archivo CSV con mis tareas.
- GIVEN se genera el CSV, WHEN lo abro, THEN cada tarea incluye al menos título, descripción, estado y fecha límite.
- GIVEN no tengo tareas **(asumido)**, WHEN exporto, THEN obtengo un CSV solo con los encabezados o un aviso de que no hay tareas que exportar.
- GIVEN exporto, WHEN se genera el archivo, THEN solo contiene mis propias tareas y no las de otros usuarios.

---

## Épica 5 — Sincronización con Google Calendar

### Historia 5.1 — Conectar cuenta de Google (OAuth)
Como usuario, quiero conectar mi cuenta de Google a FlowSync mediante OAuth, para sincronizar mis tareas con mi calendario.

- GIVEN estoy autenticado en FlowSync, WHEN inicio la conexión con Google, THEN se me lleva al flujo de autorización OAuth de Google.
- GIVEN autorizo los permisos solicitados, WHEN completo el flujo OAuth, THEN mi cuenta de Google queda conectada a FlowSync.
- GIVEN deniego los permisos o cancelo el flujo **(asumido)**, WHEN vuelvo a FlowSync, THEN veo que la conexión no se completó y mis tareas siguen intactas.
- GIVEN la conexión se completa, WHEN se guardan los tokens de Google, THEN se almacenan de forma segura.

### Historia 5.2 — Tareas con fecha como eventos
Como usuario con Google conectado, quiero que mis tareas con fecha límite aparezcan como eventos en mi Google Calendar, para verlas junto a mi agenda.

- GIVEN tengo Google conectado y creo una tarea con fecha límite, WHEN se guarda, THEN se crea un evento correspondiente en mi Google Calendar.
- GIVEN creo una tarea sin fecha límite, WHEN se guarda, THEN no se crea ningún evento en el calendario.
- GIVEN no tengo Google conectado, WHEN creo una tarea con fecha límite, THEN no se intenta crear ningún evento.
- GIVEN se crea un evento, WHEN se determina su hora **(asumido: derivada de la fecha límite, con manejo cuidadoso de la zona horaria)**, THEN coincide con la fecha límite de la tarea.

### Historia 5.3 — Actualizar evento al cambiar la fecha
Como usuario, quiero que al cambiar la fecha de una tarea se actualice su evento en Google Calendar, para mantener mi agenda al día.

- GIVEN una tarea con fecha límite ya sincronizada como evento, WHEN cambio su fecha en FlowSync, THEN el evento correspondiente en Google Calendar se actualiza.
- GIVEN una tarea sin fecha límite (sin evento aún), WHEN le añado una fecha, THEN se crea su evento en el calendario.
- GIVEN una tarea con evento, WHEN le quito la fecha límite **(asumido)**, THEN el evento correspondiente se elimina del calendario.

### Historia 5.4 — Eliminar/marcar evento al completar o borrar
Como usuario, quiero que al completar o borrar una tarea su evento en Google Calendar se elimine o marque, para que mi calendario refleje solo lo vigente.

- GIVEN una tarea sincronizada como evento, WHEN la marco como completada, THEN el evento se elimina o se marca según corresponda en Google Calendar.
- GIVEN una tarea sincronizada como evento, WHEN la borro, THEN el evento correspondiente se elimina de Google Calendar.
- GIVEN una tarea sin evento (sin fecha límite), WHEN la completo o borro, THEN no se realiza ninguna acción sobre el calendario.

### Historia 5.5 — Desconectar Google sin perder tareas
Como usuario, quiero desconectar mi cuenta de Google cuando quiera sin perder mis tareas, para tener control sobre la integración.

- GIVEN tengo Google conectado, WHEN selecciono desconectar, THEN FlowSync deja de sincronizar con Google Calendar.
- GIVEN desconecto Google, WHEN se completa la desconexión, THEN mis tareas ya creadas permanecen intactas en FlowSync.
- GIVEN desconecté Google, WHEN creo o edito tareas **(asumido)**, THEN no se intenta sincronizar con el calendario hasta volver a conectar.

### Historia 5.6 — Tolerancia a fallos de la API de Google
Como usuario, quiero que mis tareas se guarden aunque la sincronización con Google falle, para no perder mi trabajo ante errores de la API.

- GIVEN la API de Google no está disponible o devuelve error, WHEN creo o edito una tarea, THEN la tarea se guarda en FlowSync aunque la sincronización falle.
- GIVEN una operación de sincronización falló, WHEN el sistema lo detecta, THEN se reintenta más tarde.
- GIVEN ocurre un fallo de sincronización, WHEN sucede, THEN queda registrado en logs para diagnóstico.
- GIVEN una sincronización falló de forma temporal, WHEN el reintento tiene éxito **(asumido)**, THEN el evento queda finalmente reflejado en el calendario.
