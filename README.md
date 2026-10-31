# Guías de desarrollo de agentes con IA

Guías para arquitectos empresariales y de soluciones sobre cómo especificar, construir, revisar y operar agentes de IA con **Claude Code** (app de escritorio de macOS), **Gentle AI 4.x** (ODD), **Engram** y modelos locales con **Ollama**.

Cada guía está en dos formatos con el mismo nombre de fichero:

- `md/`: fuente en Markdown. Los diagramas están en Mermaid, que GitHub y Obsidian dibujan directamente.
- `pdf/`: versión maquetada para leer o imprimir, generada a partir del Markdown.

## Estructura

```text
ai-docs/
├── README.md
├── md/     # fuentes Markdown (Mermaid para los diagramas)
└── pdf/    # versión maquetada de cada guía
```

## Guías

| Guía | Qué cubre | Formatos |
| --- | --- | --- |
| **ODD aplicado a la generación de agentes** | Guía principal. Ciclo spec → ODD → CI para desarrollar agentes con Claude Code y Gentle AI 4.x: template de spec, documento de feature, evals y LLM-as-judge, estructura del repositorio, configuración (CLAUDE.md, skills, subagente `spec-reviewer`), gobierno por riesgo, primeros pasos con una spec ya escrita y tres ejemplos completos (uno de seguridad). Anexos: Ollama con qwen3.5:9b, CI para agentes propios y ahorro de costes (Engram y una tarea por sesión). | [MD](md/ODD%20aplicado%20a%20la%20generaci%C3%B3n%20de%20agentes.md) · [PDF](pdf/ODD%20aplicado%20a%20la%20generaci%C3%B3n%20de%20agentes.pdf) |
| **Patrones aplicados al desarrollo de agentes** | Catálogo de 42 patrones con diagrama, cuándo aplicarlos y cuándo no: comportamiento (ReAct con control de bucles, Plan-and-Execute, Human-in-the-loop…), coordinación multiagente, concurrencia (lease, control optimista, backpressure), agentes como sistemas distribuidos (durable execution, idempotency, saga, circuit breaker, MCP y A2A), seguridad frente a prompt injection y operación. Incluye antipatrones, árbol de decisión y lecturas recomendadas. | [MD](md/Patrones%20aplicados%20al%20desarrollo%20de%20agentes.md) · [PDF](pdf/Patrones%20aplicados%20al%20desarrollo%20de%20agentes.pdf) |
| **Patrones de agentes - cheatsheet** | Resumen en una tabla de los 42 patrones: nombre, diagrama simplificado y cuándo aplicarlo, con referencia a la sección de la guía. | [MD](md/Patrones%20de%20agentes%20-%20cheatsheet.md) · [PDF](pdf/Patrones%20de%20agentes%20-%20cheatsheet.pdf) |
| **De SDD a ODD - evolución para arquitectos** | Por qué Gentle AI pasó de SDD (varios artefactos: specs, plan, tareas) a ODD en la versión 4.x, qué se conserva, qué cambia y cómo trabaja ahora el orquestador. | [MD](md/De%20SDD%20a%20ODD%20-%20evoluci%C3%B3n%20para%20arquitectos.md) · [PDF](pdf/De%20SDD%20a%20ODD%20-%20evoluci%C3%B3n%20para%20arquitectos.pdf) |
| **RDD en el ciclo ODD - guía para arquitectos** | Receipt-Driven Development: revisión independiente basada en riesgo (passive, low, medium, high), candidato congelado, lenses, corrección acotada y cómo encaja en el ciclo ODD. | [MD](md/RDD%20en%20el%20ciclo%20ODD%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/RDD%20en%20el%20ciclo%20ODD%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |
| **CI para agentes y PoCs - guía práctica** | Paso a paso de las tres capas de CI para agentes: pre-commit, GitHub Actions con LLM fake y evals con modelo real (local con Ollama). Pensada para PoCs y agentes propios. | [MD](md/CI%20para%20agentes%20y%20PoCs%20-%20gu%C3%ADa%20pr%C3%A1ctica.md) · [PDF](pdf/CI%20para%20agentes%20y%20PoCs%20-%20gu%C3%ADa%20pr%C3%A1ctica.pdf) |
| **Worktrees en Claude Code - guía para arquitectos** | Cómo aislar el trabajo de varias sesiones de Claude Code con worktrees: cuándo usarlos, `.worktreeinclude`, `worktree.baseRef`, integración por PR y limpieza. | [MD](md/Worktrees%20en%20Claude%20Code%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/Worktrees%20en%20Claude%20Code%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |

## Por dónde empezar

| Si… | Lee, en este orden |
| --- | --- |
| Vienes de SDD (OpenCode o Gentle AI 3.x) | De SDD a ODD → ODD aplicado a la generación de agentes → RDD en el ciclo ODD |
| Vas a desarrollar tu primer agente | ODD aplicado a la generación de agentes (secciones 4 a 11) → CI para agentes y PoCs → Anexo A (Ollama) |
| Tienes que diseñar la arquitectura del agente | Patrones aplicados al desarrollo de agentes → cheatsheet como consulta rápida |
| Trabajas con varias sesiones de Claude Code a la vez | Worktrees en Claude Code → patrón Aislamiento del workspace (5.4) |
| Quieres gastar menos cupo de Claude | ODD aplicado a la generación de agentes, Anexo C (Ahorro de costes) |
| El agente va a producción | RDD en el ciclo ODD → Patrones, secciones 6 a 8 (sistemas distribuidos, seguridad, operación) |

## Documentos anteriores

Se conservan como referencia. Su contenido está superado por las guías anteriores.

| Documento | Formatos | Sustituido por |
| --- | --- | --- |
| ODD vs SDD - guía para arquitectos | [MD](md/ODD%20vs%20SDD%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/ODD%20vs%20SDD%20-%20gu%C3%ADa%20para%20arquitectos.pdf) | De SDD a ODD - evolución para arquitectos |
| SDD aplicado a la generación de agentes | [PDF](pdf/SDD%20aplicado%20a%20la%20generaci%C3%B3n%20de%20agentes.pdf) (solo PDF) | ODD aplicado a la generación de agentes |
| Plan de estudio: SDD, programación orientada a agentes y patrones | [MD](md/plan-estudio-sdd-agentes-arquitectos.md) (solo MD) | Base SDD. Para patrones, ver la guía de patrones |

## Convenciones

- **Terminología:** los términos técnicos que se usan en inglés se mantienen en inglés (spec, eval, prompt, worktree, quality gates, fan-out…).
- **Versiones de referencia:** Gentle AI 4.x (ODD como ruta por defecto), Claude Code en la app de escritorio de macOS, Ollama con qwen3.5:9b en un MacBook M2 de 16 GB.
- **Actualización:** se edita el Markdown y se regenera el PDF con el mismo nombre. Las dos versiones de una guía se actualizan en el mismo commit.
- **Cheatsheet:** sus diagramas son imágenes PNG enlazadas desde `md/img/`, porque Obsidian y GitHub no dibujan Mermaid dentro de una celda de tabla. La carpeta tiene que estar junto al Markdown.

*Última actualización: 31 de octubre de 2026.*
