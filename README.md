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
| **Patrones de agentes - cheatsheet** | Resumen de los 42 patrones, uno por sección: nombre, diagrama Mermaid y cuándo aplicarlo, con referencia a la sección de la guía. | [MD](md/Patrones%20de%20agentes%20-%20cheatsheet.md) · [PDF](pdf/Patrones%20de%20agentes%20-%20cheatsheet.pdf) |
| **A2A (Agent2Agent) - guía para arquitectos** | Protocolo Agent2Agent (especificación 1.0): diferencia con MCP, Agent Card y descubrimiento, ciclo de vida de las tareas, modos de interacción (bloqueante, streaming, push), operaciones, seguridad (incluida la prompt injection entre agentes), A2A como sistema distribuido, arquitectura empresarial de referencia, SDKs y CLI, y cómo encaja en ODD. | [MD](md/A2A%20%28Agent2Agent%29%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/A2A%20%28Agent2Agent%29%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |
| **A2A (Agent2Agent) - resumen ejecutivo** | Tres páginas para dirección y arquitectura empresarial: qué es A2A y por qué importa, cómo funciona sin entrar en detalle, A2A frente a MCP, arquitectura de referencia con registro y gateway, riesgos y cómo se gestionan, cuándo usarlo, recomendaciones y hoja de ruta. | [MD](md/A2A%20%28Agent2Agent%29%20-%20resumen%20ejecutivo.md) · [PDF](pdf/A2A%20%28Agent2Agent%29%20-%20resumen%20ejecutivo.pdf) |
| **De SDD a ODD - evolución para arquitectos** | Por qué Gentle AI pasó de SDD (varios artefactos: specs, plan, tareas) a ODD en la versión 4.x, qué se conserva, qué cambia y cómo trabaja ahora el orquestador. | [MD](md/De%20SDD%20a%20ODD%20-%20evoluci%C3%B3n%20para%20arquitectos.md) · [PDF](pdf/De%20SDD%20a%20ODD%20-%20evoluci%C3%B3n%20para%20arquitectos.pdf) |
| **RDD en el ciclo ODD - guía para arquitectos** | Receipt-Driven Development: revisión independiente basada en riesgo (passive, low, medium, high), candidato congelado, lenses, corrección acotada y cómo encaja en el ciclo ODD. | [MD](md/RDD%20en%20el%20ciclo%20ODD%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/RDD%20en%20el%20ciclo%20ODD%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |
| **ODD y OpenSpec, una buena pareja** | Cómo se complementan ODD (ejecución proporcional con evidencia) y OpenSpec (spec viva con deltas ADDED, MODIFIED y REMOVED): qué aporta cada uno, modelo de trabajo combinado y reglas de traspaso, cuándo usar cada uno (matriz y árbol de decisión), cinco ejemplos (bug, nueva capacidad, cambio de un requisito, agente de IA y proyecto existente), gobierno para arquitectos (CODEOWNERS, CI, ADR), antipatrones, adopción por fases y checklist. | [MD](md/ODD%20y%20OpenSpec%2C%20una%20buena%20pareja.md) · [PDF](pdf/ODD%20y%20OpenSpec%2C%20una%20buena%20pareja.pdf) |
| **CI para agentes y PoCs - guía práctica** | Paso a paso de las tres capas de CI para agentes: pre-commit, GitHub Actions con LLM fake y evals con modelo real (local con Ollama). Pensada para PoCs y agentes propios. | [MD](md/CI%20para%20agentes%20y%20PoCs%20-%20gu%C3%ADa%20pr%C3%A1ctica.md) · [PDF](pdf/CI%20para%20agentes%20y%20PoCs%20-%20gu%C3%ADa%20pr%C3%A1ctica.pdf) |
| **Worktrees en Claude Code - guía para arquitectos** | Cómo aislar el trabajo de varias sesiones de Claude Code con worktrees: cuándo usarlos, `.worktreeinclude`, `worktree.baseRef`, integración por PR y limpieza. | [MD](md/Worktrees%20en%20Claude%20Code%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/Worktrees%20en%20Claude%20Code%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |
| **Sandbox de seguridad para agentes - guía para arquitectos** | Cómo montar en el equipo local un sandbox para evaluar la seguridad de agentes: modelo de amenazas (OWASP Top 10 for Agentic Applications), contenedores endurecidos en red sin salida, proxy con allowlist por host, método y ruta, mundo falso determinista (búsqueda, web y pares A2A), canarios, modos de contención y de protecciones de red, catálogo de ataques, integración en ODD con `make security` y gobierno para llevarlo a CI y a la plataforma. | [MD](md/Sandbox%20de%20seguridad%20para%20agentes%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/Sandbox%20de%20seguridad%20para%20agentes%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |
| **Sandbox para defensas contra prompt injection - guía para arquitectos** | Laboratorio para demostrar los patrones de la sección 7 de la guía de patrones (lethal trifecta, Dual LLM, Plan-then-execute, Action-selector y Context minimization) con un agente de ejemplo por patrón en versión vulnerable y defendida: gateway de tools, registro de prompts, marcadores de contaminación, corpus de inyecciones, métricas de éxito de ataque, invariantes y utilidad, los patrones en la PoC A2A e integración en ODD. | [MD](md/Sandbox%20para%20defensas%20contra%20prompt%20injection%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/Sandbox%20para%20defensas%20contra%20prompt%20injection%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |
| **Skills y arneses (harness) aplicados a la IA** | Guía breve de las dos piezas que rodean al modelo: los **skills** (qué son, cuándo y cómo aplicarlos, más ejemplos y cómo evaluarlos y probarlos) y el **arnés** (qué es, componentes, cuándo y cómo aplicarlo, el arnés en la práctica), y cómo se combinan. Incluye seguridad (riesgos principales, lethal trifecta, revisión de un skill de terceros y qué va en cada capa), checklist, glosario y fuentes. | [MD](md/Skills%20y%20arneses%20aplicados%20a%20la%20IA.md) · [PDF](pdf/Skills%20y%20arneses%20aplicados%20a%20la%20IA.pdf) |
| **Cheatsheet: comandos y skills de Claude Code** | Referencia rápida basada en la documentación oficial de Claude Code: cómo funcionan los comandos slash, sesión y contexto, modelo, razonamiento y modos, proyecto, memoria y configuración, extensiones (MCP, plugins, hooks y agentes), revisión de código y Git, skills incluidos (bundled), cómo crear tus propios skills y cuenta, uso y utilidades. | [MD](md/Cheatsheet%20comandos%20y%20skills%20de%20Claude%20Code.md) · [PDF](pdf/Cheatsheet%20comandos%20y%20skills%20de%20Claude%20Code.pdf) |
| **Hooks aplicados a la IA - guía para arquitectos** | Control determinista sobre los agentes con los hooks de Claude Code: hooks frente a instrucciones y permisos, ciclo de vida y eventos, tipos de hook (command, http, mcp_tool, prompt, agent), contrato de entrada y salida, dónde se configuran, casos de uso (quality gates, protección de ficheros, contexto tras la compactación, auditoría, verificación antes de parar), relación con los patrones de guardrails, human-in-the-loop y lethal trifecta, seguridad de los propios hooks, problemas frecuentes y checklist. | [MD](md/Hooks%20aplicados%20a%20la%20IA%20-%20gu%C3%ADa%20para%20arquitectos.md) · [PDF](pdf/Hooks%20aplicados%20a%20la%20IA%20-%20gu%C3%ADa%20para%20arquitectos.pdf) |

## Por dónde empezar

| Si… | Lee, en este orden |
| --- | --- |
| Vienes de SDD (OpenCode o Gentle AI 3.x) | De SDD a ODD → ODD aplicado a la generación de agentes → RDD en el ciclo ODD |
| Vas a desarrollar tu primer agente | ODD aplicado a la generación de agentes (secciones 4 a 11) → CI para agentes y PoCs → Anexo A (Ollama) |
| Tienes que diseñar la arquitectura del agente | Patrones aplicados al desarrollo de agentes → cheatsheet como consulta rápida |
| Tus agentes deben colaborar con los de otros equipos o proveedores | A2A (Agent2Agent) → Patrones, secciones 6.9 y 7 |
| Trabajas con varias sesiones de Claude Code a la vez | Worktrees en Claude Code → patrón Aislamiento del workspace (5.4) |
| Quieres gastar menos cupo de Claude | ODD aplicado a la generación de agentes, Anexo C (Ahorro de costes) |
| El agente va a producción | RDD en el ciclo ODD → Patrones, secciones 6 a 8 (sistemas distribuidos, seguridad, operación) |
| Tu agente lee contenido no confiable y quieres probar su seguridad antes del merge | Sandbox de seguridad para agentes → A2A (Agent2Agent), sección 10 → RDD en el ciclo ODD |
| Tienes que elegir y demostrar una defensa de arquitectura contra la prompt injection | Patrones aplicados al desarrollo de agentes (sección 7) → Sandbox de seguridad para agentes → Sandbox para defensas contra prompt injection |
| Quieres que los cambios de comportamiento dejen una spec viva y revisable, sin papeleo en lo pequeño | ODD aplicado a la generación de agentes → ODD y OpenSpec, una buena pareja |
| Vas a extender Claude Code con skills, hooks o MCP | Cheatsheet: comandos y skills de Claude Code → Skills y arneses aplicados a la IA → Hooks aplicados a la IA |
| Tienes reglas que el agente no puede saltarse (proteger ficheros, auditar, bloquear acciones) | Hooks aplicados a la IA → Patrones aplicados al desarrollo de agentes (guardrails y bounded autonomy) |

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
- **Cheatsheet de patrones:** cada patrón es una sección con su diagrama Mermaid, en lugar de una fila de tabla, porque Obsidian y GitHub no dibujan Mermaid dentro de una celda de tabla.

*Última actualización: 10 de noviembre de 2026.*
