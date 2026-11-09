# Patrones aplicados al desarrollo de agentes · Cheatsheet

*Resumen de los 42 patrones de la guía para arquitectos empresariales y de soluciones*

9 de noviembre de 2026 · @whinedo · Hector Gracia de Azcoitia

> Los diagramas están en formato Mermaid: se visualizan en GitHub, GitLab, Obsidian, VS Code y la mayoría de editores Markdown.

**Licencia:** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.es). Detalles, contenido de terceros y aviso de responsabilidad en la sección «Licencia y avisos», al final del documento.

## Índice

- [Resumen](#resumen)
- Patrones
  - [Comportamiento](#comportamiento): un solo agente (§3)
  - [Coordinación](#coordinación): varios agentes (§4)
  - [Concurrencia](#concurrencia): trabajo a la vez (§5)
  - [Sistemas distribuidos](#sistemas-distribuidos): fallos parciales (§6)
  - [Seguridad](#seguridad): defensa frente a la prompt injection (§7)
  - [Operación](#operación): puesta en producción (§8)
- [Sobre la autoría](#sobre-la-autoría)
- [Licencia y avisos](#licencia-y-avisos)

## Resumen

Este cheatsheet condensa en una sola consulta los 42 patrones de la guía «Patrones aplicados al desarrollo de agentes». Para cada patrón recoge su diagrama y la señal que indica cuándo aplicarlo, agrupados en seis familias: comportamiento de un agente, coordinación entre agentes, concurrencia, sistemas distribuidos, seguridad frente a la prompt injection y operación en producción.

La idea que lo recorre es que un agente es un sistema distribuido con un componente no determinista en el centro. Por eso conviven los patrones clásicos (retry, idempotency key, saga, circuit breaker) con otros propios del LLM. La regla general es empezar por lo más simple (llamada única → workflow → agente → multiagente) y subir solo cuando la eval lo justifique.

El número de cada patrón (§3.1, §4.2…) remite a la sección de la guía con el detalle completo. La línea «Cuándo aplicarlo» sirve como primer filtro, no como sustituto del análisis de compromisos de la guía.

**Palabras clave:** agentes de IA, patrones de diseño, arquitectura de agentes, multiagente, sistemas distribuidos, prompt injection, evals.

## Comportamiento

Patrones de un solo agente: cómo razona, actúa y se controla.

### Augmented LLM · §3.1

**Cuándo aplicarlo.** Siempre: es el bloque base. Sobra solo si basta el conocimiento del modelo, sin datos ni acciones.

```mermaid
flowchart LR
  IN["Entrada"] -- "(1)" --> LLM["LLM"]
  LLM <-- "(2.a)" --> T["Tools<br/>acciones"]
  LLM <-- "(2.b)" --> R["Retrieval<br/>datos"]
  LLM <-- "(2.c)" --> M["Memoria"]
  LLM -- "(3)" --> OUT["Salida"]
```

### ReAct · §3.2

**Cuándo aplicarlo.** Los pasos dependen de lo que se descubre (búsqueda, diagnóstico). Siempre con control de bucles y presupuesto.

```mermaid
flowchart TB
  G["Objetivo"] -- "(1)" --> CK{"Control en código<br/>presupuesto, repetición,<br/>oscilación, progreso"}
  CK -- "(2) ok" --> TH["Razonar"]
  TH -- "(3)" --> D{"¿Tiene<br/>respuesta?"}
  D -- "(4) sí" --> R["Respuesta final"]
  D -- "(4) no" --> A["Actuar<br/>tool call"]
  A -- "(5)" --> OB["Observar<br/>resultado"]
  OB -- "(6) vuelve a 2" --> CK
  CK -- "(2) límite o bucle detectado" --> P["Cierre controlado<br/>respuesta parcial sin tools"]
  P -- "(3)" --> ESC["Escalar si procede"]
```

### Plan-and-Execute · §3.3

**Cuándo aplicarlo.** Tareas largas con plan visible y aprobable; modelo potente para planificar y barato para ejecutar.

```mermaid
flowchart TB
  G["Objetivo"] -- "(1)" --> P["Planner<br/>modelo potente"]
  P -- "(2)" --> PL["Plan<br/>pasos 1..n"]
  PL -- "(3)" --> E["Executor<br/>paso i"]
  E -- "(4)" --> RP{"Replanner<br/>¿plan válido?"}
  RP -- "(5) sí, quedan pasos: vuelve a 4" --> E
  RP -- "(5) no: vuelve a 2" --> P
  RP -- "(5) terminado" --> R["Resultado"]
```

### Evaluator-Optimizer · §3.4

**Cuándo aplicarlo.** Hay un criterio de calidad claro (tests, esquema, rúbrica) y la crítica mejora el resultado.

```mermaid
flowchart LR
  IN["Tarea"] -- "(1)" --> GEN["Generador"]
  GEN -- "(2)" --> C["Candidato"]
  C -- "(3)" --> EV{"Evaluador<br/>¿cumple la rúbrica?"}
  EV -- "(4) no: crítica, vuelve a 2" --> GEN
  EV -- "(4) sí" --> OUT["Salida"]
  EV -- "(4) intentos agotados" --> ESC["Escalar o<br/>mejor candidato"]
```

### Prompt chaining con gates · §3.5

**Cuándo aplicarlo.** Tarea que se divide en pasos fijos y conocidos, con contratos verificables entre pasos.

```mermaid
flowchart TB
  IN["Entrada"] -- "(1)" --> S1["Paso 1<br/>extraer"]
  S1 -- "(2)" --> G1{"Gate<br/>esquema válido"}
  G1 -- "(3) ok" --> S2["Paso 2<br/>redactar"]
  S2 -- "(4)" --> G2{"Gate<br/>política"}
  G2 -- "(5) ok" --> S3["Paso 3<br/>formatear"]
  S3 -- "(6)" --> OUT["Salida"]
  G1 -- "(3) falla" --> X["Error controlado"]
  G2 -- "(5) falla" --> X
```

### Routing · §3.6

**Cuándo aplicarlo.** Entradas de categorías distintas, o mandar lo fácil a un modelo barato o local y lo difícil a uno potente.

```mermaid
flowchart LR
  IN["Entrada"] -- "(1)" --> R{"Router"}
  R -- "(2) simple" --> A["Modelo ligero<br/>o local"]
  R -- "(2) compleja" --> B["Modelo potente"]
  R -- "(2) fuera de alcance" --> C["Respuesta fija"]
  R -- "(2) ambigua" --> H["Revisión humana"]
```

### Human-in-the-loop · §3.7

**Cuándo aplicarlo.** Acciones irreversibles o caras (pagos, borrados, envíos, producción) o riesgo alto en la spec.

```mermaid
sequenceDiagram
  participant AG as Agente
  participant ST as Checkpoint store
  participant H as Persona
  AG->>AG: (1) propone acción de riesgo alto
  AG->>ST: (2) guarda estado
  AG->>H: (3) solicita aprobación con acción y motivo
  alt aprueba o edita
    H->>AG: (4) decisión
    AG->>ST: (5) recupera estado
    AG->>AG: (6) ejecuta la acción aprobada
  else rechaza o vence el timeout
    H->>AG: (4) rechazo
    AG->>AG: (5) replanifica o termina
  end
```

### Memoria por capas · §3.8

**Cuándo aplicarlo.** Agentes de muchos turnos o documentos grandes: el contexto es un recurso escaso.

```mermaid
flowchart TB
  CTX["Ventana de contexto<br/>memoria de trabajo"]
  H["Historial largo"] -- "(1.a) compaction" --> CTX
  N["Notas externas<br/>ficheros, progreso"] <-- "(1.b) escribe y relee" --> CTX
  LT["Memoria de largo plazo<br/>Engram, vector store"] -- "(1.c) retrieval just-in-time" --> CTX
  CTX -- "(2) delega" --> SA["Subagente<br/>contexto limpio"]
  SA -- "(3) resumen corto" --> CTX
```

### Guardrails · §3.9

**Cuándo aplicarlo.** Siempre que el agente reciba entradas de terceros o pueda actuar fuera de su proceso.

```mermaid
flowchart LR
  U["Entrada"] -- "(1)" --> GI{"Guardrail<br/>de entrada"}
  GI -- "(2) ok" --> AG["Agente"]
  AG -- "(3) tool call" --> GT{"Guardrail<br/>de tool call"}
  GT -- "(4) permitida" --> T["Tools"]
  T -- "(5) resultado, vuelve a 3" --> AG
  AG -- "(3) respuesta" --> GO{"Guardrail<br/>de salida"}
  GO -- "(4) ok" --> OUT["Respuesta"]
  GI -- "(2) bloquea" --> X["Rechazo"]
  GT -- "(4) bloquea" --> X
  GO -- "(4) bloquea" --> X
```

### Bounded autonomy · §3.10

**Cuándo aplicarlo.** Todo agente con bucle: límites de pasos, tokens, tiempo y coste, con condición de parada.

```mermaid
stateDiagram-v2
  [*] --> Ejecutando: (1)
  Ejecutando --> Ejecutando: (2) paso con presupuesto disponible, se repite
  Ejecutando --> Completado: (2) objetivo cumplido
  Ejecutando --> Escalado: (2) presupuesto agotado o bucle detectado
  Ejecutando --> Fallido: (2) error no recuperable
  Escalado --> [*]: (3)
  Completado --> [*]: (3)
  Fallido --> [*]: (3)
```

## Coordinación

Cómo se reparten el trabajo varios agentes.

### Orchestrator-workers · §4.1

**Cuándo aplicarlo.** Subtareas que no se conocen de antemano pero, una vez definidas, son independientes (research).

```mermaid
flowchart TB
  IN["Tarea"] -- "(1)" --> O["Orquestador<br/>descompone"]
  O -- "(2.a)" --> W1["Worker 1"]
  O -- "(2.b)" --> W2["Worker 2"]
  O -- "(2.c)" --> W3["Worker n"]
  W1 -- "(3.a)" --> S["Orquestador<br/>sintetiza"]
  W2 -- "(3.b)" --> S
  W3 -- "(3.c)" --> S
  S -- "(4)" --> OUT["Resultado"]
```

### Jerárquico · §4.2

**Cuándo aplicarlo.** Un supervisor tendría demasiados workers o tools, o los equipos son dominios distintos.

```mermaid
flowchart TB
  T["Supervisor principal"] -- "(1.a)" --> A["Supervisor<br/>equipo A"]
  T -- "(1.b)" --> B["Supervisor<br/>equipo B"]
  A -- "(2.a)" --> A1["Worker A1"]
  A -- "(2.b)" --> A2["Worker A2"]
  B -- "(2.c)" --> B1["Worker B1"]
  B -- "(2.d)" --> B2["Worker B2"]
```

### Handoff · §4.3

**Cuándo aplicarlo.** Atención con áreas especializadas que se descubren durante la conversación. Un agente activo cada vez.

```mermaid
sequenceDiagram
  participant U as Usuario
  participant TR as Agente triage
  participant FA as Agente facturación
  participant TE as Agente técnico
  U->>TR: (1) consulta
  TR->>FA: (2) handoff con resumen y datos del cliente
  FA->>U: (3) resuelve la parte de facturación
  FA->>TE: (4) handoff, el problema es técnico
  TE->>U: (5) resuelve y cierra
```

### Group chat / debate · §4.4

**Cuándo aplicarlo.** Revisión desde varios puntos de vista o contraste de hipótesis, con condición de fin explícita.

```mermaid
flowchart TB
  M["Moderador<br/>turnos y fin"] <-- "(1) turno" --> P1["Agente<br/>seguridad"]
  M <-- "(3) turno" --> P2["Agente<br/>coste"]
  M <-- "(5) turno" --> P3["Agente<br/>arquitectura"]
  P1 <-- "(2) interviene" --> CH[("Conversación<br/>compartida")]
  P2 <-- "(4) interviene" --> CH
  P3 <-- "(6) interviene" --> CH
  M -- "(7) tras N rondas de 1 a 6, o consenso" --> D["Decisión o<br/>propuesta a humano"]
```

### Blackboard · §4.5

**Cuándo aplicarlo.** Problemas resueltos por aportaciones incrementales de especialistas sin orden fijo (diagnóstico).

```mermaid
flowchart TB
  BB[("Pizarra<br/>hipótesis, datos, decisiones")]
  C["Controlador<br/>elige quién actúa"] -- "(1) consulta el estado" --> BB
  A1["Agente logs"] <-- "(3) lee y escribe" --> BB
  A2["Agente métricas"] <-- "(3) lee y escribe" --> BB
  A3["Agente cambios"] <-- "(3) lee y escribe" --> BB
  C -. "(2) activa" .-> A1
  C -. "(2) activa" .-> A2
  C -. "(2) activa" .-> A3
```

### Pipeline secuencial · §4.6

**Cuándo aplicarlo.** Proceso con etapas claras y especialistas distintos, con contratos tipados entre etapas.

```mermaid
flowchart TB
  I["Ingesta"] -- "(1) esquema A" --> C["Clasificación"]
  C -- "(2) esquema B" --> E["Enriquecimiento"]
  E -- "(3) esquema C" --> R["Redacción"]
  R -- "(4) esquema D" --> V["Revisión"]
```

## Concurrencia

Qué hacer cuando varios agentes o instancias trabajan a la vez.

### Fan-out / fan-in · §5.1

**Cuándo aplicarlo.** Subtareas independientes donde importa la latencia: varias fuentes, ficheros o controles.

```mermaid
flowchart LR
  IN["Entrada"] -- "(1)" --> SP["Fan-out"]
  SP -- "(2.a)" --> W1["Tarea 1"]
  SP -- "(2.b)" --> W2["Tarea 2"]
  SP -- "(2.c)" --> W3["Tarea n"]
  W1 -- "(3.a)" --> AG["Fan-in<br/>agregador con timeout"]
  W2 -- "(3.b)" --> AG
  W3 -. "(3.c) tarde o fallida" .-> AG
  AG -- "(4)" --> OUT["Resultado<br/>con huecos marcados"]
```

### Map-reduce · §5.2

**Cuándo aplicarlo.** Corpus que no cabe en el contexto, sobre todo con modelos locales de contexto limitado.

```mermaid
flowchart LR
  D["Documento grande"] -- "(1)" --> SPL["Dividir en trozos"]
  SPL -- "(2.a)" --> M1["Map<br/>trozo 1"]
  SPL -- "(2.b)" --> M2["Map<br/>trozo 2"]
  SPL -- "(2.c)" --> M3["Map<br/>trozo n"]
  M1 -- "(3.a)" --> R1["Reduce<br/>parcial"]
  M2 -- "(3.b)" --> R1
  M3 -- "(3.c)" --> R2["Reduce<br/>parcial"]
  R1 -- "(4.a)" --> RF["Reduce final"]
  R2 -- "(4.b)" --> RF
```

### Voting · §5.3

**Cuándo aplicarlo.** Decisiones discretas de alto impacto donde un error cuesta más que varias llamadas.

```mermaid
flowchart LR
  IN["Entrada"] -- "(1.a)" --> V1["Ejecución 1"]
  IN -- "(1.b)" --> V2["Ejecución 2"]
  IN -- "(1.c)" --> V3["Ejecución 3"]
  V1 -- "(2.a)" --> AG{"Votación<br/>o juez"}
  V2 -- "(2.b)" --> AG
  V3 -- "(2.c)" --> AG
  AG -- "(3) consenso" --> OUT["Decisión"]
  AG -- "(3) desacuerdo" --> H["Revisión humana"]
```

### Aislamiento del workspace · §5.4

**Cuándo aplicarlo.** Varios agentes modifican el mismo repositorio o recurso a la vez.

```mermaid
flowchart LR
  REPO[("Repositorio<br/>main")] -- "(1.a)" --> WA["Worktree A<br/>agente A, rama a"]
  REPO -- "(1.b)" --> WB["Worktree B<br/>agente B, rama b"]
  WA -- "(2.a)" --> PRA["PR a + CI"]
  WB -- "(2.b)" --> PRB["PR b + CI"]
  PRA -- "(3) merge" --> REPO
  PRB -- "(4) merge" --> REPO
```

### Lease con fencing token · §5.5

**Cuándo aplicarlo.** Solo un agente debe actuar sobre un recurso y los agentes pueden colgarse o tardar.

```mermaid
sequenceDiagram
  participant A as Agente A
  participant L as Servicio de lease
  participant B as Agente B
  participant R as Recurso
  A->>L: (1) pide lease
  L-->>A: (2) concedido, token 34
  B->>L: (3) pide lease
  L-->>B: (4) denegado
  Note over A: se queda bloqueado esperando al LLM
  Note over L: el lease caduca
  B->>L: (5) pide lease
  L-->>B: (6) concedido, token 35
  B->>R: (7) escribe con token 35
  R-->>B: (8) ok
  A->>R: (9) escribe con token 34
  R-->>A: (10) rechazado, token antiguo
```

### Control optimista · §5.6

**Cuándo aplicarlo.** Estado compartido con conflictos poco frecuentes (pizarra, documento de tareas).

```mermaid
sequenceDiagram
  participant A as Agente A
  participant S as Estado compartido
  participant B as Agente B
  A->>S: (1.a) lee, versión 7
  B->>S: (1.b) lee, versión 7
  A->>S: (2) escribe si versión 7
  S-->>A: (3) ok, versión 8
  B->>S: (4) escribe si versión 7
  S-->>B: (5) conflicto
  B->>S: (6) relee, versión 8
  B->>B: (7) vuelve a decidir con el estado nuevo
  B->>S: (8) escribe si versión 8
  S-->>B: (9) ok, versión 9
```

### Backpressure · §5.7

**Cuándo aplicarlo.** Fan-out o colas contra un LLM con rate limits, o un modelo local con pocas peticiones simultáneas.

```mermaid
flowchart LR
  Q[("Cola de tareas")] -- "(1)" --> SEM{"Semáforo<br/>máx. N"}
  SEM -- "(2)" --> W["Workers"]
  W -- "(3)" --> API["API del LLM"]
  API -- "(4) 429 o latencia alta" --> CTRL["Control<br/>reduce N"]
  API -- "(4) ok sostenido" --> CTRL2["Control<br/>sube N"]
  CTRL -- "(5) vuelve a 2" --> SEM
  CTRL2 -- "(5) vuelve a 2" --> SEM
```

### Ejecución especulativa · §5.8

**Cuándo aplicarlo.** La latencia de cola importa más que el coste, solo con operaciones idempotentes.

```mermaid
sequenceDiagram
  participant C as Cliente
  participant M1 as Modelo A
  participant M2 as Modelo B
  C->>M1: (1) petición
  Note over C: supera el p95 sin respuesta
  C->>M2: (2) misma petición
  M2-->>C: (3) respuesta válida
  C->>M1: (4) cancela
```

## Sistemas distribuidos

Cómo sobrevivir a fallos parciales, red y no determinismo.

### Durable execution · §6.1

**Cuándo aplicarlo.** Agentes de larga duración, con aprobaciones humanas o pasos caros o con efectos.

```mermaid
sequenceDiagram
  participant W as Motor de workflow
  participant LOG as Registro de eventos
  participant AG as Paso del agente
  W->>AG: (1) paso 1, llamar al LLM
  AG-->>W: (2) resultado 1
  W->>LOG: (3) guarda resultado 1
  W->>AG: (4) paso 2, tool call
  AG-->>W: (5) resultado 2
  W->>LOG: (6) guarda resultado 2
  Note over W: el proceso cae
  W->>LOG: (7) reanuda y lee el registro
  Note over W: pasos 1 y 2 se reutilizan, no se repiten
  W->>AG: (8) paso 3
```

### Idempotency key · §6.2

**Cuándo aplicarlo.** Toda tool que crea, modifica o envía algo (pagos, tickets, correos, despliegues).

```mermaid
sequenceDiagram
  participant AG as Agente
  participant API as API destino
  AG->>API: (1) crear ticket, clave k1
  API->>API: (2) crea ticket 123 y guarda k1
  API--xAG: (3) timeout, no llega la respuesta
  AG->>API: (4) reintento, clave k1
  API-->>AG: (5) ticket 123, ya existía
  Note over AG,API: un solo ticket aunque haya dos peticiones
```

### Saga · §6.3

**Cuándo aplicarlo.** Acciones con efectos en varios sistemas sin transacción común.

```mermaid
flowchart LR
  S1["1 Reservar vuelo"] -- "(1)" --> S2["2 Reservar hotel"]
  S2 -- "(2)" --> S3["3 Cobrar"]
  S3 -- "(3) falla" --> C2["Compensar 2<br/>cancelar hotel"]
  C2 -- "(4)" --> C1["Compensar 1<br/>cancelar vuelo"]
  S3 -- "(3) ok" --> OK["Completado"]
```

### Transactional outbox · §6.4

**Cuándo aplicarlo.** El agente actualiza su estado y debe avisar a otros sin que ocurra lo uno sin lo otro.

```mermaid
flowchart LR
  AG["Agente"] -- "(1)" --> TX["Transacción local"]
  TX -- "(2.a)" --> ST[("Estado")]
  TX -- "(2.b)" --> OB[("Outbox")]
  OB -- "(3)" --> RL["Relay"]
  RL -- "(4)" --> BR[("Broker")]
  BR -- "(5)" --> A2["Otros agentes"]
```

### Coreografía por eventos · §6.5

**Cuándo aplicarlo.** Agentes de equipos distintos y muy desacoplados, procesos que crecen añadiendo consumidores.

```mermaid
flowchart LR
  A1["Agente ingesta"] -- "(1) documento.recibido" --> BR[("Broker")]
  BR -- "(2) documento.recibido" --> A2["Agente clasificación"]
  A2 -- "(3) documento.clasificado" --> BR
  BR -- "(4.a) documento.clasificado" --> A3["Agente cumplimiento"]
  BR -- "(4.b) documento.clasificado" --> A4["Agente archivo"]
```

### Retry con backoff y dead letter · §6.6

**Cuándo aplicarlo.** Todas las llamadas remotas (LLM, MCP, APIs). Nunca con errores permanentes.

```mermaid
flowchart LR
  CALL["Llamada<br/>con timeout"] -- "(1)" --> OK{"¿Ok?"}
  OK -- "(2) sí" --> R["Resultado"]
  OK -- "(2) no" --> TR{"¿Transitorio?"}
  TR -- "(3) no" --> DLQ[("Dead letter<br/>revisión")]
  TR -- "(3) sí" --> N{"¿Intentos<br/>restantes?"}
  N -- "(4) sí" --> W["Esperar<br/>backoff + jitter"]
  W -- "(5) vuelve a 1" --> CALL
  N -- "(4) no" --> DLQ
```

### Circuit breaker con fallback · §6.7

**Cuándo aplicarlo.** Dependencias externas que se degradan y existe una alternativa evaluada (otro modelo, local).

```mermaid
stateDiagram-v2
  [*] --> Cerrado: (1)
  Cerrado --> Abierto: (2) fallos sobre el umbral
  Abierto --> SemiAbierto: (3) pasa el tiempo de espera
  SemiAbierto --> Cerrado: (4) la llamada de prueba funciona, vuelve a 2
  SemiAbierto --> Abierto: (4) la llamada de prueba falla, vuelve a 3
  note right of Abierto
    Las llamadas van al fallback
    modelo alternativo o local
  end note
```

### Bulkhead · §6.8

**Cuándo aplicarlo.** Conviven agentes críticos e interactivos con batch, o varios equipos comparten proveedor.

```mermaid
flowchart TB
  subgraph P1["Compartimento crítico"]
    A1["Agente de soporte"] -- "(1.a)" --> Q1["Cupo 20<br/>clave 1"]
  end
  subgraph P2["Compartimento batch"]
    A2["Agente de informes"] -- "(1.b)" --> Q2["Cupo 5<br/>clave 2"]
  end
  Q1 -- "(2.a)" --> LLM["Proveedor LLM"]
  Q2 -- "(2.b)" --> LLM
```

### MCP y A2A · §6.9

**Cuándo aplicarlo.** MCP: tools reutilizables entre agentes. A2A: agentes de equipos distintos o de terceros.

```mermaid
flowchart LR
  subgraph EA["Equipo A"]
    AA["Agente A"] -- "(1) MCP" --> TA["MCP servers<br/>tools y datos de A"]
  end
  subgraph EB["Equipo B"]
    AB["Agente B"] -- "(3) MCP" --> TB["MCP servers<br/>tools y datos de B"]
  end
  AA -- "(2) A2A<br/>Agent Card, tareas" --> AB
```

### Tracing distribuido · §6.10

**Cuándo aplicarlo.** Desde el primer agente que sale del portátil, y siempre en multiagente.

```mermaid
flowchart TB
  R["invoke_agent orquestador"] -- "(1)" --> C1["chat<br/>modelo, tokens"]
  R -- "(2)" --> T1["execute_tool buscar"]
  R -- "(3)" --> S["invoke_agent worker<br/>otro proceso"]
  S -- "(4)" --> C2["chat"]
  S -- "(5)" --> T2["execute_tool leer"]
  R -- "(6)" --> C3["chat síntesis"]
```

## Seguridad

Defensas de arquitectura contra la prompt injection.

### Lethal trifecta · §7.1

**Cuándo aplicarlo.** Revisión de diseño de cualquier agente, antes de elegir sus tools.

```mermaid
flowchart LR
  P["Datos privados"] -- "(1.a)" --> X{"Las tres juntas<br/>= riesgo de exfiltración"}
  U["Contenido no confiable"] -- "(1.b)" --> X
  E["Canal de salida"] -- "(1.c)" --> X
  X -- "(2)" --> D["Defensa: quitar al menos una"]
```

### Dual LLM · §7.2

**Cuándo aplicarlo.** El agente procesa contenido no confiable y además ejecuta acciones.

```mermaid
flowchart TB
  U["Usuario"] -- "(1)" --> PL["LLM privilegiado<br/>tools, sin datos no confiables"]
  PL -- "(2) procesa $DOC" --> CT["Controlador<br/>código"]
  CT -- "(3.a)" --> QL["LLM en cuarentena<br/>sin tools"]
  EXT["Contenido no confiable"] -- "(3.b)" --> QL
  QL -- "(4) resultado como $VAR1" --> CT
  CT -- "(5) solo la referencia $VAR1" --> PL
  PL -- "(6) enviar $VAR1" --> CT
  CT -- "(7)" --> T["Tool<br/>con el valor real"]
```

### Plan-then-execute · §7.3

**Cuándo aplicarlo.** El plan se deduce de la petición del usuario, antes de leer contenido externo.

```mermaid
flowchart LR
  REQ["Petición del usuario"] -- "(1)" --> PLAN["Plan fijado<br/>1 leer, 2 resumir, 3 enviar al usuario"]
  PLAN -- "(2)" --> EX["Ejecución"]
  EXT["Contenido no confiable"] -- "(3)" --> EX
  EX -- "(4) solo acciones del plan" --> T["Tools"]
  EX -. "(4) acción nueva" .-> BL["Bloqueada"]
```

### Action-selector · §7.4

**Cuándo aplicarlo.** Traducir lenguaje natural a una acción conocida, sin encadenar pasos.

```mermaid
flowchart LR
  U["Petición"] -- "(1)" --> LLM["LLM selector"]
  LLM -- "(2)" --> L{"Lista fija<br/>de acciones"}
  L -- "(3)" --> A1["Ver pedido"]
  L -- "(3)" --> A2["Cambiar dirección"]
  L -- "(3)" --> A3["Hablar con una persona"]
  A1 -- "(4)" --> OUT["Respuesta al usuario<br/>sin volver al LLM"]
  A2 -- "(4)" --> OUT
  A3 -- "(4)" --> OUT
```

### Context minimization · §7.5

**Cuándo aplicarlo.** Chatbots sobre datos: la petición se convierte en consulta y se descarta el texto original.

```mermaid
flowchart TB
  U["Petición del usuario"] -- "(1)" --> LLM1["LLM<br/>extrae la consulta"]
  LLM1 -- "(2.a)" --> Q["Consulta estructurada"]
  Q -- "(3)" --> DB[("Datos")]
  DB -- "(4)" --> LLM2["LLM<br/>redacta la respuesta"]
  U -. "(2.b) eliminada del contexto" .-x LLM2
  LLM2 -- "(5)" --> OUT["Respuesta"]
```

## Operación

Cómo se pone en producción con confianza.

### Evals como quality gates · §8.1

**Cuándo aplicarlo.** Siempre: cualquier cambio de prompt, modelo, tool o patrón.

```mermaid
flowchart TB
  CH["Cambio"] -- "(1)" --> PC["pre-commit"]
  PC -- "(2)" --> CI["CI con LLM fake"]
  CI -- "(3)" --> EV["Evals con modelo real"]
  EV -- "(4)" --> TH{"¿Supera los<br/>umbrales por AC?"}
  TH -- "(5) sí" --> M["Merge"]
  TH -- "(5) no" --> FX["Corregir o<br/>revisar la spec"]
```

### Shadow mode · §8.2

**Cuándo aplicarlo.** Antes de sustituir un proceso existente o cambiar de modelo en un agente crítico.

```mermaid
flowchart LR
  T["Tráfico real"] -- "(1.a)" --> CUR["Sistema actual"]
  T -- "(1.b)" --> NEW["Agente nuevo<br/>en sombra, dry-run"]
  CUR -- "(2.a)" --> U["Usuario"]
  CUR -- "(2.b)" --> CMP["Comparación"]
  NEW -- "(2.c)" --> CMP
  CMP -- "(3)" --> EV["Nuevos casos de eval"]
```

### Canary con kill switch · §8.3

**Cuándo aplicarlo.** Cualquier cambio de modelo, prompt o tool en un agente en producción.

```mermaid
flowchart LR
  T["Tráfico"] -- "(1)" --> RT{"Router<br/>por porcentaje"}
  RT -- "(2) 95 %" --> V1["Versión actual"]
  RT -- "(2) 5 %" --> V2["Versión nueva"]
  V2 -- "(3)" --> MET{"¿Métricas<br/>dentro de umbral?"}
  MET -- "(4) sí" --> UP["Ampliar porcentaje"]
  MET -- "(4) no" --> RB["Revertir"]
  KS["Kill switch"] -. "en cualquier momento: desactiva" .-> V2
```

## Sobre la autoría

**Autor:** Hector Gracia de Azcoitia (@whinedo).

## Licencia y avisos

### Licencia

© 2026 Hector Gracia de Azcoitia. Este documento se distribuye bajo la licencia Creative Commons Atribución-CompartirIgual 4.0 Internacional ([CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.es)). Puedes copiarlo, redistribuirlo y adaptarlo para cualquier fin, también comercial, con dos condiciones:

- **Atribución:** reconocer la autoría, enlazar la licencia e indicar si has hecho cambios.
- **Compartir igual:** distribuir tus adaptaciones con esta misma licencia.

Atribución sugerida: «Patrones aplicados al desarrollo de agentes · Cheatsheet», de Hector Gracia de Azcoitia, licencia CC BY-SA 4.0.

### Contenido de terceros

- Los patrones se describen con palabras propias a partir de la guía «Patrones aplicados al desarrollo de agentes» y de las fuentes que esta cita; esas fuentes conservan sus propios derechos y no quedan cubiertas por esta licencia.
- Los nombres de patrones, protocolos (MCP, A2A) y marcos se mencionan por su nombre; sus especificaciones pertenecen a sus editores.
- Las marcas y nombres de productos pertenecen a sus titulares. Su mención no implica afiliación ni respaldo.

### Aviso de responsabilidad

Este cheatsheet tiene fines formativos y divulgativos. Su contenido es orientativo, puede contener errores y no sustituye el detalle de la guía ni una evaluación profesional de la arquitectura. Las herramientas, protocolos y prácticas citados cambian con rapidez, así que conviene verificar su estado antes de aplicar cualquier recomendación. El autor no se responsabiliza de las decisiones tomadas a partir de este documento.
