No existe una estructura universal que pueda ser elegida como “la mejor” para organizar un CLAUDE.md.

Sin embargo, anexamos una plantilla que a nosotros nos ha dado buenos resultados y que hemos estado utilizando en algunos de nuestros proyectos.

````markdown
# [Project Name]

[One paragraph: what the system does, who uses it, what problem it solves.]

---

## Quick Reference

- **Run**: `[command]`
- **Test**: `[command]`
- **Build**: `[command]`
- **Config**: [How to set up the local environment]

---

## Domain Context

### Users / Actors

<!-- Who interacts with the system and what they're allowed to do. -->

- **[Actor]**: [What they can do]

### Core Concepts

<!-- The main domain objects. One line each: name + what it represents. -->

- **[Concept]**: [What it is]

---

## Project Structure

<!-- Describe how your project is organized and why.
     Could be folders, modules, packages, services — whatever applies. -->

```text
[root]/
  [folder/module]/    # [What lives here and why]
  [folder/module]/    # [What lives here and why]
```

---

## How Things Work

<!-- Explain the non-obvious parts of your codebase.
     Each subsection is a pattern, convention, or system worth knowing.
     Add or remove subsections freely — only document what Claude would otherwise guess wrong. -->

### [Pattern or system name]

[Explain it. How does it work? How should new code use it?]

### [Pattern or system name]

[Explain it.]

---

## Conventions

### Naming

<!-- Explicit naming rules prevent drift. Give examples. -->

- **[Thing]**: [Rule] (`ExampleName`)
- **[Thing]**: [Rule] (`ExampleName`)

### [Other convention category]

<!-- e.g., error handling, testing, state management, styling, comments -->

- [Rule]
- [Rule]

---

## Workflow

<!-- How the team works: branching, commits, reviews, issue tracking.
     Skip anything that doesn't apply. -->

- **Branches**: [Convention]
- **Commits**: [Convention]
- **Reviews**: [Process]
- **Issues**: [Where they're tracked]
````


También recomendamos leer la [guía de buenas prácticas de Anthropic](https://code.claude.com/docs/es/best-practices#write-an-effective-claude-md).