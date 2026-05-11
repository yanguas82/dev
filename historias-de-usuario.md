# Historias de Usuario — EduScience

**Plataforma:** EduScience — Plataforma educativa para el aprendizaje de Ciencias Naturales  
**URL:** https://edu-science-jade.vercel.app/  
**Institución configurada:** Institución Educativa Bello Horizonte  
**Tecnología:** React SPA + Supabase (PostgreSQL + Storage + Realtime)  
**Fecha de relevamiento:** 2026-05-11  

---

## Índice

1. [Roles del Sistema](#roles-del-sistema)
2. [Autenticación y Acceso](#1-autenticación-y-acceso)
3. [Dashboard / Panel Principal](#2-dashboard--panel-principal)
4. [Gestión de Módulos](#3-gestión-de-módulos)
5. [Gestión de Competencias](#4-gestión-de-competencias)
6. [Gestión de Preguntas](#5-gestión-de-preguntas)
7. [Gestión de Componentes](#6-gestión-de-componentes)
8. [Gestión de Grados](#7-gestión-de-grados)
9. [Gestión de Estudiantes](#8-gestión-de-estudiantes)
10. [Gestión de Docentes](#9-gestión-de-docentes)
11. [Resultados y Reportes](#10-resultados-y-reportes)
12. [Solicitudes de Reinicio de Evaluación](#11-solicitudes-de-reinicio-de-evaluación)
13. [Notificaciones](#12-notificaciones)
14. [Configuración del Sistema](#13-configuración-del-sistema)
15. [Gestión de Permisos](#14-gestión-de-permisos)
16. [Vista del Estudiante — Exámenes](#15-vista-del-estudiante--exámenes)

---

## Roles del Sistema

| Rol | Descripción |
|-----|-------------|
| **Administrador** (`admin`) | Acceso total e irrestricto al sistema: gestión de usuarios, módulos, configuración institucional y reportes globales |
| **Docente** (`docente`) | Gestión pedagógica: módulos asignados, competencias, preguntas, estudiantes y solicitudes de repetición de su grado |
| **Estudiante** (`estudiante`) | Acceso exclusivo a sus propias evaluaciones, historial y resultados |

---

## Esquema de la Base de Datos

El sistema utiliza las siguientes tablas en Supabase (PostgreSQL):

| Tabla | Descripción |
|-------|-------------|
| `profiles` | Perfil de todos los usuarios (nombre, apellido, avatar) |
| `user_roles` | Asignación de rol a cada usuario |
| `students` | Datos de estudiantes (documento, grado, foto) |
| `teachers` | Datos de docentes (documento, teléfono, es director de curso) |
| `teacher_modules` | Relación docente ↔ módulo asignado |
| `grades` | Grados académicos (ej: 10A, 10B, 11A, 11B) |
| `modules` | Módulos de competencias (nombre, icono, color, estado) |
| `competencies` | Competencias dentro de módulos (con fechas, OVA y grados asignados) |
| `competency_students` | Relación competencia ↔ estudiante |
| `questions` | Preguntas de opción múltiple (4 opciones, respuesta correcta, puntaje) |
| `question_components` | Componentes de clasificación (Entorno Vivo, Químico, Físico, CTS) |
| `exam_attempts` | Intentos de examen (respuestas, puntaje, fecha) |
| `retake_requests` | Solicitudes de repetición de prueba |
| `document_types` | Tipos de documento (CC, TI, RC, PA, CE) |
| `permissions_config` | Permisos personalizados por rol |
| `institutions` | Datos institucionales y configuración visual |
| `notifications` | Notificaciones del sistema |

**Buckets de almacenamiento (Supabase Storage):**
- `avatars` — Logos e imágenes de branding institucional
- `banners` — Imágenes del carrusel de login
- `ova-files` — Archivos OVA (materiales de estudio interactivos)
- `question-images` — Imágenes opcionales para preguntas
- `student-photos` — Fotos de perfil de estudiantes

---

## 1. Autenticación y Acceso

### HU-001 — Inicio de sesión
**Como** usuario registrado (administrador, docente o estudiante),  
**quiero** poder iniciar sesión con mi correo electrónico y contraseña,  
**para** acceder a mi espacio educativo personalizado según mi rol.

**Criterios de aceptación:**
- La pantalla de login muestra el logo institucional, nombre de la plataforma y slogan configurado.
- Se muestran los banners configurables en un carrusel con título, subtítulo e imagen de fondo.
- El formulario solicita correo electrónico (ej: `correo@ejemplo.com`) y contraseña (mínimo 6 caracteres).
- Al ingresar credenciales válidas, el sistema redirige al dashboard del rol correspondiente.
- Al ingresar credenciales incorrectas, se muestra un mensaje de error claro.
- El pie de página muestra el texto de copyright configurable.

---

### HU-002 — Acceso restringido por rol
**Como** sistema,  
**quiero** controlar el acceso a las diferentes secciones según el rol del usuario,  
**para** garantizar que cada actor solo opere las funcionalidades que le corresponden.

**Criterios de aceptación:**
- Un estudiante no puede acceder a rutas de administración (`/students`, `/teachers`, `/configuration`, `/permissions`, `/results`).
- Un docente no puede acceder a rutas exclusivas del administrador (`/permissions`, `/configuration`).
- Al intentar acceder a una ruta no autorizada, el sistema muestra un mensaje de "Acceso Restringido".
- Las rutas inexistentes muestran una página de error 404.
- La sesión persiste entre recargas de página.

---

### HU-003 — Personalización de la pantalla de login
**Como** administrador,  
**quiero** configurar la apariencia visual de la pantalla de inicio de sesión,  
**para** reflejar la identidad de la institución educativa en el acceso a la plataforma.

**Criterios de aceptación:**
- Se pueden configurar: logo institucional, nombre del software, slogan/descripción y texto de copyright.
- Se pueden agregar, editar y eliminar banners del carrusel (cada banner tiene título, subtítulo e imagen).
- Se puede configurar la velocidad de transición del carrusel de banners.
- Los cambios se reflejan inmediatamente en la pantalla de login.

---

## 2. Dashboard / Panel Principal

### HU-004 — Dashboard del administrador
**Como** administrador,  
**quiero** ver un panel de control con métricas globales del sistema,  
**para** tener una visión general del desempeño académico de la institución.

**Criterios de aceptación:**
- El dashboard muestra tarjetas con: total de evaluaciones realizadas, módulos activos, promedio general (%), estudiantes aprobados (≥60%) y reprobados (<60%), y evaluaciones pendientes.
- Se presentan gráficas de: Promedio por Grado, Evaluaciones por Grado, Tendencia Mensual y Preguntas por Competencia.
- Los datos se actualizan en tiempo real mediante Supabase Realtime.

---

### HU-005 — Dashboard del docente
**Como** docente,  
**quiero** ver un panel de control con métricas de los módulos y estudiantes a mi cargo,  
**para** monitorear el desempeño de mis grupos.

**Criterios de aceptación:**
- El dashboard muestra estadísticas filtradas por los módulos y grados asignados al docente.
- Se muestran las mismas gráficas que el administrador pero limitadas a los datos propios.
- Acceso a navegación rápida hacia gestión de módulos, estudiantes y resultados.

---

### HU-006 — Dashboard del estudiante
**Como** estudiante,  
**quiero** ver un panel de bienvenida con un resumen de mi actividad académica,  
**para** conocer rápidamente mi estado y acceder a mis evaluaciones.

**Criterios de aceptación:**
- El dashboard muestra: pruebas realizadas, pruebas pendientes y promedio general.
- Se presenta un botón "Ver Pruebas Disponibles" para acceder directamente a los exámenes.
- Si no hay evaluaciones realizadas, se muestra el mensaje: "Aún no has realizado ninguna evaluación".

---

## 3. Gestión de Módulos

### HU-007 — Listar módulos educativos
**Como** administrador o docente,  
**quiero** ver la lista de todos los módulos de competencias disponibles,  
**para** gestionar el contenido educativo de la plataforma.

**Criterios de aceptación:**
- La página `/modules` muestra todos los módulos con su nombre, icono, color e indicador de estado (activo/inactivo).
- El administrador ve todos los módulos; el docente solo ve los módulos asignados a él.
- Existe un campo de búsqueda por nombre de módulo.
- Los módulos inactivos se distinguen visualmente de los activos.

---

### HU-008 — Crear un nuevo módulo
**Como** administrador,  
**quiero** crear un nuevo módulo educativo con nombre, icono y color personalizados,  
**para** organizar las competencias de Ciencias Naturales en la plataforma.

**Criterios de aceptación:**
- El formulario "Nuevo Módulo" solicita: nombre (obligatorio), descripción, icono (seleccionable de biblioteca visual con opciones como Libro, ADN, Brote, Cerebro, etc.) y color.
- El color puede elegirse de una paleta predefinida, ingresarse como código HEX o generarse como degradado.
- Al guardar, el módulo aparece en la lista con estado activo por defecto.
- Se muestra un mensaje de éxito al crearse correctamente.

---

### HU-009 — Editar un módulo existente
**Como** administrador o docente,  
**quiero** editar la información de un módulo (nombre, descripción, icono, color),  
**para** mantener actualizado el contenido educativo.

**Criterios de aceptación:**
- Se pueden modificar todos los campos del módulo.
- Los cambios se guardan con "Guardar cambios" y se confirman con un mensaje de éxito.
- Los cambios son visibles de inmediato en la lista de módulos.

---

### HU-010 — Desactivar / Activar un módulo
**Como** administrador o docente (con permiso),  
**quiero** activar o desactivar un módulo,  
**para** controlar qué contenido está disponible para los estudiantes sin eliminarlo.

**Criterios de aceptación:**
- El módulo desactivado deja de ser visible para los estudiantes.
- El administrador puede reactivar un módulo en cualquier momento.
- El cambio de estado se refleja inmediatamente.

---

### HU-011 — Eliminar un módulo
**Como** administrador,  
**quiero** eliminar un módulo que ya no sea relevante,  
**para** mantener limpia la estructura educativa de la plataforma.

**Criterios de aceptación:**
- Solo el administrador puede eliminar módulos.
- El sistema solicita confirmación antes de eliminar.
- Se muestra un mensaje de éxito al eliminarse correctamente.

---

### HU-012 — Ver detalle de un módulo
**Como** administrador o docente,  
**quiero** ver el detalle de un módulo y sus competencias asociadas,  
**para** gestionar el contenido pedagógico de cada área.

**Criterios de aceptación:**
- Al hacer clic en un módulo, se navega a `/modules/:moduleId`.
- Se listan las competencias del módulo con su nombre, componente y estado.
- Se puede crear y gestionar competencias desde esta vista.

---

## 4. Gestión de Competencias

### HU-013 — Listar competencias de un módulo
**Como** administrador o docente,  
**quiero** ver las competencias asociadas a un módulo,  
**para** gestionar las evaluaciones que los estudiantes deben realizar.

**Criterios de aceptación:**
- Se muestran las competencias del módulo con: nombre, componente asignado, grados, fechas y estado.
- Los módulos existentes tienen competencias de Biología (Entorno Vivo), Química (Entorno Químico) y Física (Entorno Físico).

---

### HU-014 — Crear una competencia
**Como** administrador o docente,  
**quiero** crear una nueva competencia dentro de un módulo,  
**para** definir un área de evaluación específica con su contexto pedagógico.

**Criterios de aceptación:**
- El formulario solicita: nombre, descripción, componente (Entorno Vivo / Químico / Físico / CTS), fecha inicio, fecha fin, hora inicio.
- Se pueden asignar uno o más grados a la competencia (ej: 10A, 10B, 11A, 11B).
- Se puede subir un archivo OVA (Objeto Virtual de Aprendizaje: video o recurso interactivo) como material de estudio.
- Al guardar, la competencia queda disponible para los estudiantes de los grados asignados.

---

### HU-015 — Editar y desactivar una competencia
**Como** administrador o docente,  
**quiero** editar o desactivar una competencia,  
**para** actualizar el contenido o controlar su disponibilidad.

**Criterios de aceptación:**
- Se pueden editar todos los campos de la competencia.
- Se puede desactivar para que los estudiantes no vean la competencia.
- Los cambios se confirman con un mensaje de éxito.

---

### HU-016 — Eliminar una competencia
**Como** administrador,  
**quiero** eliminar una competencia,  
**para** depurar evaluaciones obsoletas del sistema.

**Criterios de aceptación:**
- Solo el administrador puede eliminar competencias.
- El sistema solicita confirmación previa.
- Se muestra mensaje de éxito al completarse.

---

## 5. Gestión de Preguntas

### HU-017 — Ver preguntas de una competencia
**Como** administrador o docente,  
**quiero** ver las preguntas configuradas para una competencia,  
**para** revisar el banco de preguntas disponible para la evaluación.

**Criterios de aceptación:**
- La ruta `/modules/:moduleId/competencies/:competencyId` muestra el listado de preguntas.
- Se muestran: número de orden, enunciado (o imagen si la tiene), opciones A/B/C/D, respuesta correcta, componente y puntaje.
- Se indica el total de preguntas y el puntaje máximo acumulado.

---

### HU-018 — Crear una pregunta de opción múltiple
**Como** administrador o docente,  
**quiero** crear preguntas de opción múltiple para una competencia,  
**para** construir las evaluaciones de Ciencias Naturales.

**Criterios de aceptación:**
- El formulario "Nueva Pregunta" solicita: enunciado (obligatorio), imagen opcional (JPG/PNG/WebP, máx. 5 MB), 4 opciones de respuesta (A, B, C, D) y selección de la opción correcta.
- Se asigna un puntaje por pregunta (valor por defecto: 5 o 10 puntos).
- Se asocia a un componente (Entorno Vivo, Químico, Físico o CTS).
- El número de pregunta es secuencial y obligatorio.
- Se muestra mensaje de éxito al crearse.

---

### HU-019 — Importar preguntas masivamente desde Excel
**Como** administrador,  
**quiero** importar preguntas de forma masiva desde un archivo Excel,  
**para** cargar eficientemente grandes bancos de preguntas al sistema.

**Criterios de aceptación:**
- La plataforma provee una plantilla Excel descargable con el formato requerido.
- El archivo debe incluir por fila: número de pregunta (secuencial), enunciado completo, 4 opciones (A, B, C, D), opción correcta (letra A/B/C/D) y puntaje (numérico, por defecto 5).
- El sistema valida cada fila e informa errores específicos (opción inválida, puntaje no numérico, etc.).
- Las filas válidas se importan; las erróneas se muestran con advertencias detalladas.
- Las instrucciones de formato están visibles en la interfaz antes de importar.

---

### HU-020 — Editar y eliminar preguntas
**Como** administrador o docente,  
**quiero** editar o eliminar preguntas existentes,  
**para** corregir errores o actualizar el banco de preguntas.

**Criterios de aceptación:**
- Se pueden modificar todos los campos de la pregunta, incluyendo imagen y opciones.
- Se puede marcar una pregunta como inactiva sin eliminarla.
- La eliminación requiere confirmación previa.
- Los cambios se reflejan inmediatamente en los exámenes activos.

---

## 6. Gestión de Componentes

### HU-021 — Listar componentes de clasificación
**Como** administrador,  
**quiero** ver todos los componentes de clasificación de preguntas,  
**para** conocer cómo están organizadas las áreas temáticas de evaluación.

**Criterios de aceptación:**
- La página `/components` muestra todos los componentes: Entorno Vivo, Entorno Químico, Entorno Físico y Ciencia, Tecnología y Sociedad (CTS).
- Cada componente muestra nombre, descripción y estado (activo/inactivo).

---

### HU-022 — Crear un componente
**Como** administrador,  
**quiero** crear un nuevo componente de clasificación,  
**para** ampliar la taxonomía de evaluación según los lineamientos curriculares.

**Criterios de aceptación:**
- El formulario solicita: nombre y descripción detallada del componente.
- Al guardar, el componente queda disponible para asignarse a preguntas y competencias.
- Se muestra mensaje de éxito.

---

### HU-023 — Editar y eliminar componentes
**Como** administrador,  
**quiero** editar o eliminar componentes de clasificación,  
**para** mantener actualizada la taxonomía de evaluación.

**Criterios de aceptación:**
- Se pueden editar nombre y descripción del componente.
- Se puede desactivar o eliminar con confirmación previa.
- Se muestra mensaje de éxito en cada operación.

---

## 7. Gestión de Grados

### HU-024 — Listar grados académicos
**Como** administrador,  
**quiero** ver todos los grados académicos configurados en el sistema,  
**para** gestionar la organización escolar de la institución.

**Criterios de aceptación:**
- La página `/grades` muestra todos los grados: 10A, 10B, 11A, 11B (y los que se agreguen).
- Se muestra el nombre, descripción y estado de cada grado.

---

### HU-025 — Crear un nuevo grado
**Como** administrador,  
**quiero** crear un nuevo grado académico,  
**para** ampliar la estructura escolar según las necesidades de la institución.

**Criterios de aceptación:**
- El formulario solicita: nombre del grado (ej: "10A") y descripción.
- Al guardar, el grado queda disponible para asignarse a estudiantes y competencias.
- Se muestra mensaje de éxito.

---

### HU-026 — Editar y eliminar grados
**Como** administrador,  
**quiero** editar o eliminar grados académicos,  
**para** mantener actualizada la estructura escolar.

**Criterios de aceptación:**
- Se puede modificar nombre y descripción del grado.
- Se puede desactivar o eliminar con confirmación previa.
- Se muestra mensaje de éxito en cada operación.

---

## 8. Gestión de Estudiantes

### HU-027 — Listar estudiantes
**Como** administrador o docente,  
**quiero** ver la lista de estudiantes registrados,  
**para** gestionar su información y hacer seguimiento académico.

**Criterios de aceptación:**
- La página `/students` muestra todos los estudiantes con: nombre, apellido, correo, tipo de documento, número de documento, grado y estado (activo/inactivo).
- Existe un campo de búsqueda por nombre, correo o número de documento.
- El docente solo ve estudiantes de los grados a su cargo.

---

### HU-028 — Registrar un nuevo estudiante
**Como** administrador o docente (con permiso),  
**quiero** registrar manualmente un nuevo estudiante,  
**para** habilitarlo a acceder a las evaluaciones de su grado.

**Criterios de aceptación:**
- El formulario "Registrar Estudiante" solicita: nombre (obligatorio), apellido (obligatorio), correo electrónico único (obligatorio), contraseña (mínimo 6 caracteres), tipo de documento (CC / TI / RC / PA / CE), número de documento (obligatorio), grado (obligatorio) y foto de perfil (opcional, JPG/PNG/WebP, máx. 5 MB).
- Al registrar, se crea la cuenta de acceso del estudiante automáticamente.
- El correo debe ser único en el sistema.
- Se muestra mensaje de éxito al registrarse correctamente.

---

### HU-029 — Importar estudiantes masivamente desde Excel
**Como** administrador,  
**quiero** importar una lista de estudiantes desde un archivo Excel,  
**para** registrar eficientemente grupos completos.

**Criterios de aceptación:**
- La plataforma provee una plantilla Excel descargable.
- Campos obligatorios por fila: nombre, apellido, correo (único), contraseña, número de documento y grado.
- El sistema valida el formato e informa errores por fila.
- Los estudiantes válidos se registran y se crean sus cuentas de acceso.
- Los erróneos se muestran con descripción del problema.

---

### HU-030 — Editar información de un estudiante
**Como** administrador o docente (con permiso),  
**quiero** editar los datos de un estudiante,  
**para** mantener la información actualizada.

**Criterios de aceptación:**
- Se pueden editar todos los campos del estudiante, incluyendo grado y foto.
- Los cambios se guardan y confirman con mensaje de éxito.

---

### HU-031 — Desactivar o eliminar un estudiante
**Como** administrador,  
**quiero** desactivar o eliminar un estudiante del sistema,  
**para** gestionar bajas o transferencias.

**Criterios de aceptación:**
- Solo el administrador puede eliminar estudiantes; docentes solo pueden desactivar (con permiso).
- Desactivar impide el acceso del estudiante sin borrar su historial.
- Eliminar requiere confirmación previa.
- Se muestra mensaje de éxito.

---

### HU-032 — Restablecer contraseña de un estudiante
**Como** administrador o docente,  
**quiero** restablecer la contraseña de un estudiante,  
**para** ayudarlo a recuperar el acceso si la olvidó.

**Criterios de aceptación:**
- Existe la opción de restablecer contraseña desde el listado o detalle del estudiante.
- Se puede establecer una nueva contraseña para el estudiante.
- Se muestra confirmación al completarse el restablecimiento.

---

## 9. Gestión de Docentes

### HU-033 — Listar docentes
**Como** administrador,  
**quiero** ver la lista de docentes registrados,  
**para** gestionar el equipo docente de la institución.

**Criterios de aceptación:**
- La página `/teachers` muestra todos los docentes con: nombre, apellido, correo, teléfono, tipo y número de documento, módulos asignados, grado que dirige y estado.
- Existe búsqueda por nombre, correo o número de documento.

---

### HU-034 — Registrar un nuevo docente
**Como** administrador,  
**quiero** registrar un nuevo docente en el sistema,  
**para** habilitarlo a gestionar módulos y ver resultados de sus estudiantes.

**Criterios de aceptación:**
- El formulario solicita: nombre (obligatorio), apellido (obligatorio), correo (obligatorio), teléfono (formato +57 300 000 0000), tipo de documento, número de documento (obligatorio), contraseña, foto de perfil (opcional).
- Se pueden asignar uno o más módulos al docente ("Selecciona los módulos que gestionará este docente").
- Se puede designar al docente como director de curso marcando el checkbox "Es Director de Curso" y seleccionando el grado que dirige.
- Al registrar, el docente puede iniciar sesión y ver solo sus módulos asignados.
- Se muestra mensaje de éxito.

---

### HU-035 — Editar un docente
**Como** administrador,  
**quiero** editar los datos y asignaciones de un docente,  
**para** reflejar cambios en el equipo pedagógico.

**Criterios de aceptación:**
- Se pueden modificar todos los campos del docente, incluyendo módulos asignados y rol de director.
- Los cambios de asignaciones se aplican inmediatamente.
- Se muestra mensaje de éxito.

---

### HU-036 — Desactivar o eliminar un docente
**Como** administrador,  
**quiero** desactivar o eliminar un docente,  
**para** gestionar bajas del equipo docente.

**Criterios de aceptación:**
- Solo el administrador puede realizar estas acciones.
- Desactivar impide el acceso sin borrar datos.
- Eliminar requiere confirmación previa.
- Se muestra mensaje de éxito.

---

## 10. Resultados y Reportes

### HU-037 — Ver resultados de evaluaciones
**Como** administrador o docente,  
**quiero** ver los resultados de todos los intentos de examen,  
**para** evaluar el desempeño académico y tomar decisiones pedagógicas.

**Criterios de aceptación:**
- La página `/results` muestra un listado de intentos con: nombre del estudiante, número de documento, competencia evaluada, puntaje obtenido / puntaje total, porcentaje y estado (Aprobado ≥60% o Reprobado <60%).
- Se puede buscar por nombre del estudiante o nombre de la competencia.
- El docente solo ve resultados de los módulos y grados a su cargo.
- El administrador ve todos los resultados.

---

### HU-038 — Exportar resultados a Excel o PDF
**Como** administrador o docente,  
**quiero** exportar los resultados de evaluaciones a Excel o PDF,  
**para** compartirlos, archivarlos o analizarlos fuera de la plataforma.

**Criterios de aceptación:**
- Existe el botón "Exportar Excel" que descarga un archivo `.xlsx` con los datos visibles.
- Existe el botón "Exportar PDF" que genera un reporte con encabezado institucional, filtros aplicados y tabla de resultados.
- El reporte incluye: nombre del estudiante, grado, competencia, puntaje, porcentaje, fecha y estado.

---

### HU-039 — Ver historial de evaluaciones (Estudiante)
**Como** estudiante,  
**quiero** ver el historial completo de todos los exámenes que he realizado,  
**para** conocer mi evolución académica.

**Criterios de aceptación:**
- La página `/my-history` muestra todos los intentos con: módulo, competencia, fecha, puntaje obtenido/total y resultado (Aprobado/Reprobado).
- Si no hay historial, se muestra el mensaje "Sin historial".
- Se puede descargar el historial completo en PDF ("Descargar Historial PDF").

---

### HU-040 — Ver mis resultados (Estudiante)
**Como** estudiante,  
**quiero** ver el detalle de mis resultados por competencia,  
**para** identificar mis fortalezas y áreas de mejora.

**Criterios de aceptación:**
- La página `/my-results` muestra: pruebas realizadas, pruebas pendientes y promedio general.
- Por cada competencia realizada se muestra el puntaje y estado.
- Se puede acceder al detalle de cada intento ("Ver Resultado").
- Si no hay resultados, se muestra: "Aún no has realizado ninguna evaluación".

---

## 11. Solicitudes de Reinicio de Evaluación

### HU-041 — Solicitar reinicio de evaluación (Estudiante)
**Como** estudiante,  
**quiero** solicitar al docente que me permita repetir una evaluación,  
**para** tener una nueva oportunidad de aprobar.

**Criterios de aceptación:**
- Desde la vista de resultados, el estudiante puede hacer clic en "Solicitar reiniciar evaluación" / "Solicitar repetir prueba".
- El formulario solicita un motivo/argumento ("Argumenta por qué deseas realizar la prueba nuevamente").
- La solicitud queda en estado Pendiente.
- El estudiante puede ver el estado de sus solicitudes (Pendiente / Aprobada / Denegada).
- Si la solicitud es aprobada, recibe notificación: "Tu solicitud para repetir [evaluación] ha sido aprobada. Ya puedes realizar la prueba nuevamente."

---

### HU-042 — Gestionar solicitudes de reinicio (Docente / Administrador)
**Como** administrador o docente,  
**quiero** ver y gestionar las solicitudes de repetición enviadas por los estudiantes,  
**para** decidir si se les concede una nueva oportunidad de evaluación.

**Criterios de aceptación:**
- La página `/retake-requests` muestra todas las solicitudes con: nombre del estudiante, grado, competencia, motivo y estado.
- Se puede filtrar por estado (Pendiente / Aprobada / Denegada).
- El docente puede "Aprobar" o "Denegar" cada solicitud.
- Al aprobar: se configuran fecha de repetición, hora inicio y hora fin de la ventana de evaluación.
- Al denegar: el estudiante recibe notificación de denegación.
- Las solicitudes procesadas cambian de estado inmediatamente.
- El director de curso tiene acceso a gestionar las solicitudes de los estudiantes de su grado.

---

## 12. Notificaciones

### HU-043 — Recibir notificaciones del sistema
**Como** usuario (administrador, docente o estudiante),  
**quiero** recibir notificaciones dentro de la plataforma sobre eventos relevantes,  
**para** estar informado sin necesidad de revisar manualmente cada sección.

**Criterios de aceptación:**
- El sistema genera notificaciones para eventos como: solicitud de reinicio aprobada o denegada, habilitación de nuevo examen, etc.
- Las notificaciones muestran la hora en que fueron generadas.
- Las notificaciones no leídas se destacan visualmente.
- El usuario puede marcar notificaciones como leídas.
- Las notificaciones se entregan en tiempo real mediante Supabase Realtime.

---

## 13. Configuración del Sistema

### HU-044 — Gestionar datos institucionales
**Como** administrador,  
**quiero** configurar los datos de la institución educativa en la plataforma,  
**para** personalizar la plataforma con la identidad de la institución.

**Criterios de aceptación:**
- La página `/configuration` permite editar: nombre de la institución, dirección, teléfono, rector, descripción/slogan (visible en login y menú) y nombre del software.
- Los cambios se reflejan de inmediato en toda la plataforma.

---

### HU-045 — Personalizar la apariencia visual
**Como** administrador,  
**quiero** personalizar los colores y el logo de la plataforma,  
**para** adaptar la identidad visual a los colores institucionales.

**Criterios de aceptación:**
- Se puede subir el logo institucional.
- Se puede configurar color primario y secundario (selector visual con código HEX).
- Se puede aplicar un degradado de colores con colores inicial y final.
- Se puede subir una imagen personalizada para el fondo del login.
- Los cambios se previsibilizan antes de guardar.

---

### HU-046 — Gestionar banners del carrusel de login
**Como** administrador,  
**quiero** agregar, editar y eliminar banners del carrusel en la pantalla de login,  
**para** mostrar mensajes de bienvenida personalizados a los usuarios.

**Criterios de aceptación:**
- Se pueden agregar múltiples banners, cada uno con: título, subtítulo e imagen de fondo.
- Se puede arrastrar la imagen o hacer clic para seleccionarla.
- Se puede configurar la velocidad de transición del carrusel.
- Se puede eliminar banners existentes.
- Los banners actuales configurados incluyen: "Bienvenido Estudiante", "Portal Docente" y "Ciencia en Acción".
- Los cambios se guardan con "Guardar banners".

---

### HU-047 — Gestionar etiquetas de materias
**Como** administrador,  
**quiero** gestionar las etiquetas de materias del sistema,  
**para** categorizar adecuadamente el contenido según las asignaturas.

**Criterios de aceptación:**
- Se pueden agregar, editar y eliminar etiquetas de materias (ej: Biología, Química, Física, Matemáticas).
- Las etiquetas quedan disponibles para asignarse a componentes y competencias.

---

## 14. Gestión de Permisos

### HU-048 — Configurar permisos por rol
**Como** administrador,  
**quiero** configurar qué acciones puede realizar cada rol en el sistema,  
**para** adaptar los permisos a las políticas pedagógicas de la institución.

**Criterios de aceptación:**
- La página `/permissions` muestra una matriz de permisos por rol (Docente y Estudiante; el Administrador siempre tiene todos los permisos y no puede modificarse).
- Los permisos configurables incluyen acciones sobre: módulos, competencias, preguntas, estudiantes y docentes (ver, crear, editar, eliminar, desactivar).
- La matriz de permisos por defecto es:

| Permiso | Docente | Estudiante |
|---------|---------|------------|
| modules:view | ✅ | ✅ |
| modules:create | ❌ | ❌ |
| modules:edit | ✅ | ❌ |
| modules:delete | ❌ | ❌ |
| modules:deactivate | ❌ | ❌ |
| competencies:view | ✅ | ✅ |
| competencies:create | ✅ | ❌ |
| competencies:edit | ✅ | ❌ |
| competencies:delete | ❌ | ❌ |
| competencies:deactivate | ✅ | ❌ |
| questions:view | ✅ | ❌ |
| questions:create | ✅ | ❌ |
| questions:edit | ✅ | ❌ |
| questions:delete | ✅ | ❌ |
| questions:deactivate | ✅ | ❌ |
| students:view | ✅ | ❌ |
| students:create | ✅ | ❌ |
| students:edit | ✅ | ❌ |
| students:delete | ❌ | ❌ |
| students:deactivate | ❌ | ❌ |
| teachers:view | ❌ | ❌ |
| teachers:create | ❌ | ❌ |
| teachers:edit | ❌ | ❌ |
| teachers:delete | ❌ | ❌ |
| teachers:deactivate | ❌ | ❌ |

- Existe el botón "Restaurar permisos a los valores predeterminados".

---

## 15. Vista del Estudiante — Exámenes

### HU-049 — Ver exámenes disponibles
**Como** estudiante,  
**quiero** ver las competencias habilitadas para evaluación en mi grado,  
**para** planificar qué pruebas realizar.

**Criterios de aceptación:**
- La página `/my-exams` muestra "Competencias disponibles para evaluar" filtradas por el grado del estudiante.
- Se muestran: nombre del módulo, competencia, componente, fechas de disponibilidad.
- Si no hay pruebas disponibles, se muestra: "No tienes pruebas asignadas en este momento".
- Se puede acceder directamente a cada examen desde esta vista.

---

### HU-050 — Realizar un examen
**Como** estudiante,  
**quiero** responder las preguntas de una competencia en un examen en línea,  
**para** demostrar mis conocimientos en Ciencias Naturales y obtener una calificación.

**Criterios de aceptación:**
- El estudiante accede al examen desde `/exam/:competencyId`.
- Se muestran todas las preguntas con sus opciones (A, B, C, D) y opcionalmente imágenes de apoyo.
- Se lleva un contador de preguntas respondidas.
- El sistema exige responder todas las preguntas antes de finalizar ("Debes responder todas las preguntas para finalizar").
- Al finalizar, el sistema calcula el puntaje automáticamente (score / total_score × 100%).
- Si ya completó la prueba, se muestra: "Ya completaste esta prueba".
- Si la prueba no tiene preguntas, se muestra: "Esta prueba no tiene preguntas configuradas".

---

### HU-051 — Ver resultado inmediato tras completar un examen
**Como** estudiante,  
**quiero** ver mi resultado inmediatamente después de completar un examen,  
**para** conocer mi calificación y si aprobé o no.

**Criterios de aceptación:**
- Al finalizar el examen, se muestra: puntaje obtenido, puntaje total, porcentaje y estado (Aprobado ≥60% / Reprobado <60%).
- El resultado queda registrado en el historial del estudiante.
- Se habilita la opción de solicitar repetición si el resultado fue reprobatorio.

---

### HU-052 — Solicitar repetición desde la vista de resultados
**Como** estudiante,  
**quiero** solicitar desde mi página de resultados una nueva oportunidad de realizar un examen,  
**para** mejorar mi calificación si no aprobé.

**Criterios de aceptación:**
- El botón "Solicitar reiniciar evaluación" está disponible para competencias reprobadas.
- Se muestra un campo de texto para ingresar el motivo de la solicitud.
- La solicitud queda en estado Pendiente y el estudiante puede ver su estado.
- Una solicitud aprobada habilita al estudiante para repetir el examen en la ventana de tiempo configurada por el docente.

---

## Resumen de Historias de Usuario

| ID | Título | Rol Principal | Módulo |
|----|--------|---------------|--------|
| HU-001 | Inicio de sesión | Todos | Autenticación |
| HU-002 | Acceso restringido por rol | Sistema | Autenticación |
| HU-003 | Personalización de pantalla de login | Administrador | Configuración |
| HU-004 | Dashboard del administrador | Administrador | Dashboard |
| HU-005 | Dashboard del docente | Docente | Dashboard |
| HU-006 | Dashboard del estudiante | Estudiante | Dashboard |
| HU-007 | Listar módulos | Admin / Docente | Módulos |
| HU-008 | Crear módulo | Administrador | Módulos |
| HU-009 | Editar módulo | Admin / Docente | Módulos |
| HU-010 | Desactivar / Activar módulo | Admin / Docente | Módulos |
| HU-011 | Eliminar módulo | Administrador | Módulos |
| HU-012 | Ver detalle de módulo | Admin / Docente | Módulos |
| HU-013 | Listar competencias de un módulo | Admin / Docente | Competencias |
| HU-014 | Crear competencia | Admin / Docente | Competencias |
| HU-015 | Editar y desactivar competencia | Admin / Docente | Competencias |
| HU-016 | Eliminar competencia | Administrador | Competencias |
| HU-017 | Ver preguntas de una competencia | Admin / Docente | Preguntas |
| HU-018 | Crear pregunta de opción múltiple | Admin / Docente | Preguntas |
| HU-019 | Importar preguntas desde Excel | Administrador | Preguntas |
| HU-020 | Editar y eliminar preguntas | Admin / Docente | Preguntas |
| HU-021 | Listar componentes | Administrador | Componentes |
| HU-022 | Crear componente | Administrador | Componentes |
| HU-023 | Editar y eliminar componentes | Administrador | Componentes |
| HU-024 | Listar grados académicos | Administrador | Grados |
| HU-025 | Crear grado | Administrador | Grados |
| HU-026 | Editar y eliminar grados | Administrador | Grados |
| HU-027 | Listar estudiantes | Admin / Docente | Estudiantes |
| HU-028 | Registrar estudiante | Admin / Docente | Estudiantes |
| HU-029 | Importar estudiantes desde Excel | Administrador | Estudiantes |
| HU-030 | Editar estudiante | Admin / Docente | Estudiantes |
| HU-031 | Desactivar o eliminar estudiante | Administrador | Estudiantes |
| HU-032 | Restablecer contraseña de estudiante | Admin / Docente | Estudiantes |
| HU-033 | Listar docentes | Administrador | Docentes |
| HU-034 | Registrar docente | Administrador | Docentes |
| HU-035 | Editar docente | Administrador | Docentes |
| HU-036 | Desactivar o eliminar docente | Administrador | Docentes |
| HU-037 | Ver resultados de evaluaciones | Admin / Docente | Resultados |
| HU-038 | Exportar resultados a Excel/PDF | Admin / Docente | Resultados |
| HU-039 | Ver historial de evaluaciones | Estudiante | Resultados |
| HU-040 | Ver mis resultados | Estudiante | Resultados |
| HU-041 | Solicitar reinicio de evaluación | Estudiante | Reinicio |
| HU-042 | Gestionar solicitudes de reinicio | Admin / Docente | Reinicio |
| HU-043 | Recibir notificaciones del sistema | Todos | Notificaciones |
| HU-044 | Gestionar datos institucionales | Administrador | Configuración |
| HU-045 | Personalizar apariencia visual | Administrador | Configuración |
| HU-046 | Gestionar banners del login | Administrador | Configuración |
| HU-047 | Gestionar etiquetas de materias | Administrador | Configuración |
| HU-048 | Configurar permisos por rol | Administrador | Permisos |
| HU-049 | Ver exámenes disponibles | Estudiante | Exámenes |
| HU-050 | Realizar un examen | Estudiante | Exámenes |
| HU-051 | Ver resultado inmediato del examen | Estudiante | Exámenes |
| HU-052 | Solicitar repetición desde resultados | Estudiante | Exámenes |

---

*Documento generado mediante análisis funcional de la plataforma EduScience (https://edu-science-jade.vercel.app/), incluyendo exploración autenticada con usuario administrador.*  
*Institución configurada: Institución Educativa Bello Horizonte — © 2026 Nikol Riveros, Leonel O Torres.*
