# Plataforma de Reservas de Espacios — FdI
## Requerimientos con sus Historias de Usuario asociadas

---

## 1. Roles del sistema

| Rol                   | Descripción                                                                   |
| --------------------- | ----------------------------------------------------------------------------- |
| **Administrador**     | Gestiona los espacios disponibles y visualiza todas las reservas del sistema. |
| **Docente**           | Crea, edita y cancela sus propias reservas.                                   |

---

## 2. Requerimientos Funcionales (RF) e Historias de Usuario

### RF-01 — Autenticación de usuarios con roles
El sistema debe permitir el registro e inicio de sesión de usuarios, diferenciando entre el rol Administrador y el rol Docente.

**Historias de usuario:**

- **HU-01 — Registro de usuario**
  > Como **visitante**, quiero registrarme en la plataforma con mi correo y contraseña, para poder acceder al sistema y realizar reservas.
  - Criterios de aceptación:
    - No se permite registrar dos cuentas con el mismo correo.
    - La contraseña debe cumplir con un mínimo de seguridad (longitud, complejidad).
    - Al registrarse, la cuenta queda con el rol "Docente" por defecto.

- **HU-02 — Inicio de sesión**
  > Como **usuario registrado**, quiero iniciar sesión con mis credenciales, para acceder a las funcionalidades correspondientes a mi rol.
  - Criterios de aceptación:
    - Si las credenciales son correctas, el usuario accede a su panel según su rol.
    - Si son incorrectas, se muestra un mensaje de error claro.

- **HU-03 — Acceso diferenciado por rol**
  > Como **administrador**, quiero que solo yo pueda gestionar espacios y ver todas las reservas, para mantener el control administrativo del sistema.
  - Criterios de aceptación:
    - Un usuario con rol "Docente" no puede acceder a las funciones administrativas.
    - Un intento de acceso no autorizado es rechazado.


---

### RF-02 — Gestión de espacios
El sistema debe permitir al administrador crear, editar, eliminar y listar espacios, con campos de nombre, descripción, capacidad y horario de disponibilidad.

**Historias de usuario:**

- **HU-04 — Crear espacio**
  > Como **administrador**, quiero registrar un nuevo espacio con nombre, descripción, capacidad y horario de disponibilidad, para que los usuarios puedan reservarlo.
  - Criterios de aceptación:
    - No se puede guardar un espacio sin nombre, capacidad u horario.
    - La capacidad debe ser un número mayor a cero.

- **HU-05 — Editar espacio**
  > Como **administrador**, quiero modificar los datos de un espacio existente, para mantener la información actualizada.
  - Criterios de aceptación:
    - Los cambios se reflejan de inmediato para los usuarios.

- **HU-06 — Eliminar/desactivar espacio**
  > Como **administrador**, quiero eliminar o desactivar un espacio, para que deje de estar disponible para reservas sin afectar el historial existente.
  - Criterios de aceptación:
    - Un espacio desactivado ya no aparece disponible para nuevas reservas.
    - Las reservas ya realizadas para ese espacio se conservan.

- **HU-07 — Listar espacios disponibles**
  > Como **docente**, quiero ver la lista de espacios disponibles con su capacidad y horario, para elegir dónde reservar.
  - Criterios de aceptación:
    - Solo se listan los espacios activos.

---

### RF-03 — Calendario interactivo con validación de conflictos
El sistema debe mostrar un calendario con las reservas existentes y evitar conflictos de horario al momento de reservar.

**Historias de usuario:**

- **HU-08 — Ver calendario de reservas**
  > Como **docente**, quiero visualizar un calendario con las reservas existentes de un espacio, para identificar los horarios libres.
  - Criterios de aceptación:
    - El calendario muestra las reservas por espacio y por fecha.

- **HU-09 — Crear una reserva**
  > Como **docente**, quiero reservar un espacio en un horario disponible, para asegurar su uso en la fecha que necesito.
  - Criterios de aceptación:
    - No se permite reservar un espacio si ya existe otra reserva en ese horario.
    - No se permite reservar más personas de las que el espacio puede recibir.
    - Si dos usuarios intentan reservar el mismo horario al mismo tiempo, solo una reserva se concreta y al otro usuario se le informa que el horario ya no está disponible.

- **HU-10 — Editar una reserva**
  > Como **docente**, quiero modificar mi reserva, para ajustar horario o detalles, siempre que falte más de 1 día para la fecha reservada.
  - Criterios de aceptación:
    - No se permite editar una reserva si falta menos de 1 día para el evento.
    - El nuevo horario también debe estar libre de conflictos.

- **HU-11 — Cancelar una reserva**
  > Como **docente**, quiero cancelar una reserva que ya no necesito, para liberar el espacio a otros usuarios.
  - Criterios de aceptación:
    - Solo el dueño de la reserva o un administrador pueden cancelarla.
    - No se permite cancelar después de la fecha límite establecida.

- **HU-12 — Visualizar todas las reservas (admin)**
  > Como **administrador**, quiero ver todas las reservas del sistema, para supervisar el uso de los espacios.


---

### RF-04 — Envío de correos electrónicos
El sistema debe enviar automáticamente un correo de confirmación al crear una reserva y un recordatorio 24 horas antes del evento.

**Historias de usuario:**

- **HU-13 — Confirmación de reserva**
  > Como **docente**, quiero recibir un correo de confirmación al crear una reserva, para tener constancia de que se realizó correctamente.
  - Criterios de aceptación:
    - El correo se envía inmediatamente después de confirmar la reserva.

- **HU-14 — Recordatorio automático**
  > Como **docente**, quiero recibir un correo recordatorio 24 horas antes de mi reserva, para no olvidar el evento.
  - Criterios de aceptación:
    - El correo llega aproximadamente 24 horas antes de la hora reservada.


---

### RF-05 — Documentación técnica e integración
El sistema debe entregarse con documentación técnica (funcionalidades + diagrama de base de datos) y un frontend funcional que consuma correctamente la API REST.

**Historias de usuario:**

- **HU-15 — Documentación técnica**
  > Como **evaluador**, quiero contar con documentación clara del sistema, para entender sus funcionalidades y su diseño de datos.

- **HU-16 — Integración frontend-backend**
  > Como **docente**, quiero que la aplicación funcione de principio a fin sin errores, para tener una experiencia de uso completa.

---

## 3. Requerimientos No Funcionales (RNF)

| ID | Requerimiento |
|---|---|
| RNF-01 | El sistema debe manejar validación de datos tanto en frontend como en backend. |
| RNF-02 | El envío de correos debe hacerse mediante colas (queues) para no afectar el rendimiento de la API. |
| RNF-03 | El acceso a rutas administrativas debe protegerse mediante middleware de autorización. |
| RNF-04 | La documentación técnica debe incluir diagrama de base de datos y descripción de funcionalidades. |

---
