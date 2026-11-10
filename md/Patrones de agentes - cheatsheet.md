# Patrones aplicados al desarrollo de agentes · Cheatsheet

*Resumen de los 42 patrones de la guía para arquitectos empresariales y de soluciones*

9 de noviembre de 2026 · @whinedo · Hector Gracia de Azcoitia

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

El número de cada fila (§3.1, §4.2…) remite a la sección de la guía con el detalle completo. La columna «Cuándo aplicarlo» sirve como primer filtro, no como sustituto del análisis de compromisos de la guía.

**Palabras clave:** agentes de IA, patrones de diseño, arquitectura de agentes, multiagente, sistemas distribuidos, prompt injection, evals.

## Comportamiento

Patrones de un solo agente: cómo razona, actúa y se controla.

| Patrón | Diagrama | Cuándo aplicarlo |
| --- | --- | --- |
| **Augmented LLM**<br>§3.1 | ![Augmented LLM](img/01-augmented.png) | Siempre: es el bloque base. Sobra solo si basta el conocimiento del modelo, sin datos ni acciones. |
| **ReAct**<br>§3.2 | ![ReAct](img/02-react.png) | Los pasos dependen de lo que se descubre (búsqueda, diagnóstico). Siempre con control de bucles y presupuesto. |
| **Plan-and-Execute**<br>§3.3 | ![Plan-and-Execute](img/03-plan.png) | Tareas largas con plan visible y aprobable; modelo potente para planificar y barato para ejecutar. |
| **Evaluator-Optimizer**<br>§3.4 | ![Evaluator-Optimizer](img/04-evalopt.png) | Hay un criterio de calidad claro (tests, esquema, rúbrica) y la crítica mejora el resultado. |
| **Prompt chaining con gates**<br>§3.5 | ![Prompt chaining con gates](img/05-chaining.png) | Tarea que se divide en pasos fijos y conocidos, con contratos verificables entre pasos. |
| **Routing**<br>§3.6 | ![Routing](img/06-routing.png) | Entradas de categorías distintas, o mandar lo fácil a un modelo barato o local y lo difícil a uno potente. |
| **Human-in-the-loop**<br>§3.7 | ![Human-in-the-loop](img/07-hitl.png) | Acciones irreversibles o caras (pagos, borrados, envíos, producción) o riesgo alto en la spec. |
| **Memoria por capas**<br>§3.8 | ![Memoria por capas](img/08-memoria.png) | Agentes de muchos turnos o documentos grandes: el contexto es un recurso escaso. |
| **Guardrails**<br>§3.9 | ![Guardrails](img/09-guardrails.png) | Siempre que el agente reciba entradas de terceros o pueda actuar fuera de su proceso. |
| **Bounded autonomy**<br>§3.10 | ![Bounded autonomy](img/10-bounded.png) | Todo agente con bucle: límites de pasos, tokens, tiempo y coste, con condición de parada. |

## Coordinación

Cómo se reparten el trabajo varios agentes.

| Patrón | Diagrama | Cuándo aplicarlo |
| --- | --- | --- |
| **Orchestrator-workers**<br>§4.1 | ![Orchestrator-workers](img/11-orchestrator.png) | Subtareas que no se conocen de antemano pero, una vez definidas, son independientes (research). |
| **Jerárquico**<br>§4.2 | ![Jerárquico](img/12-jerarquico.png) | Un supervisor tendría demasiados workers o tools, o los equipos son dominios distintos. |
| **Handoff**<br>§4.3 | ![Handoff](img/13-handoff.png) | Atención con áreas especializadas que se descubren durante la conversación. Un agente activo cada vez. |
| **Group chat / debate**<br>§4.4 | ![Group chat / debate](img/14-groupchat.png) | Revisión desde varios puntos de vista o contraste de hipótesis, con condición de fin explícita. |
| **Blackboard**<br>§4.5 | ![Blackboard](img/15-blackboard.png) | Problemas resueltos por aportaciones incrementales de especialistas sin orden fijo (diagnóstico). |
| **Pipeline secuencial**<br>§4.6 | ![Pipeline secuencial](img/16-pipeline.png) | Proceso con etapas claras y especialistas distintos, con contratos tipados entre etapas. |

## Concurrencia

Qué hacer cuando varios agentes o instancias trabajan a la vez.

| Patrón | Diagrama | Cuándo aplicarlo |
| --- | --- | --- |
| **Fan-out / fan-in**<br>§5.1 | ![Fan-out / fan-in](img/17-fanout.png) | Subtareas independientes donde importa la latencia: varias fuentes, ficheros o controles. |
| **Map-reduce**<br>§5.2 | ![Map-reduce](img/18-mapreduce.png) | Corpus que no cabe en el contexto, sobre todo con modelos locales de contexto limitado. |
| **Voting**<br>§5.3 | ![Voting](img/19-voting.png) | Decisiones discretas de alto impacto donde un error cuesta más que varias llamadas. |
| **Aislamiento del workspace**<br>§5.4 | ![Aislamiento del workspace](img/20-aislamiento.png) | Varios agentes modifican el mismo repositorio o recurso a la vez. |
| **Lease con fencing token**<br>§5.5 | ![Lease con fencing token](img/21-lease.png) | Solo un agente debe actuar sobre un recurso y los agentes pueden colgarse o tardar. |
| **Control optimista**<br>§5.6 | ![Control optimista](img/22-optimista.png) | Estado compartido con conflictos poco frecuentes (pizarra, documento de tareas). |
| **Backpressure**<br>§5.7 | ![Backpressure](img/23-backpressure.png) | Fan-out o colas contra un LLM con rate limits, o un modelo local con pocas peticiones simultáneas. |
| **Ejecución especulativa**<br>§5.8 | ![Ejecución especulativa](img/24-hedged.png) | La latencia de cola importa más que el coste, solo con operaciones idempotentes. |

## Sistemas distribuidos

Cómo sobrevivir a fallos parciales, red y no determinismo.

| Patrón | Diagrama | Cuándo aplicarlo |
| --- | --- | --- |
| **Durable execution**<br>§6.1 | ![Durable execution](img/25-durable.png) | Agentes de larga duración, con aprobaciones humanas o pasos caros o con efectos. |
| **Idempotency key**<br>§6.2 | ![Idempotency key](img/26-idempotency.png) | Toda tool que crea, modifica o envía algo (pagos, tickets, correos, despliegues). |
| **Saga**<br>§6.3 | ![Saga](img/27-saga.png) | Acciones con efectos en varios sistemas sin transacción común. |
| **Transactional outbox**<br>§6.4 | ![Transactional outbox](img/28-outbox.png) | El agente actualiza su estado y debe avisar a otros sin que ocurra lo uno sin lo otro. |
| **Coreografía por eventos**<br>§6.5 | ![Coreografía por eventos](img/29-coreografia.png) | Agentes de equipos distintos y muy desacoplados, procesos que crecen añadiendo consumidores. |
| **Retry con backoff y dead letter**<br>§6.6 | ![Retry con backoff y dead letter](img/30-retry.png) | Todas las llamadas remotas (LLM, MCP, APIs). Nunca con errores permanentes. |
| **Circuit breaker con fallback**<br>§6.7 | ![Circuit breaker con fallback](img/31-circuit.png) | Dependencias externas que se degradan y existe una alternativa evaluada (otro modelo, local). |
| **Bulkhead**<br>§6.8 | ![Bulkhead](img/32-bulkhead.png) | Conviven agentes críticos e interactivos con batch, o varios equipos comparten proveedor. |
| **MCP y A2A**<br>§6.9 | ![MCP y A2A](img/33-mcpa2a.png) | MCP: tools reutilizables entre agentes. A2A: agentes de equipos distintos o de terceros. |
| **Tracing distribuido**<br>§6.10 | ![Tracing distribuido](img/34-tracing.png) | Desde el primer agente que sale del portátil, y siempre en multiagente. |

## Seguridad

Defensas de arquitectura contra la prompt injection.

| Patrón | Diagrama | Cuándo aplicarlo |
| --- | --- | --- |
| **Lethal trifecta**<br>§7.1 | ![Lethal trifecta](img/35-trifecta.png) | Revisión de diseño de cualquier agente, antes de elegir sus tools. |
| **Dual LLM**<br>§7.2 | ![Dual LLM](img/36-dual.png) | El agente procesa contenido no confiable y además ejecuta acciones. |
| **Plan-then-execute**<br>§7.3 | ![Plan-then-execute](img/37-planexec.png) | El plan se deduce de la petición del usuario, antes de leer contenido externo. |
| **Action-selector**<br>§7.4 | ![Action-selector](img/38-selector.png) | Traducir lenguaje natural a una acción conocida, sin encadenar pasos. |
| **Context minimization**<br>§7.5 | ![Context minimization](img/39-minimization.png) | Chatbots sobre datos: la petición se convierte en consulta y se descarta el texto original. |

## Operación

Cómo se pone en producción con confianza.

| Patrón | Diagrama | Cuándo aplicarlo |
| --- | --- | --- |
| **Evals como quality gates**<br>§8.1 | ![Evals como quality gates](img/40-evals.png) | Siempre: cualquier cambio de prompt, modelo, tool o patrón. |
| **Shadow mode**<br>§8.2 | ![Shadow mode](img/41-shadow.png) | Antes de sustituir un proceso existente o cambiar de modelo en un agente crítico. |
| **Canary con kill switch**<br>§8.3 | ![Canary con kill switch](img/42-canary.png) | Cualquier cambio de modelo, prompt o tool en un agente en producción. |

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
