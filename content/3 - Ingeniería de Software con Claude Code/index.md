El objetivo inicial de este bloque era proporcionar una plantilla general que pudiera ser usada para construir proyectos grandes usando Claude Code.
Sin embargo, conforme tratábamos de simplificar el proceso, siempre llegábamos al mismo lugar: la Ingeniería de Software.
En este bloque hacemos una pequeña introducción del concepto de ingeniería de software y cómo empezar a aplicarlo con Claude Code.

---
# Índice

- [Ingeniería de Software](#ingeniería-de-software)
  - [SDLC: Ciclo de vida del desarrollo de software](#sdlc-ciclo-de-vida-del-desarrollo-de-software)
- [Cómo construir software paso a paso](#cómo-construir-software-paso-a-paso)
  - [Paso 1: Plan](#paso-1-plan)
  - [Paso 2: Documento de especificación de requisitos](#paso-2-documento-de-especificación-de-requisitos)
  - [Paso 3: Diseño inicial](#paso-3-diseño-inicial)
  - [Paso 4: Preparar el entorno](#paso-4-preparar-el-entorno)
  - [Paso 5: Implementar historias de usuario](#paso-5-implementar-historias-de-usuario)
- [Siguientes pasos](#siguientes-pasos)

---

# Contenido

## Ingeniería de Software
En el bloque 1 le pedíamos a Claude Code que construya el proyecto a través de prompts en lenguaje natural, sin seguir algún flujo específico; esto es básicamente [vibe coding](https://en.wikipedia.org/wiki/Vibe_coding). El vibe coding funciona bastante bien para proyectos pequeños o medianos; sin embargo, cuando tenemos proyectos grandes o tenemos necesidades muy específicas, depender solamente del vibe coding puede producir resultados algo desastrosos.

Por otro lado, la **ingeniería de software** es la disciplina que implica el uso de estructuras, métodos y herramientas para la construcción del software de manera sistemática.
<br>
### SDLC: Ciclo de vida del desarrollo de software
La ingeniería de software abarca el **ciclo de vida de desarrollo de software** (SDLC). Según el autor, el nombre y número de fases puede cambiar, pero generalmente son:

**1. Planificación**
Se define el problema a resolver, se establecen los recursos del equipo (personas, herramientas, presupuesto), se identifican riesgos y se definen tiempos de entrega.

**2. Requerimientos**
Se obtienen los requerimientos de los usuarios. Se generan documentos de requerimientos que describen qué debe hacer el sistema.

**3. Diseño**
Se empieza a dar forma al sistema: se decide qué tipo de producto será (app móvil, web, escritorio), se define la arquitectura del sistema, se diseña la base de datos, se bocetan las interfaces de usuario, etc.

**4. Implementación**
Se escribe el código, se realizan revisiones de código entre el equipo y se escribe la documentación técnica necesaria.

**5. Pruebas**
Se realizan distintos tipos de pruebas para verificar que el sistema funciona correctamente.

**6. Despliegue**
Las nuevas funcionalidades se integran con el sistema en producción.

**7. Mantenimiento**
Una vez desplegado, el sistema necesita mantenimiento continuo: corregir errores, mejorar el rendimiento y aplicar actualizaciones de seguridad.


Estudiar la ingeniería de software ya es en sí una tarea muy extensa (por eso tenemos una materia entera dedicada a ello).
En este bloque solo vamos a abordar un flujo simplificado, el cual consideramos que mejorando solamente esa parte, ya se logran mejores resultados al crear proyectos.
<br><br>
## Cómo construir software paso a paso

Supongamos que nos plantean el siguiente problema:

```text
Desarrollar un sistema web que permita gestionar reservas de espacios de las diversas instalaciones de la Facultad de Ingeniería con roles de usuario diferenciados.

El sistema debe permitir que los usuarios inicien sesión según su rol. Los administradores podrán gestionar los espacios y consultar todas las reservas, mientras que los docentes podrán crear, consultar, modificar y cancelar sus propias reservas. Las modificaciones solo podrán realizarse hasta un día antes de la fecha reservada.

Se debe implementar un calendario interactivo que muestre las reservas existentes y verificará la disponibilidad de los espacios. Además, el sistema debe enviar un correo electrónico de confirmación al crear una reserva y otro como recordatorio 24 horas antes del evento
```
<br>
### Paso 1: Plan
En este paso debemos entender el problema que se quiere resolver. Es importante identificar el alcance del proyecto, nuestros recursos, plazos y responsables.
- **Problema**: La facultad necesita una forma organizada de gestionar la reservación de sus espacios.
- **Tipo de producto**: Sistema web.
- **Usuarios**: Administradores y docentes.
- **Funcionalidades clave**: Autenticación con roles, gestión de espacios, calendario interactivo, validación de conflictos y envío de correos.
- **Restricción de negocio**: Las modificaciones solo se pueden hacer hasta 1 día antes de la fecha reservada.
<br>
### Paso 2: Documento de especificación de requisitos
Un **documento de especificación de requisitos** detalla exactamente lo que el sistema debe hacer para satisfacer las necesidades del usuario. Es la fuente de verdad del sistema.

Existen muchas plantillas para estos documentos, unas más detalladas que otras. Para este curso proponemos la siguiente estructura simplificada:

```markdown
# [Nombre del Proyecto]
## Requerimientos del sistema

---

# 1. Roles del sistema

| Rol | Descripción |
|---|---|
| **[Rol 1]** | [Descripción del rol.] |
| **[Rol 2]** | [Descripción del rol.] |
| **[Rol N]** | [Descripción del rol.] |


# 2. Requerimientos Funcionales

## RF-01 — [Nombre del requerimiento]

**Descripción**
El sistema debe permitir **[describir la funcionalidad principal del requerimiento]**.


## RF-02 — [Nombre del requerimiento]

**Descripción**
El sistema debe permitir **[descripción del requerimiento]**.


# 3. Requerimientos No Funcionales
| ID | Requerimiento |
|---|---|
| RNF-01 | [Tecnología obligatoria.] |
| RNF-02 | [Seguridad.] |
| RNF-03 | [Rendimiento.] |
| RNF-04 | [Usabilidad.] |
| RNF-05 | [Disponibilidad.] |
| RNF-06 | [Escalabilidad.] |
| RNF-07 | [Documentación.] |
| RNF-08 | [Mantenibilidad.] |

```

Es importante definir los 3 puntos de nuestro documento de ejemplo:
**Roles del sistema**
¿Quiénes van a usar el sistema y qué permisos tiene cada uno? En nuestro ejemplo tenemos dos: Administrador y Docente.

**Requerimientos funcionales (RF)**
Describen *qué* debe hacer el sistema. Cada RF se identifica con un código (RF-01, RF-02, etc.) y una descripción clara. 

**Requerimientos no funcionales (RNF)**
Describen *cómo* debe comportarse el sistema: rendimiento, seguridad, mantenibilidad, etc. No son funcionalidades visibles para el usuario, pero son igual de importantes.

#### Historias de usuario
Los requerimientos funcionales nos dicen *qué* debe hacer el sistema, pero no *cómo* interactúa el usuario con él. Para eso usamos **historias de usuario**.
Una historia de usuario describe una funcionalidad desde la perspectiva del usuario final. Tiene la siguiente estructura:

> Como **[rol]**, quiero **[acción]**, para **[beneficio u objetivo]**.

Además, cada historia incluye **criterios de aceptación**: condiciones concretas que deben cumplirse para considerar la historia como terminada.

Para nuestro documento de requisitos, a cada requerimiento funcional le añadimos sus historias de usuario.
El siguiente ejemplo nos muestra cómo quedaría uno de nuestros requerimientos **Gestión de espacios** ya con sus historias de usuario detalladas.

```markdown
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

```

> **Nota**: Escribir buenos requerimientos e historias de usuario es una habilidad que se desarrolla con la práctica. No es necesario que sean perfectos; lo importante es que transmitan claramente la intención. Claude Code también puede ayudarnos a generar o refinar este documento a partir de una descripción general del problema.

 >**Nota**: Es muy común que el contenido del documento de requerimientos cambie a través del tiempo, especialmente si trabajamos bajo metodologías ágiles. Personalmente, nosotros constantemente estamos haciendo revisiones a nuestros documentos.


Mira el ejemplo completo: [[Ejemplo Requerimientos]]
<br>
### Paso 3: Diseño inicial
Antes de empezar a escribir código, necesitamos tomar algunas decisiones de diseño. Estas decisiones dependen del contexto del proyecto: las tecnologías que el equipo domina, las restricciones del problema y los recursos disponibles.

Las decisiones de diseño más comunes son:
- **Stack tecnológico**: ¿Qué lenguaje, framework y base de datos usar?
- **Arquitectura**: ¿Monolito o microservicios? ¿MVC, API REST, fullstack? 
- **Modelo de datos**: ¿Qué tablas o colecciones necesitamos? ¿Cómo se relacionan entre sí?
- **Interfaces de usuario**: ¿Cómo se van a ver las pantallas principales? (Aquí es donde Claude Design puede ayudar mucho, como vimos en el bloque 2.)

No es necesario tener todo resuelto en esta etapa. El objetivo es tener un punto de partida claro que nos evite reescribir grandes partes del proyecto después.
  
Claude Code puede ser de mucha ayuda en esta fase. Podemos usar el **Plan Mode** para que nos proponga una arquitectura, un esquema de base de datos o un stack tecnológico basándose en los requerimientos que ya tenemos escritos. 
Por ejemplo, podemos darle el siguiente prompt:
```text
Tengo el siguiente documento de requerimientos para un sistema de reservas:
@requerimientos.md

Propón un stack tecnológico, una arquitectura y un diseño inicial de base de datos. 
Quiero que sea una app web fullstack. Tengo experiencia con Vue.js y TypeScript.
```
<br>
### Paso 4: Preparar el entorno
Antes de empezar a implementar funcionalidades, necesitamos que el proyecto base esté listo: la estructura de carpetas, las dependencias instaladas, la base de datos configurada y el archivo `CLAUDE.md` creado.

Este es un buen momento para darle a Claude Code un prompt inicial que prepare todo:

```text
Crea un proyecto Nuxt 3 llamado "reservas-fdi" con TypeScript y Tailwind CSS.
Configura Prisma ORM con SQLite como base de datos.
Crea el esquema inicial de Prisma con las tablas: User, Space y Reservation,
basándote en los requerimientos del proyecto: @requerimientos.md
Genera un seed con datos de prueba y ejecútalo.
```

Una vez que el proyecto base esté listo, recordemos generar el archivo `CLAUDE.md` con `/init` para que Claude Code tenga el contexto del proyecto en futuras sesiones.
<br>
### Paso 5: Implementar historias de usuario

Ahora podemos pasar directamente las historias de usuario a Claude Code con sus criterios de aceptación.
En lugar de pedirle a Claude Code funcionalidades vagas, ahora tenemos una estructura más definida, lo que reduce la ambigüedad y los errores.

El flujo es el siguiente:

**1. Organizar las historias en el proyecto**
Podíamos simplemente tener nuestros requerimientos en un documento de Google e ir pegando las historias de usuario en el chat cada que queramos implementarlas.
Otra opción que recomendamos es hacer que los requerimientos sean parte del código fuente del proyecto
Creamos una carpeta `requerimientos/` donde cada subcarpeta representa un requerimiento funcional y dentro están las historias de usuario como archivos markdown:

```
requerimientos/
├── rf-01-autenticacion/
│   ├── 01-registro-usuario.md
│   ├── 02-inicio-sesion.md
│   └── 03-acceso-por-rol.md
├── rf-02-gestion-espacios/
│   ├── 04-crear-espacio.md
│   └── ...
├── rf-03-calendario/
│   ├── 09-crear-reserva.md
│   └── 10-editar-reserva.md
└── rf-04-emails/
    ├── 13-confirmacion-reserva.md
    └── 14-recordatorio.md
    
```

Cada archivo contiene la historia de usuario completa.

**2. Pasar la historia de usuario a Claude Code**
Para implementar una historia, simplemente le pasamos el archivo a Claude Code usando `@`:

```text
Implementa la siguiente historia de usuario: @requerimientos/rf-03-calendario/09-crear-reserva.md
```

Claude Code leerá la historia, entenderá los criterios de aceptación y generará la implementación correspondiente. Al tener criterios específicos (como "no permitir reservar si ya existe otra reserva en ese horario"), el agente sabe exactamente qué validaciones incluir sin que tengamos que pensarlas por él.

**3. Verificar y hacer commit**
Después de cada historia implementada, verificamos que funcione correctamente y hacemos commit. Esto nos permite tener un historial limpio y la posibilidad de revertir cambios si algo sale mal.

> **Tip**: Si una historia de usuario es muy grande o compleja, puedes dividirla en subtareas más pequeñas. Es mejor implementar de a poco y verificar que cada parte funciona, que intentar construir todo de golpe.



Puedes ver el resultado final de este bloque en este [repositorio](https://github.com/AleksPrz/reservas-fdi).

---

# Siguientes pasos

Te recordamos que este proceso es una simplificación del ciclo de vida del desarrollo de software.
En este ejemplo juntamos los requerimientos y las historias de usuario en un mismo documento. Usualmente viven en artefactos distintos ya que suelen llevar más nivel de detalle. También existen los casos de uso, los cuales son otro tipo de documento.
Cada equipo define los artefactos que necesita, algunos utilizan solo requerimientos y casos de uso, otros solo requerimientos e historias de usuario, e incluso hay quienes deciden usar los tres artefactos.

Cuando te haya quedado claro este bloque, te recomendamos muchísimo [este video](https://youtu.be/v4F1gFy-hqg?si=uE9VfIBxqEULSAHl), en donde se detallan las limitaciones del proceso que te mostramos y cuál es su evolución "natural" para eliminarlos.

El proceso que vimos es algo similar a empezar a aplicar el [Spec Driven Development](https://specdriven.ai/), metodología para la cual ya existen herramientas que te permiten trabajar con ella, como lo es el [GitHub Spec Kit](https://github.github.com/spec-kit/).

---

# Recursos
- [Software Development Life Cycle (SDLC) — GeeksforGeeks](https://www.geeksforgeeks.org/software-development-life-cycle-sdlc/)
- [Use Cases](https://www.figma.com/resource-library/what-is-a-use-case/)
- [User Stories — Mountain Goat Software](https://www.mountaingoatsoftware.com/agile/user-stories)
- [Claude Code: Best Practices](https://code.claude.com/docs/en/best-practices)
