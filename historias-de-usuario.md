# Historias de Usuario — EduScience

**Plataforma:** EduScience — Plataforma educativa para el aprendizaje de Ciencias Naturales  
**URL:** https://edu-science-jade.vercel.app/  
**Fecha de relevamiento:** 2026-05-11

---

## Índice

1. [Roles del Sistema](#roles-del-sistema)
2. [Autenticación y Acceso](#1-autenticación-y-acceso)
3. [Dashboard / Panel Principal](#2-dashboard--panel-principal)
4. [Gestión de Módulos](#3-gestión-de-módulos)
5. [Gestión de Componentes](#4-gestión-de-componentes)
6. [Gestión de Competencias](#5-gestión-de-competencias)
7. [Gestión de Preguntas y Exámenes](#6-gestión-de-preguntas-y-exámenes)
8. [Gestión de Estudiantes](#7-gestión-de-estudiantes)
9. [Gestión de Docentes](#8-gestión-de-docentes)
10. [Gestión de Grados](#9-gestión-de-grados)
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
| **Administrador** | Acceso total al sistema: gestión de usuarios, módulos, configuración y reportes globales |
| **Docente** | Gestión de módulos asignados, visualización de resultados de sus estudiantes, aprobación de solicitudes |
| **Estudiante** | Realización de exámenes, visualización de sus propios resultados e historial |

---

## 1. Autenticación y Acceso

### HU-001 — Inicio de sesión
**Como** usuario registrado (administrador, docente o estudiante),  
**quiero** poder iniciar sesión con mi correo electrónico y contraseña,  
**para** acceder a mi espacio educativo personalizado según mi rol.

**Criterios de aceptación:**
- El sistema muestra un formulario con campos de correo y contraseña.
- Al ingresar credenciales válidas, el sistema redirige al dashboard correspondiente al rol del usuario.
- Al ingresar credenciales incorrectas, se muestra un mensaje de error claro.
- El sistema protege las rutas privadas; un usuario no autenticado es redirigido al login.
- La sesión persiste entre recargas de página.

---

### HU-002 — Acceso restringido por rol
**Como** sistema,  
**quiero** controlar el acceso a las diferentes secciones según el rol del usuario,  
**para** garantizar que cada usuario solo vea y opere las funcionalidades que le corresponden.

**Criterios de aceptación:**
- Un estudiante no puede acceder a rutas de administración (`/students`, `/teachers`, `/configuration`, etc.).
- Un docente no puede acceder a rutas exclusivas del administrador (`/permissions`, `/configuration`).
- Al intentar acceder a una ruta no autorizada, el sistema muestra un mensaje de "Acceso Restringido".
- Las rutas inexistentes muestran una página de error 404.

---

### HU-003 — Banners de bienvenida en el login
**Como** administrador,  
**quiero** configurar imágenes de banner en la pantalla de inicio de sesión,  
**para** personalizar la experiencia visual de la plataforma para los usuarios.

**Criterios de aceptación:**
- El administrador puede agregar, editar y eliminar banners desde `/configuration`.
- Cada banner tiene un título y una imagen.
- Los banners se muestran en la pantalla de login en un carrusel o visualización destacada.
- Los cambios se guardan con el botón "Guardar banners".

---

## 2. Dashboard / Panel Principal

### HU-004 — Visualización del dashboard de administrador
**Como** administrador,  
**quiero** ver un panel de control con métricas globales del sistema,  
**para** tener una visión general del estado de la plataforma educativa.

**Criterios de aceptación:**
- El dashboard muestra el total de módulos activos.
- El dashboard muestra el promedio general de puntaje de los estudiantes (en porcentaje).
- El dashboard muestra el número total de intentos de examen realizados.
- El dashboard presenta un gráfico de "Promedio por Grado".
- El dashboard presenta un gráfico de "Rendimiento por Componente".
- El dashboard presenta un gráfico de "Tendencia de Evaluaciones (Últimos 6 meses)".

---

### HU-005 — Visualización del dashboard del estudiante
**Como** estudiante,  
**quiero** ver un panel de bienvenida con acceso a mis evaluaciones disponibles,  
**para** conocer rápidamente qué exámenes puedo realizar y mi estado general.

**Criterios de aceptación:**
- El dashboard muestra un mensaje de bienvenida personalizado ("Bienvenido Estudiante").
- El estudiante puede acceder desde el dashboard a sus exámenes disponibles.
- Se muestran indicadores de progreso o puntajes recientes.

---

## 3. Gestión de Módulos

### HU-006 — Listar módulos educativos
**Como** administrador o docente,  
**quiero** ver la lista de todos los módulos educativos disponibles,  
**para** conocer la estructura del contenido de la plataforma.

**Criterios de aceptación:**
- La página `/modules` muestra todos los módulos con su nombre e ícono.
- Los módulos pueden filtrarse o buscarse.
- Cada módulo muestra información básica: nombre, ícono, estado (activo/inactivo).

---

### HU-007 — Crear un nuevo módulo
**Como** administrador,  
**quiero** crear un nuevo módulo educativo con nombre e ícono personalizado,  
**para** organizar el contenido educativo de la plataforma.

**Criterios de aceptación:**
- El formulario "Nuevo Módulo" permite ingresar nombre del módulo.
- Se puede seleccionar un ícono representativo para el módulo.
- Al guardar, el módulo aparece en la lista de módulos.
- Se muestra un mensaje de éxito al crearse correctamente.

---

### HU-008 — Editar un módulo existente
**Como** administrador,  
**quiero** editar la información de un módulo existente,  
**para** mantener actualizado el contenido educativo.

**Criterios de aceptación:**
- El administrador puede acceder a la edición desde la lista de módulos.
- Puede modificar el nombre e ícono del módulo.
- Los cambios se guardan con "Guardar cambios".
- Se muestra confirmación de los cambios guardados.

---

### HU-009 — Eliminar un módulo
**Como** administrador,  
**quiero** eliminar un módulo que ya no sea relevante,  
**para** mantener limpia y actualizada la estructura educativa.

**Criterios de aceptación:**
- El sistema solicita confirmación antes de eliminar.
- Al confirmar, el módulo se elimina y desaparece de la lista.
- Si el módulo tiene competencias o estudiantes asociados, el sistema advierte sobre el impacto.
- Se muestra un mensaje de éxito al eliminarse correctamente.

---

### HU-010 — Ver detalle de un módulo
**Como** administrador o docente,  
**quiero** ver el detalle completo de un módulo,  
**para** gestionar sus componentes y competencias asociadas.

**Criterios de aceptación:**
- Al hacer clic en un módulo, se navega a `/modules/:moduleId`.
- Se muestran los componentes y competencias del módulo.
- Se puede navegar entre las competencias del módulo.

---

## 4. Gestión de Componentes

### HU-011 — Listar componentes del sistema
**Como** administrador,  
**quiero** ver todos los componentes de evaluación definidos en el sistema,  
**para** entender cómo están organizadas las áreas temáticas de evaluación.

**Criterios de aceptación:**
- La página `/components` muestra todos los componentes disponibles.
- Cada componente muestra su nombre.
- Se indica si el componente está activo o inactivo.

---

### HU-012 — Crear un nuevo componente
**Como** administrador,  
**quiero** crear un nuevo componente de evaluación,  
**para** ampliar las categorías de evaluación disponibles en los exámenes.

**Criterios de aceptación:**
- El formulario "Nuevo Componente" permite ingresar el nombre del componente.
- Al guardar, el componente aparece en la lista.
- Se muestra un mensaje de éxito.

---

### HU-013 — Editar y eliminar componentes
**Como** administrador,  
**quiero** editar o eliminar componentes existentes,  
**para** mantener actualizada la taxonomía de evaluación.

**Criterios de aceptación:**
- Se puede editar el nombre de un componente.
- Se puede eliminar un componente con confirmación previa.
- Se muestra mensaje de éxito en cada operación.

---

## 5. Gestión de Competencias

### HU-014 — Ver competencias de un módulo
**Como** administrador o docente,  
**quiero** ver las competencias asociadas a un módulo específico,  
**para** entender qué habilidades se evalúan en cada módulo.

**Criterios de aceptación:**
- La ruta `/modules/:moduleId/competencies/:competencyId` muestra el detalle de una competencia.
- Se muestran las preguntas asociadas a la competencia.
- Se indica el número de preguntas y el puntaje total disponible.

---

### HU-015 — Crear una nueva competencia
**Como** administrador,  
**quiero** crear una nueva competencia dentro de un módulo,  
**para** definir los criterios de evaluación que los estudiantes deben demostrar.

**Criterios de aceptación:**
- Se puede crear una competencia asignada a un módulo y componente específico.
- Se puede asignar fecha de inicio y fin a la competencia/evaluación.
- La competencia aparece disponible para los estudiantes del grado correspondiente.

---

## 6. Gestión de Preguntas y Exámenes

### HU-016 — Crear preguntas de opción múltiple
**Como** administrador o docente,  
**quiero** crear preguntas de opción múltiple para una competencia,  
**para** evaluar el conocimiento de los estudiantes en Ciencias Naturales.

**Criterios de aceptación:**
- El formulario "Nueva Pregunta" permite ingresar el enunciado de la pregunta.
- Se pueden crear exactamente 4 opciones de respuesta (A, B, C, D).
- Se selecciona cuál es la opción correcta.
- Se puede asignar un puntaje por pregunta (valor por defecto: 5).
- Se puede agregar una imagen ilustrativa a la pregunta.
- Se asigna la pregunta a un componente.

---

### HU-017 — Importar preguntas masivamente desde Excel
**Como** administrador,  
**quiero** importar preguntas de forma masiva desde un archivo Excel,  
**para** cargar eficientemente grandes cantidades de preguntas al sistema.

**Criterios de aceptación:**
- La plataforma permite importar un archivo Excel con el formato correcto.
- El archivo debe contener: número de pregunta, enunciado, 4 opciones (A, B, C, D), opción correcta y puntaje.
- El sistema valida el formato de cada fila e informa errores específicos.
- Las preguntas válidas se importan y las erróneas se muestran con advertencias.
- Las instrucciones del formato están disponibles en la interfaz.

---

### HU-018 — Subir recursos multimedia a una competencia
**Como** administrador o docente,  
**quiero** subir archivos PDF, PowerPoint o Video como material de apoyo a una competencia,  
**para** que los estudiantes tengan recursos de estudio antes de realizar el examen.

**Criterios de aceptación:**
- Se puede arrastrar o seleccionar un archivo PDF, PowerPoint o Video.
- El archivo se asocia a la competencia seleccionada.
- Se muestra un mensaje de éxito al subir el archivo correctamente.
- Los estudiantes pueden acceder al recurso desde su vista de la competencia.

---

### HU-019 — Realizar un examen
**Como** estudiante,  
**quiero** responder las preguntas de una competencia en un examen en línea,  
**para** demostrar mi conocimiento y obtener una calificación.

**Criterios de aceptación:**
- El estudiante accede al examen desde `/exam/:competencyId`.
- Se muestran todas las preguntas de la competencia con sus opciones (A, B, C, D).
- Las preguntas pueden incluir imágenes ilustrativas.
- El sistema exige responder todas las preguntas antes de finalizar ("Debes responder todas las preguntas para finalizar").
- Al finalizar, el sistema calcula el puntaje automáticamente (score/total_score × 100%).
- El resultado se muestra inmediatamente: puntaje obtenido, total y porcentaje.
- Aprobar requiere obtener 60% o más.

---

### HU-020 — Ver mis exámenes disponibles (Estudiante)
**Como** estudiante,  
**quiero** ver la lista de exámenes que tengo disponibles para realizar,  
**para** planificar mi preparación y no perder ninguna evaluación.

**Criterios de aceptación:**
- La página `/my-exams` muestra todos los exámenes disponibles para el estudiante.
- Se indica el módulo, componente y fecha de disponibilidad de cada examen.
- Se puede acceder directamente al examen desde esta vista.

---

## 7. Gestión de Estudiantes

### HU-021 — Listar estudiantes
**Como** administrador o docente,  
**quiero** ver la lista de estudiantes registrados en el sistema,  
**para** gestionar su información y seguimiento académico.

**Criterios de aceptación:**
- La página `/students` muestra todos los estudiantes registrados.
- Se muestra nombre, apellido, correo y grado de cada estudiante.
- Se puede buscar estudiantes por nombre o correo.
- Se puede filtrar por grado.

---

### HU-022 — Registrar un nuevo estudiante
**Como** administrador,  
**quiero** registrar manualmente un nuevo estudiante en el sistema,  
**para** habilitarlo a acceder a los exámenes de su grado.

**Criterios de aceptación:**
- El formulario "Registrar Estudiante" solicita: nombre, apellido, correo electrónico y grado.
- El correo debe ser único para cada estudiante.
- Al registrar, el estudiante queda habilitado para iniciar sesión.
- Se muestra mensaje de éxito al registrar correctamente.

---

### HU-023 — Importar estudiantes masivamente desde Excel
**Como** administrador,  
**quiero** importar una lista de estudiantes desde un archivo Excel,  
**para** registrar eficientemente grupos completos de estudiantes.

**Criterios de aceptación:**
- La plataforma permite importar un archivo Excel con datos de estudiantes.
- El archivo debe contener: nombre, apellido y correo (único) de cada estudiante.
- El sistema valida el formato y reporta errores por fila.
- Los estudiantes válidos se registran y los erróneos se informan con advertencias.

---

### HU-024 — Editar información de un estudiante
**Como** administrador,  
**quiero** editar los datos de un estudiante existente,  
**para** mantener la información actualizada.

**Criterios de aceptación:**
- Se puede editar nombre, apellido, correo y grado del estudiante.
- Los cambios se guardan correctamente.
- Se muestra confirmación de los cambios.

---

### HU-025 — Eliminar un estudiante
**Como** administrador,  
**quiero** eliminar un estudiante del sistema,  
**para** remover registros que ya no sean necesarios.

**Criterios de aceptación:**
- El sistema solicita confirmación antes de eliminar.
- Al confirmar, el estudiante es eliminado y no puede iniciar sesión.
- Se muestra mensaje de éxito.

---

## 8. Gestión de Docentes

### HU-026 — Listar docentes
**Como** administrador,  
**quiero** ver la lista de docentes registrados en el sistema,  
**para** gestionar sus asignaciones y accesos.

**Criterios de aceptación:**
- La página `/teachers` muestra todos los docentes registrados.
- Se muestra nombre, apellido, correo y grados asignados de cada docente.
- Se puede buscar por nombre o correo.

---

### HU-027 — Registrar un nuevo docente
**Como** administrador,  
**quiero** registrar un nuevo docente en el sistema,  
**para** habilitarlo a gestionar módulos y ver resultados de sus estudiantes.

**Criterios de aceptación:**
- El formulario "Nuevo Docente" solicita: nombre, apellido y correo.
- Se pueden asignar uno o más grados al docente.
- Se puede designar al docente como director de curso de un grado específico.
- Al registrar, el docente puede iniciar sesión con sus credenciales.
- Se muestra mensaje de éxito.

---

### HU-028 — Editar y eliminar docentes
**Como** administrador,  
**quiero** editar o eliminar la información de un docente,  
**para** mantener actualizado el equipo docente en la plataforma.

**Criterios de aceptación:**
- Se pueden editar todos los datos del docente incluyendo grados asignados.
- Se puede cambiar la asignación de director de curso.
- Se puede eliminar un docente con confirmación previa.
- Se muestran mensajes de éxito en cada operación.

---

## 9. Gestión de Grados

### HU-029 — Listar grados escolares
**Como** administrador,  
**quiero** ver todos los grados escolares configurados en el sistema,  
**para** gestionar la organización académica de la institución.

**Criterios de aceptación:**
- La página `/grades` muestra todos los grados disponibles.
- Se muestra el nombre de cada grado y si está activo.
- Se indica el docente director asignado a cada grado.

---

### HU-030 — Crear un nuevo grado
**Como** administrador,  
**quiero** crear un nuevo grado escolar en el sistema,  
**para** ampliar la estructura académica de la institución.

**Criterios de aceptación:**
- El formulario "Nuevo Grado" permite ingresar el nombre del grado.
- Al guardar, el grado queda disponible para asignarse a estudiantes y docentes.
- Se muestra mensaje de éxito.

---

### HU-031 — Editar y eliminar grados
**Como** administrador,  
**quiero** editar o eliminar un grado existente,  
**para** mantener actualizada la estructura académica.

**Criterios de aceptación:**
- Se puede editar el nombre del grado.
- Se puede eliminar con confirmación previa.
- Se muestra mensaje de éxito en cada operación.

---

## 10. Resultados y Reportes

### HU-032 — Ver resultados de evaluaciones (Administrador/Docente)
**Como** administrador o docente,  
**quiero** ver los resultados de todos los exámenes realizados por los estudiantes,  
**para** evaluar el desempeño académico y tomar decisiones pedagógicas.

**Criterios de aceptación:**
- La página `/results` muestra un listado de resultados de evaluaciones.
- Se puede filtrar por grado, módulo o componente.
- Se muestra: nombre del estudiante, módulo, competencia, puntaje obtenido, total y porcentaje.
- Se indica claramente si el estudiante aprobó (≥60%) o reprobó.

---

### HU-033 — Exportar resultados a Excel o PDF
**Como** administrador o docente,  
**quiero** exportar los resultados de evaluaciones a Excel o PDF,  
**para** compartirlos o analizarlos fuera de la plataforma.

**Criterios de aceptación:**
- Existe la opción "Exportar Excel" que descarga un archivo `.xlsx` con los datos de resultados.
- Existe la opción "Exportar PDF" que genera un reporte PDF con los datos filtrados.
- El reporte PDF incluye: nombre del estudiante, grado, módulo, puntaje, porcentaje, fecha y estado (aprobado/reprobado).

---

### HU-034 — Ver mi historial de evaluaciones (Estudiante)
**Como** estudiante,  
**quiero** ver el historial completo de todos los exámenes que he realizado,  
**para** conocer mi evolución académica a lo largo del tiempo.

**Criterios de aceptación:**
- La página `/my-history` muestra todos los intentos de examen del estudiante.
- Se muestra: módulo, competencia, fecha, puntaje obtenido, total y resultado (aprobado/reprobado).
- Se puede descargar el historial completo en formato PDF ("Descargar Historial PDF").

---

### HU-035 — Ver mis resultados (Estudiante)
**Como** estudiante,  
**quiero** ver el detalle de mis resultados por competencia,  
**para** identificar mis fortalezas y áreas de mejora.

**Criterios de aceptación:**
- La página `/my-results` muestra el resultado de cada competencia evaluada.
- Se muestra el puntaje obtenido vs. el puntaje total y el porcentaje.
- Se indica claramente si aprobé o no cada competencia.
- Se puede ver el detalle de cada intento ("Ver Resultado").

---

## 11. Solicitudes de Reinicio de Evaluación

### HU-036 — Solicitar reinicio de evaluación (Estudiante)
**Como** estudiante,  
**quiero** solicitar al docente que me permita volver a realizar una evaluación,  
**para** mejorar mi calificación si no la aprobé.

**Criterios de aceptación:**
- Desde la vista de resultados, el estudiante puede hacer clic en "Solicitar reiniciar evaluación".
- La solicitud queda registrada con estado pendiente.
- El estudiante recibe una notificación cuando la solicitud es aprobada o denegada.
- Si la solicitud es aprobada, el mensaje indica: "Tu solicitud para repetir [evaluación] ha sido aprobada. Ya puedes realizar la prueba nuevamente."

---

### HU-037 — Gestionar solicitudes de reinicio (Docente/Administrador)
**Como** administrador o docente,  
**quiero** ver y gestionar las solicitudes de reinicio de evaluación de los estudiantes,  
**para** decidir si les permito volver a realizar el examen.

**Criterios de aceptación:**
- La página `/retake-requests` muestra todas las solicitudes pendientes.
- Se muestra: nombre del estudiante, grado, módulo, competencia y fecha de solicitud.
- El docente puede "Aprobar" o "Denegar" cada solicitud.
- Al aprobar, el estudiante es habilitado para repetir el examen con nueva fecha de inicio.
- Al denegar, el estudiante recibe notificación de denegación.
- Las solicitudes procesadas cambian de estado en la lista.

---

## 12. Notificaciones

### HU-038 — Recibir notificaciones del sistema
**Como** usuario (administrador, docente o estudiante),  
**quiero** recibir notificaciones dentro de la plataforma sobre eventos relevantes,  
**para** estar informado de cambios que me afectan sin necesidad de revisar manualmente.

**Criterios de aceptación:**
- El sistema genera notificaciones para eventos como: solicitud de reinicio aprobada/denegada, habilitación de nuevo examen, etc.
- Las notificaciones muestran la hora en que fueron generadas.
- Las notificaciones pueden marcarse como leídas.
- Las notificaciones no leídas se destacan visualmente.

---

## 13. Configuración del Sistema

### HU-039 — Gestionar banners del login
**Como** administrador,  
**quiero** configurar las imágenes de banner que se muestran en la pantalla de inicio de sesión,  
**para** personalizar la presentación visual de la plataforma.

**Criterios de aceptación:**
- La página `/configuration` permite gestionar los banners del login.
- Se pueden agregar nuevos banners con título e imagen.
- Se puede editar el título de cada banner.
- Se puede eliminar banners existentes.
- Los cambios se guardan con el botón "Guardar banners".

---

### HU-040 — Subir imágenes de banner
**Como** administrador,  
**quiero** subir imágenes para los banners del login arrastrando el archivo o seleccionándolo,  
**para** personalizar visualmente la pantalla de acceso.

**Criterios de aceptación:**
- Se puede arrastrar una imagen o hacer clic para seleccionarla.
- El sistema acepta formatos de imagen estándar (JPG, PNG, etc.).
- La imagen se previsualiza después de cargarse.
- Se muestra confirmación de carga exitosa.

---

## 14. Gestión de Permisos

### HU-041 — Configurar permisos del sistema
**Como** administrador,  
**quiero** gestionar los permisos de acceso de los diferentes roles,  
**para** controlar qué acciones puede realizar cada tipo de usuario.

**Criterios de aceptación:**
- La página `/permissions` muestra la configuración de permisos del sistema.
- Se pueden ajustar los permisos por rol (Administrador, Docente, Estudiante).
- Los cambios se guardan y aplican inmediatamente.
- Solo el administrador puede acceder a esta sección.

---

## 15. Vista del Estudiante — Exámenes

### HU-042 — Acceder a exámenes disponibles por competencia
**Como** estudiante,  
**quiero** ver y acceder a los exámenes que están habilitados para mi grado,  
**para** realizar las evaluaciones dentro de los plazos establecidos.

**Criterios de aceptación:**
- El estudiante ve solo los exámenes de las competencias asignadas a su grado.
- Se muestra el nombre del módulo, componente y competencia de cada examen.
- Se indica si el examen ya fue realizado o está pendiente.
- Se puede acceder al examen haciendo clic en él.

---

### HU-043 — Ver resultado inmediato tras completar un examen
**Como** estudiante,  
**quiero** ver mi resultado inmediatamente después de completar un examen,  
**para** conocer mi calificación y saber si aprobé o no.

**Criterios de aceptación:**
- Al finalizar el examen, el sistema muestra: puntaje obtenido, puntaje total y porcentaje.
- Se indica claramente si aprobó (≥60%) o reprobó (<60%).
- Se muestra el detalle de respuestas correctas e incorrectas.
- El resultado queda registrado en el historial del estudiante.

---

### HU-044 — Solicitar reinicio de examen desde mis resultados
**Como** estudiante,  
**quiero** solicitar desde mi página de resultados una nueva oportunidad de realizar un examen,  
**para** mejorar mi calificación si no aprobé.

**Criterios de aceptación:**
- El botón "Solicitar reiniciar evaluación" aparece disponible tras un resultado reprobado.
- La solicitud se envía y queda en estado pendiente.
- El estudiante ve el estado de sus solicitudes (pendiente, aprobada, denegada).
- Una solicitud aprobada habilita al estudiante para repetir el examen.

---

## Resumen de Historias de Usuario

| ID | Título | Rol Principal | Módulo |
|----|--------|---------------|--------|
| HU-001 | Inicio de sesión | Todos | Autenticación |
| HU-002 | Acceso restringido por rol | Sistema | Autenticación |
| HU-003 | Banners de bienvenida en el login | Administrador | Configuración |
| HU-004 | Dashboard de administrador | Administrador | Dashboard |
| HU-005 | Dashboard del estudiante | Estudiante | Dashboard |
| HU-006 | Listar módulos | Admin / Docente | Módulos |
| HU-007 | Crear módulo | Administrador | Módulos |
| HU-008 | Editar módulo | Administrador | Módulos |
| HU-009 | Eliminar módulo | Administrador | Módulos |
| HU-010 | Ver detalle de módulo | Admin / Docente | Módulos |
| HU-011 | Listar componentes | Administrador | Componentes |
| HU-012 | Crear componente | Administrador | Componentes |
| HU-013 | Editar y eliminar componentes | Administrador | Componentes |
| HU-014 | Ver competencias de un módulo | Admin / Docente | Competencias |
| HU-015 | Crear competencia | Administrador | Competencias |
| HU-016 | Crear preguntas de opción múltiple | Admin / Docente | Exámenes |
| HU-017 | Importar preguntas desde Excel | Administrador | Exámenes |
| HU-018 | Subir recursos multimedia | Admin / Docente | Exámenes |
| HU-019 | Realizar un examen | Estudiante | Exámenes |
| HU-020 | Ver mis exámenes disponibles | Estudiante | Exámenes |
| HU-021 | Listar estudiantes | Admin / Docente | Estudiantes |
| HU-022 | Registrar estudiante | Administrador | Estudiantes |
| HU-023 | Importar estudiantes desde Excel | Administrador | Estudiantes |
| HU-024 | Editar estudiante | Administrador | Estudiantes |
| HU-025 | Eliminar estudiante | Administrador | Estudiantes |
| HU-026 | Listar docentes | Administrador | Docentes |
| HU-027 | Registrar docente | Administrador | Docentes |
| HU-028 | Editar y eliminar docentes | Administrador | Docentes |
| HU-029 | Listar grados | Administrador | Grados |
| HU-030 | Crear grado | Administrador | Grados |
| HU-031 | Editar y eliminar grados | Administrador | Grados |
| HU-032 | Ver resultados de evaluaciones | Admin / Docente | Resultados |
| HU-033 | Exportar resultados a Excel/PDF | Admin / Docente | Resultados |
| HU-034 | Ver historial de evaluaciones | Estudiante | Resultados |
| HU-035 | Ver mis resultados | Estudiante | Resultados |
| HU-036 | Solicitar reinicio de evaluación | Estudiante | Reinicio |
| HU-037 | Gestionar solicitudes de reinicio | Admin / Docente | Reinicio |
| HU-038 | Recibir notificaciones | Todos | Notificaciones |
| HU-039 | Gestionar banners del login | Administrador | Configuración |
| HU-040 | Subir imágenes de banner | Administrador | Configuración |
| HU-041 | Configurar permisos del sistema | Administrador | Permisos |
| HU-042 | Acceder a exámenes por competencia | Estudiante | Exámenes |
| HU-043 | Ver resultado inmediato del examen | Estudiante | Exámenes |
| HU-044 | Solicitar reinicio desde resultados | Estudiante | Reinicio |

---

*Documento generado mediante análisis funcional de la plataforma EduScience (https://edu-science-jade.vercel.app/)*
