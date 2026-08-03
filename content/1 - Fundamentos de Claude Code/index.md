
Este bloque es un tutorial guiado en el que construimos un proyecto desde cero con Claude Code a través de prompts. Puedes ver el proyecto terminado [aquí](https://github.com/AleksPrz/linkstash).
Durante el tutorial introducimos los conceptos básicos necesarios para poder empezar a utilizar Claude Code para crear proyectos desde cero.
La instalación de Claude Code no entra en el alcance de este tutorial, para eso recomendamos [la guía oficial de Inicio Rápido](https://code.claude.com/docs/es/quickstart). Para este tutorial estaremos usando Claude Code en la terminal. 

---
# Índice

- [Qué es Claude Code](#qué-es-claude-code)
- [Cómo funciona Claude Code](#cómo-funciona-claude-code)
  - [Agentic Loop](#agentic-loop)
  - [Tools](#tools)
- [El flujo de trabajo "Explore, Plan, Code, Commit"](#el-flujo-de-trabajo-explore-plan-code-commit)
- [Gestión de contexto](#gestión-de-contexto)
  - [Claude es stateless](#claude-es-stateless-sin-estado)
  - [Comandos para la gestión del context window](#comandos-para-la-gestión-del-context-window)
- [CLAUDE.md](#claudemd)

---

# Contenido

## Qué es Claude Code

**Diferencia entre las interfaces de chat y los agentes**

Muchos utilizamos [ChatGPT.com](https://chatgpt.com) o [Claude.ai](https://claude.ai) para nuestras actividades del día a día. Podemos decir que estas páginas son interfaces de chat.
En un inicio solamente recibían texto y producían texto, aunque en la actualidad tienen más capacidades como subir archivos y realizar búsquedas. Pero las acciones están limitadas al contexto del navegador, es decir, no pueden "ver" ni "hacer" más allá de lo que exista en esa pestaña del navegador.

Claude Code, por otro lado, es un agente.

Ambos, tanto chats como agentes de IA, utilizan el mismo "cerebro", el [LLM](https://en.wikipedia.org/wiki/Large_language_model), el cual les permite procesar el texto. La diferencia entre uno y otro es que mientras los chats solo responden a preguntas, **los agentes pueden realizar acciones**.

Claude Code se instala en la computadora y tiene acceso a herramientas que le permiten operar con el entorno, ejecutando acciones como:
- Leer archivos
- Escribir archivos
- Ejecutar comandos de terminal
- Correr tests

Existen distintas formas de utilizar Claude Code:
- Como una extensión de un IDE (VSCode, etc.).
- Como un programa de terminal (el más usado).

**Dato curioso**
Claude Code en realidad era una herramienta interna de Anthropic, no fue pensada como un producto comercial. Su propósito principal era automatizar la generación de código.
Empiezó a tomar forma como una herramienta de terminal alrededor del año 2024.
En febrero de 2025 se lanzó como un preview de investigación.
En la segunda mitad de 2025 ya se reportaba un crecimiento enorme en usuarios e ingresos.
<br>
## Cómo funciona Claude Code

**Explicación del proyecto**
Para introducir los conceptos fundamentales iremos construyendo un proyecto paso a paso.
El proyecto es Linkstash, una aplicación web que permite al usuario crear y guardar marcadores de páginas (como la barra de favoritos de Chrome).
Para eso basta con ir pegando en el chat de Claude Code los prompts que mostramos.


**Prompt 1: Crear el proyecto**
```
Crea una app Next.js llamada "linkstash" usando App Router, TypeScript y Tailwind CSS.
Usa better-sqlite3 para una base de datos SQLite local.

La app debe tener:
- Una tabla "bookmarks" con columnas: id, url, title, description, created_at
- Un script de seed (scripts/seed.ts) que inserte 5 marcadores de ejemplo con datos en español
- Una página principal que liste todos los marcadores (más recientes primero) mostrando título, URL y descripción
- Un formulario en la parte superior para agregar un nuevo marcador (URL + título + descripción opcional)
- Server actions para agregar y listar marcadores
- Estilizado limpio y minimalista con Tailwind
- Toda la interfaz en español (botones, labels, placeholders, títulos)

Ejecuta el script de seed después de crearlo.
```

Al mandarle el prompt, Claude Code empieza a "pensar" en lo que debe hacer para llegar a la meta que se plantea.
Como podremos notar, Claude Code pide permisos para editar archivos o ejecutar comandos.

**Resultados**
- [ ] App creada sin errores
- [ ] La página muestra 5 marcadores
- [ ] El formulario permite agregar marcadores
- [ ] Los nuevos marcadores aparecen en la lista sin recargar la página manualmente

¿Qué acaba de suceder?, ¿cómo es posible que a partir de una simple instrucción de texto ahora tengamos como resultado una aplicación bastante decente?
<br>
### Agentic Loop

Primero, el usuario introduce un prompt, usualmente una pregunta normal o una meta a alcanzar.
Luego el agente:
1. Reúne el contexto
2. Actúa
3. Verifica resultados
4. Repite hasta alcanzar la meta del prompt
Esa serie de pasos es el agentic loop (o bucle agéntico).
Durante este proceso el usuario puede interrumpir al agente para añadir más información si es necesario.

Si el agentic loop fuera código, se vería algo así:
```python
while not objective_completed:
    think()
    action = choose_tool()
    observation = execute(action)
    update_context(observation)

return final_answer
```

**¿Qué significa "think"?**
No significa que el modelo esté "consciente".
Simplemente analiza:
- ¿Qué sé?
- ¿Qué me falta?
- ¿Qué herramienta debo usar?
- ¿Qué hago después?

**¿Qué significa "act"?**
Es llamar una herramienta.
Por ejemplo:
- Leer un archivo
- Escribir un archivo
- Ejecutar un comando
- Buscar en Google
- Consultar una base de datos
- Ejecutar pruebas

Es importante notar que en este contexto, actuar se refiere a realizar acciones atómicas, es decir, Claude Code no "Crea una aplicación", esa es una tarea muy compleja; en su lugar, realiza tareas más pequeñas como "Crear carpeta project/", "Crear archivo user.ts", "Correr migración", etc., que le permiten llegar a esa meta.
<br>
### Tools
Las tools (herramientas) son funciones externas que un LLM puede invocar para hacer cosas.
Claude Code incluye varias tools básicas como:
```
Read
Write
Edit
MultiEdit
Bash
Search
Glob
Grep
```

Para agregar más funciones a nuestro proyecto, podemos seguir utilizando prompts especificando lo que queremos.

**Prompt 2: Añadir un botón para eliminar un marcador**
```
Agrega un botón de eliminar a cada marcador de la lista. 
Al hacer clic, debe eliminar el marcador de la base de datos y actualizar la lista sin recargar la página completa. 
Usa un server action para la eliminación. El botón debe decir "Eliminar" y tener un estilo rojo sutil.
```

**Prompt 3: Agregar un mensaje al eliminar un marcador**
```
Agrega una confirmación visual cuando un marcador se elimina exitosamente — una notificación toast en la parte inferior de la pantalla que desaparezca después de 3 segundos. 
El mensaje debe decir "Marcador eliminado". Hazlo simple, sin librerías externas.
```

**Resultados**
- [ ] El botón "Eliminar" aparece junto a cada marcador
- [ ] Al hacer clic, el marcador desaparece sin recargar la página
- [ ] Aparece un toast "Marcador eliminado" que desaparece en 3 segundos
<br><br>

## El flujo de trabajo "Explore, Plan, Code, Commit"

Si solo nos la pasamos escribiendo prompts pidiéndole a Claude Code que implemente nuevas funciones, inevitablemente vamos a terminar enfrentando muchos errores, y sin darnos cuenta, terminamos desperdiciando tokens tratando de arreglar cosas que se pudieron haber evitado desde un principio.

El flujo de trabajo ideal para trabajar con Claude Code es:
1. **Explore**: Entender el código actual
2. **Plan**: Definir qué cambiar y cómo
3. **Code**: Implementar los cambios aprobados
4. **Commit**: Revisar y subir los cambios

**Plan Mode**
El Plan Mode (modo plan) es un modo de Claude Code en el que solo se utilizan tools de lectura para obtener información y proponer un plan detallado antes de empezar a escribir código. El modo plan es probablemente una de las funcionalidades más útiles y utilizadas en este tipo de herramientas.

En realidad, Claude Code tiene 3 modos principales:
- **Ask permissions (Default)**: Claude te pregunta antes de cada cambio importante.
- **Accept Edits**: Claude modifica archivos automáticamente, pero sigue preguntando antes de ejecutar comandos.
- **Plan Mode**: Claude inspecciona el proyecto, explica lo que haría y presenta un plan, pero **no cambia ningún archivo**. Puedes activarlo con `/plan` para una sola petición o cambiar el modo de la sesión.

Para cambiar el modo de la sesión basta con presionar `Shift + Tab` hasta ver que debajo del input de texto diga "plan mode on".

Para el siguiente prompt, necesitamos tener el Modo Plan activo.

**Prompt 4 (Usar Plan Mode): Agregar obtención automática de metadatos**
```
Quiero agregar obtención automática de metadatos para los marcadores. Cuando un usuario guarde una URL nueva, la app debe:

1. Obtener el título, descripción e imagen de Open Graph de la página
2. Si el usuario dejó el título en blanco, usar el título OG obtenido
3. Almacenar la URL de la imagen OG y la descripción en columnas nuevas de la base de datos
4. Mostrar la imagen OG como una miniatura pequeña junto a cada marcador en la lista

Analiza qué archivos necesitan cambiar, qué librería usar para obtener/parsear el HTML, y qué casos borde manejar (URLs inválidas, páginas sin tags OG, timeouts).
Dame un plan.
```

Claude Code nos da un plan detallado sobre cómo implementar lo que le acabamos de pedir. Al finalizar, nos pregunta de qué manera queremos seguir y tenemos 3 opciones.
Utiliza la tercera opción "Tell claude what to change" la cual nos permite añadir más contexto o pedir correcciones, e introduce el siguiente prompt.

**Prompt 5: Implementar el plan**
```
Implementa el plan. Agrega las columnas og_image y og_description a la tabla bookmarks, actualiza el script de seed, e implementa la obtención de metadatos. Usa un timeout de 5 segundos en los fetch y maneja fallos graciosamente (simplemente guarda el marcador sin metadatos si falla). Muestra un texto "Sin vista previa disponible" cuando no haya imagen OG.
```

**Resultados**
- [ ] Los metadatos OG se obtienen al guardar un marcador con URL real
- [ ] Las miniaturas se muestran correctamente en la lista
- [ ] URLs inválidas se manejan sin crashear la app

<br><br>
## Gestión de contexto

### Claude es stateless (sin estado)
De manera muy simplificada, un LLM es una red neuronal muy grande con pesos fijos que puede verse como una función: recibe un texto (la conversación hasta ese punto) y devuelve el siguiente token más probable. Ese token se añade al texto y el proceso se repite una y otra vez, token a token, hasta formar la respuesta completa.

El modelo (el LLM) no guarda memoria interna de conversaciones anteriores, no está diseñado para eso, cada petición se procesa únicamente con el texto que recibe en ese momento, sin nada más. Por eso se dice que es _stateless_.

**¿Cómo es que Claude parece recordar entre peticiones?**
Como ya mencionamos antes, el modelo define una _context window_ (ventana de contexto), que determina cuántos tokens puede recibir y procesar en una sola petición (entrada + salida).

Claude Code se encarga de manejar esa ventana de contexto por nosotros. En ella almacena:
- El historial de la conversación
- Los archivos leídos en memoria
- La salida de las tool calls
- etc.

Así, para dar la sensación de que "recuerda", Claude Code reenvía el contexto acumulado de la conversación en cada nueva petición. Lo mismo ocurre en la versión de navegador (claude.ai).
<br>
### Comandos para la gestión del context window
Ejecuta cada comando en el orden en el mismo orden en el que aparecen.

**`/context`**
Muestra gráficamente el uso de la ventana de contexto.

**`/compact`**
Inicia el proceso de **compactación** del contexto manualmente. 
Claude Code resume los puntos más importantes de la conversación, manteniendo solo los detalles críticos y descartando salidas de tool calls, contenido de archivos, etc. 
Este proceso también se ejecuta automáticamente cuando la ventana de contexto está a punto de llenarse.

**`/clear`**
Borra todo el historial y empieza una sesión nueva desde cero.

**Regla general:** usa `/compact` cuando estés a mitad de la implementación de una feature y quieras conservar el historial relevante. Usa `/clear` cuando empieces a trabajar en una feature nueva y no necesites el contexto anterior.

**Por qué importan los prompts bien definidos**
Un prompt mal definido es costoso para la ventana de contexto: obliga a Claude Code a hacer más tool calls y leer más archivos para entender nuestra intención.
Por ejemplo, este prompt es demasiado ambiguo:
```
Agrega etiquetas a la app
```

En este punto, como ejecutamos el comando `/clear`, Claude Code no recuerda nada del proyecto, por lo que tendría que escanearlo completamente para entender a qué nos referimos con "etiquetas" y dónde agregarlas.

El siguiente es un ejemplo de un buen prompt:

**Prompt 6: Agregar un sistema de etiquetas**
```
Estoy trabajando en LinkStash, una app Next.js de gestión de marcadores con SQLite via better-sqlite3. Necesito agregar un sistema de etiquetas:

1. Crear una tabla "tags" (id, name) y una tabla de relación "bookmark_tags" (bookmark_id, tag_id)
2. Agregar un campo de etiquetas al formulario de nuevo marcador (etiquetas separadas por coma)
3. Mostrar las etiquetas como pills de colores debajo de cada marcador
4. Agregar una barra lateral con todas las etiquetas que filtre los marcadores al hacer clic

Sigue los server actions y el patrón de base de datos ya existentes en el proyecto.
```

>**Nota**: Puedes señalarle a Claude Code exactamente en qué archivo se encuentran los server actions o el patrón de base de datos utilizando el símbolo @ seguido del nombre del proyecto. Ejemplo: `@app/page.tsx`.


**Algunos tips para hacer mejor uso de la ventana de contexto**
- Escribir prompts específicos
- Usar los comandos `/clear` y `/compact` constantemente para descartar contexto innecesario
- Siempre que se pueda, decirle a Claude Code qué archivo leer usando el caracter @
- No mandarle archivos Word, PDF o Excel directamente a Claude Code. Es mejor convertirlos a markdown usando herramientas como [markitdown](https://github.com/microsoft/markitdown)


Como último punto, el comando `/status` muestra un resumen del estado de nuestra conectividad y, en el apartado "Usage", podemos ver cuánto porcentaje de nuestro límite de tokens hemos utilizado hasta el momento.

**Resultados**
- [ ] El sistema de etiquetas funciona (agregar, mostrar, filtrar)
<br><br>
## CLAUDE.md

Cada vez que iniciamos un chat nuevo con Claude Code empezamos con una "memoria en blanco" pues no recuerda nada. Es el mismo comportamiento que al abrir un chat nuevo en [claude.ai](https://claude.ai).
Esto es un problema ya que Claude Code tendría que analizar todo el proyecto al inicio de cada sesión para entender de qué se trata, cómo está organizado y qué convenciones sigue, gastando tokens y espacio en nuestra ventana de contexto.
El archivo `CLAUDE.md` resuelve esto: es un archivo que Claude Code carga automáticamente al inicio de cada sesión, y que le da contexto persistente sobre el proyecto. El concepto es similar al de un `README.md`, solo que en lugar de estar dirigido a otros desarrolladores, está dirigido al agente.
Este es un archivo que **no puede faltar en los proyectos en los que usemos Claude Code**.

**Jerarquía de archivos CLAUDE.md**
Claude Code puede cargar varios archivos `CLAUDE.md` a la vez, organizados en una jerarquía de mayor a menor alcance:
- **Nivel proyecto**: Se encuentra dentro de la carpeta del proyecto, ya sea en la carpeta raíz o dentro de la carpeta `.claude/`. Se utiliza para establecer el contexto y las convenciones del proyecto. Este archivo se debe compartir entre el equipo de desarrollo (es decir, debe ser parte del repositorio de git).
- **Nivel usuario**: Se coloca dentro de la carpeta de configuración de Claude Code. Se carga al iniciar cualquier conversación dentro de cualquier carpeta de nuestro sistema. Aquí se suelen escribir preferencias de usuario que se deben aplicar independientemente del proyecto.

Los archivos de niveles más generales se cargan primero y sirven de base sobre la cual se construyen los más específicos. Todos se cargan juntos en cada sesión.

**Cómo generar el archivo CLAUDE.md**
El CLAUDE.md al final no deja de ser un simple archivo de texto en formato markdown, podemos escribirlo manualmente y funcionará perfectamente; la única regla obligatoria es que tenga el nombre de archivo necesario.

Sin embargo, la manera recomendada es dejar que Claude Code lo genere por nosotros.
Para hacer que Claude Code genere un archivo `CLAUDE.md` para el proyecto actual, basta con usar el comando `/init`. Una vez aplicado el comando, empezará a analizar nuestro proyecto en busca de convenciones, comandos importantes, información relevante, etc.

A medida que el proyecto va creciendo y evolucionando, es importante mantener actualizado este archivo. También podemos realizar modificaciones manuales si así lo requerimos.

Para más detalles sobre la estructura del CLAUDE.md: [[Recomendaciones CLAUDE.md]]

>**Nota**: En otras herramientas, en lugar del archivo `CLAUDE.md` se usa `AGENTS.md`, pero el objetivo es el mismo: darle al agente el contexto necesario del proyecto al iniciar la conversación.

---

# Recursos

- [Proyecto Linkstash](https://github.com/AleksPrz/linkstash)
- [Historia de Claude Code](https://www.anthropic.com/features/making-of-claude-code)
- [Agent Loop](https://code.claude.com/docs/en/agent-sdk/agent-loop)
- [Claude Code: Session Management](https://claude.com/blog/using-claude-code-session-management-and-1m-context)
- [Claude Code: Memory](https://code.claude.com/docs/en/memory)
- [markitdown](https://github.com/microsoft/markitdown)
