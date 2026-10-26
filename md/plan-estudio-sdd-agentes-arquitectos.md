# Plan de Estudio: SDD, Programación Orientada a Agentes y Patrones de Agentes
### Para Arquitecto de Soluciones (Sistemas Distribuidos / Microservicios) y Arquitecto Empresarial

---

## 0. Cómo usar este plan

Este plan combina un **núcleo común** (Módulos 0-2) con **dos rutas especializadas**:

- **Ruta A — Arquitecto de Soluciones (Sistemas Distribuidos / Microservicios)**: foco técnico profundo en patrones de agentes, protocolos de comunicación inter-agente, contratos de servicio y resiliencia.
- **Ruta B — Arquitecto Empresarial**: foco en gobierno, portafolio, TOGAF/ArchiMate aplicado a IA agéntica, y adopción organizacional de SDD.

Ambas rutas convergen en un **módulo final de gobierno, observabilidad y proyecto capstone** (Módulo 6), porque en la práctica ambos roles terminan negociando las mismas decisiones: dónde vive la autoridad de decisión de un agente, cómo se audita, y cómo se especifica antes de construir.

Duración sugerida: **10-12 semanas**, dedicando 4-6 horas semanales (lectura + laboratorio práctico). Todos los libros marcados **[O'Reilly]** están disponibles en el catálogo de O'Reilly Learning Platform (antes "Safari Books Online"); los artículos marcados **[Medium]** son de acceso gratuito o con el límite de artículos mensuales de Medium.

---

## 1. Mapa de competencias objetivo

| Competencia | Ruta A (Soluciones) | Ruta B (Empresarial) |
|---|---|---|
| Fundamentos de SDD (Spec → Plan → Task → Code) | ✅ Profundo, con foco en contratos de servicio | ✅ Como capa de gobierno de IA |
| Patrones de arquitectura de agentes (orquestador, jerárquico, reflexivo, swarm) | ✅ Profundo | ⬜ Nivel conceptual |
| Protocolos de comunicación entre agentes (MCP, A2A) | ✅ Profundo | ⬜ Nivel conceptual, impacto en integración |
| Memoria y estado en sistemas multiagente distribuidos | ✅ Profundo | ⬜ Nivel conceptual |
| TOGAF / ArchiMate aplicado a sistemas de IA agéntica | ⬜ Nivel conceptual | ✅ Profundo |
| Gobierno, cumplimiento y "fitness functions" arquitectónicas | ✅ Aplicado a nivel de servicio | ✅ Profundo, a nivel de portafolio |
| Observabilidad, seguridad y evaluación de agentes | ✅ Profundo | ✅ Nivel de framework/política |
| Adopción organizacional y gestión del cambio | ⬜ Nivel conceptual | ✅ Profundo |

---

## 2. Módulo 0 — Fundamentos comunes de SDD y agentic AI (Semana 1-2)

**Objetivo**: entender qué es SDD, por qué surge, y qué distingue a un "agente" de un simple wrapper de LLM.

### Lecturas núcleo
- **[O'Reilly]** *AI Agents: The Definitive Guide* — especialmente el Cap. 2, "Architectures and Patterns: Planning, Reactivity, and Multi-Agent-Systems", que cubre máquinas de estado finito/jerárquicas como paradigma base de los agentes.
  `https://www.oreilly.com/library/view/ai-agents-the/0642572247775/ch02.html`
- **[O'Reilly]** *Building Agentic AI Systems* — Cap. 2, "Principles of Agentic Systems" (modelo CWD: Communication-Workflow-Decision; roles y responsabilidades entre agentes).
  `https://www.oreilly.com/library/view/building-agentic-ai/9781803238753/B31483_02.xhtml`
- **[Medium]** Manav Ghosh, *"Spec Driven Development: A Strategic Framework for AI-Driven Innovation"* — plantea SDD como capa de gobierno de IA, análoga a la arquitectura empresarial.
  `https://medium.com/@manavghosh/spec-driven-development-a-strategic-framework-for-ai-driven-innovation-f7c00844a6d0`
- **[Medium]** Victor Dibia, *"Top 5 Books to Help You Build AI Agents Like Claude Code in 2026"* — mapa curado de bibliografía adicional, útil como índice de referencia continuo durante todo el plan.
  `https://medium.com/@victor.dibia/top-books-to-help-you-build-ai-agents-like-claude-code-in-2026-074ab71ac0c1`

### Laboratorio
Instalar OpenSpec u otro framework SDD (Spec Kit) sobre un repo de juguete y recorrer el ciclo completo Spec → Plan → Task → Code una vez, sin agentes multi-rol, solo para internalizar el flujo.

---

## 3. Módulo 1 — Programación orientada a agentes: conceptos base (Semana 2-3)

**Objetivo**: dominar el vocabulario y los componentes de un agente (planificación, herramientas, memoria, bucle de razonamiento-acción) antes de pasar a sistemas multiagente.

### Lecturas núcleo
- **[O'Reilly]** *Building Applications with AI Agents: Designing and Implementing Multiagent Systems* (Michael Albada) — cubre roles, autonomía, límites de contexto compartido, y el ciclo de vida de la confianza entre agente y humano.
  `https://www.oreilly.com/library/view/building-applications-with/9781098176495/`
- **[O'Reilly]** *AI Agents in Action* — patrones de comportamiento de agentes, gestión de memoria y conocimiento, agentes production-ready.
  `https://www.oreilly.com/library/view/ai-agents-in/9781633436343/`
- **[Medium]** *"Agentic Patterns: Architectures for Coordinated AI Systems"* (The Learning Space) — buena introducción visual a por qué la coordinación multiagente reduce la carga de contexto por agente.
  `https://medium.com/@learning_37638/agentic-patterns-architectures-for-coordinated-ai-systems-34d9d8d8e1e2`

### Laboratorio
Construir un agente simple con bucle ReAct (razonar → actuar → observar) usando un framework a elección (LangGraph, CrewAI, o el SDK de Claude Agents), con una sola herramienta externa.

---

## 4. Módulo 2 — SDD aplicado en profundidad (Semana 3-4)

**Objetivo**: pasar de "SDD como concepto" a "SDD como práctica de ingeniería", incluyendo sus límites.

### Lecturas núcleo
- **[Medium]** Dave Patten, *"Spec-Driven Development with AI Agents: From Build to Runtime Diagnostics"* — trata la spec como ancla de todo el ciclo de vida, no solo de la generación de código inicial.
  `https://medium.com/@dave-patten/spec-driven-development-with-ai-agents-from-build-to-runtime-diagnostics-415025fb1d62`
- **[Medium]** Dave Patten, *"Using AI Agents to Enforce Architectural Standards"* — conecta SDD con CodeOps y con la aplicación de estándares arquitectónicos en tiempo real (relevante para ambas rutas).
  `https://medium.com/@dave-patten/using-ai-agents-to-enforce-architectural-standards-41d58af235a0`
- **[O'Reilly]** *"How Agentic AI Empowers Architecture Governance"* (O'Reilly Radar) — introduce el concepto de *fitness function* arquitectónica y su ejecución dentro del *service mesh* en arquitecturas de microservicios.
  `https://www.oreilly.com/radar/how-agentic-ai-empowers-architecture-governance/`

### Punto de reflexión (para ambos perfiles)
A esta altura conviene leer también, fuera del catálogo O'Reilly/Medium pero como referencia obligada del campo, el artículo de Birgitta Böckeler en martinfowler.com, *"Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl"* (octubre 2025), que propone la taxonomía spec-first / spec-anchored / spec-as-source y es citado como referencia por buena parte de la literatura posterior sobre SDD.

---

## Ruta A — Arquitecto de Soluciones (Sistemas Distribuidos / Microservicios)

### 5A. Módulo 3A — Patrones de arquitectura multiagente (Semana 5-6)

**Objetivo**: dominar los patrones de orquestación como si fueran los "patrones de diseño GoF" de los sistemas de agentes, y saber cuándo usar cada uno frente a un problema de distribución de responsabilidades.

### Lecturas núcleo
- **[O'Reilly]** *Agentic Architectural Patterns for Building Multi-Agent Systems* — arquitectura jerárquica orquestador/subagentes, coordinación vía protocolo Agent-to-Agent (A2A), y framework de observabilidad con *lifecycle callbacks* para trazabilidad, cumplimiento y control de costos.
  `https://www.oreilly.com/library/view/agentic-architectural-patterns/9781806029570/`
- **[Medium]** *"Multi-Agent System Patterns: A Unified Guide to Designing Agentic Architectures"* — propone describir cualquier sistema multiagente en cuatro dimensiones independientes (autoridad, secuenciación, comunicación, especialización), muy útil como *checklist* de diseño para un arquitecto.
  `https://medium.com/@mjgmario/multi-agent-system-patterns-a-unified-guide-to-designing-agentic-architectures-04bb31ab9c41`
- **[Medium]** Vipin Mishra, *"Mastering AI Agent Orchestration: A Guide to Design Patterns"* — cuatro patrones fundamentales de orquestación ilustrados con un caso de uso realista.
  `https://medium.com/@ivipin.mishra/mastering-ai-agent-orchestration-a-guide-to-design-patterns-22ca16e4dfb8`
- **[Medium]** vinitpahwa, *"7 AI Agent Orchestration Patterns Every Engineer Should Know"`
  `https://vinitpahwa.medium.com/7-ai-agent-orchestration-patterns-every-engineer-should-know-4db87222302f`
- **[Medium]** Bijit Ghosh, *"Agentic Design Patterns"* — ejemplo extendido de un sistema multiagente de trading (asignación de tareas, compartición de contexto, orquestación), útil como plantilla mental para modelar dominios propios.
  `https://medium.com/@bijit211987/agentic-design-patterns-cbd0aae2962f`

### Laboratorio
Diseñar (en un diagrama de arquitectura, no necesariamente implementado) un sistema con al menos tres patrones distintos —orquestador central, planificador+ejecutor, y agentes reflexivos con memoria— para un dominio de tu elección (ej. procesamiento de reclamos de seguros, onboarding bancario).

---

### 6A. Módulo 4A — Agentes en sistemas distribuidos y microservicios (Semana 6-7)

**Objetivo**: tender el puente explícito entre disciplina de microservicios (contratos, despliegue independiente, tolerancia a fallos) y arquitecturas de agentes.

### Lecturas núcleo
- **[O'Reilly]** *Building Microservices*, 2ª edición (Sam Newman) — referencia base sobre integración de servicios, testing y monitoreo de sistemas distribuidos; sigue siendo el marco conceptual sobre el que se apoyan casi todos los libros nuevos de "agentic microservices".
  `https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/`
- **[O'Reilly]** *AI and Microservices: Integrating AI into API Design and Distributed Microservice Architecture* — diseño de API con IA, mejores prácticas de escalabilidad y seguridad específicas para microservicios con componentes de IA.
  `https://www.oreilly.com/library/view/ai-and-microservices/9798868813061/`
- **[O'Reilly]** *Architecting AI Software Systems* — incluye un caso práctico de microservicios conversacionales con API gateway y flujo de conversación, y explica cómo evolucionar de "modelos estáticos" a "agentes dinámicos" dentro de una arquitectura de tiers clásica (cliente/presentación/aplicación/datos).
  `https://www.oreilly.com/library/view/architecting-ai-software/9781804615973/`

### Punto de énfasis para Ruta A
El artículo de Augment Code *"What Is Spec-Driven Development? A Complete Guide"* señala un problema real para arquitectos de sistemas distribuidos: las herramientas de SDD suelen mantener las especificaciones co-ubicadas con el código en un único repositorio, mientras que las arquitecturas modernas se extienden por microservicios, librerías compartidas y repositorios de infraestructura — la coordinación multi-repositorio sigue siendo un problema abierto que conviene evaluar caso por caso antes de adoptar una herramienta SDD específica a escala de plataforma.

### Laboratorio
Tomar dos microservicios existentes (o simulados) que se comunican vía API, y escribir una especificación SDD que cubra el contrato entre ambos (no solo el comportamiento interno de cada uno), incluyendo criterios de aceptación verificables por *contract testing*.

---

### 7A. Módulo 5A — Protocolos, memoria y estado distribuido (Semana 8)

**Objetivo**: entender los protocolos estándar de comunicación entre agentes (MCP, A2A) y las estrategias de memoria en sistemas multiagente distribuidos — el equivalente, en este dominio, a entender gRPC/REST/mensajería en microservicios clásicos.

### Lecturas núcleo
- **[O'Reilly]** *Agentic AI Data Architectures* — trata la memoria como infraestructura de primera clase (memoria de corto y largo plazo, episódica), y por qué la arquitectura de datos tradicional (stateless) no encaja bien con las necesidades de persistencia de los agentes; compara SQL distribuido, vector stores e híbridos.
  `https://www.oreilly.com/library/view/agentic-ai-data/0642572250140/`
- **[O'Reilly]** *AI Agents: The Definitive Guide* — capítulo sobre memoria (memoria de corto plazo, memoria de largo plazo episódica/semántica/procedimental, "topología de memoria" en sistemas multiagente).
  `https://www.oreilly.com/library/view/ai-agents-the/0642572247775/`
- **[Medium]** Balaram Panda, *"Agentic AI Design Patterns: Choosing the Right Multimodal & Multi-Agent Architecture (2022–2025)"* — catálogo comparativo de patrones de razonamiento y colaboración con implementación de referencia open source.
  `https://medium.com/@balarampanda.ai/agentic-ai-design-patterns-choosing-the-right-multimodal-multi-agent-architecture-2022-2025-046a37eb6dbe`

### Laboratorio
Diseñar la topología de memoria de un sistema con 3+ agentes distribuidos en distintos servicios: qué se guarda por agente (memoria local), qué se comparte (memoria de proyecto, tipo Engram), y qué política de expiración/consolidación aplica.

---

## Ruta B — Arquitecto Empresarial

### 5B. Módulo 3B — Arquitectura empresarial clásica y su extensión hacia IA agéntica (Semana 5-6)

**Objetivo**: partir de un dominio ya conocido por el arquitecto empresarial (TOGAF/ArchiMate) y mapear explícitamente dónde encajan agentes y SDD dentro del ADM (Architecture Development Method).

### Lecturas núcleo
- **[O'Reilly]** *Modeling Enterprise Architecture with TOGAF* (Desfray & Raymond) — referencia de modelado práctico con TOGAF, mapeo entre ArchiMate/UML/BPMN y los dominios de arquitectura (negocio, datos, aplicación, tecnología). Es la base conceptual sobre la que luego se ubican los "agentes" como nuevo tipo de actor/componente de aplicación.
  `https://www.oreilly.com/library/view/modeling-enterprise-architecture/9780124199842/`
- **[O'Reilly]** *Mastering the TOGAF® Standard: A Practical Translation of the World's Leading Architecture Framework* — traducción práctica y actualizada del estándar, incluye un capítulo sobre arquitectura ágil que es el puente más directo hacia cómo incorporar ciclos SDD dentro del Content Framework de TOGAF.
  `https://www.oreilly.com/library/view/mastering-the-togaf-r/9798868818141/`
- **[O'Reilly]** *"How Agentic AI Empowers Architecture Governance"* (O'Reilly Radar) — revisitar este artículo ya leído en el Módulo 2, pero ahora desde la óptica de portafolio: cómo las *fitness functions* de Building Evolutionary Architectures se convierten en el mecanismo de gobierno ejecutable cuando hay agentes de IA operando dentro de la arquitectura.
  `https://www.oreilly.com/radar/how-agentic-ai-empowers-architecture-governance/`

### Laboratorio
Ubicar "agentes de IA" y "especificaciones SDD" como artefactos nuevos dentro del Enterprise Metamodel de TOGAF: ¿son actores de la arquitectura de negocio, componentes de la arquitectura de aplicación, o ambos según el caso? Documentar con un diagrama ArchiMate simplificado.

---

### 6B. Módulo 4B — SDD como capa de gobierno de IA a nivel de portafolio (Semana 6-7)

**Objetivo**: usar SDD no como herramienta de un equipo de desarrollo sino como mecanismo de gobierno de las inversiones en IA a nivel de toda la organización.

### Lecturas núcleo
- **[Medium]** Manav Ghosh, *"Spec Driven Development: A Strategic Framework for AI-Driven Innovation"* (relectura enfocada) — el argumento central es que SDD funciona como capa de gobierno de IA de la misma manera en que la arquitectura empresarial guía las decisiones tecnológicas, creando un "contrato estratégico" entre la intención humana y la ejecución de la IA.
  `https://medium.com/@manavghosh/spec-driven-development-a-strategic-framework-for-ai-driven-innovation-f7c00844a6d0`
- **[Medium]** Anil Jain, *"Agentic AI Architectures And Design Patterns"* — panorama de arquitectura en capas (entrada, orquestación, agentes especializados) escrito explícitamente desde la perspectiva de un arquitecto de datos/IA, útil para dialogar con equipos técnicos sin perder el nivel de abstracción de portafolio.
  `https://medium.com/@anil.jain.baba/agentic-ai-architectures-and-design-patterns-288ac589179a`
- **[Medium]** Akanksha Sinha, *"Building Multi-Agent Architectures → Orchestrating Intelligent Agent Systems"* — plantea explícitamente por qué "ningún agente único puede automatizar de punta a punta los flujos de una organización", argumento central para justificar inversión en arquitecturas multiagente ante un comité de arquitectura.
  `https://medium.com/@akankshasinha247/building-multi-agent-architectures-orchestrating-intelligent-agent-systems-46700e50250b`

### Punto de énfasis para Ruta B
Conviene contrastar la promesa de SDD con datos de adopción real: reportes citados en la literatura reciente (p. ej. Deloitte "State of AI 2026") indican que solo una de cada cinco empresas cuenta con un modelo de gobierno maduro para agentes autónomos. Esto es un argumento útil para justificar, ante la dirección, por qué introducir SDD y gobierno de agentes de forma temprana y no como una ocurrencia tardía.

### Laboratorio
Redactar una "constitución" (constitution file, en la terminología de Spec Kit) a nivel de dominio de negocio: principios no negociables, estándares de cumplimiento y criterios de calidad que cualquier especificación SDD dentro de esa unidad de negocio debe respetar.

---

### 7B. Módulo 5B — Adopción, riesgo y gestión del cambio (Semana 8)

**Objetivo**: anticipar los problemas reales de adopción de SDD y arquitecturas de agentes a escala empresarial, más allá del entusiasmo técnico inicial.

### Lecturas núcleo
- **[Medium]** Dave Patten, *"Using AI Agents to Enforce Architectural Standards"* (relectura enfocada en la Ruta B) — el concepto de CodeOps aplicado a gobierno, cumplimiento e intención arquitectónica como artefactos versionados, ya no solo a infraestructura.
  `https://medium.com/@dave-patten/using-ai-agents-to-enforce-architectural-standards-41d58af235a0`
- **[O'Reilly]** *Modeling Enterprise Architecture with TOGAF*, capítulos sobre artefactos vinculados a partes interesadas (*stakeholder matrix*) — releer con la pregunta: ¿cómo cambia la matriz de stakeholders cuando un "agente" puede ser tanto ejecutor como, en cierto sentido, interesado técnico dentro del proceso?
  `https://www.oreilly.com/library/view/modeling-enterprise-architecture/9780124199842/B9780124199842099961.xhtml`

### Laboratorio
Diseñar un plan de adopción de SDD + gobierno de agentes en tres fases (piloto acotado → expansión controlada → estándar de portafolio), incluyendo criterios de salida/entrada entre fases y métricas de éxito no puramente técnicas (velocidad de entrega, tasa de incidentes, satisfacción de stakeholders).

---

## 8. Módulo 6 — Convergencia: gobierno, observabilidad y proyecto capstone (Semana 9-12, ambas rutas)

**Objetivo**: unir las dos rutas en un ejercicio conjunto que obligue a negociar las mismas decisiones desde ambos roles.

### Lecturas núcleo (ambas rutas)
- **[O'Reilly]** *Agentic Architectural Patterns for Building Multi-Agent Systems* — capítulo de observabilidad con *lifecycle callbacks*, trazabilidad granular para depuración, cumplimiento y gestión de costos (relevante tanto para el arquitecto de soluciones que lo implementa como para el arquitecto empresarial que lo audita).
  `https://www.oreilly.com/library/view/agentic-architectural-patterns/9781806029570/`
- **[Medium]** Dave Patten, *"Spec-Driven Development: Designing Before You Code (Again)"* — cierre del hilo de Patten sobre SDD, útil como síntesis antes del proyecto capstone.
  `https://medium.com/@dave-patten/spec-driven-development-designing-before-you-code-again-21023ac91180`

### Proyecto capstone (recomendado en pareja: un participante en cada rol)
Diseñar de punta a punta, para un dominio de negocio real o simulado con al menos 4 microservicios:

1. La **especificación SDD** de una nueva capacidad de negocio (Ruta A la escribe a nivel técnico/contrato de servicio; Ruta B valida que respete la "constitución" del dominio).
2. La **arquitectura de agentes** que va a implementarla: patrón de orquestación elegido y por qué, protocolo de comunicación entre agentes, y topología de memoria.
3. El **mapeo en TOGAF/ArchiMate** de esos agentes como artefactos de arquitectura (negocio, aplicación, tecnología).
4. Un **modelo de gobierno**: quién aprueba qué, qué decisiones puede tomar un agente de forma autónoma vs. cuáles requieren escalamiento humano, y cómo se audita.

---

## 9. Cronograma resumido

| Semana | Módulo | Enfoque |
|---|---|---|
| 1-2 | Módulo 0 | Fundamentos comunes de SDD y agentic AI |
| 2-3 | Módulo 1 | Programación orientada a agentes: conceptos base |
| 3-4 | Módulo 2 | SDD aplicado en profundidad |
| 5-6 | 3A / 3B | Patrones multiagente **(A)** / TOGAF + IA agéntica **(B)** |
| 6-7 | 4A / 4B | Agentes en microservicios **(A)** / SDD como gobierno de portafolio **(B)** |
| 8 | 5A / 5B | Protocolos y memoria distribuida **(A)** / Adopción y gestión del cambio **(B)** |
| 9-12 | Módulo 6 | Gobierno, observabilidad y proyecto capstone conjunto |

---

## 10. Recursos complementarios (fuera de O'Reilly/Medium, referencia práctica)

- **OpenSpec** (herramienta SDD open source): `https://github.com/Fission-AI/OpenSpec/`
- **GitHub Spec Kit** (implementación de referencia de SDD, multi-agente): `https://github.com/github/spec-kit`
- Birgitta Böckeler (Thoughtworks), *"Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl"*, martinfowler.com — taxonomía spec-first / spec-anchored / spec-as-source, referencia obligada citada por buena parte de la literatura posterior.

*Nota: el catálogo de O'Reilly y los artículos de Medium cambian con frecuencia (algunos títulos aparecen como "early release"). Antes de asignar una lectura a un equipo, conviene verificar en la propia plataforma que el libro siga disponible y que la edición sea la vigente.*
