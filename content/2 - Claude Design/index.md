
En este bloque abordamos [Claude Design](https://claude.com/product/design), una herramienta a la cual tienes acceso si ya cuentas con un plan de Claude que incluya Claude Code (al menos así es a día de hoy). 
El contenido de este bloque es una breve introducción a la herramienta, ya que Claude Design amerita tener su propio curso.
La inspiración de añadir esta herramienta al curso nos llegó después de ver [este video](https://youtu.be/j1oJi1Pfobs?si=zpeh0DpX44U2x9vc).
El objetivo de este bloque es que sepas que esta herramienta existe y que es perfecta para evitar los diseños de interfaces genéricos que la IA genera.

---
# Índice

- [Conceptos](#conceptos)
  - [Código de diseño](#código-de-diseño)
  - [Design System](#design-system)
- [¿Qué es Claude Design?](#qué-es-claude-design)
- [¿Por qué usarlo en lugar de solo Claude Code?](#por-qué-usarlo-en-lugar-de-solo-claude-code)
- [Flujo de trabajo: Design → Code](#flujo-de-trabajo-design--code)
- [Recomendaciones finales](#recomendaciones-finales)

---

# Contenido

## Conceptos

### Código de diseño
El **código de diseño** (o *brand code*) de un producto es el conjunto de decisiones visuales que definen su identidad: qué colores usa, qué tipografías, qué tipo de componentes, qué tono visual tiene.

Por ejemplo, el código de diseño de Notion es minimalista, con tipografía sans-serif, mucho espacio en blanco y colores apagados. El de Duolingo es vibrante, con verdes saturados, bordes redondeados y mucho contraste.

### Design System
Un **Design System** es la formalización del código de diseño en reglas reutilizables que garantizan consistencia en todo el producto. Contiene:

- **Paleta de colores**: primarios, secundarios, de acento, fondos y textos.
- **Tipografía**: familias de fuentes, tamaños, pesos y escala.
- **Componentes**: botones, tarjetas, formularios, modales, navegación, etc.
- **Layout**: sistemas de grilla, espaciado y estructura de página.

En Claude Design, el Design System se crea una sola vez por organización y se aplica automáticamente a todos los proyectos del equipo. De esta forma, todos los elementos visuales que generes estarán alineados con el producto desde el primer momento.
<br>
## ¿Qué es Claude Design?
**Claude Design** es una herramienta de diseño visual impulsada por IA. Permite crear elementos visuales de producción a partir de lenguaje natural y un Design System propio.

Claude Design puede crear:
- Maquetas e interfaces de producto
- Prototipos interactivos con animaciones
- Páginas web y landing pages responsivas
- Presentaciones y diapositivas
- Visualizaciones de datos e infografías
- Objetos y experiencias 3D (como globos interactivos o lentes giratorios)
- Materiales de marketing

> **Importante**: Claude Design no genera imágenes fotográficas. Renderiza HTML, CSS, JavaScript y SVG. Esto significa que todo lo que produce es código, lo que lo hace perfectamente compatible con Claude Code.

Para iterar sobre un diseño existen cuatro formas:
1. **Chat**: para cambios amplios o redirecciones.
2. **Comentarios en línea**: para feedback preciso sobre un elemento específico.
3. **Edición directa de texto**: para modificar copy sin re-promtear.
4. **Sliders de ajuste**: para afinar espaciado, color y layout manualmente.
<br><br>
## ¿Por qué usarlo en lugar de solo Claude Code?

Cuando le pedimos a Claude Code que construya una interfaz solo con un prompt de texto, el agente toma decisiones visuales de forma arbitraria: elige colores, fuentes, espaciados y layouts basándose en convenciones genéricas. El resultado es funcional, pero raramente coincide con lo que tenemos en mente.

El ciclo que solemos terminar siguiendo es:
1. Claude Code genera una interfaz genérica.
2. Nos damos cuenta de que no se parece a lo que queríamos.
3. Gastamos tokens corrigiendo colores, layouts, componentes, etc.
4. Repetimos el ciclo varias veces.

**Claude Design rompe ese ciclo**. En lugar de describir visualmente con palabras (algo difícil y ambiguo), diseñamos primero, vemos el resultado, lo ajustamos hasta que nos convence, y solo entonces se lo pasamos a Claude Code para que lo convierta en código de producción.

Podemos notar dos beneficios importantes:
- **Menos tokens gastados** en correcciones de código.
- **Resultados más precisos** porque Claude Code recibe un diseño concreto.
<br><br>
## Flujo de trabajo: Design → Code

La integración entre Claude Design y Claude Code que queremos seguir es el siguiente:

**1. Configurar el Design System (una sola vez)**
Crea o selecciona tu organización dentro de Claude Design y sube los materiales de tu marca: capturas del producto, guías de estilo, componentes de React, logos, tipografías. Claude analiza estos archivos y genera automáticamente el Design System con paleta de colores, tipografía, componentes y convenciones de layout.

**2. Diseñar el elemento visual**
Describe en lenguaje natural lo que quieres crear. Claude Design genera un primer borrador alineado con tu Design System. Itera usando chat, comentarios en línea y sliders hasta que el resultado sea el que buscas.

**3. Exportar el Handoff Bundle**
Una vez satisfecho con el diseño, exporta un **Handoff Bundle** para Claude Code. Este paquete contiene:
- `design.html` — el diseño completo con HTML, CSS y JS inlineados.
- `screenshots/` — capturas PNG de cada estado del diseño.
- `design-notes.md` — anotaciones sobre las decisiones de diseño.

**4. Implementar con Claude Code**
Claude Code entiende el Handoff Bundle de forma nativa. Al recibir el paquete, puede convertir el diseño en código de producción (Next.js, React, Vue, etc.) con precisión, sin necesidad de traducción ni interpretación adicional.

Otros formatos de exportación disponibles:
- **HTML**
- **PDF**
- **PPTX** 
- **Canva**
<br><br>
## Recomendaciones finales

**Ten un Design System bien definido**
El Design System es la base de todo. Cuanto más completo y preciso sea, más consistentes y alineados con tu producto serán los elementos visuales que Claude Design genere. Incluye ejemplos de diseños terminados: Claude extrae el código de diseño mucho mejor a partir de ejemplos visuales reales.

**Conoce tu código**
Claude Design genera HTML, CSS y JavaScript. Entender aunque sea los fundamentos de estos lenguajes te permite identificar si el código generado tiene problemas de accesibilidad, rendimiento o compatibilidad antes de pasárselo a Claude Code.

**Delega las tareas visuales pesadas a Claude Design**
Diseñar en código (directamente en Claude Code) puede funcionar para ajustes pequeños, pero para construir interfaces completas, presentaciones o prototipos interactivos, Claude Design es la herramienta correcta. Usa cada herramienta para lo que fue diseñada.

---

# Recursos

- [Introducing Claude Design - Anthropic](https://www.anthropic.com/news/claude-design-anthropic-labs)
- [Get started with Claude Design - Claude Help Center](https://support.claude.com/en/articles/14604416-get-started-with-claude-design)
- [Set up your Design System in Claude Design - Claude Help Center](https://support.claude.com/en/articles/14604397-set-up-your-design-system-in-claude-design)
- [Using Claude Design for prototypes and UX - Claude Tutorials](https://claude.com/resources/tutorials/using-claude-design-for-prototypes-and-ux)
- [Video Tutorial Claude Design](https://youtu.be/3RWm4inkS2E?si=DPQL-SpfnM1fViCP)
