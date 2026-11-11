# 🧑‍💼 Taller guiado: TalentFlow — Registro empresarial de hojas de vida con Ionic y Firebase

> **Asignatura:** Desarrollo de Aplicaciones Móviles  
> **Modalidad:** Individual o en parejas (máximo 2 integrantes)  
> **Duración estimada:** 3 horas  
> **Tecnologías:** Ionic + Angular (Standalone) + Firebase Cloud Firestore  
> **Entregable:** Enlace a un repositorio público de GitHub con el proyecto y un `README.md` detallado.

---

## 1. Contexto empresarial

La empresa ficticia **TalentFlow S. A.** recibe hojas de vida por correo electrónico y tiene dificultades para organizar los perfiles de los candidatos. El equipo de Talento Humano necesita una **aplicación móvil de registro de postulantes** que capture información estructurada y la almacene en la nube para consultarla posteriormente.

Tu equipo ha sido contratado para desarrollar un **prototipo funcional (MVP)**. No es necesario implementar autenticación ni cargar archivos PDF: en este taller la hoja de vida se registra mediante un formulario digital.

**Referencia inicial:** [Registro de estudiantes con Ionic y Firebase](https://github.com/jzaldumbide/registro-estudiantes-ejemplo/tree/main). Utilicen este proyecto como punto de partida conceptual para la conexión con Firestore, la creación de documentos y su consulta. **No basta con cambiar las etiquetas**: deben adaptar el modelo de datos, los formularios y la interfaz al nuevo caso empresarial.

## 2. Objetivo

Construir una aplicación Ionic que permita **registrar, validar, almacenar y consultar perfiles profesionales** mediante formularios con diferentes componentes de Ionic y persistencia en **Firebase Cloud Firestore**.

## 3. Alcance mínimo obligatorio

La aplicación debe tener **al menos dos vistas o secciones claramente diferenciadas**:

1. **Registrar candidato:** formulario completo, organizado por bloques y con validaciones.
2. **Consultar candidatos:** listado recuperado desde Firestore, con un resumen por persona y opción para visualizar todos los datos de un registro (en una pantalla, modal o acordeón).

### 3.1. Campos requeridos del formulario

| Sección | Campo | Tipo de dato | Componente Ionic sugerido | Validación mínima |
|---|---|---|---|---|
| Datos personales | Nombres y apellidos | Texto | `ion-input` | Obligatorio; mínimo 3 caracteres |
| Datos personales | Correo electrónico | Texto | `ion-input` (`type="email"`) | Obligatorio; formato válido |
| Datos personales | Teléfono | Texto | `ion-input` (`type="tel"`) | Obligatorio; validar longitud razonable |
| Datos personales | Fecha de nacimiento | Fecha | `ion-datetime` | Obligatoria; no futura |
| Perfil profesional | Cargo al que postula | Selección | `ion-select` | Obligatorio; mínimo 4 opciones |
| Perfil profesional | Nivel académico | Opción única | `ion-radio-group` + `ion-radio` | Obligatorio |
| Perfil profesional | Años de experiencia | Número | `ion-input` (`type="number"`) | Entero entre 0 y 50 |
| Perfil profesional | Modalidad de trabajo | Opción única | `ion-segment` o `ion-radio-group` | Presencial, híbrida o remota |
| Perfil profesional | Habilidades técnicas | Selección múltiple | `ion-checkbox` | Al menos 1 habilidad |
| Disponibilidad | Fecha disponible para iniciar | Fecha | `ion-datetime` | Obligatoria; no anterior a hoy |
| Disponibilidad | Aspiración salarial (USD) | Número | `ion-input` (`type="number"`) | Mayor que 0 |
| Presentación | Resumen profesional | Texto largo | `ion-textarea` | Obligatorio; mínimo 30 caracteres |
| Consentimiento | Aceptación de tratamiento de datos de prueba | Booleano | `ion-checkbox` | Debe estar marcado para guardar |

**Opciones sugeridas:** cargos: Desarrollador móvil, Analista de datos, Diseñador UX/UI y Soporte TI; nivel académico: Bachillerato, Tecnología, Tercer nivel y Posgrado; habilidades: Angular, Ionic, JavaScript, Python, SQL, Git y Firebase.

> **Privacidad:** utilizar únicamente datos ficticios. El consentimiento del formulario es parte del ejercicio de interfaz y **no reemplaza** una política real de privacidad ni vuelve segura una base de datos abierta.

### 3.2. Componentes Ionic mínimos

Usar correctamente **al menos 8 tipos diferentes de componentes**, incluyendo obligatoriamente:

- `ion-input`, `ion-datetime`, `ion-radio-group`/`ion-radio`, `ion-select`, `ion-checkbox`, `ion-textarea`, `ion-button` y `ion-list`/`ion-item`.

Se recomienda incorporar también `ion-card`, `ion-toast`, `ion-alert`, `ion-modal`, `ion-segment`, `ion-spinner` o `ion-accordion` para mejorar la experiencia de usuario.

### 3.3. Funcionalidades obligatorias

- Guardar un candidato como **nuevo documento** en la colección `candidatos` de Cloud Firestore.
- Recuperar y presentar los documentos almacenados en `candidatos`.
- Mostrar un indicador de carga durante las operaciones asíncronas.
- Informar claramente si el registro se guardó o si ocurrió un error.
- Validar los datos **antes de enviarlos** a Firebase y evitar registros incompletos.
- Limpiar el formulario después de guardar correctamente.
- Permitir consultar el **detalle completo** de cada candidato.
- Registrar una marca temporal de creación mediante `serverTimestamp()` (recomendado) o una fecha equivalente claramente documentada.

**No se exige** editar, eliminar, subir archivos ni implementar autenticación. Pueden incorporarlos como mejoras si terminan antes.

## 4. Modelo de datos de referencia

Cada documento de `candidatos` puede tener esta estructura (ajusten nombres si mantienen consistencia):

```json
{
  "nombres": "Ana Pérez",
  "correo": "ana.ejemplo@correo.test",
  "telefono": "0990000000",
  "fechaNacimiento": "2000-04-15",
  "cargo": "Desarrollador móvil",
  "nivelAcademico": "Tercer nivel",
  "experienciaAnios": 3,
  "modalidad": "Híbrida",
  "habilidades": ["Ionic", "Angular", "Git"],
  "fechaDisponibilidad": "2026-12-01",
  "aspiracionSalarial": 1200,
  "resumen": "Desarrolladora de aplicaciones móviles con experiencia en interfaces y servicios web.",
  "consentimiento": true
}
```

Además, almacenar el campo `creadoEn` con la fecha de creación. **No copiar literalmente el JSON como un único texto**: los campos deben guardarse como atributos consultables del documento Firestore.

## 5. Desarrollo guiado: plan de 3 horas

| Tiempo | Actividad | Resultado esperado |
|---|---|---|
| **0–25 min** | Clonar o estudiar el ejemplo base; crear proyecto Firebase de práctica; configurar `environment.ts` y Firestore. | Conexión funcional a Firebase. |
| **25–75 min** | Definir interfaz `Candidato`; construir formulario con secciones, fechas, radio buttons, selects, checkboxes y textarea. | Formulario visual completo. |
| **75–115 min** | Programar validaciones y servicio Firebase; guardar datos con `addDoc()`. | Registro persistente en Firestore. |
| **115–145 min** | Consultar documentos con `getDocs()`; crear listado y vista de detalle. | Consulta y visualización de perfiles. |
| **145–180 min** | Probar con datos ficticios; mejorar mensajes y diseño; documentar y subir a GitHub. | Repositorio listo para entregar. |

### Paso A. Preparación

1. Revisen el repositorio de referencia y reconozcan dónde se configura Firebase, dónde está el servicio y cómo se consume desde la página Ionic.
2. Creen su propio proyecto de Firebase para el taller (o utilicen uno de pruebas autorizado).
3. Configuren la aplicación Angular Standalone e instalen las dependencias necesarias.
4. Creen la colección `candidatos` al guardar el primer documento.

### Paso B. Diseño de formulario

Organicen la captura en tarjetas, secciones o pasos visuales. Debe ser cómoda de usar en una pantalla de teléfono. Incluyan etiquetas claras, valores iniciales apropiados y mensajes de error cerca de los campos.

### Paso C. Persistencia

Implementen un servicio dedicado (por ejemplo, `CandidatosService`) que encapsule las operaciones de Firestore. El componente visual **no debe concentrar toda la lógica de acceso a la base de datos**.

### Paso D. Consulta

Muestren al menos el nombre, cargo, modalidad y experiencia en la lista. Al seleccionar un elemento, debe ser posible revisar el resto de la información guardada.

### Paso E. Pruebas

Ingresen **al menos 5 candidatos ficticios** con combinaciones distintas de cargo, nivel académico, modalidad y habilidades. Comprueben que los datos persisten después de recargar la aplicación y que los campos inválidos no se guardan.

## 6. Seguridad de Firebase

Este taller se realiza **sin autenticación** para concentrarse en los formularios y la persistencia. Si se habilitan reglas públicas de Firestore para la demostración, deben limitarse exclusivamente a la colección de pruebas `candidatos` y permitir únicamente las operaciones necesarias. **Nunca utilizar información personal real.**

Documenten en su `README.md` qué reglas emplearon, por qué **no son aptas para producción** y cómo restringirían el acceso en una aplicación empresarial real. Al finalizar las pruebas, cierren las reglas públicas. No publiquen cuentas de servicio, claves privadas, contraseñas ni archivos de credenciales administrativas.

## 7. Entregable: repositorio de GitHub

**Un solo repositorio por pareja** (o por estudiante si trabaja individualmente). El repositorio debe contener el código fuente completo y un archivo `README.md` propio, bien presentado, con:

1. **Nombre del proyecto** y descripción del problema empresarial.
2. **Integrantes:** nombres completos de quienes participaron.
3. **Objetivo** y funcionalidades implementadas.
4. **Tecnologías** y componentes Ionic utilizados.
5. **Arquitectura:** estructura de carpetas y explicación breve del servicio Firebase.
6. **Modelo de datos:** colección, campos, tipos y ejemplo de documento ficticio.
7. **Instalación y ejecución:** requisitos, `npm install`, configuración de Firebase e inicio de la aplicación.
8. **Evidencias:** capturas del formulario, validaciones, listado, detalle y documentos en Firestore (sin datos sensibles).
9. **Pruebas:** al menos 5 casos, incluidos 2 casos de validación fallida, con resultado esperado y obtenido.
10. **Seguridad y limitaciones:** explicación de las reglas de Firestore de práctica y mejoras necesarias para producción.
11. **Distribución del trabajo:** breve descripción del aporte de cada integrante, si trabajan en pareja.

**Forma de entrega:** enviar únicamente el **enlace al repositorio de GitHub** en la plataforma indicada por el docente. Verifiquen que el repositorio sea accesible y que el README se visualice correctamente desde su página principal.

## 8. Criterios de evaluación (100 puntos)

| Criterio | Puntos |
|---|---:|
| Formulario completo y organización visual del caso empresarial | 20 |
| Uso correcto de componentes Ionic requeridos | 15 |
| Validaciones y manejo de errores | 15 |
| Guardado correcto de documentos en Firebase Firestore | 20 |
| Listado y visualización de detalles desde Firestore | 15 |
| README, evidencias, pruebas y reproducibilidad del repositorio | 15 |
| **Total** | **100** |

## 9. Retos opcionales (sin sustituir requisitos obligatorios)

Si terminan antes de las 3 horas, pueden agregar un **filtro por cargo**, búsqueda por nombre, ordenamiento por fecha de registro, una vista previa de CV en formato tarjeta o edición de perfiles (con reglas de seguridad adecuadas). Describan cualquier funcionalidad adicional en el README.

---

### ✅ Lista de verificación antes de entregar

- [ ] El formulario contiene todos los campos requeridos.
- [ ] Se utilizaron al menos 8 tipos de componentes Ionic.
- [ ] Las fechas, radio buttons, selects y checkboxes funcionan correctamente.
- [ ] No se pueden guardar formularios inválidos.
- [ ] Se guardaron 5 candidatos ficticios en Firestore.
- [ ] El listado y el detalle leen los datos de Firebase.
- [ ] Se muestran mensajes de carga, éxito y error.
- [ ] El repositorio incluye código fuente y README detallado.
- [ ] El README contiene capturas, pruebas y explicación de seguridad.
- [ ] No se publicaron datos reales ni credenciales privadas.

**Resultado esperado:** un prototipo empresarial funcional que demuestre el uso de formularios Ionic y persistencia de datos en Firebase, partiendo del ejemplo de registro de estudiantes y ampliándolo a un caso de reclutamiento de talento.
